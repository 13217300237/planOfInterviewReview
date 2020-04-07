让Activity得到内存情况的通知。



```kotlin
class ExperienceGoldGuidingActivity : BaseActivity() {
  ...

    private val tag = "onTrimMemoryTag"
    /**
     * 重写它，可以让我们的app得知当前系统的内存情况
     */
    override fun onTrimMemory(level: Int) {
        super.onTrimMemory(level)
        Log.d(tag, "level  数字越大，内存越紧张  $level ")
        when (level) {
            ComponentCallbacks2.TRIM_MEMORY_COMPLETE -> {
                Log.d(tag, "当前进程处于LRU队列的几乎最后，如果内存进一步降低，系统将会优先回收")
            }
            ComponentCallbacks2.TRIM_MEMORY_MODERATE -> {
                Log.d(tag, "当前进程处于LRU队列的中间位置，释放内存可以帮助系统更好地运行app")
            }
            ComponentCallbacks2.TRIM_MEMORY_BACKGROUND -> {
                Log.d(tag, "当前进程已经进入了LRU队列，这是一个清理资源的好机会，如果用户返回app，可以高效地快速地重新构建这些资源")
            }
            ComponentCallbacks2.TRIM_MEMORY_UI_HIDDEN -> {
                Log.d(tag, "用户已经离开了我们的app界面，这个时候应该释放带有大型UI的内存分配，以便更好地管理内存")
            }
            ComponentCallbacks2.TRIM_MEMORY_RUNNING_CRITICAL -> {
                Log.d(tag, "当前app正常运行，但是系统一斤个根据LRU缓存规则杀掉了大部分缓存的进程")
            }
            ComponentCallbacks2.TRIM_MEMORY_RUNNING_LOW -> {
                Log.d(tag, "当前app正常运行，系统的资源已经非常低了，这个时候应该释放一些不必要的资源来提升系统性能")
            }
            ComponentCallbacks2.TRIM_MEMORY_RUNNING_MODERATE -> {
                Log.d(tag, "当前app正在运行，不会被杀死，但是当前系统内存已经有点低了")
            }
        }
    }
    
    // 
}
```

这里有个梗就是：

TRIM_MEMORY_UI_HIDDEN 这个模式，和Activity本身的onStop的区别：

当然有区别，前者指的是: 整个app的UI都看不见了。后者是指的 某一个Activity不可见。