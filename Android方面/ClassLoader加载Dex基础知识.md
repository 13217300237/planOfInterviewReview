

# 基础

 ClassLoader是用来加载class的。**安卓**中的**ClassLoader**和**java的**有些不同。
 android中需要关注的**ClassLoader**是**BaseDexClassLoader**以及它的子类。
 主要有3个，**PathClassLoader**，**DexClassLoader**， 还有一个**InMemoryClassLoader**. 

  ![](C:\Users\adminstrator\AppData\Roaming\Typora\typora-user-images\image-20200525170949825.png)

 **InMemoryClassLoader** 是8.0之后才有的。

- PathClassLoader是 安卓源码中使用的类加载器。DexClassLoader是谷歌专门开放出来给开发者用的，用于加载外部的ClassLoader。

  比如，我们自己写的MainActivity 是 PathClassLoader加载的，而Activity类是BootClassLoader。

- PathClassLoader和DexClassLoader的区别

  进入源码看一眼，立马就知道，他们两个其实没有任何区别，非要说有区别，就是PathClassLoader没有指定dex文件的优化目录，它会把存放之后的**odex**放到默认位置。而 **DexClassLoader**，需要指定**Dex**优化目录，如果传空，或者空字符串，就会使用默认位置。 必须提到的是，这个odex优化目录，**必须使用app的私有目录**，不能使用/data/data/appPackageName之外的SD卡目录

- DexClassLoader 如何加载apk外部的dex文件。

  答，创建一个DexClassLoader，传入**外部dex文件的绝对路径**，**odex路径**，还有**外部依赖库（动态链接库.so,静态链接库.a）的路径**，  还有**父classLoader**，然后就可以用 这个**DexClassLoader对象**调用**loadClass**方法，传入全类名，查找到该类，如果指定位置的dex文件内没有这个类，就会报找不到class的异常。

  ![image-20200525173827649](C:\Users\adminstrator\AppData\Roaming\Typora\typora-user-images\image-20200525173827649.png)

- 什么是**双亲委托机制**？

  ClassLoader的loadClass，是啃老族，优先让**父ClassLoader**来加载。为什么要这么设计？BootClassLoader是谷歌用来加载Framework层的类的，我们自己写的类，会用PathClassLoader加载。这么设计，这样可以保证类的加载次数不重复，如果没有双亲委托机制，我们要加载一个Framework中的类，除了用PathClassLoader加载一次之外（加载之后发现找不到），然后才会去用BootClassLoader去加载Framework中的类。两次尝试加载。而使用双亲委托机制，直接使用父亲ClassLoader，直接就先加载了Framework层的类，少了一次操作。

  

- ClassLoader是如何加载一个类的？
  
  ![image-20200525175318992](C:\Users\adminstrator\AppData\Roaming\Typora\typora-user-images\image-20200525175318992.png)
  
  1. 首先，先**findLoadedClass**检查曾经是否已经加载过这个类，如果有，直接返回class对象即可。（实际上，**findLoadedClass**方法是一个native方法。。。）
  
  2. 如果不存在，也就是说 findLoadedClass返回的是空，就去检查父亲ClassLoader是不是空，如果父ClassLoader不是空，就把查找任务交给父亲ClassLoader.....一直往上递归，直到找到祖宗， 让它去加载这个类。
  
  3. 如果父亲ClassLoader是空，就会调用自身的findClass方法   
  
     ![img](https://upload-images.jianshu.io/upload_images/4100513-c50b8f76a3cf3f6a.png?imageMogr2/auto-orient/strip|imageView2/2/w/752/format/webp)
  
   查找的全过程如下：
  
     解读一下：
  
     通过dexElementList的遍历，它是一个数组 Element[] ，每一个Element相当于一个DexFile的封装，而一个DexFile相当于一个dex文件的封装。追踪到最深处，最底层还是用native去加载了一个类。
  
     
  

