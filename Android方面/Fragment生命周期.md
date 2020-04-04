# Fragment生命周期



Fragment：可重复利用的碎片零件,必须依存Actvity而作用.

加载方式：xml中静态加载 / 代码动态加载。



Activity 生命周期：onCreate-onStart-onResume      >>>      onPause-onStop-onDestroy

Fragment呢？

一旦绑定到Activity上，就会先执行 onAttach , 然后与onCreate方法相对应的，Fragment这里比较复杂，首先是onCreate创建出Fragment的对象，然后是View的创建 onCreateView  、onViewCreated。 与Activity的onStart方法对应，Fragment的也是onStart，包括onPause，onStop都一样。  然后对应Activity的onDestroy，Fragment的这里又比较多幺蛾子，首先要销毁view，执行onDestroyView，然后onDestroy自己，最后和Activity解绑onDettach.



***

# Fragment通信

同一个Activity上的多个Fragment通信如何进行。

1） 通过附加的Activity作为么中间媒介来进行数据共享通信

​	比如：Activity中有Fragment1，Fragment2，Activity中定义一个数组，让Fragment1中的操作导致数组的变化，然后右Activity通知Fragment2 数组发生了变化。

2）面向接口编程。

由Activity持有一个接口的引用。而让Fragment去实现这个接口。

3） Fragment中去调用另一个Fragment的引用，然后调用方法

FragmentManager可以拿到任何Fragment。

4） EventBus/Rxjava，响应式编程。现在的方法多得很。

***

Fragment的基本操作

add/replace



add方法，是当前的Fragment加入到一个容器中，如果要操作隐藏/ 显示，调用show/hide方法即可

replace方法是在添加进去之前，先移除掉所有id相同的Fragment，然后再添加。

Fragment的管理是 按照事务提交的方式来执行的。

***

Fragment回退栈

addToBackStack / popBackStack

***

Fragment提交事务

有两个方法 commit / commitAllowingStateLoss

Android有可能在内存紧张的时候回收Fragment，这个时候，Fragment的状态已经丢失了，如果此时用commit方法来执行事务的提交，就会爆出状态异常。但是如果使用后者 commitAllowingStateLoss，不会报异常，最多就是提交无效。



说到这个，顺嘴提一句，Fragment会和ViewPager一起使用，而ViewPager的adatper有区分有状态的 FragmentPagerAdapter和无状态的FragementStatePagerAdapter，前者适用于页面数比较少的，一旦该页面被创建出来，并不会随着滑动而销毁。但是后面这个有状态的，它只会保存当前page，以及左右最多两个其他page，其他的一律不保存。这种时候的事务提交， 也要使用允许状态丢失的commitAllowingStateLoss方法。

***

Fragment+ViewPager懒加载问题

ViewPager不仅仅会初始化当前显示的这个Fragment，还会初始化后面一个。后面的Fragment其实可以等用户划过去之后再初始化。懒加载相关，Fragment有一个setUserVisibleHit（）方法，可以告知当前Fragment是否可见，这个方法会在view创建之前执行，也就是onCreateView之前执行。

具体 setUserVisibleHit执行的时机是，

1）Fragment对象刚刚被new出来，此时参数是false，setUserVisibleHit（false）

2）当Fragment可见的时候，setUserVisibleHit（true）

3） 当Fragment由可见变为不可见的时候，setUserVisibleHint(false)

