# ANR的概念

全称：ApplicationNotResponding，应用程序未响应。Android系统对于事件要在一定时间之内完成，如果超过预定事件还没完成，系统就会判定这个程序ANR。

# 出现的场景

android四大组件都有可能造成ANR，Activity，Service，ContentProvider，BorderCast。

分别对应了：

- InputDispatching Timeout   超时时长 5秒
- Service Timeout   前台服务20秒，后台服务200秒
- BordercastQueue Timeout  前台广播10秒，后台广播60秒
- ContentProvider Timeout   10秒

# ANR日志分析

一旦发生ANR，系统会保存一份anr.trace日志在 data/anr/trace.txt

分析该文件可以:

- 定位ANR发生的时间点
- 分析是否有耗时的message，binder调用，锁的竞争，CPU资源的抢占
- 结合上下文来分析具体原因

# 如何避免ANR的发生

- 主线程只做UI相关的操作，避免耗时操作，比如：过度复杂的UI绘制，网络操作，文件IO操作。
- 避免主线程和工作线程发生锁的竞争，减少系统binder的调用，谨慎使用sharePreference，注意主线程执行provider query操作

总之，尽可能减少主线程的负载，让其空闲待命。



