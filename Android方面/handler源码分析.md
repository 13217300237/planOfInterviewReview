# Handler

事件支架。Web中也存在类似的Ajax(异步操作框架).



Handler存在的意义: 解决**线程之间的通信**。顺便支持一下 **延时任务**。

android中存在主线程和子线程，我们可以用 主线程通过handler通知子线程，也可以子线程通过handler通知主线程。

<img src="C:\Users\adminstrator\AppData\Roaming\Typora\typora-user-images\image-20200518104332378.png" alt="image-20200518104332378" style="zoom:67%;" />

Handler架构，除了明面上的 **Handler-Message-MessageQueue-Looper**之外，还有一个背后提供动力的**Thread**类。

关键点解读：

线程通信的起点是  handler.sendXXXMessage() ，它会把一个Message对象放入传送带 MessageQueue，传送带由Looper负责驱动（for死循环），当Message到达传送带的另一端的时候，handler的dispatch就会去处理这个Message，一次通信就完成了。



# 跨线程核心原理

<img src="C:\Users\adminstrator\AppData\Roaming\Typora\typora-user-images\image-20200518105609978.png" alt="image-20200518105609978" style="zoom:67%;" />

核心原理：4个字，内存共享。

所有线程都在共享共一个数据区域，也就是**MessageQueue**。

# MessageQueue关键函数

阻塞队列。

<img src="C:\Users\adminstrator\AppData\Roaming\Typora\typora-user-images\image-20200518110022218.png" alt="image-20200518110022218" style="zoom:80%;" />

所有的消息都会附带一个delay属性，也就是延迟，默认延迟0。

当有一个消息需要插入队列的时候，通过对比delay以及当前时间。经过计算，将消息按照时间顺序插入到链表中。

消息队列由Looper的for死循环来驱动，调用MessageQueue的next函数，如果消息队列已经是空了，则会执行**nativePoolOnce**，这是一个native函数，c++层实现的，

```java
private native void nativePollOnce(long ptr, int timeoutMillis); /*non-static for callbacks*/
```

参数1，是**MessageQueue**自身的指针, 参数2，timeoutMillis 毫秒数,代表将要阻塞的时长,  这个阻塞的时长，是由MessageQueue队列头部的消息的执行时间决定的。









