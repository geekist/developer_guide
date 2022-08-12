
* [一、CSS定义](#一css定义)
* [二、CSS语法](#二css语法)
* [三、CSS声明的位置](#三css声明的位置)
  * [3\.1 内联样式（在HTML元素中使用"style" 属性）](#31-内联样式在html元素中使用style-属性)
  * [3\.2 内部样式表（在HTML文档头部 &lt;head&gt; 区域使用&lt;style&gt; 元素 来包含CSS）](#32-内部样式表在html文档头部-head-区域使用style-元素-来包含css)
  * [3\.3 外部引用（使用外部 CSS 文件）](#33-外部引用使用外部-css-文件)
* [四、选择器](#四选择器)
  * [4\.1 元素选择器](#41-元素选择器)
  * [4\.2 id选择器](#42-id选择器)
  * [4\.3  class选择器](#43--class选择器)
  * [4\.4 复合选择器](#44-复合选择器)
  * [4\.5 群组选择器](#45-群组选择器)
  * [4\.6 通用选择器](#46-通用选择器)
  * [4\.7 多重样式](#47-多重样式)
  * [4\.8 后代选择器](#48-后代选择器)
  * [4\.9 伪类和伪元素](#49-伪类和伪元素)
  * [4\.10 继承](#410-继承)
  * [4\.11 多重样式优先级](#411-多重样式优先级)
* [五、CSS的属性](#五css的属性)
  * [5\.1 背景属性](#51-背景属性)
  * [5\.2 Text文本属性](#52-text文本属性)
  * [5\.3 字体](#53-字体)
  * [5\.4 表格属性](#54-表格属性)
  * [5\.5 盒子模型](#55-盒子模型)
    * [5\.5\.1 border属性](#551-border属性)
    * [5\.5\.2 轮廓outline](#552-轮廓outline)
    * [5\.5\.3 外边距margin](#553-外边距margin)
    * [5\.5\.4 填充padding](#554-填充padding)
  * [5\.6 尺寸属性](#56-尺寸属性)
  * [5\.7 CSS Display(显示) 与 Visibility（可见性）](#57-css-display显示-与-visibility可见性)
  * [5\.8 Position(定位)](#58-position定位)
  * [5\.9 overflow属性](#59-overflow属性)
  * [5\.10 Float(浮动)属性](#510-float浮动属性)
  * [5\.11 对齐属性](#511-对齐属性)

# 一、CSS定义

CSS 指层叠样式表 (Cascading Style Sheets)。CSS样式定义如何显示 HTML 元素。CSS是为了解决内容与表现分离的问题。外部样式表可以极大提高工作效率外部样式表通常存储在 CSS 文件中。

css可以用来为网页创建样式表，通过样式表可以对网页进行装饰。所谓层叠，可以将整个网页想象成是一层一层的结构，层次高的将会覆盖层次低的。而css就可以分别为网页的各个层次设置样式。

# 二、CSS语法

CSS 规则由两个主要的部分构成：选择器，以及一条或多条声明。

选择器：

通常是您需要改变样式的`HTML 元素`。

声明：

每条声明由一个属性和一个值组成。多个声明之间用分号隔开。

属性（property）是您希望设置的样式属性（style attribute）。每个属性有一个值。属性和值被冒号分开。

```
选择器 {样式名:样式值；样式名:样式值 ; }
```
例如：

```
p {color:red ; font-size:12px;}
```

![](./assets/css_0.jpg)

```css
p
{
    color:red;
    text-align:center;
}
```
CSS声明总是以分号 ; 结束，声明总以大括号 {} 括起来.

# 三、CSS声明的位置

CSS 可以通过以下方式添加到HTML中:

## 3.1 内联样式（在HTML元素中使用"style" 属性）

当特殊的样式需要应用到个别元素时，就可以使用内联样式。 使用内联样式的方法是在相关的标签中使用样式属性。样式属性可以包含任何 CSS 属性。以下实例显示出如何改变段落的颜色和左外边距。

```html
<p style="color:blue;margin-left:20px;">这是一个段落。</p>
```

## 3.2 内部样式表（在HTML文档头部 `<head>` 区域使用`<style>` 元素 来包含CSS）

当单个文件需要特别样式时，就可以使用内部样式表。你可以在<head> 部分通过 <style>标签定义内部样式表:

```html
<head>
<style type="text/css">

body {background-color:yellow;}

p {color:blue;}

</style>

</head>
```

## 3.3 外部引用（使用外部 CSS 文件）

当样式需要被应用到很多页面的时候，外部样式表将是理想的选择。使用外部样式表，你就可以通过更改一个文件来改变整个站点的外观。

```html
<head>
<link rel="stylesheet" type="text/css" href="mystyle.css">
</head>
```
# 四、选择器

选择器（selector），会告诉浏览器：网页上的哪些元素需要设置什么样的样式。比如：p这个选择器就表示选择页面中的所有的p元素，在选择器之后所设置的样式会应用到所有的p元素上。


## 4.1 元素选择器

元素选择器（标签选择器），可以根据标签的名字来从页面中选取指定的元素。

语法：

```
标签名 {}
```
比如p则会选中页面中的所有p标签，h1会选中页面中的所有h1标签。

## 4.2 id选择器

id选择器可以根据元素的id属性值选取元素。可以为标有特定 id 的 HTML 元素指定特定的样式。

HTML元素以id属性来设置id选择器,CSS 中 id 选择器以 "#" 来定义。

语法：

```
#id { }
```

比如#box会选中页面中id属性值为box的元素，和class属性不同，id属性是不能重复的。

以下的样式规则应用于元素属性 id="para1":

```css
#para1
{
    text-align:center;
    color:red;
}
```
## 4.3  class选择器

class 选择器用于描述一组元素的样式，class 选择器有别于id选择器，class可以在多个元素中使用。

class 选择器在 HTML 中以 class 属性表示, 在 CSS 中，类选择器以一个点 . 号显示：

语法：

```css
.className { }
```

在以下的例子中，所有拥有 center 类的 HTML 元素均为居中。

```css
.center {
    text-align:center;
}
```

也可以指定特定的 HTML 元素使用 class。

在以下实例中, 所有的 p 元素使用 class="center" 让该元素的文本居中:
```css
p.center {text-align:center;}
```

多个 class 选择器可以使用空格分开：
```css
.center { text-align:center; }
.color { color:#ff0000; }
```

## 4.4 复合选择器

复合选择器，可以同时使用多个选择器，这样可以选择同时满足多个选择器的元素。

语法：
```
选择器1.选择器2{}
```

例如div.box1会选中页面中具有box1这个class的div元素。

## 4.5 群组选择器

群组选择器，可以同时使用多个选择器，多个选择器将被同时应用指定的样式。

语法：

```
选择器1,选择器2,选择器3 { }
```
比如p,.hello,#box会同时选中页面中p元素，class为hello的元素，id为box的元素。

## 4.6 通用选择器

通用选择器，可以同时选中页面中的所有元素。

语法：
```css
*{ }
```

## 4.7 多重样式

如果某些属性在不同的样式表中被同样的选择器定义，那么属性值将从更具体的样式表中被继承过来。 

例如，外部样式表拥有针对 h3 选择器的三个属性：
```css
h3
{
    color:red;
    text-align:left;
    font-size:8pt;
}

```
而内部样式表拥有针对 h3 选择器的两个属性：

```css
h3
{
    text-align:right;
    font-size:20pt;
}
```

假如拥有内部样式表的这个页面同时与外部样式表链接，那么 h3 得到的样式是：

```css
color:red;
text-align:right;
font-size:20pt;
```

即颜色属性将被继承于外部样式表，而文字排列（text-alignment）和字体尺寸（font-size）会被内部样式表中的规则取代。

## 4.8 后代选择器

html标签之间的关系

![](./assets/css_05.png)

* 祖先元素 直接或间接包含后代元素的元素。

* 后代元素 直接或间接被祖先元素包含的元素。

* 父元素 直接包含子元素的元素。

* 子元素 直接被父元素包含的元素。

* 兄弟元素 拥有相同父元素的元素。

后代选择器可以根据标签的关系，为处在元素内部的代元素设置样式。

语法：
```
祖先元素 后代元素 后代元素 { }
```

• 比如p strong会选中页面中所有的p元素内的strong元素。

## 4.9 伪类和伪元素

有时候，你需要选择本身没有标签，但是仍然易于识别的网页部位，比如段落首行或鼠标滑过的连接。CSS为他们提供一些选择器：伪类和伪元素。

有四个伪类可以让你根据访问者与该链接的交互方式，将链接设置成4种不同的状态。

a:link - 正常，未访问过的链接

a:visited - 用户已访问过的链接

a:hover - 当用户鼠标放在链接上时

a:active - 链接被点击的那一刻

```css
a:link {color:#000000;}      /* 未访问链接*/
a:visited {color:#00FF00;}  /* 已访问链接 */
a:hover {color:#FF00FF;}  /* 鼠标移动到链接上 */
a:active {color:#0000FF;}  /* 鼠标点击时 */
```

文本修饰
text-decoration 属性主要用于删除链接中的下划线：

```css
a:link {text-decoration:none;}
a:visited {text-decoration:none;}
a:hover {text-decoration:underline;}
a:active {text-decoration:underline;}
```

```css
a:link {background-color:#B2FF99;}
a:visited {background-color:#FFFF85;}
a:hover {background-color:#FF704D;}
a:active {background-color:#FF704D;}
```

## 4.10 继承

就像父亲的财产会遗传给儿子一样，在CSS中祖先元素的样式同样也会被子元素继承。

继承是指应用在一个标签上的那些CSS样式会同时被应用到其内嵌标签上。比如为父元素设置了字体颜色，子元素也会应用上相同的颜色。当然并不是所有的样式都会被继承，这一点我们讲到具体样式时，再去讨论。


## 4.11 多重样式优先级

样式表允许以多种方式规定样式信息。样式可以规定在单个的 HTML 元素中，在 HTML 页的头元素中，或在一个外部的 CSS 文件中。甚至可以在同一个 HTML 文档内部引用多个外部样式表。

一般情况下，优先级如下：

**（内联样式）Inline style > （内部样式）Internal style sheet >（外部样式）External style sheet > 浏览器默认样式**

**内联样式 > id 选择器 > 类选择器 = 伪类选择器 = 属性选择器 > 标签选择器 = 伪元素选择器**

# 五、CSS的属性

## 5.1 背景属性

|Property	|描述  |
|---- | ---- |
|background	|简写属性，作用是将背景属性设置在一个声明中。|
|background-attachment	|背景图像是否固定或者随着页面的其余部分滚动。|
|background-color	|设置元素的背景颜色。|
|background-image	|把图像设置为背景。|
|background-position	|设置背景图像的起始位置。|
|background-repeat	|设置背景图像是否及如何重复。|

* 背景颜色
  
background-color 属性定义了元素的背景颜色.页面的背景颜色使用在body的选择器中:
```
body {background-color:#b0c4de;}
```

十六进制 - 如："#ff0000"
RGB - 如："rgb(255,0,0)"
颜色名称 - 如："red"


* 背景图像 background-image

background-image 属性描述了元素的背景图像.默认情况下，背景图像进行平铺重复显示，以覆盖整个元素实体.

```
body {background-image:url('paper.gif');}
```

* 背景图像 - 水平或垂直平铺 repeat

默认情况下 background-image 属性会在页面的水平或者垂直方向平铺。

一些图像如果在水平方向与垂直方向平铺，这样看起来很不协调，如下所示: 

```css
body
{
background-image:url('gradient2.png');
background-repeat:repeat-x;
}
```


* 图像位置 background-position 属性改变图像在背景中的位置:
  
```css
body
{
background-image:url('img_tree.png');
background-repeat:no-repeat;
background-position:right top;
}
```

* 背景- 简写属性
  
在以上实例中我们可以看到页面的背景颜色通过了很多的属性来控制。

为了简化这些属性的代码，我们可以将这些属性合并在同一个属性中.

背景颜色的简写属性为 "background":

```css
body {background:#ffffff url('img_tree.png') no-repeat right top;}
```

当使用简写属性时，属性值的顺序为：:

background-color
background-image
background-repeat
background-attachment
background-position
以上属性无需全部使用，可以按照页面的实际需要使用.

## 5.2 Text文本属性

|属性	|描述|
| ---- | ---- |
|color	|设置文本颜色|
|direction	|设置文本方向。|
|letter-spacing|	设置字符间距|
|line-height	|设置行高|
|text-align	|对齐元素中的文本|
|text-decoration	|向文本添加修饰|
|text-indent	|缩进元素中文本的首行|
|text-shadow	|设置文本阴影|
|text-transform	|控制元素中的字母|
|unicode-bidi	|设置或返回文本是否被重写| 
|vertical-align	|设置元素的垂直对齐|
|white-space	|设置元素中空白的处理方式|
|word-spacing	|设置字间距|

* 文本颜色
  
颜色属性被用来设置文字的颜色。

```css
body {color:red;}
h1 {color:#00ff00;}
h2 {color:rgb(255,0,0);}
```
Remark 对于W3C标准的CSS：如果你定义了颜色属性，你还必须定义背景色属性。

* 文本的对齐方式

文本排列属性是用来设置文本的水平对齐方式。

文本可居中或对齐到左或右,两端对齐.

当text-align设置为"justify"，每一行被展开为宽度相等，左，右外边距是对齐（如杂志和报纸）。

```css
h1 {text-align:center;}
p.date {text-align:right;}
p.main {text-align:justify;}
```
* 文本修饰

text-decoration 属性用来设置或删除文本的装饰。

从设计的角度看 text-decoration属性主要是用来删除链接的下划线：

```css
a {text-decoration:none;}

h1 {text-decoration:overline;}
h2 {text-decoration:line-through;}
h3 {text-decoration:underline;}
```
不建议强调指出不是链接的文本，因为这常常混淆用户。

* 文本转换

文本转换属性是用来指定在一个文本中的大写和小写字母。

可用于所有字句变成大写或小写字母，或每个单词的首字母大写。

```css
p.uppercase {text-transform:uppercase;}
p.lowercase {text-transform:lowercase;}
p.capitalize {text-transform:capitalize;}
```
* 文本缩进

文本缩进属性是用来指定文本的第一行的缩进。

```css
p {text-indent:50px;}
```

## 5.3 字体

|属性    |描述|
| ---- | ---- |
|font	|在一个声明中设置所有的字体属性|
|font-family|	指定文本的字体系列|
|font-size	|指定文本的字体大小|
|font-style	|指定文本的字体样式|
|font-variant|	以小型大写字体或者正常字体显示文本。|
|font-weight|	指定字体的粗细。|

* 字体系列

font-family 属性设置文本的字体系列。多个字体系列是逗号分隔指明：

注意: 如果字体系列的名称超过一个字，它必须用引号，如Font Family："宋体"。font-family 属性应该设置几个字体名称作为一种"后备"机制，如果浏览器不支持第一种字体，他将尝试下一种字体。

```css
p{font-family:"Times New Roman", Times, serif;}
```


* 字体样式
  
主要是用于指定斜体文字的字体样式属性。

这个属性有三个值：

正常 - 正常显示文本
斜体 - 以斜体字显示的文字
倾斜的文字 - 文字向一边倾斜（和斜体非常类似，但不太支持）

```css
p.normal {font-style:normal;}
p.italic {font-style:italic;}
p.oblique {font-style:oblique;}
```

* 字体大小

font-size 属性设置文本的大小。

能否管理文字的大小，在网页设计中是非常重要的。但是，你不能通过调整字体大小使段落看上去像标题，或者使标题看上去像段落。

请务必使用正确的HTML标签，就`<h1>` - `<h6>`表示标题和`<p>`表示段落：

字体大小的值可以是绝对或相对的大小。

绝对大小：

设置一个指定大小的文本
不允许用户在所有浏览器中改变文本大小
确定了输出的物理尺寸时绝对大小很有用
相对大小：

相对于周围的元素来设置大小
允许用户在浏览器中改变文字大小
Remark 如果你不指定一个字体的大小，默认大小和普通文本段落一样，是16像素（16px=1em）。

设置字体大小像素
设置文字的大小与像素，让您完全控制文字大小：

```css
h1 {font-size:40px;}
h2 {font-size:30px;}
p {font-size:14px;}
```

虽然可以通过浏览器的缩放工具调整文本大小，但是，这种调整是整个页面，而不仅仅是文本

用em来设置字体大小

为了避免Internet Explorer 中无法调整文本的问题，许多开发者使用 em 单位代替像素。

em的尺寸单位由W3C建议。

1em和当前字体大小相等。在浏览器中默认的文字大小是16px。

因此，1em的默认大小是16px。可以通过下面这个公式将像素转换为em：px/16=em

```css
h1 {font-size:2.5em;} /* 40px/16=2.5em */
h2 {font-size:1.875em;} /* 30px/16=1.875em */
p {font-size:0.875em;} /* 14px/16=0.875em */
```

在上面的例子，em的文字大小是与前面的例子中像素一样。不过，如果使用 em 单位，则可以在所有浏览器中调整文本大小。


使用百分比和EM组合

在所有浏览器的解决方案中，设置 `<body>`元素的默认字体大小的是百分比：

```css
body {font-size:100%;}
h1 {font-size:2.5em;}
h2 {font-size:1.875em;}
p {font-size:0.875em;}
```



## 5.4 表格属性

* 表格边框

指定CSS表格边框，使用border属性。


```css
table, th, td
{
    border: 1px solid black;
}
```

* 折叠边框

border-collapse 属性设置表格的边框是否被折叠成一个单一的边框或隔开：

```css
table
{
    border-collapse:collapse;
}
table,th, td
{
    border: 1px solid black;
}
```


* 表格宽度和高度

Width和height属性定义表格的宽度和高度。

下面的例子是设置100％的宽度，50像素的th元素的高度的表格：

```css
table 
{
    width:100%;
}
th
{
    height:50px;
}
```


* 表格文字对齐text-align

表格中的文本对齐和垂直对齐属性。

text-align属性设置水平对齐方式，向左，右，或中心：

```css
td
{
    text-align:right;
}
```

* 垂直对齐属性

设置垂直对齐，比如顶部，底部或中间：

```css
td
{
    height:50px;
    vertical-align:bottom;
}
```

* 表格填充

如需控制边框和表格内容之间的间距，应使用td和th元素的填充属性：

实例
td
{
    padding:15px;
}


* 表格颜色

下面的例子指定边框的颜色，和th元素的文本和背景颜色：

```
table, td, th
{
    border:1px solid green;
}
th
{
    background-color:green;
    color:white;
}
```

 ## 5.5 盒子模型

 所有HTML元素可以看作盒子，在CSS中，"box model"这一术语是用来设计和布局时使用。

CSS盒模型本质上是一个盒子，封装周围的HTML元素，它包括：边距，边框，填充，和实际内容。

盒模型允许我们在其它元素和周围元素边框之间的空间放置元素。

下面的图片说明了盒子模型(Box Model)：

![](./assets/css_1.gif)

Content(内容) - 盒子的内容，显示文本和图像。

Padding(内边距) - 清除内容周围的区域，内边距是透明的。

Border(边框) - 围绕在内边距和内容外的边框。

Margin(外边距) - 清除边框外的区域，外边距是透明的。

当我们指定一个 CSS 元素的宽度和高度属性时，你只是设置内容区域的宽度和高度。要知道，完整大小的元素，你还必须添加内边距，边框和外边距。

```css
div {
    width: 220px;
    padding: 10px;
    border: 5px solid gray;
    margin: 0; 
}
```
总元素的宽度=宽度+左填充+右填充+左边框+右边框+左边距+右边距

总元素的高度=高度+顶部填充+底部填充+上边框+下边框+上边距+下边距

### 5.5.1 border属性

|属性	|描述|
| ---- | ---- |
|border	|简写属性，用于把针对四个边的属性设置在一个声明。|
|border-style	|用于设置元素所有边框的样式，或者单独地为各边设置边框样式。|
|border-width	|简写属性，用于为元素的所有边框设置宽度，或者单独地为各边边框设置宽度。|
|border-color	|简写属性，设置元素的所有边框中可见部分的颜色，或为 4 个边分别设置颜色。|
|border-bottom	|简写属性，用于把下边框的所有属性设置到一个声明中。|
|border-bottom-color	|设置元素的下边框的颜色。|
|border-bottom-style	|设置元素的下边框的样式。|
|border-bottom-width	|设置元素的下边框的宽度。|
|border-left	|简写属性，用于把左边框的所有属性设置到一个声明中。|
|border-left-color	|设置元素的左边框的颜色。|
|border-left-style	|设置元素的左边框的样式。|
|border-left-width	|设置元素的左边框的宽度。|
|border-right	|简写属性，用于把右边框的所有属性设置到一个声明中。|
|border-right-color	|设置元素的右边框的颜色。|
|border-right-style	|设置元素的右边框的样式。|
|border-right-width	|设置元素的右边框的宽度。|
|border-top	|简写属性，用于把上边框的所有属性设置到一个声明中。|
|border-top-color	|设置元素的上边框的颜色。|
|border-top-style	|设置元素的上边框的样式。|
|border-top-width	|设置元素的上边框的宽度。|
|border-radius	|设置圆角的边框。|

### 5.5.2 轮廓outline

轮廓（outline）是绘制于元素周围的一条线，位于边框边缘的外围，可起到突出元素的作用。

轮廓（outline）属性指定元素轮廓的样式、颜色和宽度。

所有CSS 轮廓（outline）属性
"CSS" 列中的数字表示哪个CSS版本定义了该属性(CSS1 或者CSS2)。

|属性	|说明	|值	|CSS|
| ---- | ---- | ---- | ---- |
|outline	|在一个声明中设置所有的轮廓属性|	outline-color，outline-style，outline-width，inherit|	2|
|outline-color	|设置轮廓的颜色	|color-name，hex-number，rgb-number，invert，inherit|	2|
|outline-style	|设置轮廓的样式	|none，dotted，dashed，solid，double，groove，ridge，inset，outset，inherit	|2|
|outline-width	|设置轮廓的宽|thin，medium，thick，length，inherit|	2|


### 5.5.3 外边距margin

CSS margin(外边距)属性定义元素周围的空间。

![](./assets/css_03.png)

|属性	|描述|
| ---- | ---- |
|margin	|简写属性。在一个声明中设置所有外边距属性。|
|margin-bottom	|设置元素的下外边距。|
|margin-left	|设置元素的左外边距。|
|margin-right	|设置元素的右外边距。|
|margin-top	|设置元素的上外边距。|

### 5.5.4 填充padding

CSS padding（填充）是一个简写属性，定义元素边框与元素内容之间的空间，即上下左右的内边距。
![](./assets/css_04.png)

|属性	|说明|
| ---- | ---- |
|padding	|使用简写属性设置在一个声明中的所有填充属性|
|padding-bottom	|设置元素的底部填充|
|padding-left	|设置元素的左部填充|
|padding-right	|设置元素的右部填充|
|padding-top	|设置元素的顶部填充|


## 5.6 尺寸属性

|属性	|描述|
| ---- | ---- |
| height	|设置元素的高度。|
| line-height |	设置行高。|
| max-height	 |设置元素的最大高度。|
|max-width	|设置元素的最大宽度。|
|min-height	|设置元素的最小高度。|
|min-width	|设置元素的最小宽度。|
|width	|设置元素的宽度。|

## 5.7 CSS Display(显示) 与 Visibility（可见性）


display属性设置一个元素应如何显示，visibility属性指定一个元素应可见还是隐藏。


* 隐藏元素 display:none 或 visibility:hidden

隐藏一个元素可以通过把display属性设置为"none"，或把visibility属性设置为"hidden"。但是请注意，这两种方法会产生不同的结果。

visibility:hidden可以隐藏某个元素，但隐藏的元素仍需占用与未隐藏之前一样的空间。也就是说，该元素虽然被隐藏了，但仍然会影响布局。

display:none可以隐藏某个元素，且隐藏的元素不会占用任何空间。也就是说，该元素不但被隐藏了，而且该元素原本占用的空间也会从页面布局中消失。

## 5.8 Position(定位)


position 属性指定了元素的定位类型。

position 属性的五个值：



static 定位 元素的默认值，即没有定位，遵循正常的文档流对象。静态定位的元素不会受到 top, bottom, left, right影响。

fixed 定位  元素的位置相对于浏览器窗口是固定位置。 即使窗口是滚动的它也不会移动：

relative 定位  相对定位元素的定位是相对其正常位置。移动相对定位元素，但它原本所占的空间不会改变。

absolute 定位 绝对定位的元素的位置相对于最近的已定位父元素，absolute 定位使元素的位置与文档流无关，因此不占据空间。

absolute 定位的元素和其他元素重叠。

sticky 定位 英文字面意思是粘，粘贴，所以可以把它称之为粘性定位。基于用户的滚动位置来定位。

## 5.9 overflow属性

CSS overflow 属性可以控制内容溢出元素框时在对应的元素区间内添加滚动条。

overflow属性有以下值：

|值	|描述|
| ---- | ---- |
|visible	|默认值。内容不会被修剪，会呈现在元素框之外。|
|hidden	|内容会被修剪，并且其余内容是不可见的。|
|scroll	|内容会被修剪，但是浏览器会显示滚动条以便查看其余的内容。|
|auto	|如果内容被修剪，则浏览器会显示滚动条以便查看其余的内容。|
|inherit	|规定应该从父元素继承 overflow 属性的值。|


## 5.10 Float(浮动)属性

CSS 的 Float（浮动），会使元素向左或向右移动，其周围的元素也会重新排列。

Float（浮动），往往是用于图像，但它在布局时一样非常有用。

元素的水平方向浮动，意味着元素只能左右移动而不能上下移动。一个浮动元素会尽量向左或向右移动，直到它的外边缘碰到包含框或另一个浮动框的边框为止。浮动元素之后的元素将围绕它。浮动元素之前的元素将不会受到影响。如果图像是右浮动，下面的文本流将环绕在它左边：

```css
img
{
    float:right;
}
```

清除浮动 - 使用 clear

元素浮动之后，周围的元素会重新排列，为了避免这种情况，使用 clear 属性。

clear 属性指定元素两侧不能出现浮动元素。

使用 clear 属性往文本中添加图片廊：

```
.text_line
{
    clear:both;
}
```

## 5.11 对齐属性

* 元素居中对齐

要水平居中对齐一个元素(如 `<div>`), 可以使用 margin: auto;。

设置到元素的宽度将防止它溢出到容器的边缘。

元素通过指定宽度，并将两边的空外边距平均分配：

div 元素是居中的

```css
.center {
    margin: auto;
    width: 50%;
    border: 3px solid green;
    padding: 10px;
}
```
尝试一下 »
注意: 如果没有设置 width 属性(或者设置 100%)，居中对齐将不起作用。

* 文本居中对齐

如果仅仅是为了文本在元素内居中对齐，可以使用 text-align: center;

```css
.center {
    text-align: center;
    border: 3px solid green;
}
```

* 图片居中对齐

要让图片居中对齐, 可以使用 margin: auto; 并将它放到 块 元素中:

```css
img {
    display: block;
    margin: auto;
    width: 40%;
}
```


* 左右对齐 - 使用定位方式

我们可以使用 position: absolute; 属性来对齐元素:

```css
.right {
    position: absolute;
    right: 0px;
    width: 300px;
    border: 3px solid #73AD21;
    padding: 10px;
}
```



