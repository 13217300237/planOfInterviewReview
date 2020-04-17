# 显式Intent

明确目标（四大组件）的情况下，执行Intent。

通常会写上目标类的class对象。

创建出一个Intent，方法也不止一种。

```kotlin
// 方式1,直接当前上下文+目标class对象
val intent = Intent(this, Main2Activity::class.java)
// 方式2，先创建对象，然后指定ComponentName，ComponentName的参数仍然是 上下文+目标class对象
val intent2 = Intent()
intent2.component = ComponentName(this, Main2Activity::class.java)
// 方式3：同样是指定ComponentName，只不过创建所使用的构造函数的参数，变成了两个字符串，全包名以及全类名
val intent3 = Intent()
intent3.component = ComponentName("com.zhou.myapplication", "com.zhou.myapplication.Main2Activity")

// 方式4：····
```

方式很多，但是归根结底，源码最终都走向了 构建 ComponentName。





# 隐式Intent

通过字符串匹配的方式，来启动一个Intent。系统会提供字符串常量供外界使用。

使用隐式Intent，必须在清单文件中修改目标四大组件的参数

```xml
	<!-- 加入这个是目标Inteng -->
	<activity android:name=".Main2Activity">
            <intent-filter>
                <action android:name="com.zhou.Main2Activity" />
                <category android:name="android.intent.Category.DEFAULT" />
            </intent-filter>
     </activity>
```

这里的action，就对应代码中的 `intent4.action`：

```kotlin
		// 隐式Intent
        val intent4 = Intent()
        intent4.action = "com.zhou.Main2Activity"
        startActivity(intent4)
```

或者调用打电话的时候：

```kotlin
    fun callPhoneActivity(){
        // 隐式Intent
        val intent4 = Intent()
        intent4.action = Intent.ACTION_CALL
        startActivity(intent4)
    }
```

但是，如果我们自己定义的Action和系统定义的Action冲突了，重名了，那么系统会找出符合这个命名的所有目标组件，然后同时弹窗，让用户选择打开哪一个.

