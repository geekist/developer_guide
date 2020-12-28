

# LiveData
LiveData是JackPack中响应式编程组件，可以包含任何类型的数据，并且在数据发生变化的时候通知观察者。LiveData一般与ViewModel共同使用。

## 添加依赖项
```java

    implementation 'androidx.lifecycle:lifecycle-extensions:2.2.0'

```
## 创建一个LiveData
```java
val counter = MutableLiveData<Int>
}
```	
## 获取LiveData的值和设置LiveData的值

```java
  liveData.getValue()
  liveData.setValue() //主线程
  liveData.postValue() //非主线程
}
```	

## 观察livedata的变化
```java
viewModel.liveData.ovserve(this,Observer{count->TextView.setText(count)})	
```