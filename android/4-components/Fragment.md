

#  Fragment

需要掌握：

**1、Fragment概念**

**2、创建一个Fragment并添加到Activity中**

**3、Activity和Fragment的交互方式**

**4、Fragment之间的交互方式**

**5、Fragment的生命周期**

## Fragment的定义

一种可以嵌入在activity中的片段，和activity类似，也有自己的布局，自己的生命周期。

## 添加fragment的两种方式

### 静态方式添加fragment（类似自定义控件的添加）

* step1: 创建一个fragment类，系统自动添加fragment的布局文件。
```kotlin
class FirstFragment : Fragment() {
    override fun onCreateView(
        inflater: LayoutInflater, container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View? {
        // Inflate the layout for this fragment
        return inflater.inflate(R.layout.fragment_first, container, false)
    }
}
```
fragment的布局文件如下：
```kotlin
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <Button
        android:id="@+id/buttonNext"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="center_horizontal"
        android:text="Button">
    </Button>
</LinearLayout>
```
* step2: 在activity的布局文件中添加fragment
```kotlin
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    tools:context=".MainActivity">

    <fragment
        android:id="@+id/fragmentRight1"
        android:name="com.jon.chapter0501.FirstFragment"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
         />
</LinearLayout>
```
运行即可得到

### 动态方式添加fragment
* step1: 创建fragment类，系统自动生成其对应的layout布局文件（和静态方法相同）

* step2：在main_activity中添加一个fragment_container的布局，用来装载fragment
```kotlin
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    tools:context=".MainActivity">

    <frameLayout
        android:id="@+id/fragment_container"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
         />
</LinearLayout>
```
* step3: 在activity中添加代码，动态加载fragment到FrameLayout中
```kotlin
val fragmentManager = supportFragmentManager
val fragmentTransaction: FragmentTransaction = fragmentManager.beginTransaction()

val fragment = FirstFragment()
fragmentTransaction.add(R.id.fragment_container, fragment)
fragmentTransaction.commit()
```
如果fragment_container的位置上已经是一个fragment，则用replace的方法
```kotlin
val fragmentManager = supportFragmentManager
val fragmentTransaction = fragmentManager.beginTranscation()
val fragment = SecondFragment()
fragmentTransaction.replace(R.layout_first_fragment, fragment)
fragmentTransaction.commit()
```

* step4 在fragment中实现返回栈
多个fragment加载时，如果想要实现回退到上一个fragment的效果，则需要将事务添加到返回栈中。
```kotlin
val fragmentManager = supportFragmentManager
val fragmentTransaction = fragmentManager.beginTranscation()
val fragment = SecondFragment()
fragmentTransaction.replace(R.layout_first_fragment, fragment)
`fragmentTransaction.addToBackStack(null)`
fragmentTransaction.commit()
```

## activity和fragment的交互方式

### activity中调用fragment

```kotlin
val fragment = findFragmentById(R.layout.fragment_first) as FirstFragment
或者在添加了extensions依赖后，可用：
val fragment = firstFragment as FirstFragment
```
### fragment中调用activtiy
```kotlin
if(activity != null) {
    val activity1 = activity as MainActivity
}
```

### 不同的fragment中相互调用
可用通过activity得到不同的fragment，然后可以实现通信。

## fragment的生命周期

![](http://res.mianshigee.com/upload/article/20200111/1354170682_3824.png)
```java
package com.jon.chapter0501

import android.content.Context
import android.os.Bundle
import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import androidx.fragment.app.Fragment

class SecondFragment: Fragment() {
    companion object {
       private val TAG = SecondFragment::class.java.simpleName
    }
    
    override fun onAttach(context: Context) {
        super.onAttach(context)
    }

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
    }
    
    override fun onCreateView(
        inflater: LayoutInflater,
        container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View? {

        return inflater.inflate(R.layout.fragment_second, container, false)
    }

    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        super.onViewCreated(view, savedInstanceState)
    }

    override fun onStart() {
        super.onStart()
    }

    override fun onResume() {
        super.onResume()
    }

    override fun onPause() {
        super.onPause()
    }

    override fun onStop() {
        super.onStop()
    }

    override fun onDestroyView() {
        super.onDestroyView()
    }

    override fun onDestroy() {
        super.onDestroy()
    }

    override fun onDetach() {
        super.onDetach()
    }
}
```

## fragment的布局技巧
*  layout-large目录存放双页布局，其他目录存放单页布局
*  layout-sw600dp目录存放双页布局，其他目录存放单页布局



