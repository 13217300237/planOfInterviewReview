# LifeCycle

概念：18年的谷歌IO大会。

用来管理Activity，Fragment生命周期的组件。解决MVP开发模式的内存泄漏问题。它是基于观察者模式，使用注解的方式来让业务代码的开发更加简洁。

让业务组件可以自动感知Activity和Fragment生命周期。



其实要描述LifeCycle，一句话就可以：

> LifeCycle是一个Activity和Fragment生命周期感知的组件，基于观察者模式，谷歌创造它的目的是 解决MVP模式中P层持有V的Activity引用导致的内存泄漏问题和代码臃肿的问题。
>
> 具体的解决方式为：把Activity或者Fragment作为被观察者，把P作为观察者，当生命周期发生变化的时候，会通知P执行初始化，或者资源释放等操作。写代码的话，要用到 `@OnLifecycleEvent(Lifecycle.Event.ON_CREATE)` 这样的注解在P的接口上。

示例代码非常简单：

P接口：

```kotlin
import androidx.lifecycle.Lifecycle
import androidx.lifecycle.LifecycleObserver
import androidx.lifecycle.OnLifecycleEvent

interface BasePresenter : LifecycleObserver {
    
    @OnLifecycleEvent(Lifecycle.Event.ON_CREATE)
    fun onCreate()

    @OnLifecycleEvent(Lifecycle.Event.ON_DESTROY)
    fun onDestroy()
}
```

P实现类：

```kotlin
class MainPresenter : BasePresenter {
    override fun onCreate() {
        // 自动感知 onCreate 方法
        Log.d("MainPresenter", "onCreate")
    }

    override fun onDestroy() {
        // 自动感知 onDestroy 方法
        Log.d("MainPresenter", "onDestroy")
    }
}
```

Activity代码：

```kotlin
class MainActivity : AppCompatActivity() {

    private val mainPresenter = MainPresenter()
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        lifecycle.addObserver(mainPresenter) // 这里添加观察者P
    }
}
```

然后当app启动然后退出，你会看到这样的日志：

```java
2020-04-08 17:17:32.600 5992-5992/? D/MainPresenter: onCreate
2020-04-08 17:17:36.262 5992-5992/? D/MainPresenter: onDestroy
```

这样就避免了在Activity中频繁去写 P的资源释放的代码。