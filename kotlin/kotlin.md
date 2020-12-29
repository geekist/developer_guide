# Kotlin

## [基础](https://github.com/geekist/developer_guide/blob/main/kotlin/foundation.md)

## [Kotlin标准函数库](https://github.com/geekist/developer_guide/blob/main/kotlin/standard.md)

## [类与对象](https://github.com/geekist/developer_guide/blob/main/kotlin/class.md)

## [函数与lambda表达式](https://github.com/geekist/developer_guide/blob/main/kotlin/function&lambda.md)

## [集合](https://github.com/geekist/developer_guide/blob/main/kotlin/assembly.md)

## [Kotlin协程](https://github.com/geekist/developer_guide/blob/main/kotlin/coroutine.md)

## [Kotlin中的5种单例模式](https://github.com/geekist/developer_guide/blob/main/kotlin/singlton.md)

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
注意：接口的继承写法上不要加（）

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

ktlin的类型推导机制可以去掉类型：

val maxLenght = lsit.maxBy{fruit->fruit.length}

当lambda的参数只有一个时，可以用默认的it来代替

val maxLenght = list.maxBy{it->it.length}

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
```

如果只有一个可以复写的方法，则方法名可以省略

Thread(Runable{println("hello")}).start()

如果只有一个抽象接口类，类名可以省略

Thread({println("hello")}).start()

如果lambda是最后一个参数，可以移到括号外，如果是唯一一个参数，括号可以省略

Thread{println("hello")}.start()

又如：button.setOnClickListener{}

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
}
test(100)  
fun test(param1:String = "Tom",param2: Int) {
}
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
    println(stringBuilder.toString()
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
```kotlin
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
## 用is关键字代替java中的instanceof关键字
## 用as关键字代替java中的强制类型转换
## 常量定义 const
常量定义只有在单例类、compainion object中或者顶层函数中才能使用
```java
class Msg(val content: String, val type: Int) {
	companion object {
		const val TYPE_RECEIVED = 0
		const val TYPE_SENT = 1
	}
}
```

## 延迟初始化lateinit
```java
class A() {
private val lateinit param1: String
fun initParam(parm: String) {
    param1= parm
}
fun useParam() {
    parm1.toString()
}
```

## 密封类sealed class
```java
sealed class Result
class Success(val msg: String):Result()
class Failure(val error: Exception): Result()
fun getResultMsg(result: Result)= when(result) {
    is Success-> result.msg
    is Failure-> result.error.message)
```
## 类的扩展函数
在不修改类的源代码的基础上，向该类添加新的函数。 该函数自动拥有类的上下文，可以等同于类的成员函数来使用。类扩展函数的语法格式：
```kotlin
fun ClassName.methodName(param1: Type1,param2: Type2): Type {
...
    this.xxx//this 即使类的上下文
}
```
举例说明如下:
```kotlin
fun String.lettersCount(): Int {
	val count = 0
	for(char in this) {
		if(char.isLetter()
		count++
	}
	reurn count
}

```
调用方法：
```kotlin
fun main() {
	val count = "234owhfhr7*(0-".lettersCount()
}
```
## 运算符重载
运算符是kotlin语言的语法糖，实际上每一个运算符对应着一个函数。
比如：
|语法糖表达式 |实际调用函数|
|:--------:     |:---------:    |
|a+b            |        a:plus(b)|
|a-b            |        a:minus(b)|
|a*b            |        a:times(b)|
|a/b            |        a:div(b)|
|a==b           |        a:equals(b)|
|!a             |        a:not()|
|a in b         |       b.contains(a)|

可以使用运算符重载的方法给类添加新的运算符含义：
```ktolin
class Obj {
    operator fun plus(obj: Obj): Obj 
}
```
举例如下：
```kotlin
class Money(val value: Int) {
    operator fun plus(money: Money): Money {
        return Money(value + money.value)
    }
    operator fun plus(intMoney: Int): Money {
        return Money(value + intMoney)
    }
}

val money1 = Money(5)
val money2 = Money(7)
val money3 = money1 + money2
val money4 = money3 + 5
println(money3.value)
println(money4.valeu)
```
## 高阶函数
* 高阶函数的定义：如果一个函数接收另外一个函数作为参数，或者返回值的类型是另外一个函数，则该函数被称为是一个高阶函数。
*  函数类型：kotlin定义了一个新的类型：函数类型，其基本规则如下：（Type1，Type2）->Type3或Unit 其含义是：在箭头的左边是函数的参数类型声明，右边是返回值类型声明
*  高阶函数的表达式：
```kotlin
fun example(fun:(string,Int)->Unit) {
    fun("hello",123)
```
举一个例子：
```kotlin
首先定义两个函数：
fun plus(num1:Int,num2: Int): Int {
    return num1 + num2
}
fun minus(num1:Int,num2: Int): Int {
    return num1 - num2
}
可以定义一个高阶函数，以这种类型声明的函数作为参数：
fun operation(num1:Int,num2:Int,block: (Int,Int)->Int) {
    block(num1,num2)
}

然后可以调用这个高阶函数，付给他不同函数作为参数：
fun main() {
    var result = operation(3,3,::plus)
    println(result)
    result = operation(3,3,::minus)
    println(result)
}
```
* 用lambda表达式来代替函数的声明
一个函数实际上就是一个lambda表达式 {parameter1:type,parameter2:type -> experssion}

