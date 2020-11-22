# DrawerLayout

* DrawerLayout是一个MaterialDesign布局

在布局中允许放入两个直接的子控件：第一个子控件是主屏幕显示的内容，第二个子控件是滑动菜单中显示的内容。
```xml
<?xml version="1.0" encoding="utf-8"?>

<androidx.drawerlayout.widget.DrawerLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/drawerLayout"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <androidx.coordinatorlayout.widget.CoordinatorLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent">

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
    </androidx.coordinatorlayout.widget.CoordinatorLayout>

    <FrameLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:layout_gravity="start"
        android:background="@color/purple_500">
        <TextView
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:text="I am drawer">
        </TextView>


    </FrameLayout>

</androidx.drawerlayout.widget.DrawerLayout>
```
* 然后在home按钮加上drawer.Layout.openDrawer响应事件即可


```kotlin
 override fun onOptionsItemSelected(item: MenuItem): Boolean {
        when(item.itemId) {
            android.R.id.home->drawerLayout.openDrawer(GravityCompat.START)
            R.id.backup-> Toast.makeText(this,"backup",Toast.LENGTH_LONG).show()
            R.id.delete->Toast.makeText(this,"delete",Toast.LENGTH_LONG).show()
            R.id.settings->Toast.makeText(this,"settings",Toast.LENGTH_LONG).show()
        }
```