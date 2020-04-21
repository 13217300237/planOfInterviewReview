# Fragment和Activity的关系

- Fragment依赖于Activity，它不能独立存在。Activity是Fragment的容器
- 一个Activity可以容纳多个Fragment
- 一个Fragment可以被多个Activity重用
- Fragment有自己的生命周期，同时它的生命周期也依赖于Activity的生命周期。Fragment可以自己接收事件。
- Activity运行时可以动态添加删除Fragment

# 使用Fragment可以解决如下问题

- 模块化：为了避免Activity的臃肿，我们可以把业务代码都放到Fragment中作为一个独立模块
- 代码复用：Fragment可以被多个Activity使用。
- 适配：大屏小屏都要适配时，可以把业务代码封装到Fragment中，判断屏幕尺寸是否可以容纳多个Fragment，可以容纳，则加载多个，不能容纳，则加载一个。其实这个也是 屏幕适配的一个小方案。