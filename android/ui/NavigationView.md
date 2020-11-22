# NavigationView

## DrawerLayout实现侧滑
DrawerLayout是一个布局，在布局中允许放入两个直接的子控件：第一个子控件是主屏幕显示的内容，第二个子控件是滑动菜单中显示的内容。
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
然后在home按钮加上drawer.Layout.openDrawer响应事件即可
```kotlin
 override fun onOptionsItemSelected(item: MenuItem): Boolean {
        when(item.itemId) {
            android.R.id.home->drawerLayout.openDrawer(GravityCompat.START)
            R.id.backup-> Toast.makeText(this,"backup",Toast.LENGTH_LONG).show()
            R.id.delete->Toast.makeText(this,"delete",Toast.LENGTH_LONG).show()
            R.id.settings->Toast.makeText(this,"settings",Toast.LENGTH_LONG).show()
        }
```

## NavigationView实现漂亮的侧滑栏
NavigationView是一个用于在侧滑栏上设置布局的一个view控件，它包含两个部分，一个header和一个menu，然后将其添加到DrawrLayout的布局文件中，最后添加响应事件即可
* 首先创建header的布局文件：在layout目录下创建一个nav_header的布局文件
```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="180dp"
    android:background="@color/purple_500"
    android:orientation="vertical">
    <de.hdodenhof.circleimageview.CircleImageView
        android:id="@+id/iconImage"
        android:layout_width="70dp"
        android:layout_height="70dp"
        android:src="@drawable/nav_icon"
        android:layout_centerInParent="true">
    </de.hdodenhof.circleimageview.CircleImageView>
    <TextView
        android:id="@+id/mailText"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_alignParentBottom="true"
        android:text="jyang@nero.com"
        android:textColor="#FFF"
        android:textSize="14sp">
    </TextView>

    <TextView
        android:id="@+id/userText"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_above="@id/mailText"
        android:text="Tony Green"
        android:textColor="#FFF"
        android:textSize="14sp">
    </TextView>


</RelativeLayout>

```
然后创建menu的布局文件 nav_menu
```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
    <group android:checkableBehavior="single">
        <item android:id="@+id/nav_call"
            android:icon="@drawable/nav_call"
            android:title="Call">
        </item>
        <item android:id="@+id/navFriends"
            android:icon="@drawable/nav_friends"
            android:title="Friends">
        </item>
    </group>

    <group android:checkableBehavior="single">
        <item android:id="@+id/nav_location"
            android:icon="@drawable/nav_location"
            android:title="Location">
        </item>
        <item android:id="@+id/navMail"
            android:icon="@drawable/nav_mail"
            android:title="Mail">
        </item>

        <item android:id="@+id/nav_task"
            android:icon="@drawable/nav_task"
            android:title="Task">
        </item>
    </group>
</menu>

```


完成的NavigationView的xml
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

最后添加响应事件
```kotlin

class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        setSupportActionBar(toolbarMain)

        supportActionBar?.let {
            it.setDisplayHomeAsUpEnabled(true)
            it.setHomeAsUpIndicator(R.drawable.ic_menu)
        }

        navView.setCheckedItem(R.id.navFriends)
        navView.setNavigationItemSelectedListener {
            when (it.itemId) {
                R.id.navFriends -> {
                    Toast.makeText(this, "", Toast.LENGTH_LONG).show()
                    drawerLayout.closeDrawers()
                    true
                }
                else -> true
            }
        }
    }
```


