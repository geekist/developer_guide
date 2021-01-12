
# Kotlin基础

## 数据类型

在 Kotlin 中，所有东西都是对象，在这个意义上讲我们可以在任何变量上调用成员函数与属性。 一些些类型可以有特殊的内部表示--例如，数字、字符以及布尔值可以在运⾏时表⽰为原⽣类型值，但是对于用户来说，它们看起来就像普通的类。 

### 数字 

对于整数，有四种不同⼤⼩的类型，因此值的范围也不同。

|类型 |⼤⼩（⽐特数） |最⼩值                               |                                最⼤值|
-------------------------------------------------------------------------------------------------
|Byte |8             |-128                                |127                                  |
|Short|16            |-32768                              | 32767                              |
|Int  |32            |-2,147,483,648 (-2^31 )             |2,147,483,647 (2^31 - 1)            |
|Long |64            |-9,223,372,036,854,775,808 (-2^63 ) |9,223,372,036,854,775,807 (2^63 - 1)|

所有以未超出 Int 最⼤值的整型值初始化的变量都会推断为 Int 类型。如果初始值超过了其最⼤值，那么推断为
Long 类型。 如需显式指定 Long 型值，请在该值后追加 L 后缀。

```java
val one = 1 // Int
val threeBillion = 3000000000 // Long
val oneLong = 1L // Long
val oneByte: Byte = 1
```
对于浮点数，Kotlin 提供了 Float 与 Double 类型。 根据 IEEE 754 标准， 两种浮点类型的⼗进制位数（即可以存储多少位⼗进制数）不同。 Float 反映了 IEEE 754 单精度，⽽ Double 提供了双精度。

|类型      |⼤⼩（⽐特数） |有效数字⽐特数|指数⽐特数|⼗进制位数|
------------------------------------------------------
|Float    |32            |24           |8        |6-7|
|Double   |64            |53           |11       |15-16|

对于以⼩数初始化的变量，编译器会推断为 Double 类型。 如需将⼀个值显式指定为 Float 类型，请添加 f 或 F后缀。 如果这样的值包含多于 6〜7 位⼗进制数，那么会将其舍⼊。
```java
val pi = 3.14 // Double
val e = 2.7182818284 // Double
val eFloat = 2.7182818284f // Float，实际值为 2.7182817
```
请注意，与⼀些其他语⾔不同，Kotlin 中的数字没有隐式拓宽转换。 例如，具有 Double 参数的函数只能对 Double值调⽤，⽽不能对 Float 、 Int 或者其他数字值调⽤。

每个数字类型⽀持如下的转换:

* toByte(): Byte

* toShort(): Short

* toInt(): Int

* toLong(): Long

* toFloat(): Float

* toDouble(): Double

* toChar(): Char


### 字符

字符⽤ Char 类型表⽰。它们不能直接当作数字

```java
fun check(c: Char) {
if (c == 1) { // 错误：类型不兼容
// ……
c.toInt() //OK
}
}
```
* 字符字⾯值⽤单引号括起来: '1' 。 
* 特殊字符可以⽤反斜杠转义。 ⽀持这⼏个转义序列：\t 、\b 、\n 、\r 、\' 、\" 、\\ 与 \$ 。 编码其他字符要⽤ Unicode 转义序列语法：'\uFF00' 。

### 布尔型

布尔类型用Boolean来表示，它有两个值：true与false
  
若需要可空引⽤布尔会被装箱。
内置的布尔运算有：
|| ‒ 短路逻辑或
&& ‒ 短路逻辑与
! - 逻辑⾮

### 数组

数组在 Kotlin 中使用Array 类来表示，它定义了 get 与 set 函数（按照运算符重载约定这会转变为 [] ）以及
size 属性

```java
val a = arrayOf(1,2,3) //
val b = arrayOfNulls()
val asc = Array(5) { i -> (i * i).toString() }
```
数组类定义的源码：
```java
class Array<T> private constructor() {
val size: Int
operator fun get(index: Int): T
operator fun set(index: Int, value: T): Unit
operator fun iterator(): Iterator<T>
// ……
}

```
原生数组：
ByteArray，ShortArray，IntArray



