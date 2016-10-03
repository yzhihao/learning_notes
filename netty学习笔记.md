#netty
##IO的分类
* BIO:指的是同步阻塞IO
* IO：指的是伪异步IO,底层同步，使用消息队列等。
* NIO:指的是非阻塞IO，用IO多路复用达到1个线程处理多个客户端的目的。
* AIO:异步IO,不需要额外线程，异步处理IO。
##使用netty的原因
* jdk的原声api调用复杂，bug多，要求高，需熟悉多线程。
* netty易上手，功能强大。
##阻塞与异步
##阻塞：
首先一个IO操作其实分成了两个步骤：发起IO请求和实际的IO操作，同步IO和异步IO的区别就在于第二个步骤是否阻塞，如果实际的IO读写阻塞请求进程，那么就是同步IO，因此阻塞IO、非阻塞IO、IO复用、信号驱动IO都是同步IO，如果不阻塞，而是操作系统帮你做完IO操作再将结果返回给你，那么就是异步IO。阻塞IO和非阻塞IO的区别在于第一步，发起IO请求是否会被阻塞，如果阻塞直到完成那么就是传统的阻塞IO，如果不阻塞，那么就是非阻塞IO。
###异步:
简单的说`同步在编程里，一般是指某个操作执行完后，才可以执行后面的操作`拿到IO上来说``就是我要做完这个IO操作``才继续后面的操作```
异步则是，我交带了某个操作给系统（可以是windows，也可以是你自己的库），我呆会过来拿，我现在要去忙别的``拿到IO上说``我交带了某个IO操作给系统。。。。
（异步IO就像是开了一个系统）

##Builder模式。
Bootstrap的使用很像Builder模式，Bootstrap就是Builder，EventLoopGroup、Channel和Handler等是各种Part。稍有不同的是，准备好各种Part后，并不是直接build出一个Product来，而是直接通过connect()方法使用这个Product。（这里记得要构建tcp的粘包和拆包）

##NioEventLoop
也就是说，NioEventLoop在单线程里同时处理IO事件和其他任务，NioEventLoop尽量（但不能保证）按照给定的比率（默认为50%）来分配花在这两种事情上的时间。换句话说，我们不应该在NioEventLoop里执行耗时的操作（比如数据库操作），这样会卡死NioEventLoop，降低程序的响应性。
##channel
* Channel和ChannelHandlerContext都扩展了AttributeMap接口，因此每一个Channel和ChannelHandlerContext实例都可以像Map一样按照key来存取value
* AttributeMap实现必须是线程安全的，因此，attr()方法可以在任何线程里安全的调用
* AttributeKey必须是唯一的，因此最好定义成全局变量（比如static final类型）
* 默认的Attribute实现继承自AtomicReference，因此也是线程安全的