

# 代码层面

```kotlin
	    var str = "abc"
        println("testString:" + str.hashCode())

        str += "a"
        println("testString:" + str.hashCode())

        str += "b"
        println("testString:" + str.hashCode())

	    val str2 = "abcab"
        println("testString:" + str2.hashCode())
```

从打印的结果可以看出：

```java
04-17 11:39:35.798 20374-20374/com.zhou.myapplication I/System.out: testString:96354
04-17 11:39:35.798 20374-20374/com.zhou.myapplication I/System.out: testString:2987071
04-17 11:39:35.798 20374-20374/com.zhou.myapplication I/System.out: testString:92599299
04-17 11:39:35.798 20374-20374/com.zhou.myapplication I/System.out: testString:92599299
```

前面3个都不同，但是第三个和第四个的`hashCode`完全一样。

这说明:

1) 只要内容变化了，String对象地址也会发生变化, 不可能是同一个对象

2）只要内容相同，String对象就是同一个！



# 源码中

String是final的封闭类。

String的长度和UID不能修改，

String的内容也不可以修改。



综上所述，String是不可变的类。