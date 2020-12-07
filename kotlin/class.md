# 类与对象

## 类与继承

## 属性与字段

## 接口

## 函数式接口（SAM）
只有⼀个抽象⽅法的接⼝称为函数式接⼝或 SAM（单⼀抽象⽅法）接⼝。函数式接⼝可以有多个⾮抽象成员，但只能有⼀个抽象成员。
可以⽤ fun 修饰符在 Kotlin 中声明⼀个函数式接⼝。
```kotlin
fun interface KRunnable {
fun invoke()
}
```

对于函数式接⼝，可以通过 lambda 表达式实现 SAM 转换，从⽽使代码更简洁、更有可读性。
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

## 数据类

## 密封类

## 泛型

## 嵌套类和内部类

## 美剧类

## 对象表达式与对象声明

## 内联类

## 委托