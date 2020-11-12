

## RecyclerVieew

* 1、依赖项配置

在build.gradle中需要material的依赖

implementation 'androidx.recyclerview:recyclerview:1.1.0'


* 2、在Activity的布局文件中添加RecyclerView的布局：

```java

<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <androidx.recyclerview.widget.RecyclerView
        android:id="@+id/recycler_view"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        tools:listitem="@layout/rv_article_item" />
</LinearLayout>

```


* 3、创建一个layout文件，用来布局RecyclerView中的item。
```java
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="60dp"
    android:orientation="horizontal">

    <ImageView
        android:id="@+id/imageView"
        android:layout_width="40dp"
        android:layout_height="40dp"
        android:layout_gravity="center_vertical"
        android:layout_marginLeft="10dp"></ImageView>

    <TextView
        android:id="@+id/textView"
        android:layout_width="0dp"
        android:layout_height="56dp"
        android:layout_weight="1"
        android:textSize="24sp"
        android:gravity="center_horizontal|center_vertical"
        android:layout_gravity="center_vertical"
        android:layout_marginRight="10dp"></TextView>

</LinearLayout>

```

* 4、创建一些数据

```java

class ObjItem(val name: String, val imageId: Int) {

}

private fun initObjItems() {
    repeat(2) {
        objItemList.add(ObjItem("location black", R.drawable.ic_add_location_black_24dp))
        objItemList.add(ObjItem("bike", R.drawable.ic_directions_bike_black_24dp))
        objItemList.add(ObjItem("boat", R.drawable.ic_directions_boat_black_24dp))
        objItemList.add(ObjItem("car", R.drawable.ic_directions_car_black_24dp))
        objItemList.add(ObjItem("transit", R.drawable.ic_directions_transit_black_24dp))
        objItemList.add(ObjItem("flight", R.drawable.ic_flight_black_24dp))
    }
}

```

* 5、创建Adapter类，实现三个重写的方法，用来创建viewHolder,绑定item数据，和计算item项数量,同时需要创建一个ViewHolder的类

```java


import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import android.widget.ImageView
import android.widget.TextView
import androidx.recyclerview.widget.RecyclerView

class RecyclerViewAdapter(val objectItemList: List<ObjItem>) :
    RecyclerView.Adapter<RecyclerViewAdapter.ViewHolder>() {

    inner class ViewHolder(view: View) : RecyclerView.ViewHolder(view) {
        val objImage: ImageView = view.findViewById(R.id.imageView)
        val objName: TextView = view.findViewById(R.id.textView)
    }

    override fun onCreateViewHolder(
        parent: ViewGroup,
        viewType: Int
    ): ViewHolder {
        val view = LayoutInflater.from(parent.context)
            .inflate(R.layout.layout_listview_item2, parent, false)
        return ViewHolder(view)
    }

    override fun onBindViewHolder(holder: RecyclerViewAdapter.ViewHolder, position: Int) {
        val objItem = objectItemList[position]
        holder.objImage.setImageResource(objItem.imageId)
        holder.objName.text = objItem.name

    }

    override fun getItemCount(): Int {
        return objectItemList.size
    }
}

```

* 6、在Activity中实现RecyclerView和LayoutManger以及Adapter的绑定：


```java

    override fun onCreateView(
        inflater: LayoutInflater, container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View? {
             // Inflate the layout for this fragment
        val view =  inflater.inflate(R.layout.fragment_second, container, false)
        initObjItems2()
      //  val layoutManager = LinearLayoutManager(context)
      //  layoutManager.orientation = LinearLayoutManager.HORIZONTAL
      //    val layoutManager = GridLayoutManager(context,4)
        val layoutManager = StaggeredGridLayoutManager(3,StaggeredGridLayoutManager.VERTICAL)

        view.recyclerView.layoutManager = layoutManager
        val adapter = RecyclerViewAdapter(objItemList)
        view.recyclerView.adapter = adapter

        view.recyclerView.addItemDecoration(
            RecyclerViewDivider(
                context,
                LinearLayoutManager.HORIZONTAL,
                1,
                resources.getColor(R.color.purple_200)
            )
        )
        return view
    }

```

* 7、关于item的点击事件响应，在viewholder中创建回调，通过adapter传递。

（略）参见utils中的viewholder和adapter

