# Kotlin 标准函数库

只有十多个函数，但是用处非常大。

## NotImplementedError 顶层函数

参数为一个字符串，返回值是一个Error

```java
/**
 * An exception is thrown to indicate that a method body remains to be implemented.
 */
public class NotImplementedError(message: String = "An operation is not implemented.") : Error(message)
```

## TODO 顶层函数

参数无或字符串，返回值无，会抛出一个Error异常。

```java
/**
 * Always throws [NotImplementedError] stating that operation is not implemented.
 */

@kotlin.internal.InlineOnly
public inline fun TODO(): Nothing = throw NotImplementedError()
```

```java
/**
 * Always throws [NotImplementedError] stating that operation is not implemented.
 *
 * @param reason a string explaining why the implementation is missing.
 */
@kotlin.internal.InlineOnly
public inline fun TODO(reason: String): Nothing = throw NotImplementedError("An operation is not implemented: $reason")
```

## 作用域函数

### 作用域函数的定义

Kotlin 标准库包含几个函数，它们的唯一目的是在对象的上下文中执行代码块。

当对一个对象调用这样的函数并提供一个 lambda 表达式时，它会形成一个临时作用域。在此作用域中，可以访问该对象而无需其名称。这些函数称为作用域函数。

共有以下五种：let、run、with、apply 以及 also。

这些函数基本上做了同样的事情：在一个对象上执行一个代码块。不同的是这个对象在块中如何使用，以及整个表达式的结果是什么。


### 5个作用域函数的使用


### let 扩展函数 上下文对象作为 lambda 表达式的参数（it）来访问。返回值是 lambda 表达式的结果。

* let函数的定义


```java

/**
 * Calls the specified function [block] with `this` value as its argument and returns its result.
 *
 * For detailed usage information see the documentation for [scope functions](https://kotlinlang.org/docs/reference/scope-functions.html#let).
 */
@kotlin.internal.InlineOnly
public inline fun <T, R> T.let(block: (T) -> R): R {
    contract {
        callsInPlace(block, InvocationKind.EXACTLY_ONCE)
    }
    return block(this)
}

```

* let函数的使用


let 可用于在调用链的结果上调用一个或多个函数。例如，以下代码打印对集合的两个操作的结果：

```java
val numbers = mutableListOf("one", "two", "three", "four", "five")
val resultList = numbers.map { it.length }.filter { it > 3 }
println(resultList)


[5, 4, 4]

```

使用 let，可以写成这样：

```java
val numbers = mutableListOf("one", "two", "three", "four", "five")
numbers.map { it.length }.filter { it > 3 }.let { 
    println(it)
    // 如果需要可以调用更多函数
} 
[5, 4, 4]

```

若代码块仅包含以 it 作为参数的单个函数，则可以使用方法引用(::)代替 lambda 表达式：

```java
val numbers = mutableListOf("one", "two", "three", "four", "five")
numbers.map { it.length }.filter { it > 3 }.let(::println)
[5, 4, 4]
```


let 经常用于仅使用非空值执行代码块。如需对非空对象执行操作，可对其使用安全调用操作符 ?. 并调用 let 在 lambda 表达式中执行操作。

```java
val str: String? = "Hello" 
//processNonNullString(str)       // 编译错误：str 可能为空
val length = str?.let { 
    println("let() called on $it")
    processNonNullString(it)      // 编译通过：'it' 在 '?.let { }' 中必不为空
    it.length
}
let() called on Hello

```

使用 let 的另一种情况是引入作用域受限的局部变量以提高代码的可读性。如需为上下文对象定义一个新变量，可提供其名称作为 lambda 表达式参数来替默认的 it。

```java
val numbers = listOf("one", "two", "three", "four")
val modifiedFirstItem = numbers.first().let { firstItem ->
    println("The first item of the list is '$firstItem'")
    if (firstItem.length >= 5) firstItem else "!" + firstItem + "!"
}.toUpperCase()
println("First item after modifications: '$modifiedFirstItem'")
The first item of the list is 'one'
First item after modifications: '!ONE!'
```



### with 一个非扩展函数：上下文对象作为参数传递，但是在 lambda 表达式内部，它可以作为接收者（this）使用。 返回值是 lambda 表达式结果。

