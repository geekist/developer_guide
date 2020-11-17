
# SharedPreference

SharedPreference以键值对的方式来存取数据，文件仍然保存在data/data/packagename/shared_prefs/目录中

## 将数据写入SharedPreference
* step1：获取SharedPreference对象。

获取sharedPreference对象有两种方式，一种是从contect中获取，另外一种是从actvitity中获取。
```kotlin

class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        
		//从context中获取，需要制定文件名称
        val sharedPreference = getSharedPreferences("0701", MODE_PRIVATE)
        val editor: SharedPreferences.Editor = sharedPreference.edit()
        editor.putInt("age",20)
        editor.putString("name","jon")
        editor.apply()
        
		//从activity中获取，直接用activity的名称作为文件名
        val sp = getPreferences(MODE_PRIVATE)
        val editor1: SharedPreferences.Editor = sharedPreference.edit()
        editor1.putInt("age",20)
        editor1.putString("name","jon")
        editor1.apply()
    }
}

```
* step2： 将数据写入文件中

```kotlin
  val sharedPreference = getSharedPreferences("0701", MODE_PRIVATE)
  val editor: SharedPreferences.Editor = sharedPreference.edit()
  editor.putInt("age",20)
  editor.putString("name","jon")
  editor.apply()
```

## 将数据从SharedPreference中读取出来
```kotlin
   val sp1 = getPreferences(MODE_PRIVATE)
   val age = sp1.getInt("age",0)
   val name = sp1.getString("name","")
```

