
# 自定义一个带字体的textview--TypefaceTextView

## [attrs文件详细说明](https://blog.csdn.net/u011033906/article/details/54934103)

## 1、首先在res/values/attrs.xml中创建一个关于TypefaceTextView的字段描述文件
```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <declare-styleable name="TypefaceTextView">
        <attr name="typeface" format="string" />
    </declare-styleable>
</resources>
```

## 2、自定义一个TypefaceTextView类
```java

open class TypefaceTextView : AppCompatTextView {
    constructor(context: Context) : this(context, null)

    constructor(context: Context, attributeSet: AttributeSet?) : this(
        context,
        attributeSet,
        android.R.attr.textViewStyle
    )

    constructor(context: Context, attributeSet: AttributeSet?, defStyleAttr: Int) : super(
        context,
        attributeSet,
        defStyleAttr
    ) {
        with(context.obtainStyledAttributes(attributeSet, R.styleable.TypefaceTextView)) {
            val typeface = getString(R.styleable.TypefaceTextView_typeface)
            if (typeface.isNotEmptyStr()) {
                val font = Typeface.createFromAsset(context.assets, "fonts/$typeface") ?: Typeface.DEFAULT
                setTypeface(font)
            }else {
                setTypeface(Typeface.DEFAULT)
            }
            recycle()
        }
    }
}

```

## 3、在需要使用的地方添加这个控件的xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".splash.SplashActivity">

    <com.ytech.ui.view.TypefaceTextView
        android:id="@+id/textViewVersion"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"

        android:text="@string/app_name_version"
        android:textColor="#ccffffff"
        android:textSize="15sp"

        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        android:layout_marginBottom="50dp"
        app:typeface="Lobster-1.4.otf" />
</androidx.constraintlayout.widget.ConstraintLayout>

```