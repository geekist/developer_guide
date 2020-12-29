# Paging

* [官方文档](https://developer.android.com/topic/libraries/architecture/paging)

* [关于paging原理的介绍](https://qingmei2.blog.csdn.net/article/details/102713200)

## Paging介绍

分页库：一次加载和显示一小块数据。按需载入部分数据会减少网络带宽和系统资源的使用量。

工作流程：

1、使用DataSource从服务器获取或者从本地数据库获取数据(需要自己实现)

2、将数据保存到PageList中(Paging库已实现)

3、将PageList的数据submitList给PageListAdapter(需要自己调用)

4、PageListAdapter在后台线程对比原来的PageList和新的PageList，生成新的PageList(Paging库已实现对比操作，用户只需提供DiffUtil.ItemCallback实现)
5、PageListAdapter通知RecyclerView更新

![](https://upload-images.jianshu.io/upload_images/7037957-133babd8949f7752.gif?imageMogr2/auto-orient/strip|imageView2/2/w/800/format/webp)

## 依赖项

```java

    dependencies {
      def paging_version = "2.1.2"

    implementation "androidx.paging:paging-runtime:$paging_version" // For Kotlin use paging-runtime-ktx

    implementation "androidx.paging:paging-runtime:2.0.0"
    implementation 'androidx.recyclerview:recyclerview:1.1.0'
    implementation 'com.squareup.retrofit2:retrofit:2.9.0'
    implementation "android.arch.lifecycle:extensions:1.1.1"
    }
```    

## paging的主要组件

### PagedList
分页数据列表容器，可以对分页数据异步加载。分页库的核心构件是PagedList类, 它是一个集合, 用于异步加载应用数据块或者数据页. 该类在应用的其它架构之间充当中介.

![](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91c2VyLWdvbGQtY2RuLnhpdHUuaW8vMjAxOS8xMC8yMy8xNmRmOTI0OTAxMjFmNjc4?x-oss-process=image/format,png)

### DataSource

每一个PagedList实例从DataSource中加载最新的应用数据. 数据从应用后端或者数据库流入PagedList对象. 分页包支持多样的应用架构, 包括脱机数据库和与后台服务器通讯的数据库.
 
 DataSource有三个实现类

* ItemKeyedDataSource

列表中加载了N条数据，加载下一页数据时，会以列表中最后一条数据的某个字段为Key查询下一页数

* PageKeyedDataSource 
 
页表中加载了N条数据，每一页数据都会提供下一页数据的关键字Key作为下次查询的依据

* PositionalDataSource 

指定位置加载数据，在数据量已知的情况下使用

以PageKeyedDataSource为例：
PageKeyedDataSource中有三个抽象方法。

loadInitial 表示RecyclerView没有数据第一次请求数据
loadBefore 请求上一页数据(基本不用)
loadAfter 请求下一页数据

```java
class PageKeyedSubredditDataSource(
        private val redditApi: RedditApi,
        private val subredditName: String,
        private val retryExecutor: Executor
) : PageKeyedDataSource<String,RedditPost>(){

    override fun loadInitial(params: LoadInitialParams<String>, callback: LoadInitialCallback<String, RedditPost>) {
        val request = redditApi.getTop(
                subreddit = subredditName,
                limit = params.requestedLoadSize
        )
        val response = request.execute()
        val data = response.body()?.data
        val items = data?.children?.map { it.data } ?: emptyList()
        callback.onResult(items, data?.before, data?.after)
    }

    override fun loadAfter(params: LoadParams<String>, callback: LoadCallback<String, RedditPost>) {

        redditApi.getTopAfter(subreddit = subredditName,
                after = params.key,
                limit = params.requestedLoadSize).enqueue(
                object : retrofit2.Callback<RedditApi.ListingResponse> {
                    override fun onFailure(call: Call<RedditApi.ListingResponse>, t: Throwable) {
                    }

                    override fun onResponse(
                            call: Call<RedditApi.ListingResponse>,
                            response: Response<RedditApi.ListingResponse>) {
                        if (response.isSuccessful) {
                            val data = response.body()?.data
                            val items = data?.children?.map { it.data } ?: emptyList()
                            callback.onResult(items, data?.after)
                        } else {
//                            retry = {
//                                loadAfter(params, callback)
//                            }
//                            networkState.postValue(
//                                    NetworkState.error("error code: ${response.code()}"))
                        }
                    }
                }
        )
    }


    override fun loadBefore(params: LoadParams<String>, callback: LoadCallback<String, RedditPost>) {
        TODO("not implemented") //To change body of created functions use File | Settings | File Templates.
    }
}

```

datasource.factory的代码
```java
class DataFactory: DataSource.Factory<Int, MyDataBean>() {
 
    private val mSourceLiveData: MutableLiveData<MyDataSource> =
        MutableLiveData<MyDataSource>()
 
    override fun create(): DataSource<Int, MyDataBean> {
        val myDataSource = MyDataSource()
        mSourceLiveData.postValue(myDataSource)
        return myDataSource
    }
 
}

```

### PagedListAdapter 用于适配 RecyclerView
PagedList类通过PagedListAdapter加载数据项到RecyclerView里面. 在加载数据的时候, 这些类协同工作, 拉取数据并展示内容, 包括预取看不见的内容并在内容改变时加载动画.

```java

class MyPagedListAdapter extends PagedListAdapter<MyDataBean, MyViewHolder> {
 
 
    private static DiffUtil.ItemCallback<MyDataBean> DIFF_CALLBACK =
            new DiffUtil.ItemCallback<MyDataBean>() {
                @Override
                public boolean areItemsTheSame(MyDataBean oldConcert, MyDataBean newConcert) {
                    // 比较两个Item数据是否一样的，可以简单用 Bean 的 hashCode来对比
                    return oldConcert.hashCode() == newConcert.hashCode();
                }
 
                @Override
                public boolean areContentsTheSame(MyDataBean oldConcert,
                                                  MyDataBean newConcert) {
                    // 写法基本上都这样写死即可
                    return oldConcert.equals(newConcert);
                }
            };
 
    
    public MyPagedListAdapter() {
        // 通过 构造方法 设置 DiffUtil.ItemCallback 
        super(DIFF_CALLBACK);
    }
 
    @NonNull
    @Override
    public MyViewHolder onCreateViewHolder(@NonNull ViewGroup parent, int viewType) {
        View inflate = LayoutInflater.from(parent.getContext()).inflate(R.layout.item_my, parent, false);
        return new MyViewHolder(inflate);
    }
 
    @Override
    public void onBindViewHolder(@NonNull MyViewHolder holder, int position) {
        // getItem() 是 PagedListAdapter 内部方法，通过此方法可以获取到对应位置Item 的数据
        holder.bindData(getItem(position));
    }
 
}

```

ViewHolder的代码
```java

class MyViewHolder(val itemV: View) : RecyclerView.ViewHolder(itemV) {
 
    fun bindData(data: MyDataBean) {
        itemV.findViewById<TextView>(R.id.tvNum).text = data.position.toString()
 
        if (data.position % 2 == 0){
            itemV.findViewById<TextView>(R.id.tvNum).setBackgroundColor(itemV.context.resources.getColor(R.color.colorAccent))
        }else{
            itemV.findViewById<TextView>(R.id.tvNum).setBackgroundColor(itemV.context.resources.getColor(R.color.colorPrimaryDark))
        }
    }
}

```


在Activity中的调用

```java
class MainActivity : AppCompatActivity() {
 
 
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
 
        val myPagedListAdapter = MyPagedListAdapter()
        recyclerView.layoutManager = LinearLayoutManager(this)
        recyclerView.adapter = myPagedListAdapter
 
        /**
         *      ViewModel 绑定 Activity 生命周期
         * */
        val myViewModel = ViewModelProviders.of(this).get(MyViewModel::class.java)
 
        /**
         *      ViewModel 感知数据变化，更新 Adapter 列表
         * */
        myViewModel.getConvertList().observe(this, Observer {
            myPagedListAdapter.submitList(it)
        })
    }
 
}


```
