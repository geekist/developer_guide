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

在 2015 后的 JavaScript 版本 (ES6) 允许我们使用 const 关键字来定义一个常量，使用 let 关键字定义的限定范围内作用域的变量。

## 数据类型

JavaScript 拥有动态类型，这意味着相同的变量可用作不同的类型：

```javascript
var x;               // x 为 undefined
var x = 5;           // 现在 x 为数字
var x = "John";      // 现在 x 为字符串

变量的数据类型可以使用 typeof 操作符来查看：
typeof "John"                // 返回 string
typeof 3.14                  // 返回 number
typeof false                 // 返回 boolean
typeof [1,2,3,4]             // 返回 object
typeof {name:'John', age:34} // 返回 object
```
声明变量时，可以使用关键词 "new" 来声明其类型：
```
var carname = new String;
var x=      new Number;
var y=      new Boolean;
var cars=   new Array;
var person= new Object;
```


值类型(基本类型)：

字符串（String）

字符串是存储字符（比如 "Bill Gates"）的变量。字符串可以是引号中的任意文本。可以使用单引号或双引号：

```
var carname="Volvo XC60";
var carname='Volvo XC60';

可以在字符串中使用引号，只要不匹配包围字符串的引号即可：
var answer="It's alright";
var answer="He is called 'Johnny'";
var answer='He is called "Johnny"';
```
数字(Number)

avaScript 只有一种数字类型。数字可以带小数点，也可以不带：

```
var x1=34.00;      //使用小数点来写
var x2=34;         //不使用小数点来写

极大或极小的数字可以通过科学（指数）计数法来书写：
var y=123e5;      // 12300000
var z=123e-5;     // 0.00123
```

布尔(Boolean)

布尔（逻辑）只能有两个值：true 或 false。布尔常用在条件测试中。
```
var x=true;
var y=false;
```

空（Null）

未定义（Undefined）

Undefined 这个值表示变量不含有值。

可以通过将变量的值设置为 null 来清空变量。

实例
cars=null;
person=null;

Symbol。

引用数据类型（对象类型）：

* 对象(Object)

JavaScript 对象是拥有属性和方法的数据。在 JavaScript中，几乎所有的事物都是对象。前面的变量就是一个对象。


对象属性

可以用键值对的方式来定义对象属性。

```
var person = {
    firstName:"John",
    lastName:"Doe",
    age:50,
    eyeColor:"blue"
};
```
对象键值对的写法类似于：PHP中的关联数组；Python中的字典；C 语言中的哈希表；Java中的哈希映射；Ruby 和 Perl 中的哈希表

访问对象的属性：

person.lastName;
或者
person["lastName"];

对象方法

对象的方法定义了一个函数，并作为对象的属性存储。

对象方法通过添加 () 调用 (作为一个函数)。

创建对象方法：

```
methodName : function() {
    // 代码 
}
```

你可以使用以下语法访问对象方法：

```
objectName.methodName()
```

对象由花括号分隔。在括号内部，对象的属性以名称和值对的形式 (name : value) 来定义。属性由逗号分隔：

var person={firstname:"John", lastname:"Doe", id:5566};

上面例子中的对象 (person) 有三个属性：firstname、lastname 以及 id。

空格和折行无关紧要。声明可横跨多行：

```javascript
var person={
firstname : "John",
lastname  : "Doe",
id        :  5566
};


name=person.lastname;
name=person["lastname"];
```
数组(Array)

创建名为 cars 的数组：
```
var cars=new Array();
cars[0]="Saab";
cars[1]="Volvo";
cars[2]="BMW";

或者 (condensed array):
var cars=new Array("Saab","Volvo","BMW");
```

函数(Function)

还有两个特殊的对象：

正则（RegExp）

日期（Date）



# 三、javascript类

# 四、javascript函数

JavaScript 函数是由事件驱动的或者当它被调用时执行的可重复使用的代码块。

JavaScript 函数语法

函数就是包裹在花括号中的代码块，前面使用了关键词 function：

```js
function functionname()
{
    // 执行代码
}
```
当调用该函数时，会执行函数内的代码。

可以在某事件发生时直接调用函数（比如当用户点击按钮时），并且可由 JavaScript 在任何位置进行调用。

JavaScript 对大小写敏感。关键词 function 必须是小写的，并且必须以与函数名称相同的大小写来调用函数。

带参数的函数
```
function myFunction(var1,var2)
{
代码
}
```

在调用函数时，可以向其传递值，这些值被称为参数。参数可以在函数中使用。
```
myFunction(argument1,argument2)
```


变量和参数必须以一致的顺序出现。第一个变量就是第一个被传递的参数的给定的值，以此类推。

```html
<p>点击这个按钮，来调用带参数的函数。</p>
<button onclick="myFunction('Harry Potter','Wizard')">点击这里</button>
<script>
function myFunction(name,job){
    alert("Welcome " + name + ", the " + job);
}
</script>
```

带有返回值的函数

有时，我们会希望函数将值返回调用它的地方。通过使用 return 语句就可以实现。

在使用 return 语句时，函数会停止执行，并返回指定的值。

