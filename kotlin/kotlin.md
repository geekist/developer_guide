## 变量
* val 用来声明一个不可变的变量

val a = 10 //类型推导机制

var a: Int = 10


* var 用来声明一个可变的变量

var a = 10 
 
var a: Int = 100



## 函数
```java
fun methodName(parameter1: Int, parameter2: Int): Int {

	return 0

}

fun methodName() {
	pirntln("hello")
}

fun methodName(parameter1: Int)：Int {
 return parameter1*3
}

如果只有简单的函数体，则可以用简化写法： 

fun methodName(parameter: Int): Int = parameter * 3
或者
fun methodName(parameter: Int) = parameter * 3

```
## if 
```java
fun largerNumber(num1: Int, num2: Int): Int {
	if(a>b) {
		a
	}
	else{
		b
	}
或者：
fun largeNumber(num1:Int,num2:Int) = if(a>b){a} else {b}
或者：
fun largeNumber(num1:Int,num2:Int) = if(a>b) a else b
```

## when else
```java
fun getScore(name: String): String{
	when(name) {
		"Tommy" -> "good"
		"Jerry" -> "bad"
		else -> ""
	}
}

fun getScore(name: String)= when {
		name.startsWith("Tom") -> "good"
		name == "Tommy" -> "good"
		name == "Jerry" -> "bad"
		else -> ""	
}

fun getScore(name: String) = when(name){
	"Tom"->"good"
	else->"bad"
}
```

## 循环

* while 循环和java一样
```java
fun calculate(num1: Int) : Int {
	while(num1<100) {
		num1 += 3
		println(num1)
	}
}
```

* for in 循环

```java
fun calcu(num1:Int) : Int {
	for(i in 1..100) {
		println(i)
	}
}

```

* 区间

1..10  
1 until 10  
10 downTo 1  
1 until 10 step 2

## 类与对象
* 类的声明和对象初始化

```java
class person {

}

class Person {

var name = ""

var age = 0


fun eat() {

pirntln(name + " is eating. he is " +　age + " years old")

}
实例化
val person = Person()
person.name = "Tom"
person.age = 20
```

## 类继承
```java

open class Person {
    val name: String = ""
    val age: Int = 0
}

class Student : Person() {
    val sno = 0
    val grade = 0
}


```

## 主构造函数和次构造函数
* 主构造函数没有函数体，直接写在类的声明后面,如何需要实现其他逻辑，用init{}实现
```java
主构造函数没有函数体，直接写在类的声明后面

class Student(val sno: Int,val grade: Int) : Person() { //父类后面的空括号表示调用了父类的无参数的构造函数

    init{
        println("sno is " + sno)
        println("grade is " + grade)
    } //init给主构造函数添加了业务逻辑
}


fun main(){
    println("hello")
    
    val student: Student = Student(100,100)
}
```

如果父类也有含参数的主构造函数，则应该
```java

class Student(val sno: Int,val grade: Int,name: String, age: Int) : Person(name,age) {
    init{
        println("sno is " + sno)
        println("grade is " + grade)
    }
}

```

* 次构造函数用constructor关键字

```java

class Student(val sno: Int,val grade: Int,name: String, age: Int) : Person(name,age) {
    init{
        println("sno is " + sno)
        println("grade is " + grade)
    }

    constructor(name: String, age: Int) : this(0,0,name,age)
    constructor() : this(0,0,"hello",0)
}


```


## 接口
```java

interface Study{
	fun readBook()
	fun doHomework()

	fun test() {
	}
}

class Student(val sno: Int,val grade: Int,name: String, age: Int) : Person(name,age),Study {
    init{
        println("sno is " + sno)
        println("grade is " + grade)
    }

    constructor(name: String, age: Int) : this(0,0,name,age)
    constructor() : this(0,0,"hello",0)

	override func readBook() {
	}
	overide fun doHomework() {
	}
 }
```

## 函数的可见性修饰符
```java

public  默认都是public

private 本类中可见

protected 当前类、子类可见

internal 当前模块可见



```


## 数据类

```java

data class MyClass {}


```

## 单例类

```java
object Singleton {
	fun singletonTest() {
	}
 }

Signleton.singletonTest();

```

## list set and map
```java

 val list = ArrayList<String>()
    list.add("apple")
    list.add("banana")
    list.add("orange")

    val list1 = listOf("apple","banana","orange")
    for(fruit in list1) {
        println(fruit)
    }

    var list2 = mutableListOf<String>()
    list2.add("apple")

    var list3 = mutableListOf("apple","banana","orange","peal")
    list3.add("coconut")
    for(fruit in list3) {
        println(fruit)
    }

    var list4 = setOf("car","motor","bike","vechile")

    var list5 = mutableSetOf("car","motor","bike","vichile")
    list5.add("ship")

    val map = HashMap<Int,String>()
    map.put(1,"car")
    map.put(2,"bicycle")
    map[3]= "ship"

    val var1 = map[1]

    var map1 = mapOf( 1 to "car", 2 to "motor")
    var map2 = mutableMapOf( 1 to "car", 2 to "motor")

    for((number,value) in map2) {
        println("number is " + number + " value is " + value)
    }


```
## lambda表达式
lambda表达式是指一小段可以作为参数传递的代码。其格式如下：

```java
{parameter1:type,parameter2:type -> experssion}

```

## 集合的函数式API
* maxBy

