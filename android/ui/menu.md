## Menu

* 首先创建一个menu的布局文件：

```java

<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
    <item
        android:id="@+id/add_item"
        android:title="Add" />

    <item
        android:id="@+id/remove_item"
        android:title="Remove"/>
</menu>


```

* 添加onCreateOptionsMenu将布局文件和Activity绑定起来

```java

  override fun onCreateOptionsMenu(menu: Menu?): Boolean {
        menuInflater.inflate(R.menu.main, menu)
        return super.onCreateOptionsMenu(menu)
    }


```

* 调用onOptionsItemSelected相应点击事件
```java

    override fun onOptionsItemSelected(item: MenuItem): Boolean {
        when (item.itemId) {
            R.id.add_item -> Toast.makeText(this, "add is selected", Toast.LENGTH_LONG).show()
            R.id.remove_item -> Toast.makeText(this, "add is selected", Toast.LENGTH_LONG).show()
        }
        return super.onOptionsItemSelected(item)
    }

```




