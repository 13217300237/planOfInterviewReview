**Activity**是 使用频率最高的，它是UI界面的基本单位,android架构中四大组件之一(另外几个是service，广播 broadecast，内容提供器**ContentProvider**)



一个Activity，其内部构成其实是，

![image-20200518111743550](C:\Users\adminstrator\AppData\Roaming\Typora\typora-user-images\image-20200518111743550.png)

如上图，Window其实是一个顶层窗口，Window是一个抽象类，它有自己唯一一个实现类 PhoneWindow。

再往下，才是DecorView，它是一个FrameLayout,它内部包含一个LinearLayout线性布局，线性布局上半部分是TitleView，下半部分就是我们熟悉的ContentView。





