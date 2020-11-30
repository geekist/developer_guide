# Fragmentation

## 添加gradle的依赖项：
```java
    implementation 'me.yokeyword:fragmentationx:1.0.2'
```

## 初始化
```java


```

## 基本类

### SupportActivity

* **loadRootFragment**

动态加载一个fragment

* **pop**

将fragment栈当前的fragment出栈

* **popTo**

将fragment栈当前的fragment出栈到指定的fragment

* **getTopFragment** 

获取fragment栈顶的fragment

* **findFragment**

在fragment栈内找到指定类的fragment

* **start**

启动fragment栈内的一个fragment，可以带有栈模式

* **startWithPopTo**

启动fragment栈内的一个fragment，同时将fragment栈内的fragment出栈到指定的栈。





### SupportFrgment


* **loadRootFragment**

动态加载一个fragment

* **pop**

将fragment栈当前的fragment出栈

* **popTo**

一个fragment将fragment栈内的fragment出栈到指定的fragment处。


* **putNewBundle(Bundle newBundle)** 

fragment放置bundle，用于向后传递


* **onNewBundle(Bundle newBundle)** 

在新启动的fragment获得bundle


* **start**

一个fragment启动另外一个fragment


* **startWithPop**

一个fragment启动另外一个fragment，同时将fragment栈内自身出栈。


* **startWithPopTo**

一个fragment启动另外一个fragment，同时将fragment栈内的fragment出栈到targetfragment处。
用于无论从哪里启动fragment，最终只返回给定的fragment。

* **startForResult**

一个fragment启动另外一个fragment，并在OnFragementResult中获得返回结果

* **onFragmentResult**

在OnFragementResult中获得返回结果

* **setFragmentResult**

在Fragement中设置返回结果

* **findChildFragment**

找到嵌套的子fragment

* **replaceFragment**

替换当前fragment









    

