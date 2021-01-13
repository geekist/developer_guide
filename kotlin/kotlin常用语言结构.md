
# Kotlin常用语言结构

## 结构声明

一个解构声明同时创建多个变量。

```java
class Person(val name: String = "jon", val age: Int = 40) {

fun component1() :String {
return name}

fun componnent2() :Int {
return age}


fun main() {
	val person = Person()
	val (name,age) = person
}

```

kotlin 中data class 和 map 类型自带实现componentN函数

## 类型检测与类型转换  is and as

## This表达式

This 表达式为了表示当前的 接收者 我们使用 this 表达式：

在类的成员中，this 指的是该类的当前对象。
在扩展函数或者带有接收者的函数字面值中， this 表示在点左侧传递的 接收者 参数。
如果 this 没有限定符，它指的是最内层的包含它的作用域。要引用其他作用域中的 this，请使用 标签限定符：

* 限定的 this


要访问来自外部作用域的this（一个类 或者扩展函数， 或者带标签的带有接收者的函数字面值）我们使用this@label，其中 @label 是一个代指 this 来源的标签：

```java
class A { // 隐式标签 @A
    inner class B { // 隐式标签 @B
        fun Int.foo() { // 隐式标签 @foo
            val a = this@A // A 的 this
            val b = this@B // B 的 this
​
            val c = this // foo() 的接收者，一个 Int
            val c1 = this@foo // foo() 的接收者，一个 Int
​
            val funLit = lambda@ fun String.() {
                val d = this // funLit 的接收者
            }
​
​
            val funLit2 = { s: String ->
                // foo() 的接收者，因为它包含的 lambda 表达式
                // 没有任何接收者
                val d1 = this
            }
        }
    }
}
```

* Implicit this
 
当对 this 调用成员函数时，可以省略 this. 部分。 但是如果有一个同名的非成员函数时，请谨慎使用，因为在某些情况下会调用同名的非成员：

```java
fun printLine() { println("Top-level function") }
​
class A {
    fun printLine() { println("Member function") }
​
    fun invokePrintLine(omitThis: Boolean = false)  { 
        if (omitThis) printLine()
        else this.printLine()
    }
}
​
A().invokePrintLine() // Member function
A().invokePrintLine(omitThis = true) // Top-level function


Member function
Top-level function

```

## 相等性

Kotlin 中有两种类型的相等性：

- 结构相等（用 equals() 检测）；
- 引用相等（两个引用指向同一对象）。

## Try 是一个表达式

try 是一个表达式，即它可以有一个返回值：

val a: Int? = try { parseInt(input) } catch (e: NumberFormatException) { null }
try-表达式的返回值是 try 块中的最后一个表达式或者是（所有）catch 块中的最后一个表达式。 finally 块中的内容不会影响表达式的结果。

## 作用域函数（参见标准函数库）
