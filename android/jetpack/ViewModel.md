

# ViewModel

用来承担原本activity或fragment承担的数据部分，将数据与activity解构，viewmodel的生命周期和activity无关

## 添加依赖项
```java

    implementation 'androidx.lifecycle:lifecycle-extensions:2.2.0'

```

## 创建一个ViewModel类
MainViewModel类继承于ViewModel类，向外提供了一根LiveData的数据类型供观测，这里的LiveData是只读模式的，防止外面对LiveData进行修改。

```java

class MainViewModel(counterReserved: Int): ViewModel() {
   
 val counter: LiveData<Int>
          get() = _counter

    private val _counter = MutableLiveData<Int>()

    init{
        _counter.value = counterReserved
    }

    fun plusOne() {
        val count = counter.value ?: 0
        _counter.value = count + 1
    }

    fun clear() {
        _counter.value = 0
    }
}

```

## 不带参数的viewmodel的实例化
```java
val viewModel = ViewModelProvider(this).get(MainViewModel::class.java)
```
通过ViewModelProvider来实例化ViewmOdel，其生命周期和Activity无关。传入的第一个参数是activity类的实例。第二个参数是MainVIewModel的class类。

## 带参数的ViewModel的实例化
如果ViewModel带有参数，则上面的方法就不能使用了，因为无法将参数传递进去，这时可以用工厂方法来创建一个ViewModel的实例
```java

class MainViewModelFactory(private val counterReserved: Int): ViewModelProvider.Factory {
    override fun <T: ViewModel> create(modelClass: Class<T>) : T {
        return MainViewModel(counterReserved) as T
    }
使用时可以用：

 viewModel = ViewModelProvider(this,MainViewModelFactory(count))
            .get(MainViewModel::class.java)
```

## 观测viewmodel中数据的变化
```java
viewModel.counter.observe(this, Observer { textViewNum.text = it.toString() })
```
调用liveData的observer函数，第一个参数是lifecycleOwener对象，第二个参数是Observer接口的匿名实现类，在其中可以进行数据的处理

## map和switchmap用来观测部分数据
* map

map和Rxjava中map的概念是一致的，将一种数据类型转换为另外一种数据类型。

```java
class MainViewModel(counterReserved: Int) : ViewModel() {

    private val userLiveData = MutableLiveData<User>()

    val userName: LiveData<String>
        get() = Transformations
                .map(userLiveData) {
                    "${it.firstName} ${it.secondName}"
                }
}
```
使用时只需要观察userName即可
```java
  viewModel.userName.observe(this, Observer {
            textViewNum.text = it.firstName.toString()
        })

```

* switchMap

switchMap用来将可变的livedata数据转换为一个不可变的livedata数据进行观察
```java

object Repository {
    fun getUser(userId: String) : LiveData<User> {
        val liveData = MutableLiveData<User>()
        liveData.value = User(userId,userId,0)
        return liveData
    }
}

```
单例Repository每次调用都可以得到一个新的livedata数据，如果直接用这个数据来观察的话，每一次都会变化LiveData本身，而不是其内部的value，所以需要进行一个脱壳的转换。
```java

class MainViewModel(counterReserved: Int) : ViewModel() {

    private val userIdLiveData = MutableLiveData<String>()

    val user: LiveData<User>
        get() = Transformations
                .switchMap(userIdLiveData) {
                    Repository.getUser(it)
                }

    fun getUser(userId: String) {
        userIdLiveData.value = userId
    }
}

```
首先创建一个Livedata用来接收参数变化，然后用switchMap将其映射到一个新的user对象中，这样，如果外部调用了getUser方法传进参数，则该livedata会调用switchMap创建一个映射，该映射会返回一个新的livedata数据，并映射到新的user对象中，这样在外部观测user对象即可。
```java
  viewModel.user.observe(this, Observer {
            textViewNum.text = it.firstName.toString()
        })
```