```java

val list = listOf("apple","banana","orangle","pear","grape")
val maxLength = list.maxBy{it.length}

```
下面进行剖析：
maxBy是一个普通的函数，它需要的参数是一个lambda类型的:

val maxLength = list.maxBy({fruit:String->fruit.length)}

当lambda参数是函数的最后一个参数时，可以把lambda移到括号外面

val maxLength = list.maxBy(){fruit:String->fruit.lenght}

当lambda是函数的唯一一个参数时，括号可以省略

val maxLength = list.maxBy{fruit:String->fruit.length}

kotlin的类型推导机制可以去掉类型：

val maxLenght = lsit.maxBy{fruit->fruit.length}

当lambda的参数只有一个时，可以用默认的it来代替

val maxLenght = list.maxBy{it->fruit.length}


* map

```java
val list = listOf("apple","banana","orangle","pear","grape")
val newList = list.map{it.toUpperCase()}

```
* filter


```java
val list = listOf("apple","banana","orangle","pear","grape")
val newList = list.filter{it.length<5}.map{it.toUpperCase()}
for(fruit in newList){
println(fruit)
}
```

* any all

```java
val list = listOf("apple","banana","orangle","pear","grape")
val anyOne = list.any{it.length<5}
val allOne = list.all{it.toUpperCase()}

println(anyOne)
}

```

## java 函数式API
```java

new Thread(new Runable() {
		@Override
		public void run() {
			println("hello")
		}
	}
}.start();

Thread(object: Runable {
override fun run() {
println("hello")
})}.start()

如果只有一个可以复写的方法，则方法名可以省略
Thread(Runable{println("hello")}).start()
如果只有一个抽象接口类，类名可以省略
Thread({println("hello")}).start()
如果lambda是最后一个参数，可以移到括号外，如果是唯一一个参数，括号可以省略
Thread{println("hello")}.start()

又如：button.setOnClickListener{}

```

## 空指针检查 默认所有的类型都是非空的
kotlin在编译时会进行强制的判空检查，默认所有的参数都不能为空。

## 可以为空的类型 类型？
```java

fun main() {
	doSomething(null)
}

fun doSomething(stu: String?) {
	if(stu!= null) {
		stu.test()
	}
}

```

## 判空辅助工具？.
```java
if(stu!= null) {
stu.test()
}

stu?.test()

```

## 判空操作符 ？：
```java

if(a!=null) {
a}
else{
b}

a ?: b

```

## 强型非空断言符号 ！！
```java

fun print() {
	val upper = content!!.toUpperCase()
}

```


## 字符串内嵌表达式
```java
val name = "Tom"

println("hello, nice to meet you！ $name ")

```

## 可以带默认值的函数参数
```java

fun test(param1:Int, param2: Int = "echo") {

}

test(100)  

fun test(param1:String = "Tom",param2: Int) {

}

test(100)  这里会出问题

fun test(param1:String = "Tom",param2: Int = 100) {
}

test(param2 = 100)


```



## Kotlin标准函数介绍

* let函数  

提供了函数式API的接口，并将原始对象作为参数传递到lambda表达式中

```java
a?.let{
it.doSomething()
it.test()
}

```

* with函数

with函数接收两个参数，一个是上下文，另外一个是lambda表达式。with函数以上下文为对象执行lambda表达式，并且返回该lambda表达式的最后一条语句的结果

```java
fun main() {
    val list = listOf("apple", "banana", "orange", "pear")
    val result = with(StringBuilder()) {
        append("Start eat fruits\n")
        for (fruit in list) {
            append(fruit + "\n")
        }
        append("It's delicious\n")
        toString()
    }
}


```


* run函数

和with函数非常类似，将第一个参数提取出来，作为调用with的对象。然后依次执行lambda表达式，并且返回该lambda表达式的最后一条语句结果

```java

fun runTest(list:List<String>) {
    val stringBuilder = StringBuilder()

    val result = stringBuilder.run {
        append("Start eat fruits\n")
        for (fruit in list) {
            append(fruit + "\n")
        }
        append("It's delicious\n")
        toString()
    }

    println(result)
}


```



* apply函数
和with函数非常类似，将第一个参数提取出来，作为调用with的对象。然后依次执行lambda表达式，区别是没有返回值。
```java
fun applyTest(list:List<String>) {
    val stringBuilder = StringBuilder()

    stringBuilder.apply {
        append("Start eat fruits\n")
        for (fruit in list) {
            append(fruit + "\n")
        }
        append("It's delicious\n")
    }

    println(stringBuilder.toString())
}

```

## 静态方法的实现
在Kotlin中实现静态方法，主要有下面两种：

* 将类定义成object 类型

```java
object  Class1 {
    fun test1() {
        println("this is object class")
    }
}

```

* 在普通类中添加companion object

将要静态的方法添加jvmStatic注解

```java

class Class2() {
    companion object {

        fun test2() {
            println("this is companion class")
        }
    }
}

```

上面两种实际上不是真正的class级别的静态方法，但是要是真正实现class级别的静态方法也有两种方法：
* 1、在上面的两种情况下，对需要静态的方法添加@JvmStatic注解
```

class Class2() {
    companion object {
        @JvmStatic
        fun test2() {
            println("this is companion class")
        }
    }
}
```

* 2、顶层函数，即没有包含在任何类当中的函数
```java

fun main() {
    val list = listOf("apple", "banana", "orange", "pear")
    //applyTest(list)
    Class1.test1()
    Class2.test2()
  //  val class2 = Class2();
   // class2.test2()
}


```






