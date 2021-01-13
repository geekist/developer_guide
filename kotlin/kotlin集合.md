
# Kotlin集合

## 集合概述

集合通常包含相同类型的一些（数目也可以为零）对象。集合中的对象称为元素或条目。

### Kotlin 标准库提供了基本集合类型的实现： set、list 以及 map。
 
* List 是一个有序集合，可通过索引（反映元素位置的整数）访问元素。元素可以在 list 中出现多次。列表的一个示例是一句话：有一组字、这些字的顺序很重要并且字可以重复。

* Set 是唯一元素的集合。它反映了集合（set）的数学抽象：一组无重复的对象。一般来说 set 中元素的顺序并不重要。例如，字母表是字母的集合（set）。

* Map（或者字典）是一组键值对。键是唯一的，每个键都刚好映射到一个值。值可以重复。map 对于存储对象之间的逻辑连接非常有用，例如，员工的 ID 与员工的位置。

### 一对接口代表每种集合类型：
 
* 一个 只读 接口，提供访问集合元素的操作。

* 一个 可变 接口，通过写操作扩展相应的只读接口：添加、删除和更新其元素。


![接口图表](https://www.kotlincn.net/assets/images/reference/collections-overview/collections-diagram.png)

## 集合的基本操作

### 构造
创建集合的最常用方法是使用标准库函数 listOf<T>()、setOf<T>()、mutableListOf<T>()、mutableSetOf<T>()。同样的，Map 也有这样的函数 mapOf() 与 mutableMapOf()。映射的键和值作为 Pair 对象传递（通常使用中缀函数 to 创建） 

如果以逗号分隔的集合元素列表作为参数，编译器会自动检测元素类型。

创建空集合时，须明确指定类型。

```java

//list

 val list1 = listOf<Int>()

 val list2 = arrayListOf<Int>()

 val list3 = arrayListOf(1,2,3)

 val list4 = emptyList<Int>()

//set

 val set1 = setOf<String>()

 val set2 = setOf("1","2","3")

 val set3 = emptySet<Int>()

 val hashSet = HashSet<Int>(32)

//map

 val map1 = mapOf<Int,Int>()

 val map2 = mapOf(1 to 3,2 to 5,3 to 100)

 val map3 = emptyMap<Int,Int>()

//operation
 val sourceList = mutableListOf(1, 2, 3)
 val copySet = sourceList.toMutableSet()

```
### 集合遍历

* 迭代器

Iterable<T> 接口的继承者（包括 Set 与 List）可以通过调用 iterator() 函数获得迭代器。

```java
val numbers = listOf("one", "two", "three", "four")
val numbersIterator = numbers.iterator()
while (numbersIterator.hasNext()) {
    println(numbersIterator.next())
}

```

* for

```java
val numbers = listOf("one", "two", "three", "four")
for (item in numbers) {
    println(item)
}

```

* forEach

```java
val numbers = listOf("one", "two", "three", "four")
numbers.forEach {
    println(it)
}
```

### 集合的转换

Kotlin 标准库为集合 转换 提供了一组扩展函数。 这些函数根据提供的转换规则从现有集合中构建新集合。

* map


映射转换从另一个集合的元素上的函数结果创建一个集合。 基本的映射函数是 map()。 它将给定的 lambda 函数应用于每个后续元素，并返回 lambda 结果列表。 结果的顺序与元素的原始顺序相同。 如需应用还要用到元素索引作为参数的转换，请使用 mapIndexed()。
```java
val numbers = setOf(1, 2, 3)
println(numbers.map { it * 3 })
println(numbers.mapIndexed { idx, value -> value * idx })


[3, 6, 9]
[0, 2, 6]
```

如果转换在某些元素上产生 null 值，则可以通过调用 mapNotNull() 函数取代 map() 或 mapIndexedNotNull() 取代 mapIndexed() 来从结果集中过滤掉 null 值。

```java
val numbers = setOf(1, 2, 3)
println(numbers.mapNotNull { if ( it == 2) null else it * 3 })
println(numbers.mapIndexedNotNull { idx, value -> if (idx == 0) null else value * idx })


[3, 9]
[2, 6]
```
映射转换时，有两个选择：转换键，使值保持不变，反之亦然。 要将指定转换应用于键，请使用 mapKeys()；反过来，mapValues() 转换值。 这两个函数都使用将映射条目作为参数的转换，因此可以操作其键与值。

```java
val numbersMap = mapOf("key1" to 1, "key2" to 2, "key3" to 3, "key11" to 11)
println(numbersMap.mapKeys { it.key.toUpperCase() })
println(numbersMap.mapValues { it.value + it.key.length })


{KEY1=1, KEY2=2, KEY3=3, KEY11=11}
{key1=5, key2=6, key3=7, key11=16}
```

* zip

合拢转换是根据两个集合中具有相同位置的元素构建配对 在 Kotlin 标准库中，这是通过 zip() 扩展函数完成的。 在一个集合（或数组）上以另一个集合（或数组）作为参数调用时，zip() 返回 Pair 对象的列表（List）。 接收者集合的元素是这些配对中的第一个元素。

如果集合的大小不同，则 zip() 的结果为较小集合的大小；结果中不包含较大集合的后续元素。 zip() 也可以中缀形式调用 a zip b 。

```java
val colors = listOf("red", "brown", "grey")
val animals = listOf("fox", "bear", "wolf")
println(colors zip animals)
​
val twoAnimals = listOf("fox", "bear")
println(colors.zip(twoAnimals))

[(red, fox), (brown, bear), (grey, wolf)]
[(red, fox), (brown, bear)]
```

也可以使用带有两个参数的转换函数来调用 zip()：接收者元素和参数元素。 在这种情况下，结果 List 包含在具有相同位置的接收者对和参数元素对上调用的转换函数的返回值。
```java
val colors = listOf("red", "brown", "grey")
val animals = listOf("fox", "bear", "wolf")
​
println(colors.zip(animals) { color, animal -> "The ${animal.capitalize()} is $color"})

[The Fox is red, The Bear is brown, The Wolf is grey]

```

当拥有 Pair 的 List 时，可以进行反向转换 unzipping——从这些键值对中构建两个列表：

第一个列表包含原始列表中每个 Pair 的键。
第二个列表包含原始列表中每个 Pair 的值。
要分割键值对列表，请调用 unzip()。

```java
val numberPairs = listOf("one" to 1, "two" to 2, "three" to 3, "four" to 4)
println(numberPairs.unzip())

([one, two, three, four], [1, 2, 3, 4])

```

* associatedwith 关联


关联转换允许从集合元素和与其关联的某些值构建 Map。 在不同的关联类型中，元素可以是关联 Map 中的键或值。

基本的关联函数 associateWith() 创建一个 Map，其中原始集合的元素是键，并通过给定的转换函数从中产生值。 如果两个元素相等，则仅最后一个保留在 Map 中。
```java
val numbers = listOf("one", "two", "three", "four")
println(numbers.associateWith { it.length })
{one=3, two=3, three=5, four=4}

```

为了使用集合元素作为值来构建 Map，有一个函数 associateBy()。 它需要一个函数，该函数根据元素的值返回键。如果两个元素相等，则仅最后一个保留在 Map 中。 还可以使用值转换函数来调用 associateBy()。

```java
val numbers = listOf("one", "two", "three", "four")
​
println(numbers.associateBy { it.first().toUpperCase() })
println(numbers.associateBy(
	keySelector = { it.first().toUpperCase() }, 
	valueTransform = { it.length }))

{O=one, T=three, F=four}
{O=3, T=5, F=4}
```

另一种构建 Map 的方法是使用函数 associate()，其中 Map 键和值都是通过集合元素生成的。 它需要一个 lambda 函数，该函数返回 Pair：键和相应 Map 条目的值。

请注意，associate() 会生成临时的 Pair 对象，这可能会影响性能。 因此，当性能不是很关键或比其他选项更可取时，应使用 associate()。

后者的一个示例：从一个元素一起生成键和相应的值。

```java
val names = listOf("Alice Adams", "Brian Brown", "Clara Campbell")
println(names.associate { name -> parseFullName(name).let { it.lastName to it.firstName } }) 
 
{Adams=Alice, Brown=Brian, Campbell=Clara}
```
此时，首先在一个元素上调用一个转换函数，然后根据该函数结果的属性建立 Pair。


* 打平 -- flapMap()


如需操作嵌套的集合，则可能会发现提供对嵌套集合元素进行打平访问的标准库函数很有用。

第一个函数为 flatten()。可以在一个集合的集合（例如，一个 Set 组成的 List）上调用它。 该函数返回嵌套集合中的所有元素的一个 List。

```java
val numberSets = listOf(setOf(1, 2, 3), setOf(4, 5, 6), setOf(1, 2))
println(numberSets.flatten())

[1, 2, 3, 4, 5, 6, 1, 2]
```


另一个函数——flatMap() 提供了一种灵活的方式来处理嵌套的集合。 它需要一个函数将一个集合元素映射到另一个集合。 因此，flatMap() 返回单个列表其中包含所有元素的值。 所以，flatMap() 表现为 map()（以集合作为映射结果）与 flatten() 的连续调用。

```java

val containers = listOf(
    StringContainer(listOf("one", "two", "three")),
    StringContainer(listOf("four", "five", "six")),
    StringContainer(listOf("seven", "eight"))
)

println(containers.flatMap { it.values })

[one, two, three, four, five, six, seven, eight]
```

*  join


如果需要以可读格式检索集合内容，请使用将集合转换为字符串的函数：joinToString() 与 joinTo()。

joinToString() 根据提供的参数从集合元素构建单个 String。 joinTo() 执行相同的操作，但将结果附加到给定的 Appendable 对象。

当使用默认参数调用时，函数返回的结果类似于在集合上调用 toString()：各元素的字符串表示形式以空格分隔而成的 String。

```java
val numbers = listOf("one", "two", "three", "four")
​
println(numbers)         
println(numbers.joinToString())
​
val listString = StringBuffer("The list of numbers: ")
numbers.joinTo(listString)
println(listString)


[one, two, three, four]
one, two, three, four
The list of numbers: one, two, three, four
```

要构建自定义字符串表示形式，可以在函数参数 separator、prefix 与 postfix中指定其参数。 结果字符串将以 prefix 开头，以 postfix 结尾。除最后一个元素外，separator 将位于每个元素之后。

```java

val numbers = listOf("one", "two", "three", "four")    
println(numbers.joinToString(separator = " | ", prefix = "start: ", postfix = ": end"))
start: one | two | three | four: end
```

对于较大的集合，可能需要指定 limit ——将包含在结果中元素的数量。 如果集合大小超出 limit，所有其他元素将被 truncated 参数的单个值替换。

```java
val numbers = (1..100).toList()
println(numbers.joinToString(limit = 10, truncated = "<...>"))
1, 2, 3, 4, 5, 6, 7, 8, 9, 10, <...>
```


最后，要自定义元素本身的表示形式，请提供 transform 函数。

```java
val numbers = listOf("one", "two", "three", "four")
println(numbers.joinToString { "Element: ${it.toUpperCase()}"})


Element: ONE, Element: TWO, Element: THREE, Element: FOUR
```

### 集合过滤

过滤是最常用的集合处理任务之一。在Kotlin中，过滤条件由 谓词 定义——接受一个集合元素并且返回布尔值的 lambda 表达式：true 说明给定元素与谓词匹配，false 则表示不匹配。

标准库包含了一组让你能够通过单个调用就可以过滤集合的扩展函数。这些函数不会改变原始集合，因此它们既可用于可变集合也可用于只读集合。为了操作过滤结果，应该在过滤后将其赋值给变量或链接其他函数。

* 按谓词过滤
基本的过滤函数是 filter()。当使用一个谓词来调用时，filter() 返回与其匹配的集合元素。对于 List 和 Set，过滤结果都是一个 List，对 Map 来说结果还是一个 Map。

```java
val numbers = listOf("one", "two", "three", "four")  
val longerThan3 = numbers.filter { it.length > 3 }
println(longerThan3)
​
val numbersMap = mapOf("key1" to 1, "key2" to 2, "key3" to 3, "key11" to 11)
val filteredMap = numbersMap.filter { (key, value) -> key.endsWith("1") && value > 10}
println(filteredMap)


[three, four]
{key11=11}
```

filter() 中的谓词只能检查元素的值。如果想在过滤中使用元素在集合中的位置，应该使用 filterIndexed()。它接受一个带有两个参数的谓词：元素的索引和元素的值。

如果想使用否定条件来过滤集合，请使用 filterNot()。它返回一个让谓词产生 false 的元素列表。

```java
val numbers = listOf("one", "two", "three", "four")
​
val filteredIdx = numbers.filterIndexed { index, s -> (index != 0) && (s.length < 5)  }
val filteredNot = numbers.filterNot { it.length <= 3 }
​
println(filteredIdx)
println(filteredNot)


[two, four]
[three, four]

```

还有一些函数能够通过过滤给定类型的元素来缩小元素的类型：

filterIsInstance() 返回给定类型的集合元素。在一个 List<Any> 上被调用时，filterIsInstance<T>() 返回一个 List<T>，从而让你能够在集合元素上调用 T 类型的函数。

```java
val numbers = listOf(null, 1, "two", 3.0, "four")
println("All String elements in upper case:")
numbers.filterIsInstance<String>().forEach {
    println(it.toUpperCase())
}
All String elements in upper case:
TWO
FOUR

```

filterNotNull() 返回所有的非空元素。在一个 List<T?> 上被调用时，filterNotNull() 返回一个 List<T: Any>，从而让你能够将所有元素视为非空对象。

```java
val numbers = listOf(null, "one", "two", null)
numbers.filterNotNull().forEach {
    println(it.length)   // 对可空的 String 来说长度不可用
}
3
3

```


* 划分
* 
另一个过滤函数 – partition() – 通过一个谓词过滤集合并且将不匹配的元素存放在一个单独的列表中。因此，你得到一个 List 的 Pair 作为返回值：第一个列表包含与谓词匹配的元素并且第二个列表包含原始集合中的所有其他元素。

```java
val numbers = listOf("one", "two", "three", "four")
val (match, rest) = numbers.partition { it.length > 3 }
​
println(match)
println(rest)
[three, four]
[one, two]

```

检验谓词
最后，有些函数只是针对集合元素简单地检测一个谓词：

如果至少有一个元素匹配给定谓词，那么 any() 返回 true。
如果没有元素与给定谓词匹配，那么 none() 返回 true。
如果所有元素都匹配给定谓词，那么 all() 返回 true。注意，在一个空集合上使用任何有效的谓词去调用 all() 都会返回 true 。这种行为在逻辑上被称为 vacuous truth。

```java
val numbers = listOf("one", "two", "three", "four")
​
println(numbers.any { it.endsWith("e") })
println(numbers.none { it.endsWith("a") })
println(numbers.all { it.endsWith("e") })
​
println(emptyList<Int>().all { it > 5 })   // vacuous truth
true
true
false
true

```

any() 和 none() 也可以不带谓词使用：在这种情况下它们只是用来检查集合是否为空。 如果集合中有元素，any() 返回 true，否则返回 false；none() 则相反。

```java
val numbers = listOf("one", "two", "three", "four")
val empty = emptyList<String>()
​
println(numbers.any())
println(empty.any())
​
println(numbers.none())
println(empty.none())
true
false
false
true
```
### 加减运算符

### 分组-- groupBy

Kotlin 标准库提供用于对集合元素进行分组的扩展函数。 基本函数 groupBy() 使用一个 lambda 函数并返回一个 Map。 在此 Map 中，每个键都是 lambda 结果，而对应的值是返回此结果的元素 List。 例如，可以使用此函数将 String 列表按首字母分组。

还可以使用第二个 lambda 参数（值转换函数）调用 groupBy()。 在带有两个 lambda 的 groupBy() 结果 Map 中，由 keySelector 函数生成的键映射到值转换函数的结果，而不是原始元素。

```java
val numbers = listOf("one", "two", "three", "four", "five")
​
println(numbers.groupBy { it.first().toUpperCase() })
println(numbers.groupBy(keySelector = { it.first() }, valueTransform = { it.toUpperCase() }))


{O=[one], T=[two, three], F=[four, five]}
{o=[ONE], t=[TWO, THREE], f=[FOUR, FIVE]}

```

如果要对元素进行分组，然后一次将操作应用于所有分组，请使用 groupingBy() 函数。 它返回一个 Grouping 类型的实例。 通过 Grouping 实例，可以以一种惰性的方式将操作应用于所有组：这些分组实际上是刚好在执行操作前构建的。

即，Grouping 支持以下操作：

eachCount() 计算每个组中的元素。
fold() 与 reduce() 对每个组分别执行 fold 与 reduce 操作，作为一个单独的集合并返回结果。
aggregate() 随后将给定操作应用于每个组中的所有元素并返回结果。 这是对 Grouping 执行任何操作的通用方法。当折叠或缩小不够时，可使用它来实现自定义操作。

```java
val numbers = listOf("one", "two", "three", "four", "five", "six")
println(numbers.groupingBy { it.first() }.eachCount())


{o=1, t=2, f=2, s=1}



```
### 取集合的一部分

### 取单个元素 first next，last，random

### 集合排序 compareTo

### 聚合操作  max，min，avearge


## Sequence

除了集合之外，Kotlin 标准库还包含另一种容器类型——序列（Sequence<T>）

### 构造

* 由元素


要创建一个序列，请调用 sequenceOf() 函数，列出元素作为其参数。

```java
val numbersSequence = sequenceOf("four", "three", "two", "one")
```

* 由 Iterable

如果已经有一个 Iterable 对象（例如 List 或 Set），则可以通过调用 asSequence() 从而创建一个序列。
```java
val numbers = listOf("one", "two", "three", "four")
val numbersSequence = numbers.asSequence()
```

* 由函数


创建序列的另一种方法是通过使用计算其元素的函数来构建序列。 要基于函数构建序列，请以该函数作为参数调用 generateSequence()。 （可选）可以将第一个元素指定为显式值或函数调用的结果。 当提供的函数返回 null 时，序列生成停止。因此，以下示例中的序列是无限的。

```java
val oddNumbers = generateSequence(1) { it + 2 } // `it` 是上一个元素
println(oddNumbers.take(5).toList())
//println(oddNumbers.count())     // 错误：此序列是无限的。

```

要使用 generateSequence() 创建有限序列，请提供一个函数，该函数在需要的最后一个元素之后返回 null。

```java
val oddNumbersLessThan10 = generateSequence(1) { if (it + 2 < 10) it + 2 else null }
println(oddNumbersLessThan10.count())
Target platform: JVMRunning on kotlin v. 1.4.20
```

* 由组块


最后，有一个函数可以逐个或按任意大小的组块生成序列元素——sequence() 函数。 

此函数采用一个 lambda 表达式，其中包含 yield() 与 yieldAll() 函数的调用。 它们将一个元素返回给序列使用者，并暂停 sequence() 的执行，直到使用者请求下一个元素。 yield() 使用单个元素作为参数；yieldAll() 中可以采用 Iterable 对象、Iterable 或其他 Sequence。yieldAll() 的 Sequence 参数可以是无限的。 当然，这样的调用必须是最后一个：之后的所有调用都永远不会执行。

### 序列操作

