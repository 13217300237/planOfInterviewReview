# 所谓代理模式

该模式中存在几个角色，**1，访问者，2，真实对象，3，代理对象。**

访问者只能通过代理对象，来访问真实对象。不能直接访问真实对象。代理对象是一个中介的作用。

项目中常见的代理模式。电脑桌面的快捷方式，用于可以通过快捷方式打开真实程序。

另外，当需要跨进程访问某一个对象的时候，比如，ActivityManagerService 。它会提供很多功能，但是客户端app需要使用的只是一个很少的一部分。这时候，利用一个代理对象，来控制给app开放的指定一些接口，而不需要向app开放很多用不着的接口。

**优点**：这种做法可以降低**使用者**和**被使用者**之间的耦合，也能降低系统的开销，

**缺点**：要写额外的代码，并且增加了一个中间对象，会降低程序执行的效率，增加系统代码的复杂性。

# 静态代理

所谓静态代理，就是这个代理类是固定的，他服务于一种需求，而不能兼顾多种需求。想要兼容另一种需求，必须另外搞一个新的代理类。

# 动态代理

相比于静态代理，动态代理类是动态变化的，我们在使用的时候甚至不知道代理类的名称，其实也不需要知道。

![image-20200528162218727](C:\Users\adminstrator\AppData\Roaming\Typora\typora-user-images\image-20200528162218727.png)

使用JDK的Proxy类，传入classLoader，需要实现的接口（可以多个，但是一般一个），然后是行为拦截 业务接口。

优点: 避免创建多个静态代理。

缺点：内核使用了反射，效率不高。

经典代表，Retrofit，它集成了 注解，反射，动态代理。对网络层的代码进行解耦。







