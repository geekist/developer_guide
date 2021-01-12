# 类与对象



## 类的声明

Kotlin 中使⽤关键字 class 声明类

class Invoice { /*……*/ }

类声明由类名、类头（指定其类型参数、主构造函数等）以及由花括号包围的类体构成。类头与类体都是可选的； 如果⼀个类没有类体，可以省略花括号。

class Empty

在 Kotlin 中的⼀个类可以有⼀个主构造函数以及⼀个或多个次构造函数

### 主构造函数

主构造函数是类头的的一部分：它跟在类名（与可选的类型参数）后。

class Person constructor(firstName: String) { /*……*/ }

如果主构造函数没有任何注解或者可.性修饰符，可以省略这个 constructor 关键字。

class Person(firstName: String) { /*……*/ }

主构造函数不能包含任何的代码。


### 初始化代码init

初始化的代码可以放到以 init 关键字作为前缀的初始化块（initializerblocks）中。

在实例初始化期间，初始化块按照它们出现在类体中的顺序执.，与属性初始化器交织在.起：
```java
class InitOrderDemo(name: String) {
val firstProperty = "First property: $name".also(::println)
init {
println("First initializer block that prints ${name}")
}
val secondProperty = "Second property: ${name.length}".also(::println)
init {
println("Second initializer block that prints ${name.length}")
}
}
```

### 次构造函数

类也可以声明前缀有 constructor的次构造函数：
```java
class Person {

var children: MutableList<Person> = mutableListOf<>()

constructor(parent: Person) {

parent.children.add(this)

}
}

```

### 类实例的创建

```java

val invoice = Invoice()

val customer = Customer("Joe Smith")

```
### 类的继承

在 Kotlin 中所有类都有⼀个共同的超类 Any ，这对于没有超类型声明的类是默认超类：

class Example // 从 Any 隐式继承

Any 有三个⽅法：equals() 、 hashCode() 与 toString() 。

因此，为所有 Kotlin 类都定义了这些⽅法。

默认情况下，Kotlin 类是最终（final）的：它们不能被继承。 要使⼀个类可继承，请⽤ open 关键字标记它。

open class Base // 该类开放继承

如需声明⼀个显式的超类型，请在类头中把超类型放到冒号之后：

open class Base(p: Int)

class Derived(p: Int) : Base(p)

如果派⽣类有⼀个主构造函数，其基类可以（并且必须） ⽤派⽣类主构造函数的参数就地初始化。

### open method

Kotlin 对于可覆盖的成员（我们称之为开放）以及覆盖后的成员需要显式修饰符：

open class Shape {

open fun draw() { /*……*/ }

fun fill() { /*……*/ }
}

class Circle() : Shape() {

override fun draw() { /*……*/ }
}

### open field

属性覆盖与⽅法覆盖类似；在超类中声明然后在派⽣类中重新声明的属性必须以 override 开头，并且它们必须具有兼容的类型。 每个声明的属性可以由具有初始化器的属性或者具有 get ⽅法的属性覆盖。

open class Shape {
open val vertexCount: Int = 0
}
class Rectangle : Shape() {
override val vertexCount = 4
}

### 抽象类

类以及其中的某些成员可以声明为 abstract。 抽象成员在本类中可以不⽤实现。 需要注意的是，我们并不需要⽤open 标注⼀个抽象类或者函数⸺因为这不⾔⽽喻。

我们可以⽤⼀个抽象成员覆盖⼀个⾮抽象的开放成员

open class Polygon {
open fun draw() {}
}

abstract class Rectangle : Polygon() {
abstract override fun draw()
}

### 对象声明---单例类 object

object DataProviderManager {
fun registerDataProvider(provider: DataProvider) {
// ……
}
val allDataProviders: Collection<DataProvider>
get() = // ……
}
这称为对象声明。并且它总是在 object 关键字后跟一个名称。 就像变量声明，对象声明不是个表达式，不能用在赋值语句的右边。

如需引用该对象，我们直接使用其名称即可：

DataProviderManager.registerDataProvider(……)

这些对象可以有超类型：
object DefaultListener : MouseAdapter() {
override fun mouseClicked(e: MouseEvent) { …… }
override fun mouseEntered(e: MouseEvent) { …… }
}
注意：对象声明不能在局部作.域（即直接嵌套在函数内部），但是它们可以嵌套到其他对象声明或.内部类中。


### 伴⽣对象 compain object

如果你需要写⼀个可以⽆需⽤⼀个类的实例来调⽤、但需要访问类内部的函数（例如，⼯⼚⽅法），你可以把它写成该类内对象声明中的⼀员。

更具体地讲，如果在你的类内声明了⼀个伴⽣对象， 你就可以访问其成员，只是以类名作为限定符。

