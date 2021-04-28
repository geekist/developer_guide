# 函数式接口和lambda表达式

## 函数式接口

每个接口只包含一个抽象方法的接口，称为函数式方法 。

在编写接口时，可以使用 @FunctionalInterface 注解强制执行此“函数式方法”模式：




## lambda表达式

### Lambda 表达式的基本语法是：

* 参数。

* 接着 ->，可视为“产出”。

* -> 之后的内容都是方法体。



[1] 当只用一个参数，可以不需要括号 ()。 然而，这是一个特例。

[2] 正常情况使用括号 () 包裹参数。 为了保持一致性，也可以使用括号 () 包裹单个参数，虽然这种情况并不常见。

[3] 如果没有参数，则必须使用括号 () 表示空参数列表。

[4] 对于多个参数，将参数列表放在括号 () 中。

[5] 如果在 Lambda 表达式中确实需要多行，则必须将这些行放在花括号中。 在这种情况下，就需要使用 return。

## 使用函数式接口表示lambda表达式

```java

interface Description {
  String brief();
}

interface Body {
  String detailed(String head);
}

interface Multi {
  String twoArg(String head, Double d);
}

public class LambdaExpressions {

  static Body bod = h -> h + " No Parens!"; // [1]

  static Body bod2 = (h) -> h + " More details"; // [2]

  static Description desc = () -> "Short info"; // [3]

  static Multi mult = (h, n) -> h + n; // [4]

  static Description moreLines = () -> { // [5]
    System.out.println("moreLines()");
    return "from moreLines()";
  };


```