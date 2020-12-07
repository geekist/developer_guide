# 函数与lambda表达式

## 函数

### 函数声明：

函数有fun关键字声明，基本声明格式为：
```kotlin
fun powerOf(number: Int = 4, exponent: Int = 5) { /*……*/ }
```
函数参数可以有默认的值。

### 函数调用：

调用时使用传统方法

```kotlin
val result = powerOff(3,3)
val result = powerOff(4)
val result = powerOff(number =4,exponent = 5)
```
* 如果默认参数之后的最后一个参数是lambda表达式，则既可以作为具名参数在括号内传入，也可以在括号外传入
```kotlin
fun foo(
bar: Int = 0,
baz: Int = 1,
qux: () -> Unit,
) { /*……*/ }
foo(1) { println("hello") } // 使⽤默认值 baz = 1
foo(qux = { println("hello") }) // 使⽤两个默认值 bar = 0 与 baz = 1
foo { println("hello") } // 使⽤两个默认值 bar = 0 与 baz = 1

```

* 返回Unit的函数，其返回值可以不用声明：
```kotlin
fun printHello(name: String?) { …… }

```

* 单表达式函数
返回一个表达式的函数，可以省略大括号并用等号指定代码体
```kotlin
fun double(x: Int): Int = x * 2
fun double(x: Int) = x * 2
```
* 可变数量的参数vararg标记
```kotlin
fun <T> asList(vararg ts: T): List<T> {
val result = ArrayList<T>()
for (t in ts) // ts is an Array
result.add(t)
return result
}
调用：
val list = asList(1, 2, 3)

```

* 伸展操作符*
```kotlin
val a = arrayOf(1, 2, 3)
val list = asList(-1, 0, *a, 4)

```

* 用infix关键字标注的中缀（二元）函数
```kotlin
infix fun INt.sh1(x: Int) : Int {
......
}

2.shi1(3)
```

### 函数作用域
局部函数:定义在一个函数内部的函数，局部函数可以访问外部函数闭包的布局变量
```java
fun dfs(graph: Graph) {
val visited = HashSet<Vertex>()
fun dfs(current: Vertex) {
if (!visited.add(current)) return
for (v in current.neighbors)
dfs(v)
}
dfs(graph.vertices[0])
}

```

成员函数：类的内部定义的函数


### 泛型函数

```kotlin

fun <T> test(name:T) : T {
......
}
```

## 函数类型

### 声明函数类型：
```kotlin
 (Int，String) -> String
或
() -> Unit

```

* 所有函数类型都有⼀个圆括号括起来的参数类型列表以及⼀个返回类型：(A, B) -> C 表⽰接受类型分别为 A与 B 两个参数并返回⼀个 C 类型值的函数类型。 参数类型列表可以为空，如 () -> A 。Unit 返回类型不可省略。

* 函数类型可以有⼀个额外的接收者类型，它在表⽰法中的点之前指定： 类型 A.(B) -> C 表⽰可以在 A 的接收者对象上以⼀个 B 类型参数来调⽤并返回⼀个 C 类型值的函数。 带有接收者的函数字⾯值通常与这些类型⼀起使⽤。


* 挂起函数属于特殊种类的函数类型，它的表⽰法中有⼀个 suspend 修饰符 ，例如 suspend () -> Unit 或者suspend A.(B) -> C 。


* 函数类型表⽰法可以选择性地包含函数的参数名：(x: Int, y: Int) -> Point 。 这些名称可⽤于表明参数的含义。

* 如需将函数类型指定为可空，请使⽤圆括号：((Int, Int) -> Int)?。


有⼏种⽅法可以获得函数类型的实例：
---使⽤函数字⾯值的代码块，采⽤以下形式之⼀：
* lambda 表达式: { a, b -> a + b } 


* 匿名函数: fun(s: String): Int { return s.toIntOrNull() ?: 0 }

带有接收者的函数字⾯值可⽤作带有接收者的函数类型的值。

---使⽤已有声明的可调⽤引⽤：
* 顶层、局部、成员、扩展函数：::isOdd 、 String::toInt ，

*顶层、成员、扩展属性：List<Int>::size ，

*构造函数：::Regex

这包括指向特定实例成员的绑定的可调⽤引⽤：foo::toString 。

