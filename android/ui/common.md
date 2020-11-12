## UI 控件的常用属性
```java

	//ID是每一个控件必须有的属性，且定义要和java文件中定义一致，因为无论是viewBinding和Kotlin都需要直接从这里读取值。
    android:id="@+id/textview_first"


//指定宽度和高度，可以有match_parent、wrap_content和自定义dp三种方式，在constraintLayout中match_parent需要用0dp代替

    android:layout_width="wrap_content"
	android:layout_height="wrap_content"

//控件上显示的文字

    android:text="@string/hello_first_fragment"
    






```