第一个参数是一个第一个泛型类，第二个参数是第一个泛型类上的扩展函数，返回值是第二个泛型类

* with函数定义


```java
/**
 * Calls the specified function [block] with the given [receiver] as its receiver and returns its result.
 *
 * For detailed usage information see the documentation for [scope functions](https://kotlinlang.org/docs/reference/scope-functions.html#with).
 */
@kotlin.internal.InlineOnly
public inline fun <T, R> with(receiver: T, block: T.() -> R): R {
    contract {
        callsInPlace(block, InvocationKind.EXACTLY_ONCE)
    }
    return receiver.block()
}
```

* with函数使用


我们建议使用 with 来调用上下文对象上的函数，而不使用 lambda 表达式结果。 在代码中，with 可以理解为“对于这个对象，执行以下操作。”

```java
val numbers = mutableListOf("one", "two", "three")
with(numbers) {
    println("'with' is called with argument $this")
    println("It contains $size elements")
}


'with' is called with argument [one, two, three]
It contains 3 elements
```

with 的另一个使用场景是引入一个辅助对象，其属性或函数将用于计算一个值。

```java
val numbers = mutableListOf("one", "two", "three")
val firstAndLast = with(numbers) {
    "The first element is ${first()}," +
    " the last element is ${last()}"
}
println(firstAndLast)


The first element is one, the last element is three
```

### run 上下文对象 作为接收者（this）来访问。 返回值 是 lambda 表达式结果。

参数是一个函数类型，或者是一个扩展的函数类型，返回值是一个泛型。

* run函数定义


```java
/**
 * Calls the specified function [block] and returns its result.
 *
 * For detailed usage information see the documentation for [scope functions](https://kotlinlang.org/docs/reference/scope-functions.html#run).
 */
@kotlin.internal.InlineOnly
public inline fun <R> run(block: () -> R): R {
    contract {
        callsInPlace(block, InvocationKind.EXACTLY_ONCE)
    }
    return block()
}

/**
 * Calls the specified function [block] with `this` value as its receiver and returns its result.
 *
 * For detailed usage information see the documentation for [scope functions](https://kotlinlang.org/docs/reference/scope-functions.html#run).
 */
@kotlin.internal.InlineOnly
public inline fun <T, R> T.run(block: T.() -> R): R {
    contract {
        callsInPlace(block, InvocationKind.EXACTLY_ONCE)
    }
    return block()
}
```

* run函数使用

run 和 with 做同样的事情，但是调用方式和 let 一样——作为上下文对象的扩展函数.

当 lambda 表达式同时包含对象初始化和返回值的计算时，run 很有用。

```java
val service = MultiportService("https://example.kotlinlang.org", 80)
​
val result = service.run {
    port = 8080
    query(prepareRequest() + " to port $port")
}
​
// 同样的代码如果用 let() 函数来写:
val letResult = service.let {
    it.port = 8080
    it.query(it.prepareRequest() + " to port ${it.port}")
}

Result for query 'Default request to port 8080'
Result for query 'Default request to port 8080'
```


除了在接收者对象上调用 run 之外，还可以将其用作非扩展函数。 非扩展 run 可以使你在需要表达式的地方执行一个由多个语句组成的块。

```java
val hexNumberRegex = run {
    val digits = "0-9"
    val hexDigits = "A-Fa-f"
    val sign = "+-"
​
    Regex("[$sign]?[$digits$hexDigits]+")
}
​
for (match in hexNumberRegex.findAll("+1234 -FFFF not-a-number")) {
    println(match.value)
}


+1234
-FFFF
-a
be
```


### apply 上下文对象 作为接收者（this）来访问。 返回值 是上下文对象本身。

参数是泛型类的扩展函数或成员函数，返回值是泛型类

* apply函数定义


```java
/**
 * Calls the specified function [block] with `this` value as its receiver and returns `this` value.
 *
 * For detailed usage information see the documentation for [scope functions](https://kotlinlang.org/docs/reference/scope-functions.html#apply).
 */
@kotlin.internal.InlineOnly
public inline fun <T> T.apply(block: T.() -> Unit): T {
    contract {
        callsInPlace(block, InvocationKind.EXACTLY_ONCE)
    }
    block()
    return this
}
```

* apply函数使用
 
