# DataBinding的概念和用法



概念：

DataBinding是谷歌为了解决 view和model之间通信的问题专门设计的一个框架。现在常用于MVVM开发模式。

用法：

1）app module的build.gradle文件中，找到android{} 在其中加入 

```groovy
    dataBinding {
        enabled = true
    }
```

2) 布局文件xml 改造成layout结构，引入具体的viewModel

```xml
<layout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools">

    <data>

        <variable
            name="userModel"
            type="com.example.myapplication.User" />
    </data>
    <androidx.constraintlayout.widget.ConstraintLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        tools:context=".MainActivity">

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="@={userModel.name}"
            app:layout_constraintBottom_toBottomOf="parent"
            app:layout_constraintLeft_toLeftOf="parent"
            app:layout_constraintRight_toRightOf="parent"
            app:layout_constraintTop_toTopOf="parent" />

    </androidx.constraintlayout.widget.ConstraintLayout>
 </layout>
```

3) Java/Kotlin代码中，使用DataBindingUtil来加载视图，同时绑定ViewModel

```kotlin
class MainActivity : AppCompatActivity() {

    private val user = User()

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        val dataBinding =
            DataBindingUtil.setContentView<ActivityMainBinding>(this, R.layout.activity_main)
        dataBinding.userModel = user
    }

}
```

注意，这里的ActivityMainBinding （它是谷歌自动生成的一个类文件），它是对应着R.layout.activity_main的。



***

# 数据的单向绑定和双向绑定

上面的写法就是单向绑定（所谓单向绑定，就是Model-> View ，数据决定UI显示的内容，但是UI上内容如果变了，model并不受影响），

而双向绑定，就是数据变了，UI变，而且 UI变了,数据变。

单向绑定:

```xml
 <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="@{userModel.name}"
            />
```

```xml
 <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="@={userModel.name}"
            />
```

只是多了一个等于号.

基本知识就这么多。