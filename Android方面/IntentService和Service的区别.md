# Service:

Activity 是Android的门面负责前端，那么Service可以认为是Android的后台，负责执行不想被用户看到的一些任务。Service并不是运行在一个单独的线程中，所以如果在Service中进行耗时操作，同样会引起Anr无响应。

多聊几句吧

Service一般有两个状态，一个是启动状态，一个是绑定状态。前者用来执行后台任务，后者用于进程间通信。





# IntentService:

但是，如果使用IntentService，就不必担心耗时操作的问题。使用IntentService，只需要重写onHandleIntent，在这个方法中去写耗时操作就可以了。

进入IntentSerivce的源码可以发现：

在onCreate的时候，创建了一个线程，并且把线程的looper赋值给了一个Handler对象。 然后在onStart方法里执行了handler.sendMessage. 最后handler执行到了onHandlerIntent。



所以这样就必然导致IntentSerivce的几个特性：

1. intentService可以直接写耗时操作，因为它是由子线程去执行的耗时操作，不会引起主线程阻塞，也就不会引起ANR。

2. 多个任务可以同时start，但是任务会排队执行，因为排队是handler的特性。

   ```java
   04-04 16:30:14.540 1915-1939/com.example.myapplication D/MyIntentService: 11111
   04-04 16:30:17.540 1915-1939/com.example.myapplication D/MyIntentService: 计时完毕，22222
   04-04 16:30:17.541 1915-1939/com.example.myapplication D/MyIntentService: 11111
   04-04 16:30:20.542 1915-1939/com.example.myapplication D/MyIntentService: 计时完毕，22222
   ```

   注意看任务时间：第一个任务，是14->17,第二个任务是17->20

除此之外，intentSerivce还有几个特性，

1） IntentService的工作线程不容易被系统杀死，因为在创建的时候，线程的优先级是0

2）IntentSerivce执行完毕之后，会自动结束。在handler的handerMessage中，有一句代码stopSelf()，由AMS结束这个service.

所以说，如果我们有一个耗时操作，想要后台去执行，不想让用户感知，并且很危险，有可能崩溃，那么可以让IntentService去干。