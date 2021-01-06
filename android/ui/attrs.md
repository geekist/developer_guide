# attrs style和theme的区别

## 1、概念说明

* Attr：

属性，风格样式的最小单元；

* Style：

风格，它是一系列Attr的集合用以定义一个**View**的样式，比如height、width、padding等；

* Theme：

主题，它与Style作用一样，不同之处在于Style作用于个一个单独View，而它是作用于Activity上或是整个application应用。

## 2、Attrs的使用
attrs（属性）定义在res/values/attrs.xml文件中，其作用是定义属性的名称和取值类型，即告诉系统这些属性被定义过了。通常在自定义view中使用的比较多。

例如，下面的xml定义了一个BottomNavigationView的属性

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <declare-styleable name="BottomRoundNavigation">
        <!-- 底部导航栏的高度(不包括阴影的高度和凸起的高度) -->
        <attr name="brn_tab_height" format="dimension" />
        <!-- 图标大小 -->
        <attr name="brn_icon_size" format="dimension" />
        <!-- 导航栏背景颜色 -->
        <attr name="brn_background_color" format="color" />
        <!-- 是否需要导航栏中间突出 -->
        <attr name="brn_is_linear" format="boolean" />
        <!-- 导航栏中间突出宽度 -->
        <attr name="brn_bezier_width" format="dimension" />
        <!-- 导航栏中间突出高度 -->
        <attr name="brn_bezier_height" format="dimension" />
        <!-- 导航栏阴影大小 -->
        <attr name="brn_shadow_radius" format="dimension" />
        <!-- 导航栏阴影颜色 -->
        <attr name="brn_shadow_color" format="color" />
    </declare-styleable>
 </resources>
```

然后，我们可以在activity或fragment的布局文件中使用这些属性：
```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <FrameLayout
        android:id="@+id/fl_Container"
        android:layout_width="0dp"
        android:layout_height="0dp"
        app:layout_constraintBottom_toTopOf="@+id/mLine"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

    <com.ytech.ui.tabs.BottomRoundNavigation
        android:id="@+id/mTabs"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        app:brn_is_linear="true"
        app:brn_shadow_color="#dfdfdf"
        app:brn_shadow_radius="1dp"
        app:brn_tab_height="50.5dp"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent" />
</androidx.constraintlayout.widget.ConstraintLayout>
```

最后，在自定义view的代码中可以进行属性的解析和赋值

```java

class BottomRoundNavigation : ConstraintLayout {
    
    constructor(context: Context, attributeSet: AttributeSet?, defStyleAttr: Int) : super(
        context,
        attributeSet,
        defStyleAttr
    ) {
        with(context.obtainStyledAttributes(attributeSet, R.styleable.BottomRoundNavigation)) {
            mTabHeight = getDimensionPixelSize(
                R.styleable.BottomRoundNavigation_brn_tab_height,
                dp2px(context, 50.5f)
            )
            mIconSize = getDimensionPixelSize(
                R.styleable.BottomRoundNavigation_brn_icon_size, dp2px(context, 20f)
            )
         ......
            recycle()
        }
    }

    override fun onDraw(canvas: Canvas?) {
        super.onDraw(canvas)
        val centerX = measuredWidth * 0.5f
        var x: Float
        mPath.reset()
        when (mIsLinear) {
            true -> {
                // 线性
                mPath.moveTo(0f, mShadowRadius)
                mPath.lineTo(0f, measuredHeight.toFloat())
                mPath.close()
            }
            else -> {
                // 中间凸起
                mPath.moveTo(0f, mBezierHeight + mShadowRadius)
          ......
                mPath.lineTo(measuredWidth.toFloat(), mBezierHeight + mShadowRadius)
                mPath.lineTo(measuredWidth.toFloat(), measuredHeight.toFloat())
                mPath.lineTo(0f, measuredHeight.toFloat())
                mPath.close()
            }
        }

        canvas?.drawPath(mPath, mShadowPaint)
        canvas?.drawPath(mPath, mPaint)
    }

