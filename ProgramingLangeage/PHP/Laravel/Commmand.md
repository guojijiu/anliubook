###计划任务源码分析

1.
```
* * * * * cd /path-to-your-project && php artisan schedule:run >> /dev/null 2>&1
```
如果是通过php启动，这是计划任务执行的命令，内容说明是指向你的项目，并且执行schedule:run，
```
* * * * * ：每分钟执行一次
/dev/null：输出到空设备，表示丢掉输出信息。
2>&1：将输出到标准错误的信息输出到标准输出设备(通常是屏幕)
```
2.
```
namespace Illuminate\Console\Scheduling;class ScheduleRunCommand extends Command
{
    public function handle()
    {
        foreach ($this->schedule->dueEvents($this->laravel) as $event) {
            if (! $event->filtersPass($this->laravel)) {
                continue;
            }

            if ($event->onOneServer) {
                $this->runSingleServerEvent($event);
            } else {
                $this->runEvent($event);
            }

            $this->eventsRan = true;
        }

        if (! $this->eventsRan) {
            $this->info('No scheduled commands are ready to run.');
        }
    }
}
```
解析：
当运行schedule:run后，会执行handle里面的方法，循环写在Console/Kernel.php下schdele(Schedule $schedule)中的命令；
在dueEvents函数中，主要做的就是转换为cron表达式的时间与当前时间戳进行对比，主要的代码片段：$this->schedule->dueEvents($this->laravel)
```
namespace Cron;
class CronExpression
{
   public function isDue($currentTime = 'now', $timeZone = null)
    {
        $timeZone = $this->determineTimeZone($currentTime, $timeZone);

        if ('now' === $currentTime) {
            $currentTime = new DateTime();
        } elseif ($currentTime instanceof DateTime) {
            //
        } elseif ($currentTime instanceof DateTimeImmutable) {
            $currentTime = DateTime::createFromFormat('U', $currentTime->format('U'));
        } else {
            $currentTime = new DateTime($currentTime);
        }
        $currentTime->setTimeZone(new DateTimeZone($timeZone));

        // drop the seconds to 0
        $currentTime = DateTime::createFromFormat('Y-m-d H:i', $currentTime->format('Y-m-d H:i'));

        try {
            return $this->getNextRunDate($currentTime, 0, true)->getTimestamp() === $currentTime->getTimestamp();
        } catch (Exception $e) {
            return false;
        }
    }
}
```
其中，需要注意的是，他拿的是当前时刻的所有任务，去一一比对，如果有任务中有比较耗时的，正好错过了下一次对比任务的时间，就会出现任务不执行的情况，对此的解决方案是，在运行比较耗时的计划中，增加->runInBackground()，将其放到后台去执行，也就是重新开一个artisan进程去处理，并行处理计划任务；

$event->filtersPass($this->laravel)
这个没怎么明白，看上去像是判断绑定进入了该容器；

$event->onOneServer
这个比较好理解，是否在一台服务器上执行

$this->runEvent($event);
这段是运行的核心代码，代码逻辑是：
```
    public function run(Container $container)
    {
        if ($this->withoutOverlapping &&
            ! $this->mutex->create($this)) {
            return;
        }

        $this->runInBackground
                    ? $this->runCommandInBackground($container)
                    : $this->runCommandInForeground($container);
    }
```
是否有失效时间，并且失效时间是否过去，补充一点，如果计划任务运行完了，会自动删掉缓存，具体使用的是什么缓存，根据CACHE_DRIVER配置而来；
接着$this->runInBackground，就是上面提到的，是否在后台运行，其实就是把脚本命令，后面加一个&；
最终，执行的计划任务代码：
```
    protected function runCommandInForeground(Container $container)
    {
        $this->callBeforeCallbacks($container);

        (new Process(
            $this->buildCommand(), base_path(), null, null, null
        ))->run();

        $this->callAfterCallbacks($container);
    }
```
$this->callBeforeCallbacks($container)和$this->callAfterCallbacks($container)，都很好理解，运行计划任务以前需要处理的事情，和运行计划任务以后，需要处理的事情，至于中间就是真正运行计划任务的代码，主要就是开启一个进程，将编写的计划任务转化为执行命令，执行完以后，关闭进程，再开启下一个进程，执行下一个计划任务。

如果你有自己的见解或者指错的地方，请发到邮箱l644522319@gmail.com