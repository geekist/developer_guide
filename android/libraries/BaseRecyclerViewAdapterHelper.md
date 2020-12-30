# BaseRecyclerViewAdapterHelper
BRVAH是一个灵活强大的循环适配器。用来代替RecyclerView的adapter，官网介绍可以节省70%的代码。

Github网址：https://github.com/CymChad/BaseRecyclerViewAdapterHelper

官方网站：http://www.recyclerview.org/

使用指南：https://www.jianshu.com/p/b343fcff51b0

更新地址：https://github.com/CymChad/BaseRecyclerViewAdapterHelper/releases


## 依赖项：

先在 build.gradle(Project:XXXX) 的 repositories 添加:
```jva
    allprojects {
        repositories {
            ...
            maven { url "https://jitpack.io" }
        }
    }

```
然后在 build.gradle(Module:app) 的 dependencies 添加:
```jva
       dependencies {
            compile 'com.github.CymChad:BaseRecyclerViewAdapterHelper:2.9.30'
    }
```

## 简单使用方法：

### 1、创建一个子类继承BaseQuickAdapter类.

#### 1-1：定义子类时实化两个泛型.

```java
public abstract class BaseQuickAdapter<T, K extends BaseViewHolder> extends RecyclerView.Adapter<K> {

```
第一个泛型是输入到adapter的数据列表的元素类型，第二个泛型是BaseViewHolder的一个子类。

#### 1-2： 构造函数：

BaseQuickAdapter提供了3个构造函数：目的是输入item的layout的Id和填充adapter的数据列表，可以在后面设置。
子类的构造函数可以根据实际情况使用任意一个。

```java
  public BaseQuickAdapter(@LayoutRes int layoutResId, @Nullable List<T> data) {
        this.mData = data == null ? new ArrayList<T>() : data;
        if (layoutResId != 0) {
            this.mLayoutResId = layoutResId;
        }
    }

    public BaseQuickAdapter(@Nullable List<T> data) {
        this(0, data);
    }

    public BaseQuickAdapter(@LayoutRes int layoutResId) {
        this(layoutResId, null);
    }
```




public class HomeAdapter extends BaseQuickAdapter<HomeItem, BaseViewHolder> {
    public HomeAdapter(int layoutResId, List data) {
        super(layoutResId, data);
    }

    @Override
    protected void convert(BaseViewHolder helper, HomeItem item) {
        helper.setText(R.id.text, item.getTitle());
        helper.setImageResource(R.id.icon, item.getImageResource());
        // 加载网络图片
      Glide.with(mContext).load(item.getUserAvatar()).crossFade().into((ImageView) helper.getView(R.id.iv));
    }
}

#### 1-3：覆写convert函数，类型于adapter的onBindVIewHolder，将数据项填充到UI控件。

一个简单的adapter的定义
```java
public class HomeAdapter extends BaseQuickAdapter<HomeItem, BaseViewHolder> {
    public HomeAdapter(int layoutResId, List data) {
        super(layoutResId, data);
    }

    @Override
    protected void convert(BaseViewHolder helper, HomeItem item) {
        helper.setText(R.id.text, item.getTitle());
        helper.setImageResource(R.id.icon, item.getImageResource());
        // 加载网络图片
      Glide.with(mContext).load(item.getUserAvatar()).crossFade().into((ImageView) helper.getView(R.id.iv));
    }
}

```

### 2、在Activity或fragment中使用BaseQuickAdapter
```java

//首先实例化一个adapter类，传入数据列表，设置点击响应事件，以及头部（可选）
  private val homeAdapter by lazy {
        HomeAdapter(homeItemData).apply {
            animationEnable = true

            val top = layoutInflater.inflate(R.layout.top_view, binding.recyclerView, false)
            addHeaderView(top)
            setOnItemClickListener(this@HomeActivity)
        }
    }

//在点击事件中设置响应事件
    override fun onItemClick(adapter: BaseQuickAdapter<*, *>, view: View, position: Int) {
        val item = adapter.data[position] as HomeEntity
        if (!item.isHeader) {
            startActivity(Intent(this@HomeActivity, item.activity))
        }
    }

//将adapter和recyclerView绑定
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        binding = ActivityHomeBinding.inflate(layoutInflater)
        setContentView(binding.root)

        binding.recyclerView.adapter = homeAdapter

```

## adapter设置方法详解：

### 1-给adapter设置数据

## 4、BaseQuickAdapter主要属性、方法说明## 4、BaseQuickAdapter主要属性、方法说明

|  | Java | Kotlin | 说明 |
| :--- | ---- | ---- | ---- |
|获取Context|getContext()|context||
| **数据相关** |      |      | |
| 获取Adapter中数据 | getData() | data | 只能get |
| 设置新的数据实例 | setNewData() | setNewData() | 将会替换List指针引用 |
| 添加数据 | addData() | addData() |  |
| 移除数据 | remove() | remove() |  |
| 改变某一位置的数据 | setData() | setData() |  |
| 替换整个数据 | replaceData() | replaceData() | 不会更改原数据的引用 |
| 设置Diff数据（异步，推荐） | setDiffCallback() | setDiffCallback() | 配置数据差异化比较的Callback |
|  | setDiffConfig() | setDiffConfig() | 更高程度的自定义化配置 |
|  | setDiffNewData(List<T>) | setDiffNewData(List<T>?) | 必须先设置setDiffCallback() 或者 setDiffConfig()，否则不生效 |
| 设置Diff数据 | setDiffNewData(DiffResult, List<T>) | setDiffNewData(DiffResult, List<T>?) | 通过DiffResult设置数据，Adapter内部不关心Diff过程，只要结果。 |
| **空布局** | |||

|  | Java | Kotlin | 说明 |
| :--- | ---- | ---- | ---- |
|获取Context|getContext()|context||
| **数据相关** |      |      | |
| 获取Adapter中数据 | getData() | data | 只能get |
| 设置新的数据实例 | setNewData() | setNewData() | 将会替换List指针引用 |
| 添加数据 | addData() | addData() |  |
| 移除数据 | remove() | remove() |  |
| 改变某一位置的数据 | setData() | setData() |  |
| 替换整个数据 | replaceData() | replaceData() | 不会更改原数据的引用 |
| 设置Diff数据（异步，推荐） | setDiffCallback() | setDiffCallback() | 配置数据差异化比较的Callback |
|  | setDiffConfig() | setDiffConfig() | 更高程度的自定义化配置 |
|  | setDiffNewData(List<T>) | setDiffNewData(List<T>?) | 必须先设置setDiffCallback() 或者 setDiffConfig()，否则不生效 |
| 设置Diff数据 | setDiffNewData(DiffResult, List<T>) | setDiffNewData(DiffResult, List<T>?) | 通过DiffResult设置数据，Adapter内部不关心Diff过程，只要结果。 |
 |||





