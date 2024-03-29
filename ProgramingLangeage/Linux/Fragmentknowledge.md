### 碎片知识

1. I/O多路复用

```
I/O多路复用实际上是指多个连接的管理可以在同一进程。多路是指网络连接，复用只是同一个线程。在网络服务中，I/O多路复用起的作用是一次性把多个连接的事件通知业务代码处理，处理的方式由业务代码来决定。在I/O多路复用模型中，最重要的函数调用就是I/O 多路复用函数，该方法能同时监控多个文件描述符（fd）的读写情况，当其中的某些fd可读/写时，该方法就会返回可读/写的fd个数。
示例：将用户socket对应的fd注册进epoll（实际上服务器和操作系统之间传递的不是socket的fd而是fd_set的数据结构），然后epoll只告诉哪些需要读/写的socket，只需要处理那些活跃的、有变化的socket fd的就好了。这样，整个过程只在调用epoll的时候才会阻塞，收发客户消息是不会阻塞的。
```

2. 内存溢出；

```
内存空间<需要写入内存的数据；
比方：系统内存还剩下3M，但是要创建一个4M的数据，这时候内存溢出；
```

3. 内存泄漏：

```
内存使用完后，未释放空间，一直占用内存空间，长此以往，每次生成内存都未释放，这就是内存泄漏；
比方：系统用2G内存，一个画图进程占用500M，图画完了，内存没释放，就会一直占用500M，下一个用画图进程，也会占用500M，系统当前可用内存，除去现在正在使用的500M，只剩下1G，再来两次，内存就满了。
```
