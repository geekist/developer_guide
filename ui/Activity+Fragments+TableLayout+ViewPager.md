Activity+Fragments+TableLayout+ViewPager

# 实现一个Activity多个Fragments在TableLayout下实现fragments横向滑动的效果

## 1、依赖项配置

在build.gradle中需要material的依赖

implementation 'com.google.android.material:material:1.2.1'



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
        android:orientation="vertical" />
</LinearLayout>

```


### 3、创建三个fragment，默认设置不更改

### 4、创建一个Adapter文件处理ViewPager中fragment项

```java
class MyAdapter extends FragmentPagerAdapter {
    ArrayList<Fragment> list;
    String[] titles;

    public MyAdapter(FragmentManager fm, ArrayList<Fragment> list, String[] titles) {
        super(fm);
        this.list = list;
        this.titles = titles;
    }

    @Override
    public Fragment getItem(int position) {
        return list.get(position);
    }

    @Override
    public int getCount() {
        return list.size();
    }

    //tablelayout 中调用，作为tablelayout的title来显示
    @Override
    public CharSequence getPageTitle(int position) {
        return titles[position];
    }
}
```

## 5、在activity中调用方式：(两行代码实现绑定）

```java
public class MainActivity extends AppCompatActivity {
    private TabLayout tabLayout;
    private ViewPager viewPager;
    
    private ArrayList<Fragment> fragments;
    private MyAdapter adapter;
    
    private String[] titles = {"title1","title2","title3"};

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        
        fragments = new ArrayList<>();
        fragments.add(new ItemFragment());
        fragments.add(new Item2Fragment());
        fragments.add(new Item3Fragment());

        viewPager = (ViewPager) findViewById(R.id.vp_container);
        tabLayout = (TabLayout) findViewById(R.id.tl_title);
        adapter = new MyAdapter(getSupportFragmentManager(), fragments,titles);
        
        //1、将ViewPager和Adapter绑定起来
        viewPager.setAdapter(adapter);
        
        //2、将TableLayout和ViewPager绑定起来
        tabLayout.setupWithViewPager(viewPager);
    }
}
```




