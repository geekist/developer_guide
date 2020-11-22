# SwipeRefreshLayout

SwipeRefreshLayout是用于实现下拉刷新的核心类，只要把想实现下拉刷新的控件放置到
SwipeRefreshLayout中，就可以让该控件支持下拉刷新

* 依赖性
```xml
    implementation 'androidx.swiperefreshlayout:swiperefreshlayout:1.0.0'
```
* 布局文件
```xml
  <androidx.swiperefreshlayout.widget.SwipeRefreshLayout
            android:id="@+id/swipeRefresh"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            app:layout_behavior="@string/appbar_scrolling_view_behavior">

            <androidx.recyclerview.widget.RecyclerView
                android:id="@+id/recyclerView"
                android:layout_width="match_parent"
                android:layout_height="match_parent">
                
            </androidx.recyclerview.widget.RecyclerView>
        </androidx.swiperefreshlayout.widget.SwipeRefreshLayout>
```

* 响应事件
```kotlin
   swipeRefresh.setColorSchemeResources(R.color.purple_200)
        swipeRefresh.setOnRefreshListener {
            refreshFruits(adapter)
        }


   private fun refreshFruits(adapter: FruitAdapter) {
        thread {
            Thread.sleep(2000)
            runOnUiThread {
                initFruits()
                adapter.notifyDataSetChanged()
                swipeRefresh.isRefreshing = false
            }
        }
    }

```