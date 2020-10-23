

# 如何向应用中添加一个fragment

## 1、依赖项配置

如果没有特殊的依赖项，这里可以不需要。


## 2、为Fragment新建一个布局文件

res->layout->右键添加一个布局文件，命名位fragment_xxx(其中xxx是功能）。

下面的示例是android studio新建一个空的fragment时生成的布局文件。
```java
<?xml version="1.0" encoding="utf-8"?>
<FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".Item1Fragment">

    <!-- TODO: Update blank fragment layout -->
    <TextView
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:text="@string/hello_blank_fragment" />

</FrameLayout>
```
或者可以将fragment直接包含在Activity的布局文件中

res/layout-large/news_articles.xml

```java
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:orientation="horizontal"
        android:layout_width="fill_parent"
        android:layout_height="fill_parent">

        <fragment android:name="com.example.android.fragments.HeadlinesFragment"
                  android:id="@+id/headlines_fragment"
                  android:layout_weight="1"
                  android:layout_width="0dp"
                  android:layout_height="match_parent" />

        <fragment android:name="com.example.android.fragments.ArticleFragment"
                  android:id="@+id/article_fragment"
                  android:layout_weight="2"
                  android:layout_width="0dp"
                  android:layout_height="match_parent" />

    </LinearLayout>
    
```

### 3、创建一个Fragment文件，命名为XXXFragment。

```java

public class Item1Fragment extends Fragment {

    // TODO: Rename parameter arguments, choose names that match
    // the fragment initialization parameters, e.g. ARG_ITEM_NUMBER
    private static final String ARG_PARAM1 = "param1";
    private static final String ARG_PARAM2 = "param2";

    // TODO: Rename and change types of parameters
    private String mParam1;
    private String mParam2;

    public Item1Fragment() {
        // Required empty public constructor
    }

    /**
     * Use this factory method to create a new instance of
     * this fragment using the provided parameters.
     *
     * @param param1 Parameter 1.
     * @param param2 Parameter 2.
     * @return A new instance of fragment Item1Fragment.
     */
    // TODO: Rename and change types and number of parameters
    public static Item1Fragment newInstance(String param1, String param2) {
        Item1Fragment fragment = new Item1Fragment();
        Bundle args = new Bundle();
        args.putString(ARG_PARAM1, param1);
        args.putString(ARG_PARAM2, param2);
        fragment.setArguments(args);
        return fragment;
    }

    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        if (getArguments() != null) {
            mParam1 = getArguments().getString(ARG_PARAM1);
            mParam2 = getArguments().getString(ARG_PARAM2);
        }
    }

    @Override
    public View onCreateView(LayoutInflater inflater, ViewGroup container,
                             Bundle savedInstanceState) {
        // Inflate the layout for this fragment
        return inflater.inflate(R.layout.fragment_item1, container, false);
    }
}
```
其中最重要的是onCreateView，实现了布局文件的加载。
```java

    @Override
    public View onCreateView(LayoutInflater inflater, ViewGroup container,
                             Bundle savedInstanceState) {
        // Inflate the layout for this fragment
        return inflater.inflate(R.layout.fragment_item1, container, false);
    }

```


### 4、在Fragment类中还需要注意的几个方法：

片段所在 Activity 的生命周期会直接影响片段的生命周期，其表现为，Activity 的每次生命周期回调都会引发每个片段的类似回调。例如，当 Activity 收到 onPause() 时，Activity 中的每个片段也会收到 onPause()。

不过，片段还有几个额外的生命周期回调，用于处理与 Activity 的唯一交互，从而执行构建和销毁片段界面等操作。这些额外的回调方法是：

- onAttach()

在片段已与 Activity 关联时进行调用（Activity 传递到此方法内）。

- onCreateView()

调用它可创建与片段关联的视图层次结构。

- onActivityCreated()

当 Activity 的 onCreate() 方法已返回时进行调用。


- onDestroyView()

在移除与片段关联的视图层次结构时进行调用。

- onDetach()

在取消片段与 Activity 的关联时进行调用。

### 5、向Activity添加fragment的两种方法：

- 在 Activity 的布局文件内声明片段。
```java
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="horizontal"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    <fragment android:name="com.example.news.ArticleListFragment"
            android:id="@+id/list"
            android:layout_weight="1"
            android:layout_width="0dp"
            android:layout_height="match_parent" />
    <fragment android:name="com.example.news.ArticleReaderFragment"
            android:id="@+id/viewer"
            android:layout_weight="2"
            android:layout_width="0dp"
            android:layout_height="match_parent" />
</LinearLayout>

```
创建此 Activity 布局时，系统会将布局中指定的每个片段实例化，并为每个片段调用 onCreateView() 方法，以检索每个片段的布局。系统会直接插入片段返回的 View，从而代替 <fragment> 元素。

- 通过编程方式实现添加

```java

FragmentManager fragmentManager = getSupportFragmentManager();
FragmentTransaction fragmentTransaction = fragmentManager.beginTransaction();

ExampleFragment fragment = new ExampleFragment();
fragmentTransaction.add(R.id.fragment_container, fragment);
fragmentTransaction.commit();

```

- fragment的动态管理：replace、add、delete操作

```java

// Create new fragment and transaction
Fragment newFragment = new ExampleFragment();
FragmentTransaction transaction = getSupportFragmentManager().beginTransaction();

// Replace whatever is in the fragment_container view with this fragment,
// and add the transaction to the back stack
transaction.replace(R.id.fragment_container, newFragment);
transaction.addToBackStack(null);

// Commit the transaction
transaction.commit();

```

### 6、在RecyclerView和ViewPager中使用fragment：

在Activity的代码中：
```java
@Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        fragments = new ArrayList<>();
        fragments.add(new Item1Fragment());
        fragments.add(new Item2Fragment());
        fragments.add(new Item3Fragment());

        final ViewPager2 ViewPager2 = findViewById(R.id.vp_container);
        final TabLayout tableLayout = findViewById(R.id.tl_title);
        myFragmentAdapet = new MyFragmentAdapter(this,fragments);

        //1、绑定ViewPager和adapter
        ViewPager2.setAdapter(myFragmentAdapet);

        //2、绑定TabLayout和ViewPager
        new TabLayoutMediator(tableLayout, ViewPager2, new TabLayoutMediator.TabConfigurationStrategy() {
            @Override
            public void onConfigureTab(@NonNull TabLayout.Tab tab, int position) {
                tab.setText(titles[position]);
            }
        }).attach();
    }
}

```
在Adapter的代码中：
```java
class MyFragmentAdapter extends FragmentStateAdapter {
    private List<Fragment> fragments;

    public MyFragmentAdapter(@NonNull FragmentActivity fragmentActivity, List<Fragment> fragments) {
        super(fragmentActivity);
        this.fragments = fragments;
    }

    @NonNull
    @Override
    public Fragment createFragment(int position) {
        return fragments.get(position);
    }

    @Override
    public int getItemCount() {
        return fragments.size();
    }
}

```

