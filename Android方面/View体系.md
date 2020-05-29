1、解释下对View的理解

2、Activity，Window，View，ViewRootImpl

3、LinearLayout，RelativeLayout，FrameLayout的区别

4、自定义View，如何创建自定义View

5、什么是ViewTree，如何优化它的深度

这类问题，都要从View的基础开始。

- Activity和Window

  Activity负责生命周期，以及接收响应事件。

  Window是接口，唯一实现是 PhoneWindow。每一个Activity都会有一个PhoneWindow，它的作用是负责容纳View体系。
  
  Window相关的管理者是WMS（WindowManagerService），它负责所有window的隐藏和显示。任何window窗口都有自己的级别type，比如1-99是系统窗口，开发者不可以随意用。子窗口1000-1999等。Activity上显示的任何内容的绘制，其实都是被管理在Window中, 更具体一点，是**PhoneWindow -> DecorView -> ViewRootImpl**, 我们看到的任何View的变动，其实都是由ViewRootImpl内的代码在操控。
  
  
  
  Activity的管理者是AMS（ActivityManagerService），所有Activity都会受到ActivityManagerService的管束，由当前app进程与AMS进程进行binder通信，从而控制Activity的生命周期。
  
- Activity层级图
  
  ![image-20200528101612237](C:\Users\adminstrator\AppData\Roaming\Typora\typora-user-images\image-20200528101612237.png)
  
  那么，activity，window，wms之间是什么关系呢？
  
  ![image-20200528101822479](C:\Users\adminstrator\AppData\Roaming\Typora\typora-user-images\image-20200528101822479.png)
  
  每一个Activity都会存在一个顶层ViewRootImpl，而手机上多个app的多个viewRootImpl都会收到WMS的管控。ViewRootImpl与WMS通信都是通过Binder。
  
- 关于软键盘

  弹出的软件盘也是属于Window的一种。

- 绘制流程的开始在哪里？

  在**ViewRootImpl**的**doTraversal()**, 之后会顺序执行 measure layout 和draw .

- View 到底是个啥子东西？

  View体系下大概有这么一些子类，当然不包括自定义View：

  <img src="C:\Users\adminstrator\AppData\Roaming\Typora\typora-user-images\image-20200528104203063.png" alt="image-20200528104203063" style="zoom:150%;" />

- View的绘制原理

  一个View从创建出view对象到 显示在屏幕上，要经历 测量Measure 布局layout 绘制draw 三个过程。测量决定View的大小尺寸，布局决定view在父容器中的位置，draw决定View的内容。测量布局绘制的核心方法，都是在 ViewRootImpl的performTraversal里面，分别是performMeasure，performLayout，performDraw 这3个方法。

- 自定义View的分类

  自定义View 重写onMeasure，onDraw

  自定义ViewGroup，重写onMeasure，onLayout

  注意View才是基类，ViewGroup是继承View的。

- View自定义的4个构造函数

  第一个，一个context参数的， 是在代码中创建view对象时

  第二个，多了一个AttributeSet，这是在xml中使用View时会被系统调用到

  第三个，再多一个styleAttr参数，一般不会手动去调用

  第四个，再多出一个defStyleRes参数，当view有style属性时会调用，一般也很少用。（SDK21之后才有）

  这4个构造函数最好是写全，因为如果不写全，在AndroidStudio中预览自定义View的效果，可能看不到效果。

- 自定义属性AttributeSet

- 关于View坐标系

  以左上角为（0，0）原点，往右是x轴正方向，往下是Y轴正方向。一个View的width，height，其实是由left,top,right,bottom 运算得到的。

  getRawXxx()和getXxx() 前者是相当于屏幕的x方向偏移量。后者是相对于父容器。

- 为什么需要Measure这个过程，measure具体做了什么事？

  android为了让View自适应各种屏幕尺寸，给view的宽高尺寸 预设了wrap_content（-1），match_parent（-2）两个值，我们也可以直接写具体的100dp这样的。但是我们在xml里面给的尺寸值，在代码里面测量的时候都会转化成**MeasureSpec**对象，它用一个32位的int值来包含MODE和SIZE两种属性。

  **MODEL**：包含 **EXACT** 精确值,**AT_MOST** 想要多大有多大，但是不能超过,**UNSPECIFIED**父View未指定你有多大，你想要多大就可以多大。