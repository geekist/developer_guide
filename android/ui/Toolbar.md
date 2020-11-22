# Toolbar

##  ActionBar
* ActionBar

系统原生的顶部菜单栏，功能比较单一，不能实现复杂的效果，所以，一般在创建新的项目的时候，需要将ActionBar去除掉。

如何去除ActionBar？

首先我们可以看一下ActionBar是如何定义的？它是通过Theme来定义的。在Android.manifest中我们可以看到app的theme应用
```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.jon.chapter1201">

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/**Theme.Chapter1201">**
        <activity android:name=".MainActivity">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>

</manifest>


```
然后在res/values/themes.xml中可以找到：
```xml
<resources xmlns:tools="http://schemas.android.com/tools">
    <!-- Base application theme. -->
    **<style name="Theme.Chapter1201" parent="Theme.MaterialComponents.DayNight.DarkActionBar">**
        <!-- Primary brand color. -->
        <item name="colorPrimary">@color/purple_500</item>
        <item name="colorPrimaryVariant">@color/purple_700</item>
        <item name="colorOnPrimary">@color/white</item>
        <!-- Secondary brand color. -->
        <item name="colorSecondary">@color/teal_200</item>
        <item name="colorSecondaryVariant">@color/teal_700</item>
        <item name="colorOnSecondary">@color/black</item>
        <!-- Status bar color. -->
        <item name="android:statusBarColor" tools:targetApi="l">?attr/colorPrimaryVariant</item>
        <!-- Customize your theme here. -->
    </style>
</resources>

```
将这里的

 **<style name="Theme.Chapter1201" parent="Theme.MaterialComponents.DayNight.DarkActionBar">**
更改为：
**<style name="Theme.Chapter1201" parent="Theme.MaterialComponents.Light.NoActionBar">
**
可以看到actionbar已经没有了。

##  AppBarLayout
AppBarLayout是Material库中提供的一个垂直布局的layout，采用了MaterialDesign的理念，并且在内部封装了很多滚动事件的响应，可以解决Toolbar被遮挡，滚动view时toolbar的事件行为等。
使用AppBarLayout也很简单，将toolbar放在AppbarLayout中，并且指定behaviour就可以了。
```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.coordinatorlayout.widget.CoordinatorLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <com.google.android.material.appbar.AppBarLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content">
        <androidx.appcompat.widget.Toolbar
            android:layout_width="match_parent"
            android:layout_height="?attr/actionBarSize"
            android:background="@color/purple_500"
            android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"
            app:popupTheme="@style/ThemeOverlay.AppCompat.Light"
            **app:layout_scrollFlags="scroll|enterAlways|snap"**
            >
        </androidx.appcompat.widget.Toolbar>
    </com.google.android.material.appbar.AppBarLayout>
</androidx.coordinatorlayout.widget.CoordinatorLayout>
```
注意这里的

  **app:layout_scrollFlags="scroll|enterAlways|snap"** 
即为我们增加的actionbarlayout的控制行为。

## Toolbar
Toolbar是Androidx库提供的，继承了ActionBar的所有功能，且具有更强大的功能和灵活性。

### 添加Toolbar
* 首先需要将布局文件加入
```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.coordinatorlayout.widget.CoordinatorLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <com.google.android.material.appbar.AppBarLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content">
        <androidx.appcompat.widget.Toolbar
            android:layout_width="match_parent"
            android:layout_height="?attr/actionBarSize"
            android:background="@color/purple_500"
            android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"
            app:popupTheme="@style/ThemeOverlay.AppCompat.Light"
            app:layout_scrollFlags="scroll|enterAlways|snap"
            >
        </androidx.appcompat.widget.Toolbar>
    </com.google.android.material.appbar.AppBarLayout>
</androidx.coordinatorlayout.widget.CoordinatorLayout>
```
这里需要解释一下theme的作用
```java
  android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"
  app:popupTheme="@style/ThemeOverlay.AppCompat.Light"
```
第一句话是将toolbar的主题置为暗色，因为app的主题是亮色，所以toolbar作为对应色应该是暗色
第二句话是将popup菜单的主题置为亮色，因为弹出的菜单如何暗色，会分辨不清。

toolbar上的文字默认是activity的label

* 在Activity的onCreate中加入代码设置系统actionbar成toolbar

```kotlin

class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        **setSupportActionBar(toolbarMain)**
    }
}

```
### 为ActionBar添加Menu
* 首先为Toolbar创建一个menu的layout布局文件
```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools">
    <item android:id="@+id/backup"
        android:icon="@drawable/ic_backup"
        android:title="Backup"
        app:showAsAction="always">
    </item>

    <item android:id="@+id/delete"
        android:icon="@drawable/ic_delete"
        android:title="Delete"
        app:showAsAction="ifRoom">
    </item>

    <item android:id="@+id/settings"
        android:icon="@drawable/ic_settings"
        android:title="Settings"
        app:showAsAction="never">
    </item>

</menu>

```
* 然后在Activity中重载onCreateOptionsMenu来加载布局文件

```kotlin
  override fun onCreateOptionsMenu(menu: Menu?): Boolean {
        menuInflater.inflate(R.menu.toolbar_item,menu)
        return true
        //return super.onCreateOptionsMenu(menu)
    }
```

* 为menu增加相应事件--重载onOptionItemSelected

```kotlin
 override fun onOptionsItemSelected(item: MenuItem): Boolean {
        when(item.itemId) {
            R.id.backup-> Toast.makeText(this,"backup",Toast.LENGTH_LONG).show()
            R.id.delete->Toast.makeText(this,"delete",Toast.LENGTH_LONG).show()
            R.id.settings->Toast.makeText(this,"settings",Toast.LENGTH_LONG).show()
        }
        return true
       // return super.onOptionsItemSelected(item)
    }

```

### 为Toolbar增加home按钮
* 在activity的onCreate中增加代码
* 
```kotlin
 override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        setSupportActionBar(toolbarMain)

        supportActionBar?.let{
            it.setDisplayHomeAsUpEnabled(true)
            it.setHomeAsUpIndicator(R.drawable.ic_menu)
        }
    }

``` 
* 添加相应事件：

```kotlin
 override fun onOptionsItemSelected(item: MenuItem): Boolean {
        when(item.itemId) {
            android.R.id.home->Toast.makeText(this,"home",Toast.LENGTH_LONG).show()
            R.id.backup-> Toast.makeText(this,"backup",Toast.LENGTH_LONG).show()
            R.id.delete->Toast.makeText(this,"delete",Toast.LENGTH_LONG).show()
            R.id.settings->Toast.makeText(this,"settings",Toast.LENGTH_LONG).show()
        }
        return true
       // return super.onOptionsItemSelected(item)
    }

```

















