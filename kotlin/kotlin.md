## 变量
* val 用来声明一个不可变的变量

val a = 10 //类型推导机制

var a: Int = 10


* var 用来声明一个可变的变量

var a = 10
var a: Int = 100

目前看，变量定义时必须赋初值，否则编译不过。
如：var a: Int 是不能别编译通过的。

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


