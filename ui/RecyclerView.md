

# 实现大量元素的滑动组件


RecyclerView是support-v7包中的新组件，是一个用于大量数据展示的强大的滑动组件（Android 5.0开始），可以用来代替传统的ListView和GridView。

RecyclerVieew包括下面几个类：


- 1、Layout Manager布局管理器

LayoutManager负责RecyclerView的布局，其中包含了Item View的获取与回收。

RecyclerView提供了三种布局管理器：

*LinerLayoutManager 以垂直或者水平列表方式展示Item*
	 
*GridLayoutManager 以网格方式展示Item*

*StaggeredGridLayoutManager 以瀑布流方式展示Item*

- 2、ViewHolder

ViewHolder负责呈现RecyclerView中的item

- 3、Adapter适配器

Adapter负责RecyclerVIew中数据的管理和绑定。

在Adapter中必须实现三个方法的重载：

*onCreateViewHolder()*

*onBindViewHolder()*

*getItemCount()*


## 1、依赖项配置

在build.gradle中需要material的依赖

implementation 'androidx.recyclerview:recyclerview:1.1.0'1


## 2、在Activity的布局文件中添加RecyclerView的布局：

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


### 3、创建一个layout文件，用来布局RecyclerView中的item。
```java

<?xml version="1.0" encoding="utf-8"   ?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"

    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:background="?android:attr/selectableItemBackground"
    android:padding="@dimen/size_16dp">

    <TextView
        android:id="@+id/tvTime"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_alignParentRight="true"
        android:layout_marginTop="@dimen/size_2dp"
        android:layout_marginBottom="@dimen/size_1dp"
        android:textColor="@color/text_9"
        android:textSize="@dimen/size_12sp" />

    <TextView
        android:id="@+id/tvTitle"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_alignBaseline="@+id/tvTime"
        android:layout_marginTop="@dimen/size_2dp"
        android:layout_marginBottom="@dimen/size_1dp"
        android:layout_toLeftOf="@+id/tvTime"
        android:layout_weight="1"
        android:maxLines="1"
        android:text="hello my title is hello"
        android:textColor="@color/text_3"
        android:textSize="@dimen/size_16sp" />

    <ImageView
        android:id="@+id/ivCollect"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_below="@+id/tvTime"
        android:layout_alignParentRight="true"
        android:layout_marginTop="@dimen/size_1dp"
        android:layout_marginBottom="@dimen/size_2dp"
        android:src="@drawable/collect_select_selector" />

    <TextView
        android:id="@+id/tvAuthor"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_below="@+id/tvTitle"
        android:layout_alignBaseline="@+id/ivCollect"
        android:layout_marginTop="@dimen/size_1dp"
        android:layout_marginBottom="@dimen/size_2dp"
        android:layout_toLeftOf="@+id/ivCollect"
        android:text="hello, i am the author"
        android:textColor="@color/text_6"
        android:textSize="@dimen/size_12sp" />
</RelativeLayout>

```

### 4、创建ViewHolder类。

```java

 class ViewHolder extends RecyclerView.ViewHolder {
        View dataView;

        TextView tvTime;
        TextView tvAuthor;
        TextView tvTitle;
        ImageView imgCollect;

        public ViewHolder(View view) {
            super(view);
            dataView = view;
            tvTime = view.findViewById(R.id.tvTime);
            tvAuthor = view.findViewById(R.id.tvAuthor);
            tvTitle = view.findViewById(R.id.tvTitle);
            imgCollect = view.findViewById(R.id.ivCollect);
        }
    }

```

## 5、创建Adapter类，实现三个重写的方法，用来创建viewHolder,绑定item数据，和计算item项数量：

```java

public class CDataAdapter extends RecyclerView.Adapter<CDataAdapter.ViewHolder> {
    private List<CData> mDataList;

    public CDataAdapter(List<CData> fruitList) {
        mDataList = fruitList;
    }

    @NonNull
    @Override
    public ViewHolder onCreateViewHolder(@NonNull ViewGroup parent, int viewType) {
        View view = LayoutInflater.from(parent.getContext())
                .inflate(R.layout.rv_article_item, parent, false);
        final ViewHolder holder = new ViewHolder(view);
        return holder;
    }

    @Override
    public void onBindViewHolder(@NonNull ViewHolder holder, int position) {
        CData data = mDataList.get(position);
        holder.tvAuthor.setText(data.author);
        holder.tvTime.setText(data.time);
        holder.tvTitle.setText(data.name);
    }

    @Override
    public int getItemCount() {
        return mDataList.size();
    }

}

```

## 6、在Activity中实现RecyclerView和LayoutManger以及Adapter的绑定：
```java

public class MainActivity extends AppCompatActivity {

    private List<CData> dataList = new ArrayList<>();

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        RecyclerView recyclerView = findViewById(R.id.recycler_view);
        LinearLayoutManager layoutManager = new LinearLayoutManager(this);
        recyclerView.setLayoutManager(layoutManager);
        CDataAdapter adapter = new CDataAdapter(dataList);
        recyclerView.setAdapter(adapter);

	//添加自定义分割线：可自定义分割线高度和颜色
        recyclerView.addItemDecoration(new RecyclerViewDivider(
                this, LinearLayoutManager.HORIZONTAL, 20, getResources().getColor(R.color.scanner_text_hint)));
    }
}

```

## 7、关于item的点击事件响应，在viewholder中创建回调，通过adapter传递。

（略）参见utils中的viewholder和adapter

## 8、附上一个创建item项的分割线的类RecyclerViewDivider
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