对于不返回值且主要在接收者（this）对象的成员上运行的代码块使用 apply。apply 的常见情况是对象配置。这样的调用可以理解为“将以下赋值操作应用于对象”。

```java
val adam = Person("Adam").apply {
    age = 32
    city = "London"        
}
println(adam)
Person(name=Adam, age=32, city=London)

```

将接收者作为返回值，你可以轻松地将 apply 包含到调用链中以进行更复杂的处理。


### also 上下文对象作为 lambda 表达式的参数（it）来访问。 返回值是上下文对象本身。

第一个参数是以泛型类为参数的函数，返回值是泛型类

* also函数定义


```java
/**
 * Calls the specified function [block] with `this` value as its argument and returns `this` value.
 *
 * For detailed usage information see the documentation for [scope functions](https://kotlinlang.org/docs/reference/scope-functions.html#also).
 */
@kotlin.internal.InlineOnly
@SinceKotlin("1.1")
public inline fun <T> T.also(block: (T) -> Unit): T {
    contract {
        callsInPlace(block, InvocationKind.EXACTLY_ONCE)
    }
    block(this)
    return this
}

```

* also 函数使用

also 对于执行一些将上下文对象作为参数的操作很有用。 对于需要引用对象而不是其属性与函数的操作，或者不想屏蔽来自外部作用域的 this 引用时，请使用 also。

当你在代码中看到 also 时，可以将其理解为“并且用该对象执行以下操作”。
```java
val numbers = mutableListOf("one", "two", "three")
numbers
    .also { println("The list elements before adding new one: $it") }
    .add("four")

The list elements before adding new one: [one, two, three]

```


## takeIf 
 
```java
/**
 * Returns `this` value if it satisfies the given [predicate] or `null`, if it doesn't.
 */
@kotlin.internal.InlineOnly
@SinceKotlin("1.1")
public inline fun <T> T.takeIf(predicate: (T) -> Boolean): T? {
    contract {
        callsInPlace(predicate, InvocationKind.EXACTLY_ONCE)
    }
    return if (predicate(this)) this else null
}


```


### 作用域函数的区别

作用域函数没有引入任何新的技术，但是它们可以使你的代码更加简洁易读。

由于作用域函数的相似性质，为你的案例选择正确的函数可能有点棘手。选择主要取决于你的意图和项目中使用的一致性。下面我们将详细描述各种作用域函数及其约定用法之间的区别。

由于作用域函数本质上都非常相似，因此了解它们之间的区别很重要。每个作用域函数之间有两个主要区别：

- 引用上下文对象的方式
- 返回值

以下是根据预期目的选择作用域函数的简短指南：

- 对一个非空（non-null）对象执行 lambda 表达式：let


- 将表达式作为变量引入为局部作用域中：let


- 对象配置：apply


- 对象配置并且计算结果：run


- 在需要表达式的地方运行语句：非扩展的 run


- 附加效果：also


- 一个对象的一组函数调用：with


不同函数的使用场景存在重叠，你可以根据项目或团队中使用的特定约定选择函数。

尽管作用域函数是使代码更简洁的一种方法，但请避免过度使用它们：这会降低代码的可读性并可能导致错误。避免嵌套作用域函数，同时链式调用它们时要小心：此时很容易对当前上下文对象及 this 或 it 的值感到困惑。





## takeUnless

```java

/**
 * Returns `this` value if it _does not_ satisfy the given [predicate] or `null`, if it does.
 */
@kotlin.internal.InlineOnly
@SinceKotlin("1.1")
public inline fun <T> T.takeUnless(predicate: (T) -> Boolean): T? {
    contract {
        callsInPlace(predicate, InvocationKind.EXACTLY_ONCE)
    }
    return if (!predicate(this)) this else null
}

```

## repeat 顶层函数，将一个函数执行制定的次数

```java

/**
 * Executes the given function [action] specified number of [times].
 *
 * A zero-based index of current iteration is passed as a parameter to [action].
 *
 * @sample samples.misc.ControlFlow.repeat
 */
@kotlin.internal.InlineOnly
public inline fun repeat(times: Int, action: (Int) -> Unit) {
    contract { callsInPlace(action) }

    for (index in 0 until times) {
        action(index)
    }
}
```




