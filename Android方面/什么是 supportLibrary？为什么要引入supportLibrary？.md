# support Library 

支持库。

 存在价值：版本兼容。



我们开发android程序，往往会有3个版本需要我们去选择，最小版本 minSdkVersion, 目标版本 tragetSDkVersion, 编译版本 compileSdkVersion. 

如果我们目标版本targetSdkVersion是24，但是谷歌出了最新的SDK版本是28，我们要保证我们的程序能够在28上正常运行（不崩溃，但是效果不一定一样），保证向后兼容。

 现在支持库比较省心了，直接引入androidX即可，不用再手动选择之前的v4,v9,v13,,,,,

