Java泛型：

他想知道的是，面试官考察的是，你平时有没有写自定义框架的习惯。

泛型T，传入到泛型类之后，其实是可以获取到它的真实类型的，我们可以拿到真实类型之后，把需要的参数转成真实对象的参数返回出去。

示例：

```kotlin
class MainActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        var callback: HttpCallback<HttpResData> = object : HttpCallback<HttpResData>() {
            override fun onSuccess(s: HttpResData?) {
                val code = s?.code
                val result = s?.result

                Log.d("callback", "$code $result")

            }

        }

        callback.onSuccess("{\"code\":\"200\",\"result\":\"ok\"}")
    }
}
```

```kotlin
/**
 * 又学了一招，原来泛型，还可以根据参数化类型，转化成实际类型的对象
 *
 * @param <T>
 */
abstract class HttpCallback<T> {
    fun onSuccess(json: String?) { // 我要json字符串转化成T的实际类型的对象
        val gson = Gson()
        // 通过反射API来获取实际类型
        val clz = analysisClassInfo(this)
        val objT = gson.fromJson(json, clz) as T
        onSuccess(objT)
    }

    /**
     * 获取对象的参数化类型,并且把第一个参数化类型的class对象返回出去
     *
     * @param obj
     * @return
     */
    private fun analysisClassInfo(obj: Any): Class<*> {
        val type = obj.javaClass.genericSuperclass //
        val param =
            (type as ParameterizedType?)!!.actualTypeArguments //  获取参数化类型
        return param[0] as Class<*>
    }

    abstract fun onSuccess(t: T?)
}
```

```kotlin
class HttpResData {
    var code: String? = null
    var result: String? = null
}
```

当**app**运行起来之后：

```java
05-03 16:48:30.504 2246-2246/? D/callback: 200 ok
```

# 结论

要把一个未知类型对象转化成泛型的实际类型对象：

常见的做法是：

```kotlin
obj.javaClass.genericSuperclass // 得到包含原始类型，参数化，数组，类型变量，基本数据
```

用上面的方法先获取到参数化类型，然后：

```kotlin
val param = (type as ParameterizedType?)!!.actualTypeArguments //  获取参数化类型
```

最后转化成class对象反馈出去，就是泛型对应的真实类型：

````kotlin
 return param[0] as Class<*>
````

最后用Gson将字符串转化成 这个类型的对象,反馈出去。这样就能避免手动对json字符串进行解析，直接用gson一步到位。

