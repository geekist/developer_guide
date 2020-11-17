# SQLite
存储大量关系型数据，data/data/package/database目录下。

Android提供了一个抽象类SQLiteOpenHelper进行数据库管理。要实现该类首先需要创建一个子类继承SQLiteOpenHelper。  
```kotlin
class MySQLiteOpenHelper(val context: Context, name: String, version: Int): SQLiteOpenHelper(context,name,null,version) {
    override fun onCreate(db: SQLiteDatabase?) {
       // db?.execSQL(createBook)
    }

    override fun onUpgrade(db: SQLiteDatabase?, oldVersion: Int, newVersion: Int) {

    }

```

## 创建数据库 
调用SQLiteOpenHelper的getReadableDatabase或getWriteableDatabase来实现数据库的创建或打开。
```kotlin
val dbHelper = MySQLiteOpenHelper(this,"BookStore.db",1)
dbHelper.writableDatabase
```
打开一个命名位BookStore的数据库，如果不存在，就创建它。

## 升级数据库
* 当创建一个新的数据库时，onCreate函数会被调用，可以在该函数中创建数据库表。
```kotlin
class MySQLiteOpenHelper(val context: Context, name: String, version: Int): SQLiteOpenHelper(context,name,null,version) {
    private val createBook = "create table Book (" +
            " id integer primary key autoincrement, " +
            "author text, " +
            "price real, " +
            "pages integer, " +
            "name text)"

    override fun onCreate(db: SQLiteDatabase?) {
        db?.execSQL(createBook)
    }

    override fun onUpgrade(db: SQLiteDatabase?, oldVersion: Int, newVersion: Int) {

    }
}

```
如果该应用下没有名为BookStore的数据库，则调用下面的方法会创建新的数据库，并创建一个名为book的数据库表。

```kotlin
val dbHelper = MySQLiteOpenHelper(this,"BookStore.db",1)
dbHelper.writableDatabase
```

* 当需要在数据库中添加新的表时，以高版本号打开数据库表，onUpgrade函数会被调用，可以在该函数中创建数据库表。

```kotlin
class MySQLiteOpenHelper(val context: Context, name: String, version: Int): SQLiteOpenHelper(context,name,null,version) {
    private val createCategory = "create table Category (" +
            " id integer primary key autoincrement, " +
            "author text, " +
            "price real, " +
            "pages integer, " +
            "name text)"

    override fun onCreate(db: SQLiteDatabase?) {
       
    }

    override fun onUpgrade(db: SQLiteDatabase?, oldVersion: Int, newVersion: Int) {
 		db?.execSQL(createBook)
    }
}

```
调用时，将版本号升级为2，则onUpgrade会被调用，

```kotlin
val dbHelper = MySQLiteOpenHelper(this,"BookStore.db",2)
dbHelper.writableDatabase
```
## 创建数据库表
```java
 private val createBook = "create table Book (" +
            " id integer primary key autoincrement, " +
            "author text, " +
            "price real, " +
            "pages integer, " +
            "name text)"

val db = MySQLiteOpenHelper(this,"BookStore.db",1).writeableDatabase
db?.execSQL(createBook)
```

## 删除数据库表
```java
val db = MySQLiteOpenHelper(this,"BookStore.db",1).writeableDatabase
db?.execSQL("drop table if exist Book")
```
## 数据库表中插入数据
```java
val db = MySQLiteOpenHelper(this,"BookStore.db",1).writeableDatabase
val values = ContentValues.apply{
put("name","Jon")
put("age",20)
put("price",3.3)
}
db.insert("Book",null,values)

```

## 数据库表中删除数据
```java
val db = MySQLiteOpenHelper(this,"BookStore.db",1).writeableDatabase

db.delete("Book","pages > ?",arrayOf("500"))

```
## 数据库表中更新数据
```java
val db = MySQLiteOpenHelper(this,"BookStore.db",1).writeableDatabase
val values = ContentValues()
values.put("price",3.5)
db.update("Book",values,"name = ?", arrayOf("Jon"))
```
## 数据库表中查询数据
```java
val db = MySQLiteOpenHelper(this,"BookStore.db",1).writeableDatabase
val cursor = db.query("Book",null,null,null,null,null)
if(cursor.moveToFirst()){
do {
 	val name = cursor.getString(cursor.getColumnIndex("name"))
 	val author = cursor.getString(cursor.getColumnIndex("author"))
}while(cursor.moveToNext())

cursor.close()

```
## 数据库用SQL直接操作

可以向添加数据库表一样，拼装SQL语句，直接调用execSQL来执行。

```kotlin

db.execSQL("delete from Book where pages > ?",arrayOf("500")

db.execSQL("update Book set price = ? where name = ?",arrayOf("10.99","Jon")

val cursor = db.rawQuery("select * from Book",null)

```


