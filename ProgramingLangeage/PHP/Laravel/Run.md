### 执行流程

```
laravel 运行流程：
1. 单入口，从index.php进入；
2. 加载composer的自动加载设置；
3. 在bootstrap/app.php，选择对应的实例（由http请求或者控制台内核）；
    http请求：
        1）在app/Http/Kernel.php，继承至Illuminate\Foundation\Http\Kernel，其中定义了一个bootstrappers，用来确定请求执行前的运行；
        2）还有middleware,middlewareGroups,routeMiddleware,middlewarePriority等请求中间件；
        3）Http内核的handle方法，相当于输入一个Http请求，返回Http响应；
    控制台请求：
        1）在app/Console/Kernel.php，继承至Illuminate\Foundation\Console\Kernel，其中定义了一个bootstrappers，用来确定请求执行前的运行；
        2）还有commands数组，定一个有哪些控制台脚本，以及schedule方法，定义了脚本执行过程中的一些细则；
4. 内核启动中最重要的是服务提供者，在config/app.php中providers中配置各个需要注册的类，在对应的类中调用register方法，当所有服务提供者注册后，boot方法才会被调用；
```