* 8、附上一个创建item项的分割线的类RecyclerViewDivider
```java

package com.jon.recycleview;

import android.content.Context;
import android.content.res.TypedArray;
import android.graphics.Canvas;
import android.graphics.Paint;
import android.graphics.Rect;
import android.graphics.drawable.Drawable;
import android.view.View;

import androidx.core.content.ContextCompat;
import androidx.recyclerview.widget.LinearLayoutManager;
import androidx.recyclerview.widget.RecyclerView;

public class RecyclerViewDivider extends RecyclerView.ItemDecoration {

    private Paint mPaint;
    private Drawable mDivider;
    private int mDividerHeight = 2;//分割线高度，默认为1px
    private int mOrientation;//列表的方向：LinearLayoutManager.VERTICAL或LinearLayoutManager.HORIZONTAL
    private static final int[] ATTRS = new int[]{android.R.attr.listDivider};//使用系统自带的listDivider

    /**
     * 默认分割线：高度为2px，颜色为灰色
     *
     * @param context
     * @param orientation 列表方向
     */
    public RecyclerViewDivider(Context context, int orientation) {
        if (orientation != LinearLayoutManager.VERTICAL && orientation != LinearLayoutManager.HORIZONTAL) {
            throw new IllegalArgumentException("请输入正确的参数！");
        }
        mOrientation = orientation;

        final TypedArray a = context.obtainStyledAttributes(ATTRS);//使用TypeArray加载该系统资源
        mDivider = a.getDrawable(0);
        a.recycle();//缓存
    }

    /**
     * 自定义分割线
     *
     * @param context
     * @param orientation 列表方向
     * @param drawableId  分割线图片
     */
    public RecyclerViewDivider(Context context, int orientation, int drawableId) {
        this(context, orientation);
        mDivider = ContextCompat.getDrawable(context, drawableId);
        mDividerHeight = mDivider.getIntrinsicHeight();
    }

    /**
     * 自定义分割线
     *
     * @param context
     * @param orientation   列表方向
     * @param dividerHeight 分割线高度
     * @param dividerColor  分割线颜色
     */
    public RecyclerViewDivider(Context context, int orientation, int dividerHeight, int dividerColor) {
        this(context, orientation);
        mDividerHeight = dividerHeight;
        mPaint = new Paint(Paint.ANTI_ALIAS_FLAG);
        mPaint.setColor(dividerColor);
        mPaint.setStyle(Paint.Style.FILL);
    }


    //获取分割线尺寸
    @Override
    public void getItemOffsets(Rect outRect, View view, RecyclerView parent, RecyclerView.State state) {
        super.getItemOffsets(outRect, view, parent, state);
        //设置偏移的高度是mDivider.getIntrinsicHeight，该高度正是分割线的高度
        outRect.set(0, 0, 0, mDividerHeight);
    }

    //绘制分割线
    @Override
    public void onDraw(Canvas c, RecyclerView parent, RecyclerView.State state) {
        super.onDraw(c, parent, state);
        if (mOrientation == LinearLayoutManager.VERTICAL) {
            drawVertical(c, parent);
        } else {
            drawHorizontal(c, parent);
        }
    }

    //绘制横向 item 分割线
    private void drawHorizontal(Canvas canvas, RecyclerView parent) {
        final int left = parent.getPaddingLeft();//获取分割线的左边距，即RecyclerView的padding值
        final int right = parent.getMeasuredWidth() - parent.getPaddingRight();//分割线右边距
        final int childSize = parent.getChildCount();
        //遍历所有item view，为它们的下方绘制分割线
        for (int i = 0; i < childSize; i++) {
            final View child = parent.getChildAt(i);
            RecyclerView.LayoutParams layoutParams = (RecyclerView.LayoutParams) child.getLayoutParams();
            final int top = child.getBottom() + layoutParams.bottomMargin;
            final int bottom = top + mDividerHeight;
            if (mDivider != null) {
                mDivider.setBounds(left, top, right, bottom);
                mDivider.draw(canvas);
            }
            if (mPaint != null) {
                canvas.drawRect(left, top, right, bottom, mPaint);
            }
        }
    }

    //绘制纵向 item 分割线
    private void drawVertical(Canvas canvas, RecyclerView parent) {
        final int top = parent.getPaddingTop();
        final int bottom = parent.getMeasuredHeight() - parent.getPaddingBottom();
        final int childSize = parent.getChildCount();
        for (int i = 0; i < childSize; i++) {
            final View child = parent.getChildAt(i);
            RecyclerView.LayoutParams layoutParams = (RecyclerView.LayoutParams) child.getLayoutParams();
            final int left = child.getRight() + layoutParams.rightMargin;
            final int right = left + mDividerHeight;
            if (mDivider != null) {
                mDivider.setBounds(left, top, right, bottom);
                mDivider.draw(canvas);
            }
            if (mPaint != null) {
                canvas.drawRect(left, top, right, bottom, mPaint);
            }
        }
    }
}




```







