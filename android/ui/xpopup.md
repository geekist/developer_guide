
# xpopup的内置用法：

显示确认和取消对话框

```
new XPopup.Builder(getContext()).asConfirm("我是标题", "我是内容",
      new OnConfirmListener() {
            @Override
             public void onConfirm() {
                 toast("click confirm");
             }
       })
       .show();

```

## 自定义UI的对话框

```
new XPopup.Builder(getContext())
   .asConfirm(“标题”, 
              "内容",
              "关闭", 
               "XPopup牛逼",
                new OnConfirmListener() {
                            @Override
                            public void onConfirm() {
                                toast("click confirm");
                            }
                        }, 
              null, 
              R.layout.my_confim_popup)//绑定已有布局
            .show();

```

## 展示一个集成view的自定义对话框

### step1： 首先定义一个UI界面

```xml

<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    tools:layout_gravity="bottom">

    <com.yuya.parent.ui.widget.PressedConstraintLayout
        android:id="@+id/mContentLayout"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:layout_marginStart="@dimen/common_lr_margin"
        android:layout_marginEnd="@dimen/common_lr_margin"
        android:layout_marginBottom="8dp"
        app:layout_constraintBottom_toTopOf="@+id/mTvCancel"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent"
        app:pcl_corner_radius="12dp"
        app:pcl_enable_press="false"
        app:pcl_shadow_radius="0dp">

        <androidx.recyclerview.widget.RecyclerView
            android:id="@+id/mRvList"
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:overScrollMode="never"
            app:layout_constraintBottom_toBottomOf="parent"
            app:layout_constraintLeft_toLeftOf="parent"
            app:layout_constraintRight_toRightOf="parent"
            app:layout_constraintTop_toTopOf="parent" />
    </com.yuya.parent.ui.widget.PressedConstraintLayout>

    <com.noober.background.view.BLTextView
        android:id="@+id/mTvCancel"
        android:layout_width="0dp"
        android:layout_height="44dp"
        android:layout_marginStart="@dimen/common_lr_margin"
        android:layout_marginEnd="@dimen/common_lr_margin"
        android:layout_marginBottom="8dp"
        android:gravity="center"
        android:text="@string/cancel"
        android:textColor="#51A6F9"
        android:textSize="15sp"
        app:bl_corners_radius="12dp"
        app:bl_pressed_solid_color="@color/background_f8"
        app:bl_unPressed_solid_color="@color/backgroundPanel"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent" />
</androidx.constraintlayout.widget.ConstraintLayout>
```

### step2: 然后定义一个类，继承xpopup的view

```kotlin


package com.yuya.parent.ui.dialog

import android.content.Context
import androidx.recyclerview.widget.LinearLayoutManager
import com.lxj.xpopup.XPopup
import com.lxj.xpopup.core.BottomPopupView
import com.yuya.parent.lib.ext.click
import com.yuya.parent.lib.ext.generateHorizontalDivider
import com.yuya.parent.ui.R
import com.yuya.parent.ui.adapter.BottomSheetAdapter
import kotlinx.android.synthetic.main.common_layout_bottom_sheet_dialog.view.*

class BottomSheetDialog(context: Context) : BottomPopupView(context) {
    private val mSheetList by lazy { arrayListOf<String>() }
    private val mSheetAdapter by lazy { BottomSheetAdapter() }

    private var mOnItemClick: ((position: Int) -> Unit)? = null

    fun onItemClick(l: (position: Int) -> Unit): BottomSheetDialog {
        this.mOnItemClick = l
        return this
    }

    override fun getImplLayoutId() = R.layout.common_layout_bottom_sheet_dialog

    override fun onCreate() {
        mRvList.layoutManager = LinearLayoutManager(context)
        mRvList.addItemDecoration(context.generateHorizontalDivider(0.5f, R.color.dark05))
        mRvList.adapter = mSheetAdapter
        mSheetAdapter.replaceData(mSheetList)
        mSheetAdapter.setOnItemClickListener { _, _, position ->
            dismissWith { mOnItemClick?.invoke(position) }
        }

        mTvCancel.click { dismiss() }
    }

    fun with(data: List<String>): BottomSheetDialog {
        mSheetList.clear()
        mSheetList.addAll(data)
        return this
    }
}

```

### step3:使用的时候显示该dialog

```

fun showBottomSheetDialog(
    context: Context,
    vararg data: String,
    onItemClick: (position: Int) -> Unit
) {
    XPopup.Builder(context)

        .asCustom(
            BottomSheetDialog(context)
                .with(data.toList())
                .onItemClick(onItemClick)
        )
        .show()
}


```