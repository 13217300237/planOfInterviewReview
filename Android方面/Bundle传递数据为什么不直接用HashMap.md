# android传递数据为什么不直接用hashMap而用Bundle

1. HashMap是数组+链表的结构，它适用于大数据量的存取。而Bundle内部使用的是ArrayMap，它适用于小数据量的情况。而android内部传递数据用Intent+Bundle，大多数场景是小数据量，如果使用HashMap反而会效率更低，而且浪费更多内存。所以考虑使用场景，Bundle要优于hashMap.
2. android使用的序列化方式是Parcelable，HashMap的序列化方式是serializable, 而Bundle使用的是Parcelable。

#  