### 无符号整数


### 字符串
 字符串用String类型表示，字符串是不可变的。

**字符串模板**

字符串字⾯值可以包含模板表达式 ，即⼀些⼩段代码，会求值并把结果合并到字符串中。 模板表达式以美元符（ $ ）开头，由⼀个简单的名字构成:
val i = 10
println("i = $i") // 输出“i = 10”

或者⽤花括号括起来的任意表达式:
val s = "abc"
println("$s.length is ${s.length}") // 输出“abc.length is 3”

原始字符串与转义字符串内部都⽀持模板。 如果你需要在原始字符串中表⽰字⾯值 $ 字符（它不⽀持反斜杠转义），
你可以⽤下列语法：
val price = """
${'$'}9.99
"""
## 变量

定义只读局部变量使⽤关键字 val 定义。只能为其赋值⼀次。

```java
val a: Int = 1 // ⽴即赋值
val b = 2 // ⾃动推断出 `Int` 类型
val c: Int // 如果没有初始值类型不能省略
c = 3 // 明确赋值
```

可重新赋值的变量使⽤ var 关键字：

```java
var x = 5 // ⾃动推断出 `Int` 类型
x += 1
```
顶层变量：

```
val PI = 3.14
var x = 0
fun incrementX() {
x += 1
}
```
## 控制流之选择

### if 

在 Kotlin 中，if是⼀个表达式，即它会返回⼀个值。 因此就不需要三元运算符（条件 ? 然后 : 否则），因为普通的 if 就能胜任这个⻆⾊。

// 传统⽤法
```java
var max = a
if (a < b) max = b

// With else
var max: Int
if (a > b) {
max = a
} else {
max = b
}
```

// 作为表达式
```java
val max = if (a > b) a else b
```

if 的分⽀可以是代码块，最后的表达式作为该块的值：
```java
val max = if (a > b) {
print("Choose a")
a
} else {
print("Choose b")
b
}
```
如果你使⽤ if 作为表达式⽽不是语句（例如：返回它的值或者把它赋给变量），该表达式需要有 else 分⽀。

### when

when 表达式取代了类 C 语⾔的 switch 语句。其最简单的形式如下：
```java
when (x) {
1 -> print("x == 1")
2 -> print("x == 2")
else -> { // 注意这个块
print("x is neither 1 nor 2")
}
}
```

```java
when {
x.isOdd() -> print("x is odd")
y.isEven() -> print("y is even")
else -> print("x+y is even.")
}

fun hasPrefix(x: Any) = when(x) {
is String -> x.startsWith("prefix")
else -> false
}

when (x) {
in 1..10 -> print("x is in the range")
in validNumbers -> print("x is valid")
!in 10..20 -> print("x is outside the range")
else -> print("none of the above")
}

```

## 控制流之循环

### for

for (item in collection) print(item)

### while

while 与 do while 循环照常使用

while (x > 0) {
x--
}

do {
val y = retrieveData()
} while (y != null) // 


## 区间

Kotlin 可通过调⽤ kotlin.ranges 包中的 rangeTo() 函数及其操作符形式的 .. 轻松地创建两个值的区间。 通常，rangeTo() 会辅以 in 或 !in 函数。

if (i in 1..4) { // 等同于 1 <= i && i <= 4
print(i)
}

整数类型区间（IntRange、LongRange、CharRange）还有⼀个拓展特性：可以对其进⾏迭代。 这些区间也是相应整数类型的等差数列。 
这种区间通常⽤于 for 循环中的迭代。

for (i in 1..4) print(i)

要反向迭代数字，请使⽤ downTo 函数⽽不是 .. 。
for (i in 4 downTo 1) print(i)

也可以通过任意步⻓（不⼀定为 1 ）迭代数字。 这是通过 step 函数完成的。
for (i in 1..8 step 2) print(i)
println()
for (i in 8 downTo 1 step 2) print(i)

要迭代不包含其结束元素的数字区间，请使⽤ until 函数：
for (i in 1 until 10) { // i in [1, 10), 10被排除
print(i)
}
