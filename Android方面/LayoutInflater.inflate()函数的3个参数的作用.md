# 以 最终调用的函数为准

![image-20200523145052914](LayoutInflater.inflate()函数的3个参数的作用.assets/image-20200523145052914.png)

一个`XmlPullParser parser`, `ViewGroup root`,`boolean attachToRoot`



##　第一个参数 parser

，我们一般不是传入的都是 R.layout.xxxx ，但是最终这个布局int值，系统会先取找到这个int值对应的具体布局文件，解析它的内容（xml格式的文件）成为一个XmlPullParser对象。所以，布局太复杂， 深度太大，造成xml解析太慢，也是Activity启动较慢的原因之一。



## 第二个参数，ViewGroup。

刚才第一个参数中，这个parser对象，会被转成一个View树。而这个View树，最终会附加到哪一个View的下面呢？就是这里的root参数指定的。源码中最终可以找到依据，

![image-20200523150259823](LayoutInflater.inflate()函数的3个参数的作用.assets/image-20200523150259823.png)

只有在root不为空的情况下，R.layout.xxxx布局中顶层View的属性才会生效，变成LayoutParams设置给temp（temp是createViewFromTag创建出来的View对象）

如果你给第二个参数root设置为空， 那么你的布局中的宽高属性，还有其他layoutparam相关属性都会生效。![image-20200523150614541](LayoutInflater.inflate()函数的3个参数的作用.assets/image-20200523150614541.png)

## 第三个参数 attachToRoot

跟踪三处代码：

![image-20200523151408621](LayoutInflater.inflate()函数的3个参数的作用.assets/image-20200523151408621.png)

第一处，如果root不为空，且attachToRoot为false，刚才说的布局参数才会生效，

![image-20200523151430060](LayoutInflater.inflate()函数的3个参数的作用.assets/image-20200523151430060.png)

如果root不为空，且attachToRoot是true，布局参数生效，并且直接把创建出来的view加到root中去。

如果root是空，或者attachToRoot是false，最终这个inflator方法会返回一个非空的View出去。

![image-20200523153057422](LayoutInflater.inflate()函数的3个参数的作用.assets/image-20200523153057422.png)

