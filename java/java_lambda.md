# Lambda表达式基本语法与应用



## Lambda简介

Lambda表达式是Java8中提供的一种新的特性，它支持Java也能进行简单的“函数式编程”，即Lambda允许你通过表达式来代替功能接口，即可使用更少的代码来实现同样的功能。

用官方的解释就是：

A lambda expression is a block of code with parameters.

(Lambda表达式是一个带有参数的表达式)


## 添加支持

首先Java版本需要为1.8，然后在build.gradle中添加：

```
android {
    ……
    defaultConfig {
        ……
        jackOptions{
            enabled true
        }
    }
    compileOptions {
        //升级Android Studio的Language level为1.8
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
}
```

## 语法

### 完整示例

```
(int x, int y) -> {
    Log.i("TAG", "x:" + x + "  y:" + y);
    return x + y;
}
```

这是一个完整的Lambda表达式的写法，通常由三部分组成：

1. (int x, int y)：Lambda表达式的参数部分，包括参数类型与参数名
2. “->”：箭头goes to，指向代码块
3. 代码块：用”{}”包裹的代码


### 忽略参数类型

在大多数情况下，参数的类型系统可根据上下文推断出来。这种情况参数类型就可以忽略不写。

```
(x, y) -> {
    Log.i("TAG", "x:" + x + "  y:" + y);
    return x + y;
}
```

### 忽略：”()”

当只有一个参数时，”()”可以忽略不写。

```
x -> {
    Log.i("TAG", "x:" + x);
    return x;
}
```

### 当没有参数时，”()”不可忽略。

() -> {
    Log.i("TAG", "无参数");
    return 0;
}


### 忽略：”{}”

当代码块只包含一条语句时可忽略”{}”不写。

```
(x, y) -> return x + y;
```

### 忽略return

而return关键字也是可以忽略不写的。

```
(x, y) -> x + y;
```
精简到最后只需要一行代码就可以搞定，是不是很方便。

## 应用

### 无参数+语句/代码块

* 常规写法：

```
new Thread(new Runnable() {
    @Override
    public void run() {
        Log.i("TAG", "测试无参数+语句/代码块");
    }
}).start();
```

* Lambda写法：

```
new Thread(() -> Log.i("TAG", "测试无参数+语句/代码块")).start();
```
　　

### 适用于匿名内部类中方法无参数的情况

有参数+语句

* 常规写法：

```
findViewById(R.id.button).setOnClickListener(new View.OnClickListener() {
    @Override
    public void onClick(View v) {
        Log.i("TAG", "测试有参数+语句");
    }
});
```
　
* Lambda写法：

```
findViewById(R.id.button).setOnClickListener(v -> Log.i("TAG", "测试有参数+语句"));
```　　

### 适用于匿名内部类中方法只有一个参数的情况

有参数+代码块
　　
* 常规写法：

```
CheckBox checkBox = (CheckBox) findViewById(R.id.checkBox);
checkBox.setOnCheckedChangeListener(new CompoundButton.OnCheckedChangeListener() {
    @Override
    public void onCheckedChanged(CompoundButton buttonView, boolean isChecked) {
        Log.i("TAG", "测试有参数+代码块1");
        Log.i("TAG", "测试有参数+代码块2");
    }
});
```
　　
* Lambda写法：

```
checkBox.setOnCheckedChangeListener((buttonView, isChecked) -> {
    Log.i("TAG", "测试有参数+代码块1");
    Log.i("TAG", "测试有参数+代码块2");
});
```
　　
适用于匿名内部类中方法不止一个参数且执行语句不止一行的情况

## 总结

Lambda表达式的应用场景很多，例如可与RxJava，Retrofit等进行完美配合，更多的就等待各位码友去实践发掘了。而到此，本篇关于Lambda表达式的详解与应用就讲解完毕了。技术渣一枚，有写的不对的地方欢迎大神们留言指正。