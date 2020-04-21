# 生命周期

onCreate（Activity对象被创建出来） 

-> onStart(可见不可交互)

-> onResume(可见可交互)

到了这里，就正常运行了一个Activity



当自己被另一个Activity盖住时：

-> onPause (部分可见，不可交互)

-> onStop(当被完全盖住时，会执行onStop，是盖住之后调用onStop！)



如果一个Activity被finish

onDestory(销毁Activity对象)



当从被盖住的状态 转向前台的时候

onRestart-> onStart->onResume



上面所说的都是正常情况下的生命周期，但是有异常情况，当手机内存不够用，当前Activity处于后台，系统可能会把Activity回收。一旦kill，我们可以重写 onSaveInstanceState()方法来保存当前Activity的状态，这里是使用bundle保存数据。当我们再次打开activity的时候，我们可以重写onRestoreInstanceState或者onCreate()中，通过参数bundle恢复数据。

# 这里并没有说到重要的点：

## 跳转

两个Activity A B，当从A跳转到B的时候，他们两个的生命周期顺序是：

首先是A执行 onPause，然后B会执行onCreate->onStart->onResume ，然后····。（**记忆方式：等幕后演员就绪了之后，幕布才能放下，不然让观众看到就很尴尬，看到一个ActivityB的半成品状态么？那肯定不行。**）

这里要分情况讨论：

- 如果B是完全覆盖住A，那么A之后会执行onStop。

  如果点击回退键，从B返回到A，那么：B会执行销毁流程：onPause->onStop->onDestory. 但是如果穿插A的生命周期函数，实际上，它是这样的。

  | B    | onPause   |
  | ---- | --------- |
  | A    | onRestart |
  | A    | onStart   |
  | A    | onResume  |
  | B    | onStop    |
  | B    | onDestory |

  **同样的记忆方式，这次A变成了幕后的，同样不能让观众看到A还没准备好的样子，所以必须等A 走完onRestart-onStart-onResume 到达完全可见可交互的状态之后，B才能onStop - onDestory。不能让观众看到A衣服还没穿好的样子。**

- 如果B是透明主题或者是对话框样式，A不会执行onStop，因为它毕竟还可见。

  这种情况下，A不会执行onStop。它就保持在onPause的状态。

  当此时点回退键的时候，B消失，A，则onResume恢复到可见可交互的状态。

  

# 横竖屏切换

一个Activity进行屏幕方向切换的时候，Activity会销毁重建，比如从原始的竖屏切换到横屏，Activity会完全重建。也就是说，当一个Activity正常竖屏显示之后，你切成横屏，它会，从onPause-onStop-onDestory 完全销毁，然后。onCreate-onStart-onResume重建成横屏Activity。

如果此时你再切成竖屏，是不是觉得还会重建？这个其实是要分情况讨论的，安卓架构的时候应该就考虑过这类问题，所以在清单文件中提供了类似的配置。如果你不配置（也就是使用默认配置）它就会销毁重建的过程。

算了，再多我也记不住了。知道一点，可以通过清单文件中的配置，来调整生命周期是否完整执行销毁重建的流程。