    override fun onMeasure(widthMeasureSpec: Int, heightMeasureSpec: Int) {
        super.onMeasure(widthMeasureSpec, heightMeasureSpec)
        val lp = layoutParams
        lp.width = LayoutParams.MATCH_PARENT
        lp.height = mTabHeight +
                if (mIsLinear) mShadowRadius.toInt() else (mBezierHeight + mShadowRadius).toInt()
        layoutParams = lp
    }

    override fun onSizeChanged(w: Int, h: Int, oldw: Int, oldh: Int) {
        super.onSizeChanged(w, h, oldw, oldh)
        handler.post { requestLayout() }
    }
```

### style的使用

sytle风格，定一个在res/values/styles.xml文件中，其实质是将activity或fragment的布局文件中的共有部分抽取出来以供复用。

style的定义如下：
首先可以在styles.xml文件中定义style的内容
```xml
<resources xmlns:tools="http://schemas.android.com/tools">
    <style name="HorizontalDivider">
        <item name="android:layout_width">match_parent</item>
        <item name="android:layout_height">@dimen/common_divider</item>
        <item name="android:background">@color/dark05</item>
    </style>
</resources>
```
然后在布局文件中可以使用
```xml
<View
    style="@style/HorizontalDivider"
    android:layout_height="wrap_content"
    android:layout_width="wrap_content"/>

```

### Theme的使用
theme 和style在定义上完全一致，只不过使用的范围不同。

应用于Application、Activity，主题Theme就是用来设置界面UI风格，可以设置整个应用或者某个活动Activity的界面风格。在Android SDK中内置了下面的Theme，可以按标题栏Title Bar和状态栏Status Bar是否可见来分类：

```xml
android:theme="@android:style/Theme.Dialog" : Activity显示为对话框模式
android:theme="@android:style/Theme.NoTitleBar" : 不显示应用程序标题栏
android:theme="@android:style/Theme.NoTitleBar.Fullscreen" : 不显示应用程序标题栏，并全屏
android:theme="@android:style/Theme.Light ": 背景为白色
android:theme="@android:style/Theme.Light.NoTitleBar" : 白色背景并无标题栏
android:theme="@android:style/Theme.Light.NoTitleBar.Fullscreen" : 白色背景，无标题栏，全屏
android:theme="@android:style/Theme.Black" : 背景黑色
android:theme="@android:style/Theme.Black.NoTitleBar" : 黑色背景并无标题栏
android:theme="@android:style/Theme.Black.NoTitleBar.Fullscreen" : 黑色背景，无标题栏，全屏.全屏将会没有
android:theme="@android:style/Theme.Wallpaper" : 用系统桌面为应用程序背景
android:theme="@android:style/Theme.Wallpaper.NoTitleBar" : 用系统桌面为应用程序背景，且无标题栏
android:theme="@android:style/Theme.Wallpaper.NoTitleBar.Fullscreen" : 用系统桌面为应用程序背景，无标题栏，全屏
android:theme="@android:style/Theme.Translucent : 透明背景
android:theme="@android:style/Theme.Translucent.NoTitleBar" : 透明背景并无标题
android:theme="@android:style/Theme.Translucent.NoTitleBar.Fullscreen" : 透明背景并无标题，全屏
android:theme="@android:style/Theme.Panel ": 面板风格显示
android:theme="@android:style/Theme.Light.Panel" : 平板风格显示

```

其源代码如下：
```xml
    <style name="Theme.Black">
        <item name="android:windowBackground">@android:color/black</item>
        <item name="android:colorBackground">@android:color/black</item>
    </style>
```

使用方式：
1、添加在Android.manifest文件中

```xml
<application
        android:allowBackup="true"
        android:icon="@drawable/ic_launcher"
        android:label="@string/app_name"
        android:theme="@android:style/Theme.Black.NoTitleBar" >
        <activity
            android:name=".MainActivity"
            android:label="@string/app_name"
            android:theme="@android:style/Theme.Light.NoTitleBar">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>
```


2、添加在代码中
```java
@Override
    public void onCreate(Bundle savedInstanceState){
	super.onCreate(savedInstanceState);
	setTheme(android.R.style.Theme_Translucent_NoTitleBar);
	setContentView(R.layout.main);
    }

```