---使⽤实现函数类型接⼝的⾃定义类的实例：
```kotlin
class IntTransformer: (Int) -> Int {
override operator fun invoke(x: Int): Int = TODO()
}
val intFunction: (Int) -> Int = IntTransformer()
```


## lambda表达式
lambda表达式是指一小段可以作为参数传递的代码。其格式如下：
```java
{parameter1:type,parameter2:type -> experssion}
```
* lambda 表达式总是括在花括号中， 完整语法形式的参数声明放在花括号内，并有可选的类型标注， 函数体跟在⼀个-> 符号之后。如果推断出的该 lambda 的返回类型不是 Unit ，那么该 lambda 主体中的最后⼀个（或可能是单个）表达式会视为返回值。

* lambda 表达式与匿名函数是未声明的函数， 但做为表达式传递。

* 如果函数的最后⼀个参数是函数，那么作为相应参数传⼊的 lambda 表达式可以放在圆括号
之外：
```kotlin
val product = items.fold(1) { acc, e -> acc * e }
这种语法也称为拖尾 lambda 表达式。
如果该 lambda 表达式是调⽤时唯⼀的参数，那么圆括号可以完全省略：
run { println("...") }
```

* 如果 lambda 表达式的参数未使⽤，那么可以⽤下划线取代其名称：
map.forEach { _, value -> println("$value!") }

## 匿名函数
除了省略名称，匿名函数看起开和一个常规的函数声明没有区别，可以作为函数类型的参数使用。
```kotlin
fun(x: Int, y: Int): Int {
return x + y
}
```

## 带有接受者的函数字面值
和扩展函数类似，直接拥有接受者的上下文
这⾥有⼀个带有接收者的函数字⾯值及其类型的⽰例，其中在接收者对象上调⽤了 plus ：
val sum: Int.(Int) -> Int = { other -> plus(other) }


## 高阶函数
⾼阶函数是将函数⽤作参数或返回值的函数。
⼀个不错的⽰例是集合的函数式⻛格的 fold， 它接受⼀个初始累积值与⼀个接合函数，并通过将当前累积值与每个集合元素连续接合起来代⼊累积值来构建返回值：
```kotlin
fun <T, R> Collection<T>.fold(
initial: R,
combine: (acc: R, nextElement: T) -> R
): R {
var accumulator: R = initial
for (element: T in this) {
accumulator = combine(accumulator, element)
}
return accumulator
}
```
在上述代码中，参数 combine 具有函数类型 (R, T) -> R ，因此 fold 接受⼀个函数作为参数， 该函数接受类型分别为 R 与 T 的两个参数并返回⼀个 R 类型的值。 在 for-循环内部调⽤该函数，然后将其返回值赋值给accumulator 。

高阶函数的调用：
为了调⽤ fold ，需要传给它⼀个函数类型的实例作为参数，⽽在⾼阶函数调⽤处，lambda 表达 式⼴泛⽤于此⽬的。
```kotlin
val items = listOf(1, 2, 3, 4, 5)
// Lambdas 表达式是花括号括起来的代码块。
items.fold(0, 
          {// 如果⼀个 lambda 表达式有参数，前⾯是参数，后跟“->”
            acc: Int, i: Int ->
            print("acc = $acc, i = $i, ")
            val result = acc + i
            println("result = $result")
            // lambda 表达式中的最后⼀个表达式是返回值：
            result
          }
        )
// lambda 表达式的参数类型是可选的，如果能够推断出来的话：
val joinedToString = items.fold("Elements:", { acc, i -> acc + " " + i })
// 函数引⽤也可以⽤于⾼阶函数调⽤：
val product = items.fold(1, Int::times)
```
## 内联函数
高阶函数的本质是kotlin添加了一个匿名类来实现功能。可能会带来效率上的问题，一般建议将高阶函数尽可能地定义为内联函数.内联函数的作用是将其中的函数参数中的代码直接赋值到内联函数体内。
```kotlin
inline fun(block: (Int, String)->Unit, block2: (Int,String) ->Unt) {
}
```
如果有些函数参数不希望被复制，则可以使用noinline关键字，不复制这一段代码
如果inline函数的函数体内出现了其他的不可控类型，比如runnable，则可以用crossinline来协调。
