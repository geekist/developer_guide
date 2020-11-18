

# ContentProvider

用于在不同的应用程序之间实现数据共享，它提供了一套完整的机制，允许一个应用程序访问另外一个应用程序，同时能保证被访问程序的安全性。

## 运行时权限申请
Android6.0系统及以后的系统中增加了运行时权限申请的功能，用户不需要在安装软件的时候一次授权所有的权限，可以在软件使用过程中对某一权限进行授权。
```kotlin
class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        buttonDia.setOnClickListener{
           if(ContextCompat.checkSelfPermission(this, Manifest.permission.CALL_PHONE) != PackageManager.PERMISSION_GRANTED){
               ActivityCompat.requestPermissions(this,arrayOf(Manifest.permission.CALL_PHONE),1)
           }else {
               call()
           }
        }
    }

    override fun onRequestPermissionsResult(requestCode: Int, permissions: Array<out String>, grantResults: IntArray) {
        super.onRequestPermissionsResult(requestCode, permissions, grantResults)
        when(requestCode) {
            1->{
                if(grantResults.isNotEmpty() && grantResults[0] == PackageManager.PERMISSION_GRANTED) {
                    call()
                }else{
                    Toast.makeText(this,"you denied the permission",Toast.LENGTH_LONG).show()
                }
            }
        }
    }

    private fun call() {
        try {
            val intent = Intent(Intent.ACTION_CALL)
            intent.data = Uri.parse("tel:10086")
            startActivity(intent)
        }catch (e: SecurityException) {
            e.printStackTrace()
        }
    }
}

```
## 访问contentProvider

### ContentResolver类的基本用法

#### 1、获取contentResolver实例

`val contentResolver = Context.getContentResolver()`


#### 2、内容URI

`协议名：//authority/path`

例如：
val str = "content://com.example.app.provider/table1"
val uri = Uri.parse(str)

#### 3、用contentResolver进行操作
* 查询操作

```kotlin
val cursor = contentResolver.query(
	uri,
	projection,
	selection,
	selectionArgs,
	sortOrder)

while(cursor.moveToFirst()) {

}
cursor.close()
```

* 增加操作

```java
val values = contentValuesOf("column1" to "","column2" to 1)
contentResolver.insert(values)
```

* 删除操作 
 
```java
contentResolver.delete(uri,"column2= ?",arrayOf("1"))

```

* 更新操作

```java
val values = contentValuesOf("column1" to "")
contentResolver.update(uri,values,"column1= ? and column2 = ?",arrayOf("text","1"))

```

## 一个读取通讯录的实例
* 首先在manifest文件中申请权限

```xml

<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.jon.chapter0801">

    <uses-permission android:name="android.permission.READ_CONTACTS"></uses-permission>
  
    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.Chapter0801">
        <activity android:name=".MainActivity">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>

</manifest>
```
* 然后利用ContentReslove读取通讯录

```java
class MainActivity : AppCompatActivity() {

    private val contactsList = ArrayList<String>()
   // private lateinit var adapter: ArrayAdapter<String>

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

     
        if (ContextCompat.checkSelfPermission(this, Manifest.permission.READ_CONTACTS) != PackageManager.PERMISSION_GRANTED) {
            ActivityCompat.requestPermissions(this, arrayOf(Manifest.permission.READ_CONTACTS), 1)
        } else {
            getPhoneNumbers()
        }
    }

    override fun onRequestPermissionsResult(requestCode: Int, permissions: Array<out String>, grantResults: IntArray) {
        super.onRequestPermissionsResult(requestCode, permissions, grantResults)
        when (requestCode) {
            1 -> {
                if (grantResults.isNotEmpty() && grantResults[0] == PackageManager.PERMISSION_GRANTED) {
                    getPhoneNumbers()
                } else {
                    Toast.makeText(this, "you denied the permission", Toast.LENGTH_LONG).show()
                }
            }
        }
    }

    private fun getPhoneNumbers() {
        try {
            contentResolver.query(ContactsContract.CommonDataKinds.Phone.CONTENT_URI, null, null, null, null)?.apply {
                while (moveToNext()) {
                    val displayName = getString(getColumnIndex(ContactsContract.CommonDataKinds.Phone.DISPLAY_NAME))
                    //
                    val number = getString(getColumnIndex(ContactsContract.CommonDataKinds.Phone.NUMBER))
                    contactsList.add("$displayName\n$number")
                }
                //adapter.notifyDataSetChanged()
                close()
            }
        } catch (e: SecurityException) {
            e.printStackTrace()
        }
    }
}

```



## 创建自己的ContentProvider

