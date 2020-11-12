## UI 控件的常用属性

* 控件Id和kt文件自动绑定

```java
plugins {
    id 'com.android.application'
    id 'kotlin-android'
    id 'kotlin-android-extensions'
}
```

* 控件的通用属性
```java

	//ID是每一个控件必须有的属性，且定义要和java文件中定义一致，因为无论是viewBinding和Kotlin都需要直接从这里读取值。
    android:id="@+id/textview_first"


//指定宽度和高度，可以有match_parent、wrap_content和自定义dp三种方式，在constraintLayout中match_parent需要用0dp代替

    android:layout_width="wrap_content"
	android:layout_height="wrap_content"

//控件上显示的文字

    android:text="@string/hello_first_fragment"
    
//控件对齐方式，有top、bottom、start、end、center等选项，可以用|隔开

	android:gravity="center"

//控件的可见性，有VISIBLE、INVISIBLE、GONE几种
	android:visibility="VISIBLE"



```

* 控件的单击事件一般有两种方式，直接设置，或者在父activity/fragment中实现OnClickListener接口

```java
 view.textview_first.setOnClickListener{
            Toast.makeText(context,"text view is clicked",Toast.LENGTH_LONG).show()
        }

```

```java




```