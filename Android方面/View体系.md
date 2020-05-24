1、解释下对View的理解

2、Activity，Window，View，ViewRootImpl

3、LinearLayout，RelativeLayout，FrameLayout的区别

4、自定义View，如何创建自定义View

5、什么是ViewTree，如何优化它的深度

这类问题，都要从View的基础开始。

- Activity和Window

  Activity负责生命周期，以及接收响应事件。Window是接口，唯一实现是 PhoneWindow。每一个Activity都会有一个PhoneWindow，它的作用是负责容纳View体系。

  