我们将上面的函数定义可以用lambda表达式来表示；

plus： {value1:Int,value2:Int->value1+value2}

minus: {value1:Int,value2:Int->value1-value2}

则上面在main中的调用可以变为：
```kotlin
fun main() {
/*
    var result = operation(3,3,{value1: Int,value2: Int ->value1+value2})
   因为最后一个参数是lambda表达式，可以将lambda表达式提取出来
   val result = operation(3,3){value1:Int,value2: Int->value1+value2}
    因为lambda表达式的类型推导功能，lambda表达式中的参数类型可以省略
    */
    val result = operation(3,3){value1,value2->value1+value2}
    println(result)
}
```
例子：用高阶函数来实现一个类似apply的函数方法
```kotlin
fun StringBuilder.build(block:StringBuilder.()->Unit): StringBuilder {
    block()
    return this
}

fun main() {
    val list = listOf("banana","apple","orange")
    val strBuilder = StringBuilder()
    for(fruit in list) {
        strBuilder.build1 {
            append(fruit + "\n")
        }
    }
    println(strBuilder.toString())
}
```
## inline函数和outline、crossline关键字
高阶函数的本质是kotlin添加了一个匿名类来实现功能。可能会带来效率上的问题，一般建议将高阶函数尽可能地定义为内联函数.内联函数的作用是将其中的函数参数中的代码直接赋值到内联函数体内。
```kotlin
inline fun(block: (Int, String)->Unit, block2: (Int,String) ->Unt) {
}
```
如果有些函数参数不希望被复制，则可以使用noinline关键字，不复制这一段代码
如果inline函数的函数体内出现了其他的不可控类型，比如runnable，则可以用crossinline来协调。

## 泛型

### 泛型类
```java
class ClassName<T>(val param: Type, val param: Type) {
    fun method(param1: T)：T {
        return param1
    }
}
```
调用的时候可以用：
```java
val obj = Obj<String>(param1,param2)
val ret = obj.method("hello")
```

### 泛型方法
```kotlin
class ClassName{

fun <T> method(param: T): T {
    return param
}

val obj = ClassName()
val ret = obj.method<String>("hello")
或者: val ret = obj.method("hello")

```
泛型类和方法可以指定上限T:Number等

## 委托 by 关键字
### 类委托
类委托是委托（代理）模式的一种应用。
```kotlin
class MySet<T>(val helperSet: HashSet<T>): Set<T> by helperSet {
```
即由helperSet来实现MeSet的所有接口方法，MySet只需要重载自己需要的方法就可以
```koltin
class MySet<T>(val helperSet: HashSet<T>): Set<T> by helperSet {
    override val size: Int
    get() = helperSet.size
    
    //helperSet 实现了Myset的大部分接口方法
    override fun contains(element: T) = helperSet.contains(element)
    override fun isEmpty() = helperSet.isEmpty()
    ......
    
    //MySet只需要重载自己需要实现的部分
    fun helloworld() = println("hello world")
    override isEmpty() = false
}
```
### 属性委托
属性委托的核心思想是将一个属性的get/set具体实现委托给另外一个类去完成
语法结构
```koltin
class MyClass {
    val p by Delegate()
}
```
看看其背后的实现
```koltin
class Delegate {
    var propValue: Any? = null
    
    operator fun getValue(myClass: MyClass, prop: KProperty<*>): Any? {
        return propValue
    }
    
    operator fun setValue(muClass: MyClass, prop: KProperty<*>, value: Any?) {
        propValue = value
    }
}
```

`实现自己的一个lazy函数`
首先定义一个泛型类Later，这个类包含一个函数类型的成员变量，以及getValue的方法
```java
class Later<T>(val block: ()->T) {
    val value: Any? = null
    
    operator fun getValue(any: Any?, prop: KProperty<*>): T {
        if(value == null) {
            block()
        }else {
            //
        }
        
        return value as T
    }
}
```

然后设计一个顶层函数later
```java
fun <T> later(block: ()->T) = Later(block())
```
最后，我们可以直接使用这个函数来实现later加载

