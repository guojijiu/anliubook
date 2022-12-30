### 队列源码分析

### 对象关系：

```
消息主体，生产者，消费者
```

### 生产者提交消息

#### 代码解析
```php
# 使用rabbitmq队列
       TestJob::dispatch();
```

1. 调用Illuminate\Foundation\Bus\Dispatchable类下的静态方法dispatch；
2. 调用Illuminate\Foundation\Bus\PendingDispatch类，在类的析构函数中调用Illuminate\Bus\Dispatcher类下的dispatch方法；
3. 继续调用同类的dispatchToQueue方法，在该方法中调用pushCommandToQueue方法;
4. 再调用VladimirYuldashev\LaravelQueueRabbitMQ\Queue\RabbitMQQueue类的push方法；
5. 调用同类的pushRaw方法，将消息进行提交。
