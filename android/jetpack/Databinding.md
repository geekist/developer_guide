[Android Databinding](https://blog.csdn.net/qq_38861828/article/details/103735865)

[官方介绍](https://developer.android.com/topic/libraries/data-binding)

# Databing

## 1-DataBing介绍
DataBinding 是谷歌官方发布的一个框架，其作用是实现数据绑定（Data binding），同时，也是因为有它，可以在安卓（Android）上实现MVVM架构。

## 2-依赖项添加
在app的build.gradle中添加闭包

```xml
......
android {
    ......
    dataBinding {
        enabled = true
    }
}
```
## 3-如何在布局文件中引入数据变量

### 3-1首先定义一个数据类

```java
public class PeopleBean {
    public String name;
    public int age;

    public PeopleBean(String name, int age) {
        this.name = name;
        this.age = age;
    }
}
```

### 3-2 在布局文件中引入数据类
* 3.2.1 首先将布局文件的根布局改为layout布局。

方法是在最顶层布局，alt + enter，选择convert to data binding 布局。

* 3.2.2 添加数据标签

```xml
 <data>
        <import type="com.lkdot.mvvm.bean.PeopleBean"/>

        <variable
            name="data"
            type="PeopleBean" />

        <variable
            name="data2"
            type="PeopleBean" />

        <variable
            name="data3"
            type="PeopleBean" />

</data>
```
* 3.2.3 在xml文件中使用数据

```xml
<?xml version="1.0" encoding="utf-8"?>
<layout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools">

    <data>

        <import type="com.lkdot.mvvm.bean.PeopleBean" />

        <variable
            name="data"
            type="PeopleBean" />

    </data>

    <android.support.constraint.ConstraintLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:background="#FFFFFF"
        tools:context=".MainActivity">

        <TextView
            android:id="@+id/textView2"
            android:layout_width="match_parent"
            android:layout_height="100dp"
            android:background="#03A9F4"
            android:gravity="center"
            android:text="@{data.name}"
            android:textColor="#FFFFFF"
            android:textStyle="bold"
            tools:ignore="MissingConstraints" />

        <TextView
            android:layout_width="match_parent"
            android:layout_height="100dp"
            android:layout_marginTop="8dp"
            android:background="#03A9F4"
            android:gravity="center"
            android:text="@{String.valueOf(data.age)}"
            android:textColor="#FFFFFF"
            android:textStyle="bold"
            app:layout_constraintTop_toBottomOf="@+id/textView2"
            tools:ignore="MissingConstraints"
            tools:layout_editor_absoluteX="0dp" />


    </android.support.constraint.ConstraintLayout>
</layout>

```
### 3-3 在Activity和Fragment中初始化数据。

* 3.3.1 在activity中初始化和给控件赋值

```java

public class MainActivity extends AppCompatActivity {

    private PeopleBean peopleBean;
    private ActivityMainBinding activityMainBinding;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        
        //1-实例化ActivityMainBinding
        activityMainBinding = DataBindingUtil.setContentView(this, R.layout.activity_main);

        //2-实例化PeopleBean
        peopleBean = new PeopleBean("NorthernBrain", 25);

        //3-布局中variable标签中的name是data，所以这里是setData
        activityMainBinding.setData(peopleBean);
    }
}

或者：    val binding: ActivityMainBinding = ActivityMainBinding.inflate(getLayoutInflater())

//使用ActivityMainBinding为id是textView2的TextView赋值
activityMainBinding.textView2.setText("我是新的值");

控件也可以有初始值：
<TextView
	......
    android:text="@{data.name,default = HelloWorld}"
    ...... />


```
* 3.3.2 在fragment、ListView或RecyclerView中初始化

```java
    val listItemBinding = ListItemBinding.inflate(layoutInflater, viewGroup, false)
    // or
    
val listItemBinding = DataBindingUtil.inflate(layoutInflater, R.layout.list_item, viewGroup, false)
```

## 4、单向绑定数据刷新
当一个可观察数据对象绑定到界面并且该数据对象的属性发生更改时，界面会自动更新。

单向绑定有三种：
1-使用 BaseObservable
2-使用 ObservableField
3-使用 ObservableCollection

### 4-1： ObservableField
ObservableField来修饰变量：
ObservableField<String>、ObservableField<Integer> 、
ObservableShort、ObservableBoolean、ObservableByte等,将字段变成可观察字段。

```java
    class User {
        val firstName = ObservableField<String>()
        val lastName = ObservableField<String>()
        val age = ObservableInt()
    }
```
### 4-2 可观察集合Observable<Collection>
ObservableArrayMap<String, Any>().apply {
        put("firstName", "Google")
        put("lastName", "Inc.")
        put("age", 17)
    }
在XML布局文件中，可以使用字符串键找到地图

```java
<data>
        <import type="android.databinding.ObservableMap"/>
        <variable name="user" type="ObservableMap<String, Object>"/>
    </data>
    …
    <TextView
        android:text="@{user.lastName}"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"/>
    <TextView
        android:text="@{String.valueOf(1 + (Integer)user.age)}"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"/>
    

```


### 4-3：可观察对象BaseObservable，可以自己来决定什么刷新UI。
BaseObservable提供了两个刷新UI的方法，分别是 notifyPropertyChanged() 和 notifyChange() 。

notifyPropertyChanged(); 只会刷新属于它的UI，就如代码，他只会更新name。
notifyChange(); 会刷新所有UI。

notifyPropertyChanged() 需要传入一个int类型的参数，这个参数就是int fieldId，指的就是那个形参 name
**这里要注意public类型要是用 @Bindable 进行注解，否则会报错。**

```java
    class User : BaseObservable() {

        @get:Bindable
        var firstName: String = ""
            set(value) {
                field = value
                notifyPropertyChanged(BR.firstName)
            }

        @get:Bindable
        var lastName: String = ""
            set(value) {
                field = value
                notifyPropertyChanged(BR.lastName)
            }
    }
    
```
如此设置之后，则当数据发生变化时，即set方法被调用时，触发notify消息，UI 会自动更新。

## 5、双向数据绑定
@={} 表示法（其中重要的是包含“=”符号）可接收属性的数据更改并同时监听用户更新。

```xml
    <CheckBox
        android:id="@+id/rememberMeCheckBox"
        android:checked="@={viewmodel.rememberMe}"
    />
```
为了对后台数据的变化作出反应，您可以将您的布局变量设置为 Observable（通常为 BaseObservable）的实现，并使用 @Bindable 注释，如以下代码段所示
```java
    class LoginViewModel : BaseObservable {
        // val data = ...

        @Bindable
        fun getRememberMe(): Boolean {
            return data.rememberMe
        }

        fun setRememberMe(value: Boolean) {
            // Avoids infinite loops.
            if (data.rememberMe != value) {
                data.rememberMe = value

                // React to the change.
                saveData()

                // Notify observers of a new value.
                notifyPropertyChanged(BR.remember_me)
            }
        }
    }
```

## 6、使用LiveData作为绑定数据源

可以使用 LiveData 对象作为数据绑定来源，自动将数据变化通知给界面。与实现 Observable 的对象（例如可观察字段）不同，LiveData 对象了解订阅数据更改的观察器的生命周期。在 Android Studio 版本 3.1 及更高版本中，可以在数据绑定代码中将可观察字段替换为 LiveData 对象。

要将 LiveData 对象与绑定类一起使用，需要指定生命周期所有者来定义 LiveData 对象的范围
```java
    class ViewModelActivity : AppCompatActivity() {
        override fun onCreate(savedInstanceState: Bundle?) {
            // Inflate view and obtain an instance of the binding class.
            val binding: UserBinding = DataBindingUtil.setContentView(this, R.layout.user)

            // Specify the current activity as the lifecycle owner.
            binding.setLifecycleOwner(this)
        }
    }
```









