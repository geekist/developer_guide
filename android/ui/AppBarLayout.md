# AppBarLayout

AppBarLayout是Material库中的另外一个工具，其实是一个垂直方向上的LinearLayout，但是在内部做了很多滚动事件的封装，并采用了一些Material Design的理念。

AppBarLayout通过给其他空间设置layoutBehavior的方式，来协调和控制appbarlayout中的控件和其他控件的关系。

```java
  <com.google.android.material.appbar.AppBarLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content">

            <androidx.appcompat.widget.Toolbar
                android:id="@+id/toolbarMain"
                android:layout_width="match_parent"
                android:layout_height="?attr/actionBarSize"
                android:background="@color/purple_500"
                android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"
                app:layout_scrollFlags="scroll|enterAlways|snap"
                app:popupTheme="@style/ThemeOverlay.AppCompat.Light">

            </androidx.appcompat.widget.Toolbar>
        </com.google.android.material.appbar.AppBarLayout>


```
比如，这里的

  **app:layout_scrollFlags="scroll|enterAlways|snap"**

scroll表示当RecyclerView向上滚动时，Toolbar会一起向上滚动并实现隐藏。
enterAlwarys表示当RecyclerView向下滚动时，Toolbar会向下一起滚动并重新显示。
snap表示当Toolbar还没有完全隐藏或完全展示的时候，自动选择隐藏还是显示。

再比如，在RecyclerView中添加
```xml
  <androidx.recyclerview.widget.RecyclerView
            android:id="@+id/recyclerView"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            app:layout_behavior="@string/appbar_scrolling_view_behavior">
        </androidx.recyclerview.widget.RecyclerView>
```

** app:layout_behavior="@string/appbar_scrolling_view_behavior"  **

则表示RecyclerView不会遮挡toolbar