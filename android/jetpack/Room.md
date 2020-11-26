

# Room
Jetpack的新的数据库模型：entity，dao，database三层。


## 添加依赖项
```java

plugins {
    id 'com.android.application'
    id 'kotlin-android'
    id 'kotlin-android-extensions'
    id 'kotlin-kapt'
}


dependencies {
    implementation 'androidx.room:room-runtime:2.1.0'
    kapt "androidx.room:room-compiler:2.1.0"
}


```
## Entity
```java
@Entity
data class User(val firstName:String , val lastName:String , val age: Int) {

    @PrimaryKey(autoGenerate = true)
    var id : Long = 0
}
```	
## 关联observer和observable
```java
@Dao
interface UserDao {
    @Insert
    fun insertUser(user: User) : Long

    @Update
    fun updateUser(user: User)

    @Query("select * from User where age > :age")
    fun loadUsersOldThan(age: Int) : List<User>

    @Delete
    fun deleteUser(user: User)

    @Query("delete from User where lastName = :lastName")
    fun deleteUser(lastName: String)
}
```	

## database
```java

abstract class AppDatabase : RoomDatabase() {
    abstract fun userDao(): UserDao

    companion object {
        private var instance: AppDatabase? = null

        @Synchronized
        fun getDatabase(context: Context): AppDatabase {
            instance?.let {
                return it
            }
            return Room.databaseBuilder(
                context.applicationContext,
                AppDatabase::class.java,
                "app_database"
            )
                .build().apply {
                    instance = this
                }
        }
    }
}


```	

## 调用
```java
   val userDao = AppDatabase.getDatabase(this).userDao()
   userDao.insertUser(User("sgg","sfs",25))
```
