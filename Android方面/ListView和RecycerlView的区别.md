# 区别

只说几个重要的区别

1. RecyclerView 已经把ViewHolder模式融入了源码， 强制使用ViewHolder模式。ListView

| 序号 | ListView                                                     | RecyclerView               |
| ---- | ------------------------------------------------------------ | -------------------------- |
| 1    | 强制使用ViewHolder，默认已经使用了                           | 并不强制，只是建议         |
| 2    | 滑动方向可以横着可以竖着                                     | 只能竖着                   |
| 3    | 缓存机制4级缓存（屏幕内缓存，屏幕外缓存，自定义缓存，RecylerViewPool） | 两级缓存（屏幕内，屏幕外） |
| 4    | 缓存的是ViewHolder                                           | 缓存的是View               |
| 5    | 支持自定义子View的排布方式，瀑布流等                         | 只能竖着依次排下           |

