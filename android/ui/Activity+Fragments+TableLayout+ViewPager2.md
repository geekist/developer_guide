

# 实现一个Activity多个Fragments在TableLayout下实现fragments横向滑动的效果

## 1、依赖项配置

在build.gradle中需要material的依赖

  implementation 'androidx.viewpager2:viewpager2:1.0.0-alpha01'

  implementation 'androidx.appcompat:appcompat:1.2.0'


## 2、新建一个activity，布局文件如下：

```java

<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"

    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    tools:context=".MainActivity">

    <com.google.android.material.tabs.TabLayout
        android:id="@+id/tl_title"
        android:layout_width="match_parent"
        android:layout_height="50dp"
        app:tabGravity="fill"
        app:tabIndicatorColor="@color/purple_200"
        app:tabMode="fixed"
        app:tabSelectedTextColor="@color/purple_500"
        app:tabTextColor="@color/black" />

    <androidx.viewpager.widget.ViewPager
        android:id="@+id/vp_container"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="horizontal" />
</LinearLayout>

```


### 3、创建三个fragment，默认设置不更改。





### 4、创建一个Adapter文件处理ViewPager中fragment项。

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

## 5、在activity中调用方式：

```java

public class MainActivity extends AppCompatActivity {
    private List<Fragment> fragments;
    private MyFragmentAdapter myFragmentAdapet;
    private String[] titles = {"Fragment1","Fragment2","Fragment3"};
    
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

## 6、ViewPager和ViewPager2在用法上的区别：
- 1、Adapter继承自FragmentStateAdapter，而且只需要两个重载的方法，getTitleName不需要了。
- 2、MainActivity中使用TablayoutMediator类实现绑定Adapter，tab的title不再从adapter中拿，adapter和TabLayout解耦。