类内部的对象声明可以. companion 关键字标记：
class MyClass {
companion object Factory {
fun create(): MyClass = MyClass()
}
}
该伴生对象的成员可通过只使.类名作为限定符来调.：

val instance = MyClass.create()


可以省略伴⽣对象的名称，在这种情况下将使⽤名称 Companion ：
class MyClass {
companion object { }
}

val x = MyClass.Companion


## 属性与字段

声明⼀个属性的完整语法是
var <propertyName>[: <PropertyType>] [= <property_initializer>]
[<getter>]
[<setter>]
其初始器（initializer）、getter 和 setter 都是可选的。属性类型如果可以从初始器（ 或者从其 getter 返回值，如下⽂所⽰）中推断出来，也可以省略。

例如:
var allByDefault: Int? // 错误：需要显式初始化器，隐含默认 getter 和 setter
var initialized = 1 // 类型 Int、默认 getter 和 setter

### 幕后字段

在 Kotlin 类中不能直接声明字段。然⽽，当⼀个属性需要⼀个幕后字段时，Kotlin 会⾃动提供。这个幕后字段可以使⽤field 标识符在访问器中引⽤：

var counter = 0 // 注意：这个初始器直接为幕后字段赋值
set(value) {
  if (value >= 0) field = value
}

field 标识符只能⽤在属性的访问器内。

**如果属性⾄少⼀个访问器使⽤默认实现，或者⾃定义访问器通过 field 引⽤幕后字段，将会为该属性⽣成⼀个幕后字段。**

例如，下⾯的情况下， 就没有幕后字段：
val isEmpty: Boolean
get() = this.size == 0

### 幕后属性

如果你的需求不符合这套“隐式的幕后字段”.案，那么总可以使. 幕后属性（backing property）：

```java
private var _table: Map<String, Int>? = null
public val table: Map<String, Int>
get() {
if (_table == null) {
_table = HashMap() // 类型参数已推断出
}
return _table ?: throw AssertionError("Set to null by another thread")
}
```
### 编译器常量const

如果只读属性的值在编译期是已知的，那么可以使⽤ const 修饰符将其标记为编译期常量。 这种属性需要满⾜以下要求：

- 位于顶层或者是 object 声明 或 companion object 的⼀个成员
- 以 String 或原⽣类型值初始化
- 没有⾃定义 getter

### 延迟初始化属性

```java
public class MyTest {
lateinit var subject: TestSubject
@SetUp fun setup() {
subject = TestSubject()
}
@Test fun test() {
subject.method() // 直接解引⽤
}
}


if (foo::bar.isInitialized) {
println(foo.bar)
}

```


## 接口

### 接口定义

Kotlin 的接⼝可以既包含抽象⽅法的声明也包含实现。与抽象类不同的是，接⼝⽆法保存状态。它可以有属性但必须声明为抽象或提供访问器实现。

使⽤关键字 interface 来定义接⼝

interface MyInterface {
fun bar()
fun foo() {
// 可选的⽅法体
}
}

### 接口实现

⼀个类或者对象可以实现⼀个或多个接⼝。
class Child : MyInterface {
override fun bar() {
// ⽅法体
}
}



## 函数式接口（SAM）

只有⼀个抽象⽅法的接⼝称为函数式接⼝或 SAM（单⼀抽象⽅法）接⼝。函数式接⼝可以有多个⾮抽象成员，但只能有⼀个抽象成员。

可以⽤ fun 修饰符在 Kotlin 中声明⼀个函数式接⼝。

```kotlin
fun interface KRunnable {
fun invoke()
}
```

对于函数式接⼝，可以通过lambda 表达式实现 SAM 转换，从⽽使代码更简洁、更有可读性。
使⽤ lambda 表达式可以替代⼿动创建实现函数式接⼝的类。 通过 SAM 转换， Kotlin 可以将其签名与接⼝的单个抽象⽅法的签名匹配的任何 lambda 表达式转换为实现该接⼝的类的实例。

例如，有这样⼀个 Kotlin 函数式接⼝：
fun interface IntPredicate {
fun accept(i: Int): Boolean
}

如果不使⽤ SAM 转换，那么你需要像这样编写代码：
// 创建⼀个类的实例
val isEven = object : IntPredicate {
override fun accept(i: Int): Boolean {
return i % 2 == 0
}
}

通过利⽤ Kotlin 的 SAM 转换，可以改为以下等效代码：
// 通过 lambda 表达式创建⼀个实例
val isEven = IntPredicate { it % 2 == 0 }

可以通过更短的 lambda 表达式替换所有不必要的代码。
fun interface IntPredicate {
fun accept(i: Int): Boolean
}
val isEven = IntPredicate { it % 2 == 0 }
fun main() {
println("Is 7 even? - ${isEven.accept(7)}")
}

