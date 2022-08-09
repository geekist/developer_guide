# 一、javascript基本用法

## javascript在html中的位置
HTML 中的 Javascript 脚本代码必须位于 <script> 与 </script> 标签之间。

Javascript 脚本代码可被放置在 HTML 页面的 <body> 和 <head> 部分中。


## javascript输出数据的方式

* 使用 window.alert() 弹出警告框。

```html
<!DOCTYPE html>
<html>
<body>

<h1>我的第一个页面</h1>
<p>我的第一个段落。</p>

<script>
window.alert(5 + 6);
</script>

</body>
</html>
```

* 使用 document.write() 方法将内容写到 HTML 文档中。
  
```html
<!DOCTYPE html>
<html>
<body>

<h1>我的第一个 Web 页面</h1>

<p>我的第一个段落。</p>

<script>
document.write(Date());
</script>

</body>
</html>
```

* 使用 innerHTML 写入到 HTML 元素。

```html
<!DOCTYPE html>
<html>
<body>

<h1>我的第一个 Web 页面</h1>

<p id="demo">我的第一个段落</p>

<script>
document.getElementById("demo").innerHTML = "段落已修改。";
</script>

</body>
</html>
```

* 使用 console.log() 写入到浏览器的控制台。

```html
<!DOCTYPE html>
<html>
<body>

<h1>我的第一个 Web 页面</h1>

<script>
a = 5;
b = 6;
c = a + b;
console.log(c);
</script>

</body>
</html>
```

# 二、javascript语法

## 字面量（常量）

在javascript编程语言中，一般固定值称为字面量，如 3.14。

* 数字（Number）字面量 

可以是整数或者是小数，或者是科学计数(e)。3.14   1001  123e5

* 字符串（String）字面量 

可以使用单引号或双引号: "John Doe" 'John Doe'

* 表达式字面量 

用于计算：5 + 6  5 * 10

* 数组（Array）字面量

 定义一个数组：

[40, 100, 1, 5, 25, 10]

* 对象（Object）字面量 

定义一个对象：

{firstName:"John", lastName:"Doe", age:50, eyeColor:"blue"}

* 函数（Function）字面量 

定义一个函数：

function myFunction(a, b) { return a * b;}

## 变量

变量是用于存储信息的"容器"。

* 声明（创建） JavaScript 变量

在 JavaScript 中创建变量通常称为"声明"变量。我们使用 var 关键词来声明变量：

var carname;

变量声明之后，该变量是空的（它没有值）。

如需向变量赋值，请使用等号：

carname="Volvo";

不过，您也可以在声明变量时对其赋值：

var carname="Volvo";


一个好的编程习惯是，在代码开始处，统一对需要的变量进行声明。

一条语句，多个变量

您可以在一条语句中声明很多变量。该语句以 var 开头，并使用逗号分隔变量即可：

var lastname="Doe", age=30, job="carpenter";


* 变量规定

变量也能以 $ 和 _ 符号开头（不过我们不推荐这么做）

变量名称对大小写敏感（y 和 Y 是不同的变量）

JavaScript 语句和 JavaScript 变量都对大小写敏感。






# 三、javascript类

# 四、javascript函数

# 五、DOM

# 六、BOM


