# CoordinatorLayout

 CoordinatorLayout是一个加强版的FrameLayout，由AndroidX提供。在普通的情况下其作用和FrameLayout相同，但是它拥有一些额外的Material能力。
比如，它可以监听所有子控件的事件，并自动为我们做出最合适的响应，比如floatingActionBar被snackBar遮挡的问题，它监听到snackBar弹出事件后，会自动将内部的FloatingActionBar上移，从而确保不会被遮挡。

用法很简单，同FrameLayout一样使用就可以了


```java
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

        <com.google.android.material.floatingactionbutton.FloatingActionButton
            android:id="@+id/fab"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_gravity="bottom|end"
            android:layout_margin="10dp"
            android:backgroundTint="@color/purple_500"
            app:srcCompat="@drawable/ic_done" />
    </androidx.coordinatorlayout.widget.CoordinatorLayout>

   <com.google.android.material.navigation.NavigationView
       android:id="@+id/navView"
       android:layout_width="match_parent"
       android:layout_height="match_parent"
       android:layout_gravity="start"
       app:headerLayout="@layout/nav_header"
       app:menu="@menu/nav_item">
   </com.google.android.material.navigation.NavigationView>

</androidx.drawerlayout.widget.DrawerLayout>



```