## 可见性修饰符


## 扩展

Kotlin 能够扩展⼀个类的新功能⽽⽆需继承该类或者使⽤像装饰者这样的设计模式。这通过叫做 扩展 的特殊声明完成。例如，你可以为⼀个你不能修改的、来⾃第三⽅库中的类编写⼀个新的函数。这个新增的函数就像那个原始类本来就有的函数⼀样，可以⽤普通的⽅法调⽤。这种机制称为 扩展函数 。此外，也有 扩展属性 ，允许你为⼀个已经存在的类添加新的属性。

声明⼀个扩展函数，我们需要⽤⼀个 接收者类型 也就是被扩展的类型来作为他的前缀。下⾯代码MutableList<Int> 添加⼀个 swap 函数：

```java
fun MutableList<Int>.swap(index1: Int, index2: Int) {
val tmp = this[index1] // “this”对应该列表
this[index1] = this[index2]
this[index2] = tmp
}
```
这个 this 关键字在扩展函数内部对应到接收者对象（传过来的在点符号前的对象）现在，

我们对任意
MutableList<Int> 调⽤该函数了：

```java
val list = mutableListOf(1, 2, 3)
list.swap(0, 2) // “swap()”内部的“this”会保存“list”的值
```
当然，这个函数对任何 MutableList<T> 起作⽤，我们可以泛化它：

```java
fun <T> MutableList<T>.swap(index1: Int, index2: Int) {
val tmp = this[index1] // “this”对应该列表
this[index1] = this[index2]
this[index2] = tmp
}
```

为了在接收者类型表达式中使⽤泛型，我们要在函数名前声明泛型参数。

## 数据类
```java
data class User(val name: String, val age: Int)
```

编译器⾃动从主构造函数中声明的所有属性导出以下成员：

- equals() / hashCode() 对；
- toString() 格式是 "User(name=John, age=42)" ；
- componentN() 函数 按声明顺序对应于所有属性；
- copy() 函数（⻅下⽂）。

为了确保⽣成的代码的⼀致性以及有意义的⾏为，数据类必须满⾜以下要求：
- 
- 主构造函数需要⾄少有⼀个参数；
- 主构造函数的所有参数需要标记为 val 或 var ；
- 数据类不能是抽象、开放、密封或者内部的；
- （在1.1之前）数据类只能实现接⼝。


## 密封类

密封类⽤来表⽰受限的类继承结构：当⼀个值为有限⼏种的类型、⽽不能有任何其他类型时。在某种意义上，他们是枚举类的扩展：枚举类型的值集合也是受限的，但每个枚举常量只存在⼀个实例，⽽密封类的⼀个⼦类可以有可包含状态的多个实例。

要声明⼀个密封类，需要在类名前⾯添加 sealed 修饰符。虽然密封类也可以有⼦类，但是所有⼦类都必须在与密封类⾃⾝相同的⽂件中声明。（在 Kotlin 1.1 之前，该规则更加严格：⼦类必须嵌套在密封类声明的内部）。

sealed class Expr
data class Const(val number: Double) : Expr()
data class Sum(val e1: Expr, val e2: Expr) : Expr()
object NotANumber : Expr()

（上⽂⽰例使⽤了 Kotlin 1.1 的⼀个额外的新功能：数据类扩展包括密封类在内的其他类的可能性。）
⼀个密封类是⾃⾝抽象的，它不能直接实例化并可以有抽象（abstract）成员。

密封类不允许有⾮-private 构造函数（其构造函数默认为 private）。

请注意，扩展密封类⼦类的类（间接继承者）可以放在任何位置，⽽⽆需在同⼀个⽂件中。

使⽤密封类的关键好处在于使⽤ when 表达式 的时候，如果能够验证语句覆盖了所有情况，就不需要为该语句再添加⼀个 else ⼦句了。当然，这只有当你⽤ when 作为表达式（使⽤结果）⽽不是作为语句时才有⽤。
fun eval(expr: Expr): Double = when(expr) {
is Const -> expr.number
is Sum -> eval(expr.e1) + eval(expr.e2)
NotANumber -> Double.NaN
// 不再需要 `else` ⼦句，因为我们已经覆盖了所有的情况
}


## 泛型类

与 Java 类似，Kotlin 中的类也可以有类型参数：
class Box<T>(t: T) {
var value = t
}

⼀般来说，要创建这样类的实例，我们需要提供类型参数：

val box: Box<Int> = Box<Int>(1)

但是如果类型参数可以推断出来，例如从构造函数的参数或者从其他途径，允许省略类型参数：

val box = Box(1) // 1 具有类型 Int，所以编译器知道我们说的是 Box<Int>。

### 声明处形变
java泛型不支持形变，即List<String> 不是 List<Object>的子类。

