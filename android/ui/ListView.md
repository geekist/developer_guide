# ListView
ListView 通常绑定ArrayAdapter使用，使用过程中注意有三种使用方法：

## 不使用ArrayAdapter，直接添加简单数据：
* step1：在布局文件中添加一个listView
```java
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".FirstFragment">
    
    <ListView
        android:id="@+id/lv_list"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:divider="@android:color/holo_blue_dark"
        android:dividerHeight="1dp"
        android:entries="@array/list"></ListView>
</androidx.constraintlayout.widget.ConstraintLayout>
```
* 2、配置一些数据：在res的string中添加数组：

```java
<resources>
    <string name="app_name">ArrayAdapter数组适配器</string>
    <string-array name = "list">   <!--数组资源名称为list 与layout中对应-->
        <item>第一行</item>
        <item>第二行</item>
        <item>第三行</item>
        <item>第四行</item>
      </string-array>
</resources>
```
* 3、 程序中直接使用listview

```java

public class MainActivity extends AppCompatActivity {
    ListView listView;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_list);
 
        listView = (ListView) findViewById(R.id.lv_list);
        listView.setOnItemClickListener(new AdapterView.OnItemClickListener() {
            @Override
            public void onItemClick(AdapterView<?> parent, View view, int position, long id) {
                String result = parent.getItemAtPosition(position).toString();//获取选择项的值
                Toast.makeText(MainActivity.this,"您点击了"+result,Toast.LENGTH_SHORT).show();
            }
        });
    }
}

```
## 第二种方法，使用Adapter，但是不用自己创建list-item布局文件，使用系统自带的android.R.layout.simple_list_item_1

* step1：在布局文件中添加一个listView
```java
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".FirstFragment">
    
    <ListView
        android:id="@+id/lv_list"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:divider="@android:color/holo_blue_dark"
        android:dividerHeight="1dp"
        android:entries="@array/list"></ListView>
</androidx.constraintlayout.widget.ConstraintLayout>
```

* 3、 程序中直接配置一些数据，使用listview
```java
public class MainActivity extends AppCompatActivity {
    ListView listView;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_list);

 		listView = (ListView) findViewById(R.id.lv_list);
 
        String data[] = {"1","2","3","4","5","6","7","8","9","10","11","12","13"};
           
		//创建数组适配器，作为数据源和列表控件联系的桥梁
           //第一个参数：上下文环境
           //第二个参数：当前列表项加载的布局文件
           //第三个参数：数据源
        arrayAdapter = new ArrayAdapter<String>(this,
						android.R.layout.simple_list_item_1,
						data);

        listView.setAdapter(arrayAdapter);        //listview视图加载适配器

        listView.setOnItemClickListener(new AdapterView.OnItemClickListener() {
            @Override
            public void onItemClick(AdapterView<?> parent, View view, int position, long id) {
                String result = parent.getItemAtPosition(position).toString();//获取选择项的值
                Toast.makeText(MainActivity.this,"您点击了"+result,Toast.LENGTH_SHORT).show();
            }
        });

    }
}

```
## 第三种方法，自己创建一个list_item布局文件，但是ArrayAdapter构造方法要用4个参数，其中包含了一个textVIew

* step1：在布局文件中添加一个listView
```java
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".FirstFragment">
    
    <ListView
        android:id="@+id/lv_list"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:divider="@android:color/holo_blue_dark"
        android:dividerHeight="1dp"
        android:entries="@array/list"></ListView>
</androidx.constraintlayout.widget.ConstraintLayout>

* step2：同时创建一个listView的item布局layout，layout_lsitview_item.xml用来展示listview的每一项
:
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
```

* 3、 程序中直接配置一些数据，使用listview
```java
public class MainActivity extends AppCompatActivity {
    ListView listView;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_list);

 	
        listView = (ListView) findViewById(R.id.lv_list);
 
        initData();//数据源
      //  String data[] = {"1","2","3","4","5","6","7","8","9","10","11","12","13"};
 
        //创建数组适配器，作为数据源和列表控件联系的桥梁
        arrayAdapter = new ArrayAdapter<String>(this,
								R.layout.activity_list,
								R.id.tv_info,
								data);
        //listview视图加载适配器
        listView.setAdapter(arrayAdapter);
        //为列表视图中选中的项添加响应事件
        listView.setOnItemClickListener(new AdapterView.OnItemClickListener() {
            @Override
            public void onItemClick(AdapterView<?> parent, View view, int position, long id) {
                String result = parent.getItemAtPosition(position).toString();//获取选择项的值
                Toast.makeText(MainActivity.this,"您点击了"+result,Toast.LENGTH_SHORT).show();
 
            }
        });


    }
}
```

## 第四种方法：自定义一个ArrayAdapter的子类
* step1：在布局文件中添加一个listView
```java
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".FirstFragment">
    
    <ListView
        android:id="@+id/listView"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        ></ListView>
</androidx.constraintlayout.widget.ConstraintLayout>


```

* step2：同时创建一个listView的item布局layout，layout_lsitview_item.xml用来展示listview的每一项
:
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

* 创建一些数据

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

* 创建Adapter加载数据


```java

class ListViewAdapter( activity: Activity,  val resourceId: Int, data: List<ObjItem>):
    ArrayAdapter<ObjItem>(activity,resourceId,data) {

    inner class ViewHolder(val itemImage: ImageView, val itemName: TextView) {

    }

    override fun getView(position: Int, convertView: View?, parent: ViewGroup): View {
        lateinit var view: View
        val viewHolder: ViewHolder

        if(convertView == null) {
            view = LayoutInflater.from(context).inflate(resourceId,parent,false)
            val objImage: ImageView = view.findViewById(R.id.imageView)
            val objName: TextView = view.findViewById(R.id.textView)
            viewHolder = ViewHolder(objImage,objName)
            view.tag = viewHolder
        }else {
            view = convertView
            viewHolder = view.tag as ViewHolder
        }

        val objItem = getItem(position)
        if(objItem != null) {
            viewHolder.itemImage.setImageResource(objItem.imageId)
            viewHolder.itemName.text = objItem.name
        }


        return view;
    }
}

```

* 将adapter和listView绑定起来

主要做的事情是：

1、准备一份数据列表

2、创建一个adapter的实例，将数据列表传入adapter

3、将adapter和listview绑定起来

4、设置listview的每一项的监听事件回调


```java


    override fun onCreateView(
        inflater: LayoutInflater, container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View? {
        // Inflate the layout for this fragment
        val view = inflater.inflate(R.layout.fragment_first, container, false)
        initObjItems()
        val adatper =
            ListViewAdapter(activity as Activity, R.layout.layout_listview_item, objItemList)

        view.listView.adapter = adatper

        view.listView.setOnItemClickListener { _, _, position, _ ->
            val objItem = objItemList[position].name
            Toast.makeText(context, objItem, Toast.LENGTH_LONG).show()
        }
        return view
    }

```