```kotlin
val uriMatcher by later{
    val matcher = UriMathcer(UriMatcher.MO_MATCH)
    matcher.addURI()
    matcher.addURI()
    matcher.addURI()
    matcher.addURI()
    matcher
}


```

## 协程
协程允许我们在单线程模式下模拟多线程编程的效果，代码执行与挂起完全由编程语言来控制，与操作系统无关。

###  GlobalScope.launch  顶层协程
顶层协程域的launch函数在当前线程启动一个协程，当当前线程结束的时候，无论顶层协程域是否执行完成，都会被同时结束。
```kotlin
fun main() {
	GlobalScope.launch{
		println("")
	}
	Thread.sleep(1000)
}

```

### runBlocking阻塞当前线程的协程
函数runBlocking会创建一个协程，并在该协程执行完成之前，阻塞当前线程。
```kotlin
fun main() {
	runBlocking {
		println("")
	}
	Thread.sleep(1000)
}

```

### launch函数
launch函数只能在协程的作用域之下才能被调用，其次他会在当前协程下创建一个子协程。
```kotlin
fun main() {
	runBlocking {
		launch{
			println("")
		}
	}
	Thread.sleep(1000)
}

```

### delay() 挂起函数
delay函数是一个非阻塞的挂起函数，它会挂起当前协程，但是不影响其他协程或线程的运行。


### suspend关键字修饰的函数是一个挂起函数.
执行时也将当前协程挂起，直到执行结束。suspend 修饰的关键字只能将一个函数声明成为挂起函数，但是无法提供协程域。即挂起函数只能在协程域中才能执行，而且该挂起函数中无法创建子协程，即不能调用launch函数。
```kotlin
suspend fun printDot() {
	println(".")
    delay(100)
}
```

### coroutineScope函数。
coroutineScope是一个挂起函数，但是它能够继承外部的协程作用域并且在其内部能够用launch创建子协程。由coroutineScope函数会阻塞当前协程，直到其内部代码或子协程全部执行完为止。
```kotlin
fun main() {
	runBlocking{
		coroutineScope{
			launch{
				for(i in 1..10) {
					println(i)
					delay(1000)
				}
			}
		}
		println("")
	}
	pirntln("")
}

```

### 协程的取消 和CoroutineScope函数（类）
launch函数的返回值是一个Job类对象，可以通过调用job的cancel方法取消协程。
CoroutineScope函数的参数是job，会返回一个CoroutineScope类对象，可以在这个类对象的协程域中调用launch来创建子协程。
```kotlin
val job = Job()
val scope = CorountineScope(job)
scope.launch{
}
.......
job.cancel()
```

### async函数和await函数
async函数相当于launch函数，可以在协程作用域内创建一个子协程，但是可以通过await函数将lambda表达式的值返回。
async函数返回一个Deferred对象，调用该对象的await方法可以将async函数的执行结果返回。
```kotlin
fun main() {
	runBlocking {
		val result = async {
			5+5
		}.await()
		println(result)
	}
}
```

### withContext()函数
withContext函数是一个挂起函数，可以理解成为是async函数的一个简化版本。大致相当于asyn{}.await
withContext强制要求指定一个线程参数。
```kotlin
fun main() {
	runBlocking {
		val result = withContext(Dispatchers.Default) {
			5+5
		}
		println(result)
	}
}
```

### 线程参数
前面所学的所有挂起函数，出了coroutineScipe函数外，其他所有的函数都可以指定线程参数。
而withContext是强制指定的。
* Dispatchers.Default  默认低并发的线程策略
* Dispatchers.IO 高并发的线程策略，使用与网路传输
* Dispatchers.Main 主线程

### suspendCoroutine函数
suspendCoroutine函数必须在协程域或者挂起函数中使用，它接受一个lambda表达式作为参数，将当前协程挂起，然后在一个普通线程执行lambda表达式，该lambda表达式带有参数Continuation参数，调用该参数的resume或resumeWithException函数可以让协程恢复执行。
```kotlin
val appService = ServiceCreator.create<AppService>()
appService.getAppData().enqueue(object: Callback<List<App>>{
	override fun onResponse(call: Call<List<App>>,responese: Response<List<App>>) {
	}
	override fun OnFailure(call: Call<Lst<App>>,t: throwable) {

	}
})

suspend <T> fun Call<T>.await() : T {
	return suspendCoroutine{ continuation ->
		enqueue(object: Callback<T> {
			override fun onResponse(call: Call<T>,response: Response<T>) {
				val body = response.body()
				if(body != null) continuation.resume(body)
				else continuation.resumeWithExceptin(
					RuntimeException("response body is null"))
				}
			}

			overide fun onFailure(call:Call<T>, t:Throwable) {
				continuation.resumeWithException(t)
			}
		}
}

appService.getAppData().await();

```

















