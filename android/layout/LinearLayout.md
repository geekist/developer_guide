## LinearLayout

线性布局，将包含的控件在线性方向上依次排列

android：orientation 指定布局方向：horizonital和vertical

android:layout_weight：为控件分配layout空间。要指定长度或宽度为0

android:layout_gravity 指控件在layout中的对齐方式，有top,bottom,center,等
android:gravity 指控件的文字在控件空的对齐方式

## include 和 merge标签
* inlcude


如果你已经知道哪个布局需要重用，就创建一个新的xml文件来定义布局。如，下面是一个来自G-Kenya代码库里定义标题栏的布局，它可以被插到每个Activity里：
```java
<FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width=”match_parent”
    android:layout_height="wrap_content"
    android:background="@color/titlebar_bg">
    <ImageView android:layout_width="wrap_content"
               android:layout_height="wrap_content" 
               android:src="@drawable/gafricalogo" />
</FrameLayout>
```
根视图应该刚好和你在其他要插入这个视图的视图里相应位置一样。

使用<include/>标签

在你要添加可复用布局的布局里，添加<include/>标签。下面是添加标题栏

```java
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical" 
    android:layout_width=”match_parent”
    android:layout_height=”match_parent”
    android:background="@color/app_bg"
    android:gravity="center_horizontal">
    <include layout="@layout/titlebar"/>
    <TextView android:layout_width=”match_parent”
              android:layout_height="wrap_content"
              android:text="@string/hello"
             android:padding="10dp" />
    ...
</LinearLayout>
```



* merge

<merge/>标签帮助你排除把一个布局插入到另一个布局时产生的多余的View Group.如，你的被复用布局是一个垂直的线性布局，包含两个子视图，当它作为一个被复用的元素被插入到另一个垂直的线性布局时，结果就是一个垂直的LinearLayout里包含一个垂直的LinearLayout。这个嵌套的布局并没有实际意义，而且会让UI性能变差。

为了避免插入类似冗余的View Group,你可以使用<merge/>标签标签作为可复用布局的根节点，如：
```xml
<merge xmlns:android="http://schemas.android.com/apk/res/android">
    <Button
        android:layout_width="fill_parent" 
        android:layout_height="wrap_content"
        android:text="@string/add"/>
    <Button
        android:layout_width="fill_parent" 
        android:layout_height="wrap_content"
        android:text="@string/delete"/>
</merge>
```
现在，当你使用include标签插入这个布局到另一个布局时，系统会忽略merge标签，直接把两个Button替换到include标签的位置。