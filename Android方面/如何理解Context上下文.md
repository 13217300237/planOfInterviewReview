按照关键字来理解吧





# 概念

Context，翻译：上下文。可以理解为应用的运行环境。`app`的一切行动，都会直接间接地依赖Context。

# 类继承关系

终极祖宗：Context

直接子类：`ContextImpl`, `ContextWrapper`

再往下：Service，Application，Activity（但是Activity和前面两个不是平级，它还有一个父类`ContextThemeWrapper`,这是由于Activity涉及到UI主题）

这是一个典型的 **装饰模式**，21种设计模式中的一种。

`ContextWrapper`并不会直接具有什么功能，具体的功能代码都在`ContextImpl`里面. 



# Activity,Service,Application 3个实现类的区别

与UI相关的，就只有 Activity，其他两个都不涉及到UI。

所以，show一个dialog，只能依赖Activity。而不能是其他两个。dialog必然依赖Activity。

# 内存泄漏的问题

不要让静态对象持有Activity的引用，否则会引起内存泄漏。可以改换为 长生命周期的applicationContext







