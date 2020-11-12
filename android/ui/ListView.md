## ListView
* 在布局文件中添加一个listView


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

    <Button
        android:id="@+id/button_first"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/next"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@id/textview_first" />
</androidx.constraintlayout.widget.ConstraintLayout>


```

* 同时创建一个listView的item布局layout，layout_lsitview_item.xml用来展示listview的每一项


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