在 Kotlin 中，有⼀种⽅法向编译器解释这种情况。这称为声明处型变：我们可以标注 Source 的类型参数 T 来确保它仅从 Source<T> 成员中返回（⽣产），并从不被消费。为此，我们提供 out 修饰符：
interface Source<out T> {
fun nextT(): T
}
fun demo(strs: Source<String>) {
val objects: Source<Any> = strs // 这个没问题，因为 T 是⼀个 out-参数
// ……
}


### 类型投影

## 泛型函数

不仅类可以有类型参数。函数也可以有。类型参数要放在函数名称之前：
f
un <T> singletonList(item: T): List<T> {
// ……
}
fun <T> T.basicToString(): String { // 扩展函数
// ……
}

要调⽤泛型函数，在调⽤处函数名之后指定类型参数即可：

val l = singletonList<Int>(1)

可以省略能够从上下⽂中推断出来的类型参数，所以以下⽰例同样适⽤：

val l = singletonList(1)

## 嵌套类

类可以嵌套在其他类中：
class Outer {
private val bar: Int = 1
class Nested {
fun foo() = 2
}
}
val demo = Outer.Nested().foo() // == 2

## 内部类

标记为 inner 的嵌套类能够访问其外部类的成员。内部类会带有⼀个对外部类的对象的引⽤：
class Outer {
private val bar: Int = 1
inner class Inner {
fun foo() = bar
}
}
val demo = Outer().Inner().foo() // == 1


## 匿名内部类--用对象表达式来表达

使⽤对象表达式创建匿名内部类实例：

```java
window.addMouseListener(object : MouseAdapter() {
	override fun mouseClicked(e: MouseEvent) { …… }
	override fun mouseEntered(e: MouseEvent) { …… }
})

```
注：对于 JVM 平台, 如果对象是函数式 Java 接⼝（即具有单个抽象⽅法的 Java 接⼝）的实例，你可以使⽤带接⼝类型
前缀的lambda表达式创建它：
val listener = ActionListener { println("clicked") }



## 枚举类

枚举类的最基本的⽤法是实现类型安全的枚举：
enum class Direction {
NORTH, SOUTH, WEST, EAST
}

每个枚举常量都是⼀个对象。枚举常量⽤逗号分隔。

因为每⼀个枚举都是枚举类的实例，所以他们可以是这样初始化过的：
enum class Color(val rgb: Int) {
RED(0xFF0000),
GREEN(0x00FF00),
BLUE(0x0000FF)
}


## 对象表达式与对象声明

要创建⼀个继承⾃某个（或某些）类型的匿名类的对象，我们会这么写：

window.addMouseListener(object : MouseAdapter() {
override fun mouseClicked(e: MouseEvent) { /*……*/ }
override fun mouseEntered(e: MouseEvent) { /*……*/ }
})

如果超类型有⼀个构造函数，则必须传递适当的构造函数参数给它。 多个超类型可以由跟在冒号后⾯的逗号分隔的列表指定：

open class A(x: Int) {

public open val y: Int = x

}

interface B { /*……*/ }

val ab: A = object : A(1), B {

	override val y = 15
}

任何时候，如果我们只需要“⼀个对象⽽已”，并不需要特殊超类型，那么我们可以简单地写：
fun foo() {
val adHoc = object {
var x: Int = 0
var y: Int = 0
}
print(adHoc.x + adHoc.y)
}

## 内联类

```java
inline class Password(val value: String)

```

## 委托

### 属性委托

有⼀些常⻅的属性类型，虽然我们可以在每次需要的时候⼿动实现它们， 但是如果能够为⼤家把他们只实现⼀次并放⼊⼀个库会更好。例如包括：
延迟属性（lazy properties）: 其值只在⾸次访问时计算；
可观察属性（observable properties）: 监听器会收到有关此属性变更的通知；
把多个属性储存在⼀个映射（map）中，⽽不是每个存在单独的字段中。
为了涵盖这些（以及其他）情况，Kotlin ⽀持 委托属性:

class Example {
var p: String by Delegate()
}

语法是： val/var <属性名>: <类型> by <表达式> 。

在 by 后⾯的表达式是该 委托， 因为属性对应的 get()（与set() ）会被委托给它的 getValue() 与 setValue() ⽅法。 属性的委托不必实现任何的接⼝，但是需要提供⼀个 getValue() 函数（与 setValue() ⸺对于 var 属性）。 例如:

import kotlin.reflect.KProperty
class Delegate {
operator fun getValue(thisRef: Any?, property: KProperty<*>): String {
return "$thisRef, thank you for delegating '${property.name}' to me!"
}
operator fun setValue(thisRef: Any?, property: KProperty<*>, value: String) {
println("$value has been assigned to '${property.name}' in $thisRef.")
}
}