```
function myFunction()
{
    var x=5;
    return x;
}
```
上面的函数会返回值 5。函数调用将被返回值取代
```
var myVar=myFunction();
myVar 变量的值是 5，也就是函数 "myFunction()" 所返回的值。

document.getElementById("demo").innerHTML=myFunction();
"demo" 元素的 innerHTML 将成为 5，也就是函数 "myFunction()" 所返回的值。
```

局部 JavaScript 变量

在 JavaScript 函数内部声明的变量（使用 var）是局部变量，所以只能在函数内部访问它。（该变量的作用域是局部的）。

您可以在不同的函数中使用名称相同的局部变量，因为只有声明过该变量的函数才能识别出该变量。

只要函数运行完毕，本地变量就会被删除。

全局 JavaScript 变量

在函数外声明的变量是全局变量，网页上的所有脚本和函数都能访问它。

JavaScript 变量的生存期

JavaScript 变量的生命期从它们被声明的时间开始。

局部变量会在函数运行以后被删除。

全局变量会在页面关闭后被删除。

向未声明的 JavaScript 变量分配值

如果您把值赋给尚未声明的变量，该变量将被自动作为 window 的一个属性。

这条语句：
```
carname="Volvo";
将声明 window 的一个属性 carname。
```
非严格模式下给未声明变量赋值创建的全局变量，是全局对象的可配置属性，可以删除。

## 事件

事件可以用于处理表单验证，用户输入，用户行为及浏览器动作:

页面加载时触发事件
页面关闭时触发事件
用户点击按钮执行动作
验证用户输入内容的合法性
等等 ...
可以使用多种方法来执行 JavaScript 事件代码：

HTML 事件属性可以直接执行 JavaScript 代码
HTML 事件属性可以调用 JavaScript 函数
你可以为 HTML 元素指定自己的事件处理程序
你可以阻止事件的发生。
等等 ...

|事件	|描述|
| ---- | ---- |
|onchange	|HTML 元素改变|
|onclick	|用户点击 HTML 元素|
|onmouseover|	鼠标指针移动到指定的元素上时发生|
|onmouseout	|用户从一个 HTML 元素上移开鼠标时发生|
|onkeydown	|用户按下键盘按键|
|onload	|浏览器已完成页面的加载|

## 运算符

算数运算符
y=5

|运算符	|描述|	例子|	x 运算结果|	y 运算结果	|
| ---- | ---- | ---- | ---- | ---- |
|+	|加法|	x=y+2|	7|	5|	
|-	|减法|	x=y-2|	3|	5|	
|*	|乘法|	x=y*2|	10|	5|	
|/	|除法|	|x=y/2	|2.5	|5|	
|%	|取模（余数）|	x=y%2	|1|	5|	
|++	|自增	|x=++y	|6	|6	|
|--	|自减	|x=--y	|4	|4|	

JavaScript 赋值运算符

赋值运算符用于给 JavaScript 变量赋值。

给定 x=10 和 y=5，下面的表格解释了赋值运算符：

|运算符|	例子|	等同于|	运算结果|	
| ---- | ---- | ---- | ---- |
|=	|x=y|	 	x=5| |	
|+=	|x+=y|	x=x+y	|x=15|	
|-=	|x-=y|	x=x-y	|x=5|	
|*=	|x*=y|	x=x*y	|x=50|	
|/=	|x/=y|	x=x/y	|x=2|	
|%=|	x%=y	|x=x%y	|x=0	|


用于字符串的 + 运算符

+ 运算符用于把文本值或字符串变量加起来（连接起来）。

如需把两个或多个字符串变量连接起来，请使用 + 运算符。

```
txt1="What a very";
txt2="nice day";
txt3=txt1+txt2;
txt3 运算结果如下:
What a verynice day
```



对字符串和数字进行加法运算

两个数字相加，返回数字相加的和，如果数字与字符串相加，返回字符串，如下实例：

```
x=5+5;
y="5"+5;
z="Hello"+5;
x,y, 和 z 输出结果为:

10
55
Hello5
```
条件运算符

|运算符	|描述	|比较	|返回值	|
| ---- | ---- | ---- | ---- | 
|==	|等于|	x==8|	false|	
|===	|绝对等于（值和类型均相等）|	x==="5"|	false|	
|!=	 |不等于|	x!=8|	true|	
|!==	| 不绝对等于（值和类型有一个不相等，或两个都不相等）|	x!=="5"|	true|	
|>	 |大于	|x>8	|false	|
|<	 |小于	|x<8	|true|	
|>=	 |大于或等于|	x>=8|	false	|
|<=	| 小于或等于|	x<=8|	true|

逻辑运算符

逻辑运算符用于测定变量或值之间的逻辑。

|运算符	|描述	|例子|
| ---- | ---- | ---- |
| && |	and	|(x < 10 && y > 1) 为 true|
|||	|or	|(x==5 || y==5) 为 false|
|!	|not|	!(x==y) 为 true|
（x = 6 y = 3)

## 顺序、条件和循环语句



# 五、DOM

# 六、BOM


