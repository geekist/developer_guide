
- [一、HTML介绍](#一html介绍)
- [二、HTML基本概念：](#二html基本概念)
  - [2.1 HTML文档](#21-html文档)
  - [2.2 标签](#22-标签)
  - [2.3 属性和值](#23-属性和值)
  - [2.4 元素](#24-元素)
- [三、HTML元素](#三html元素)
  - [标题`<h1>-<h6>`](#标题h1-h6)
  - [水平线`<hr>`](#水平线hr)
  - [注释](#注释)
  - [段落`<p>`](#段落p)
  - [换行`<br>`](#换行br)
  - [文本格式化标签](#文本格式化标签)
  - [字符实体](#字符实体)
  - [超链接`<a>`](#超链接a)
  - [头部元素`<head>`](#头部元素head)
  - [css](#css)
  - [图像`<img>`](#图像img)
  - [表格`<table>`](#表格table)
  - [列表](#列表)
  - [区块元素div](#区块元素div)
  - [内联元素`<span>`](#内联元素span)
  - [表单`<form>`](#表单form)
  - [框架`<iframe>`](#框架iframe)
  - [颜色](#颜色)
- [四、html5新元素](#四html5新元素)
  - [画布Canvas](#画布canvas)
  - [矢量图SVG](#矢量图svg)
  - [math](#math)
  - [拖放（drag and drop）](#拖放drag-and-drop)
  - [Geolocation（地理定位）](#geolocation地理定位)
  - [Video](#video)
  - [audio](#audio)
  - [input](#input)
  - [表单新元素](#表单新元素)
  - [websocket](#websocket)


# 一、HTML介绍

HTML（HyperText Markup Language，超文本标记语言） 是一种描述语言，用来定义网页结构。

1990 年，由于对 Web 未来发展的远见，Tim Berners-Lee 提出了 超文本 的概念，并在第二年在 SGML (en-US) 的基础上将其正式定义为一个标记语言。IETF (en-US) 于 1993 年正式开始制定 HTML 规范，并在经历了几个版本的草案后于 1995 年发布了 HTML 2.0 版本。1994 年，Berners-Lee 为了 Web 发展而成立了 W3C (en-US)。1996 年，W3C 接管了 HTML 的标准化工作，并在 1 年后发布了 HTML 3.2 推荐标准。1999 年，HTML 4.0 发布，并在 2000 年成为 ISO (en-US) 标准。

自那以后，W3C 几乎放弃了 HTML 而转向 XHTML，并于 2004 年成立了另一个独立的小组 WHATWG。幸运的是，WHATWG 后来又转回来参与了 HTML5 标准的制定，两个组织（译注：即 W3C 和 WHATWG）在 2008 年发布了第一份草案，并在 2014 年发布了 HTML5 标准的最终版。

# 二、HTML基本概念：

## 2.1 HTML文档

一个简单的html文档样式如下：

![](../assets/../../assets/html_0.jpg)

HTML 文档是包含多个 HTML 元素 的文本文档。每个元素都用一对开始和结束标签包裹。每个标签以尖括号（<>）开始和结束。也有一部分标签中不需要包含文本，这些标签称为空标签，如 <img>。

HTML 文件通常会以 .htm 或 .html 为扩展名。用户可以从 Web 服务器 中下载，并使用任一 Web 浏览器 来解析和显示。

## 2.2 标签

HTML 标记标签通常被称为 HTML 标签 (HTML tag)。

HTML 标签是由尖括号包围的关键词，比如 `<html>` 标签对中的第一个标签是开始标签，第二个标签是结束标签

HTML 标签通常是成对出现的，比如 `<b>` 和 `</b>`.

## 2.3 属性和值

HTML属性是HTML元素提供的附加信息。

属性可以在元素中添加附加信息，属性一般描述于开始标签，总是以名称/值对的形式出现，比如：name="value"。

例如：
HTML 链接由 <a> 标签定义。链接的地址在 href 属性中指定：
```html
<a href="http://www.runoob.com">这是一个链接</a>
```

属性值应该始终被包括在引号内,双引号是最常用的，不过使用单引号也没有问题。在某些个别的情况下，比如属性值本身就含有双引号，那么您必须使用单引号，例如：name='John "ShotGun" Nelson'

属性和属性值对大小写不敏感。不过，万维网联盟在其 HTML 4 推荐标准中推荐小写的属性/属性值。而新版本的 (X)HTML 要求使用小写属性。

id属性

id属性作为标签的唯一标识，在同一个网页中不能出现相同的id属性值。

class属性

class属性用来为标签分组，拥有相同class属性的标签我们认为就是一组，可以出现相同的class属性，可以为一个元素指定多个class。

## 2.4 元素 

元素指标签和标签包含的内容、属性等。

![](../assets/../../assets/html_1.png)

开始标签（Opening tag）：包含元素的名称（本例为 p），被左、右角括号所包围。表示元素从这里开始或者开始起作用 —— 在本例中即段落由此开始。

结束标签（Closing tag）：与开始标签相似，只是其在元素名之前包含了一个斜杠。这表示着元素的结尾 —— 在本例中即段落在此结束。初学者常常会犯忘记包含结束标签的错误，这可能会产生一些奇怪的结果。

内容（Content）：元素的内容，本例中就是所输入的文本本身。

元素（Element）：开始标签、结束标签与内容相结合，便是一个完整的元素。


# 三、HTML元素

## 标题`<h1>-<h6>`

标题（Heading）是通过 `<h1> - <h6>` 标签进行定义的。`<h1>` 定义最大的标题。 `<h6>` 定义最小的标题。搜索引擎使用标题为您的网页的结构和内容编制索引。浏览器会自动地在标题的前后添加空行。

```html
<h1>这是一个标题。</h1>
<h2>这是一个标题。</h2>
<h3>这是一个标题。</h3>
```

## 水平线`<hr>`

`<hr>` 标签在 HTML 页面中创建水平线。可用于分隔内容。

```html
<p>这是一个段落。</p>
<hr>
<p>这是一个段落。</p>
<hr>
<p>这是一个段落。</p>
```

## 注释

可以将注释插入 HTML 代码中，这样可以提高其可读性，使代码更易被人理解。浏览器会忽略注释，也不会显示它们。

注释写法如下:
```html
<!-- 这是一个注释 -->
```

## 段落`<p>`

段落是通过 `<p>` 标签定义的,可以将文档分割为若干段落。浏览器会自动地在段落的前后添加空行

```html
<p>这是一个段落 </p>
<p>这是另一个段落</p>
```
## 换行`<br>`

如果您希望在不产生一个新段落的情况下进行换行（新行），请使用 `<br>` 标签： `<br/>` 元素是一个空的 HTML 元素。由于关闭标签没有任何意义，因此它没有结束标签。

```html
<p>这个<br>段落<br>演示了分行的效果</p>
```

## 文本格式化标签

HTML 文本格式化标签

| 标签       | 描述         |
| ---------- | ------------ |
| `<b>`      | 定义粗体文本 |
| `<em>`     | 定义着重文字 |
| `<i>`      | 定义斜体字   |
| `<small>`  | 定义小号字   |
| `<strong>` | 定义加重语气 |
| `<sub>`    | 定义下标字   |
| `<sup>`    | 定义上标字   |
| `<ins>`    | 定义插入字   |
| `<del>`    | 定义删除字   |

HTML "计算机输出" 标签

| 标签     | 描述               |
| -------- | ------------------ |
| `<code>` | 定义计算机代码     |
| `<kbd>`  | 定义键盘码         |
| `<samp>` | 定义计算机代码样本 |
| `<var>`  | 定义变量           |
| `<pre>`  | 定义预格式文本     |

HTML 引文, 引用, 及标签定义

| 标签           | 描述               |
| -------------- | ------------------ |
| `<abbr>`       | 定义缩写           |
| `<address>`    | 定义地址           |
| `<bdo>`        | 定义文字方向       |
| `<blockquote>` | 定义长的引用       |
| `<q>`          | 定义短的引用语     |
| `<cite>`       | 定义引用、引证     |
| `<dfn>`        | 定义一个定义项目。 |

## 字符实体

在HTML中预留了一些字符。这些预留字符是不能在网页中直接使用的。比如<和>,我们不能直接在页面中使用<和>号，因为浏览器会将它解析为html标签。为了可以使用这些预留字符，我们必须在html中使用字符实体。

语法: 

&实体名;

| 预留字符  | 实体字符  |
| --------- | --------- |
| 小于号<   | `&lt;`    |
| 大于号>   | `&gt;`    |
| 空格      | `&nbsp;`  |
| 和符号&   | `&amp;`   |
| 版权©     | `&copy;`  |
| 引号”     | `&quot;`  |
| 注册商标® | `&reg;`   |
| 商标™     | `&trade;` |

## 超链接`<a>`

HTML使用标签 <a>来设置超文本链接。超链接可以是一个字，一个词，或者一组词，也可以是一幅图像，您可以点击这些内容来跳转到新的文档或者当前文档中的某个部分。当您把鼠标指针移动到网页中的某个链接上时，箭头会变为一只小手。

在标签<a> 中使用了href属性来描述链接的地址。

默认情况下，链接将以以下形式出现在浏览器中：

* 一个未访问过的链接显示为蓝色字体并带有下划线。

* 访问过的链接显示为紫色并带有下划线。

* 点击链接时，链接显示为红色并带有下划线。

* 如果为这些超链接设置了CSS样式，展示样式会根据 CSS 的设定而显示。

HTML 链接语法

```html
<a href="url">链接文本</a>
```
链接属性：

href 属性描述了链接的目标。

target 属性定义被链接的文档在何处显示。可选值：_blank新窗口、_self当前窗口。

id 属性用于创建一个HTML文档书签。（书签不会以任何特殊方式显示，即在 HTML 页面中是不显示的，所以对于读者来说是隐藏的）

```html
在新窗口打开文档：
<a href="https://www.runoob.com/" target="_blank" rel="noopener noreferrer">访问菜鸟教程!</a>

在HTML文档中插入ID:
<a id="tips">有用的提示部分</a>

在HTML文档中创建一个链接到"有用的提示部分(id="tips"）"：
<a href="#tips">访问有用的提示部分</a>

从另一个页面创建一个链接到"有用的提示部分(id="tips"）"：
<a href="https://www.runoob.com/html/html-links.html#tips">访问有用的提示部分</a>
```

请始终将正斜杠添加到子文件夹。假如这样书写链接：href="https://www.runoob.com/html"，就会向服务器产生两次 HTTP 请求。这是因为服务器会添加正斜杠到这个地址，然后创建一个新的请求，就像这样：href="https://www.runoob.com/html/"。

## 头部元素`<head>`

`<head>` 元素包含了所有的头部标签元素。在 `<head>`元素可以插入脚本（scripts）, 样式文件（CSS），及各种meta信息。

可以添加在头部区域的元素标签为: `<title>, <style>, <meta>, <link>, <script>, <noscript> 和 <base>`。

| 标签       | 描述                               |
| ---------- | ---------------------------------- |
| `<head>`   | 定义了文档的信息                   |
| `<title>`  | 定义了文档的标题                   |
| `<base>`   | 定义了页面链接标签的默认链接地址   |
| `<link>`   | 定义了一个文档和外部资源之间的关系 |
| `<meta>`   | 定义了HTML文档中的元数据           |
| `<script>` | 定义了客户端的脚本文件             |
| `<style>`  | 定义了HTML文档的样式文件           |


```html

<!DOCTYPE html>
<html>
<head> 
<meta charset="utf-8"> 
<title>文档标题</title>
</head>
 
<body>
文档内容......
</body>
 
</html>
```

 
```html
<head>

base标签描述了基本的链接地址/链接目标，该标签作为HTML文档中所有的链接标签的默认链接:
<base href="http://www.runoob.com/images/" target="_blank">


link标签定义了文档与外部资源之间的关系。标签通常用于链接到样式表:
<link rel="stylesheet" type="text/css" href="mystyle.css">

style标签定义了HTML文档的样式文件引用地址.
style元素中也可以直接添加样式来渲染 HTML 文档:
<style type="text/css">

meta标签提供了元数据.元数据也不显示在页面上，但会被浏览器解析。通常用于指定网页的描述，关键词，文件的最后修改时间，作者，和其他元数据。
元数据可以使用于浏览器（如何显示内容或重新加载页面），搜索引擎（关键词），或其他Web服务。
<meta name="keywords" content="HTML, CSS, XML, XHTML, JavaScript">


为网页定义描述内容:
<meta name="description" content="免费 Web & 编程 教程">

定义网页作者:
<meta name="author" content="Runoob">

每30秒钟刷新当前页面:
<meta http-equiv="refresh" content="30">

设置页面的字符集
<meta charset="utf-8">

设置网页的描述
<meta name="description" content="">

设置网页的关键字
<meta name="keywords" content="">

请求的重定向
<meta http-equiv="refresh" content="5;url=地址"/>

script元素
script标签用于加载脚本文件，如： JavaScript。

</head>
```

## css

CSS (Cascading Style Sheets) 用于渲染HTML元素标签的样式。

CSS 是在 HTML 4 开始使用的,是为了更好的渲染HTML元素而引入的.

CSS 可以通过以下方式添加到HTML中:

* 内联样式- 在HTML元素中使用"style" 属性

当特殊的样式需要应用到个别元素时，就可以使用内联样式。 使用内联样式的方法是在相关的标签中使用样式属性。样式属性可以包含任何 CSS 属性。以下实例显示出如何改变段落的颜色和左外边距。

```html
<p style="color:blue;margin-left:20px;">这是一个段落。</p>
```

* 内部样式表 -在HTML文档头部 `<head>` 区域使用`<style>` 元素 来包含CSS

当单个文件需要特别样式时，就可以使用内部样式表。你可以在<head> 部分通过 <style>标签定义内部样式表:

```html
<head>
<style type="text/css">

body {background-color:yellow;}

p {color:blue;}

</style>

</head>
```

* 外部引用 - 使用外部 CSS 文件

当样式需要被应用到很多页面的时候，外部样式表将是理想的选择。使用外部样式表，你就可以通过更改一个文件来改变整个站点的外观。

```html
<head>
<link rel="stylesheet" type="text/css" href="mystyle.css">
</head>
```

## 图像`<img>` 

在 HTML 中，图像由`<img>` 标签定义。浏览器将图像显示在文档中图像标签出现的地方。如果你将图像标签置于两个段落之间，那么浏览器会首先显示第一个段落，然后显示图片，最后显示第二段。


`<img>` 是空标签，意思是说，它只包含属性，并且没有闭合标签。

要在页面上显示图像，你需要使用源属性（src）。src 指 "source"。源属性的值是图像的 URL 地址。

定义图像的语法是：
```html
<img src="url" alt="some_text">
```

* Alt属性

alt 属性用来为图像定义一串预备的可替换的文本。替换文本属性的值是用户定义的。在浏览器无法载入图像时，替换文本属性告诉读者她们失去的信息。此时，浏览器将显示这个替代性的文本而不是图像。为页面上的图像都加上替换文本属性是个好习惯，这样有助于更好的显示信息，并且对于那些使用纯文本浏览器的人来说是非常有用的。

```html
<img src="boat.gif" alt="Big Boat">
```

* height（高度） 与 width（宽度）属性用于设置图像的高度与宽度。

属性值默认单位为像素:

```html
<img src="pulpit.jpg" alt="Pulpit rock" width="304" height="228">
```

## 表格`<table>`

表格由 `<table>` 标签来定义。每个表格均有若干行（由 `<tr>` 标签定义），每行被分割为若干单元格（由 `<td>` 标签定义）。字母 td 指表格数据（table data），即数据单元格的内容。数据单元格可以包含文本、图片、列表、段落、表单、水平线、表格等等。

```html
<table border="1">
    <tr>
        <td>row 1, cell 1</td>
        <td>row 1, cell 2</td>
    </tr>
    <tr>
        <td>row 2, cell 1</td>
        <td>row 2, cell 2</td>
    </tr>
</table>
```
* 边框属性  
  
  如果不定义边框属性，表格将不显示边框。有时这很有用，但是大多数时候，我们希望显示边框。

```
<table border="1">
    <tr>
        <td>Row 1, cell 1</td>
        <td>Row 1, cell 2</td>
    </tr>
</table>
```

* 表格表头
* 
表格的表头使用 `<th>` 标签进行定义。

大多数浏览器会把表头显示为粗体居中的文本：

```html
<table border="1">
    <tr>
        <th>Header 1</th>
        <th>Header 2</th>
    </tr>
    <tr>
        <td>row 1, cell 1</td>
        <td>row 1, cell 2</td>
    </tr>
    <tr>
        <td>row 2, cell 1</td>
        <td>row 2, cell 2</td>
    </tr>
</table>
```

HTML 表格标签

| 标签         | 描述                 |
| ------------ | -------------------- |
| `<table>`    | 定义表格             |
| `<th>`       | 定义表格的表头       |
| `<tr>`       | 定义表格的行         |
| `<td>`       | 定义表格单元         |
| `<caption>`  | 定义表格标题         |
| `<colgroup>` | 定义表格列的组       |
| `<col>`      | 定义用于表格列的属性 |
| `<thead>`    | 定义表格的页眉       |
| `<tbody>`    | 定义表格的主体       |
| `<tfoot>`    | 定义表格的页脚       |

## 列表

HTML 支持有序、无序和定义列表:

| 标签   | 描述                 |
| ------ | -------------------- |
| `<ol>` | 定义有序列表         |
| `<ul>` | 定义无序列表         |
| `<li>` | 定义列表项           |
| `<dl>` | 定义列表             |
| `<dt>` | 自定义列表项目       |
| `<dd>` | 定义自定列表项的描述 |

* 无序列表
  
无序列表是一个项目的列表，此列项目使用粗体圆点（典型的小黑圆圈）进行标记。无序列表使用 `<ul>` 标签

```html
<ul>
<li>Coffee</li>
<li>Milk</li>
</ul>
```

HTML 有序列表

同样，有序列表也是一列项目，列表项目使用数字进行标记。 有序列表始于 `<ol>` 标签。每个列表项始于 `<li>` 标签。

列表项使用数字来标记

```html
<ol>
<li>Coffee</li>
<li>Milk</li>
</ol>
```

* 自定义列表
* 
自定义列表不仅仅是一列项目，而是项目及其注释的组合。自定义列表以 `<dl>` 标签开始。每个自定义列表项以 `<dt>` 开始。每个自定义列表项的定义以 `<dd>` 开始。

```html
<dl>
<dt>Coffee</dt>
<dd>- black hot drink</dd>
<dt>Milk</dt>
<dd>- white cold drink</dd>
```
```
Coffee
- black hot drink
Milk
- white cold drink
```

## 区块元素div 

大多数 HTML 元素被定义为块级元素或内联元素。

块级元素在浏览器显示时，通常会以新行来开始（和结束）。

如：`<h1>, <p>, <ul>, <table>`


内联元素在显示时通常不会以新行开始。

r如：`<b>, <td>, <a>, <img>`


div元素是块级元素，它可用于组合其他 HTML 元素的容器。该元素没有特定的含义。除此之外，由于它属于块级元素，浏览器会在其前后显示折行。如果与 CSS 一同使用，`<div>` 元素可用于对大的内容块设置样式属性。div元素的另一个常见的用途是文档布局。它取代了使用表格定义布局的老式方法。使用 `<table>` 元素进行文档布局不是表格的正确用法。`<table>` 元素的作用是显示表格化的数据。

下面的例子展示了用div来对页面进行分块：

```html
<!DOCTYPE html>
<html>
<head> 
<meta charset="utf-8"> 
<title>菜鸟教程(runoob.com)</title> 
</head>
<body>
 
<div id="container" style="width:500px">
 
<div id="header" style="background-color:#FFA500;">
<h1 style="margin-bottom:0;">主要的网页标题</h1></div>
 
<div id="menu" style="background-color:#FFD700;height:200px;width:100px;float:left;">
<b>菜单</b><br>
HTML<br>
CSS<br>
JavaScript</div>
 
<div id="content" style="background-color:#EEEEEE;height:200px;width:400px;float:left;">
内容在这里</div>
 
<div id="footer" style="background-color:#FFA500;clear:both;text-align:center;">
版权 © runoob.com</div>
 
</div>
 
</body>
</html>
```

## 内联元素`<span>`

span元素是内联元素，可用作文本的容器。span元素也没有特定的含义。当与 CSS 一同使用时，`<span>` 元素可用于为部分文本设置样式属性。

## 表单`<form>`

HTM表单用于收集用户的输入信息。表单表示文档中的一个区域，此区域包含交互控件，将用户收集到的信息发送到 Web 服务器。

表单元素是允许用户在表单中输入内容，比如：文本域（textarea）、下拉列表（select）、单选框（radio-buttons）、复选框（checkbox） 等等

多数情况下被用到的表单标签是输入标签`<input>`。输入类型是由 type 属性定义。


* 文本域（Text Fields）

文本域通过`<input type="text">` 标签来设定，当用户要在表单中键入字母、数字等内容时，就会用到文本域。

```html
<form>
First name: <input type="text" name="firstname"><br>
Last name: <input type="text" name="lastname">
</form>
```

注意:表单本身并不可见。同时，在大多数浏览器中，文本域的默认宽度是 20 个字符。

* 密码字段

密码字段通过标签`<input type="password">`来定义:

```html
<form>
Password: <input type="password" name="pwd">
</form>
```
注意：密码字段字符不会明文显示，而是以星号 * 或圆点 . 替代。

* 单选按钮（Radio Buttons）

`<input type="radio">` 标签定义了表单的单选框选项:

```html
<form action="">
<input type="radio" name="sex" value="male">男<br>
<input type="radio" name="sex" value="female">女
</form>
```

* 复选框（Checkboxes）

`<input type="checkbox">`定义了复选框。

复选框可以选取一个或多个选项：

```
<form>
<input type="checkbox" name="vehicle" value="Bike">我喜欢自行车<br>
<input type="checkbox" name="vehicle" value="Car">我喜欢小汽车
</form>
```
* 下拉菜单select

```html
<form action="">
<select name="cars">
<option value="volvo">Volvo</option>
<option value="saab">Saab</option>
<option value="fiat" selected>Fiat</option>
<option value="audi">Audi</option>
</select>
</form>
```

* 提交按钮(Submit)

`<input type="submit">`定义了提交按钮。当用户单击确认按钮时，表单的内容会被传送到服务器。表单的动作属性 action 定义了服务端的文件名。

action 属性会对接收到的用户输入数据进行相关的处理:

```html
<form name="input" action="html_form_action.php" method="get">
Username: <input type="text" name="user">
<input type="submit" value="Submit">
</form>
```

说明：
假如在上面的文本框内键入几个字母，然后点击确认按钮，那么输入数据会传送到 html_form_action.php 文件，该页面将显示出输入的结果。

以上实例中有一个 method 属性，它用于定义表单数据的提交方式，可以是以下值：

post：指的是 HTTP POST 方法，表单数据会包含在表单体内然后发送给服务器，用于提交敏感数据，如用户名与密码等。

get：默认值，指的是 HTTP GET 方法，表单数据会附加在 action 属性的 URL 中，并以 ?作为分隔符，一般用于不敏感信息，如分页等。例如：https://www.runoob.com/?page=1，这里的 page=1 就是 get 方法提交的数据。

## 框架`<iframe>`

通过使用框架，可以在同一个浏览器窗口中显示不止一个页面。

iframe语法:

```html
<iframe src="URL"></iframe>
```

* iframe - 设置高度与宽度

height 和 width 属性用来定义iframe标签的高度与宽度。

属性默认以像素为单位, 但是你可以指定其按比例显示 (如："80%")。

```html
<iframe loading="lazy" src="demo_iframe.htm" width="200" height="200"></iframe>
```

* iframe - 移除边框

frameborder 属性用于定义iframe表示是否显示边框。

设置属性值为 "0" 移除iframe的边框:

```html
<iframe src="demo_iframe.htm" frameborder="0"></iframe>
```

使用 iframe 来显示目标链接页面
iframe 可以显示一个目标链接的页面

目标链接的属性必须使用 iframe 的属性，如下实例:

```html
<iframe src="demo_iframe.htm" name="iframe_a"></iframe>
<p><a href="https://www.runoob.com" target="iframe_a" rel="noopener">RUNOOB.COM</a></p>
```
## 颜色

HTML 颜色由红色、绿色、蓝色混合而成。

HTML 颜色由一个十六进制符号来定义，这个符号由红色、绿色和蓝色的值组成（RGB）。

每种颜色的最小值是0（十六进制：#00）。最大值是255（十六进制：#FF）。


# 四、html5新元素

## 画布Canvas

`<canvas>`元素是HTML5中的新元素，通过使用该元素，可以在网页中绘制所需的图形。用于图形的绘制，通过脚本 (通常是JavaScript)来完成.

标签只是图形容器，必须使用脚本来绘制图形。可以通过多种方法使用Canva绘制路径,盒、圆、字符以及添加图像。

```html

<canvas id="myCanvas" width="200" height="100"></canvas>

<canvas id="myCanvas" width="200" height="100"
style="border:1px solid #000000;">
</canvas>
```

## 矢量图SVG

SVG表示可缩放矢量图形，是基于可扩展标记语言（标准通用标记语言的子集），用于描述二维矢量图形的一种图形格式，它在2003年1月14日成为W3C推荐标准。

HTML5 支持内联 SVG。

```html
<!DOCTYPE html>
<html>
<body>

<svg xmlns="http://www.w3.org/2000/svg" version="1.1" height="190">
  <polygon points="100,10 40,180 190,60 10,60 160,180"
  style="fill:lime;stroke:purple;stroke-width:5;fill-rule:evenodd;">
</svg>

</body>
</html>
```

SVG 与 Canvas两者间的区别

SVG 是一种使用 XML 描述 2D 图形的语言。

Canvas 通过 JavaScript 来绘制 2D 图形。

SVG 基于 XML，这意味着 SVG DOM 中的每个元素都是可用的。您可以为某个元素附加 JavaScript 事件处理器。

在 SVG 中，每个被绘制的图形均被视为对象。如果 SVG 对象的属性发生变化，那么浏览器能够自动重现图形。

Canvas 是逐像素进行渲染的。在 canvas 中，一旦图形被绘制完成，它就不会继续得到浏览器的关注。如果其位置发生变化，那么整个场景也需要重新绘制，包括任何或许已被图形覆盖的对象。


## math

HTML5 可以在文档中使用 MathML 元素，对应的标签是 <math>...</math> 。

MathML 是数学标记语言，是一种基于XML（标准通用标记语言的子集）的标准，用来在互联网上书写数学符号和公式的置标语言。

注意：目前只有 Firefox 或 Safari 浏览器支持，大部分浏览器还不支持 MathML 标签，如果你的浏览器不支持该标签，可以使用最新版的 Firefox 或 Safari 浏览器查看。

## 拖放（drag and drop）


拖放是一种常见的特性，即抓取对象以后拖到另一个位置。拖放（Drag 和 drop）是 HTML5 标准的组成部分。在 HTML5 中，拖放是标准的一部分，任何元素都能够拖放。


下面的例子是一个简单的拖放实例：

```html
<!DOCTYPE HTML>
<html>
<head>
<meta charset="utf-8"> 
<title>菜鸟教程(runoob.com)</title>
<style type="text/css">
#div1 {width:350px;height:70px;padding:10px;border:1px solid #aaaaaa;}
</style>
<script>
function allowDrop(ev)
{
    ev.preventDefault();
}
 
function drag(ev)
{
    ev.dataTransfer.setData("Text",ev.target.id);
}
 
function drop(ev)
{
    ev.preventDefault();
    var data=ev.dataTransfer.getData("Text");
    ev.target.appendChild(document.getElementById(data));
}
</script>
</head>
<body>

<p>拖动 RUNOOB.COM 图片到矩形框中:</p>
 
<div id="div1" ondrop="drop(event)" ondragover="allowDrop(event)"></div>
<br>
<img loading="lazy" id="drag1" src="/images/logo.png" draggable="true" ondragstart="drag(event)" width="336" height="69">
 
</body>
</html>
```



* 设置元素为可拖放
 
首先，为了使元素可拖动，把 draggable 属性设置为 true ：
```
<img draggable="true">
```

* 拖动什么 - ondragstart 和 setData()


然后，规定当元素被拖动时，会发生什么。

在上面的例子中，ondragstart 属性调用了一个函数，drag(event)，它规定了被拖动的数据。

dataTransfer.setData() 方法设置被拖数据的数据类型和值：

function drag(ev)
{
    ev.dataTransfer.setData("Text",ev.target.id);
}

Text 是一个 DOMString 表示要添加到 drag object 的拖动数据的类型。值是可拖动元素的 id ("drag1")。

* 放到何处 - ondragover

ondragover 事件规定在何处放置被拖动的数据。

默认地，无法将数据/元素放置到其他元素中。如果需要设置允许放置，我们必须阻止对元素的默认处理方式。

这要通过调用 ondragover 事件的 event.preventDefault() 方法：

event.preventDefault()

* 进行放置 - ondrop

当放置被拖数据时，会发生 drop 事件。

在上面的例子中，ondrop 属性调用了一个函数，drop(event)：

function drop(ev)
{
    ev.preventDefault();
    var data=ev.dataTransfer.getData("Text");
    ev.target.appendChild(document.getElementById(data));
}

代码解释：

调用 preventDefault() 来避免浏览器对数据的默认处理（drop 事件的默认行为是以链接形式打开）
通过 dataTransfer.getData("Text") 方法获得被拖的数据。该方法将返回在 setData() 方法中设置为相同类型的任何数据。
被拖数据是被拖元素的 id ("drag1")
把被拖元素追加到放置元素（目标元素）中

## Geolocation（地理定位）

HTML5 Geolocation（地理定位）用于定位用户的位置。

HTML5 Geolocation API 用于获得用户的地理位置。

* 显示地理位置信息


下例是一个简单的地理定位实例，可返回用户位置的经度和纬度:

```html
var x=document.getElementById("demo");
function getLocation()
{
    if (navigator.geolocation)
    {
        navigator.geolocation.getCurrentPosition(showPosition);
    }
    else
    {
        x.innerHTML="该浏览器不支持获取地理位置。";
    }
}
 
function showPosition(position)
{
    x.innerHTML="纬度: " + position.coords.latitude + 
    "<br>经度: " + position.coords.longitude;  
}
```

检测是否支持地理定位.如果支持，则运行 getCurrentPosition() 方法。如果不支持，则向用户显示一段消息。如果 getCurrentPosition() 运行成功，则向参数showPosition中规定的函数返回一个 coordinates 对象.showPosition() 函数获得并显示经度和纬度.

* 在地图中显示结果
  
如需在地图中显示结果，需要访问可使用经纬度的地图服务，比如谷歌地图或百度地图：

```html
function showPosition(position)
{
    var latlon=position.coords.latitude+","+position.coords.longitude;
 
    var img_url="http://maps.googleapis.com/maps/api/staticmap?center="
    +latlon+"&zoom=14&size=400x300&sensor=false";
    document.getElementById("mapholder").innerHTML="<img src='"+img_url+"'>";
}
```

## Video

HTML5 规定了一种通过 video 元素来包含视频的标准方法。

`<video>` 元素提供了 播放、暂停和音量控件来控制视频。

同时 `<video>` 元素也提供了 width 和 height 属性控制视频的尺寸.如果设置的高度和宽度，所需的视频空间会在页面加载时保留。如果没有设置这些属性，浏览器不知道大小的视频，浏览器就不能再加载时保留特定的空间，页面就会根据原始视频的大小而改变。

`<video>` 与`</video>` 标签之间插入的内容是提供给不支持 video 元素的浏览器显示的。

`<video>` 元素支持多个 `<source>` 元素. `<source>` 元素可以链接不同的视频文件。浏览器将使用第一个可识别的格式：

`<video>` 和 `<audio>` 元素同样拥有方法、属性和事件。

`<video>` 和 `<audio>` 元素的方法、属性和事件可以使用JavaScript进行控制.

其中的方法用于播放、暂停以及加载等。其中的属性（比如时长、音量等）可以被读取或设置。其中的 DOM 事件能够通知您，比方说，`<video>` 元素开始播放、已暂停，已停止，等等。

```html
<!DOCTYPE html> 
<html> 
<head> 
<meta charset="utf-8"> 
<title>菜鸟教程(runoob.com)</title> 
</head>
<body> 

<div style="text-align:center"> 
  <button onclick="playPause()">播放/暂停</button> 
  <button onclick="makeBig()">放大</button>
  <button onclick="makeSmall()">缩小</button>
  <button onclick="makeNormal()">普通</button>
  <br> 
  <video id="video1" width="420">
    <source src="mov_bbb.mp4" type="video/mp4">
    <source src="mov_bbb.ogg" type="video/ogg">
    您的浏览器不支持 HTML5 video 标签。
  </video>
</div> 

<script> 
var myVideo=document.getElementById("video1"); 

function playPause()
{ 
	if (myVideo.paused) 
	  myVideo.play(); 
	else 
	  myVideo.pause(); 
} 

	function makeBig()
{ 
	myVideo.width=560; 
} 

	function makeSmall()
{ 
	myVideo.width=320; 
} 

	function makeNormal()
{ 
	myVideo.width=420; 
} 
</script> 

</body> 
</html>
```

## audio

HTML5 规定了在网页上嵌入音频元素的标准，即使用 `<audio>` 元素。

```
<audio controls>
  <source src="horse.ogg" type="audio/ogg">
  <source src="horse.mp3" type="audio/mpeg">
您的浏览器不支持 audio 元素。
</audio>
```
audio 元素的control 属性供添加播放、暂停和音量控件。

在`<audio>` 与 `</audio>` 之间你需要插入浏览器不支持的`<audio>`元素的提示文本 。

`<audio>` 元素允许使用多个 `<source>` 元素. `<source>` 元素可以链接不同的音频文件，浏览器将使用第一个支持的音频文件

## input

HTML5 拥有多个新的表单输入类型。这些新特性提供了更好的输入控制和验证。

如：color、date、datetime、datetime-local、email、month、number、range、search、tel、time、url、week
注意:并不是所有的主流浏览器都支持新的input类型，不过您已经可以在所有主流的浏览器中使用它们了。即使不被支持，仍然可以显示为常规的文本域。

例如：

```html
从拾色器中选择一个颜色:
选择你喜欢的颜色: <input type="color" name="favcolor">

Input 类型: date
date 类型允许你从一个日期选择器选择一个日期。
定义一个时间控制器:
生日: <input type="date" name="bday">

Input 类型: datetime
datetime 类型允许你选择一个日期（UTC 时间）。
定义一个日期和时间控制器（本地时间）:
生日 (日期和时间): <input type="datetime" name="bdaytime">

Input 类型: datetime-local
datetime-local 类型允许你选择一个日期和时间 (无时区).
定义一个日期和时间 (无时区):
生日 (日期和时间): <input type="datetime-local" name="bdaytime">

Input 类型: email
email 类型用于应该包含 e-mail 地址的输入域。
在提交表单时，会自动验证 email 域的值是否合法有效:
E-mail: <input type="email" name="email">

Input 类型: month
month 类型允许你选择一个月份。
定义月与年 (无时区):
生日 (月和年): <input type="month" name="bdaymonth">

Input 类型: number
number 类型用于应该包含数值的输入域。
定义一个数值输入域(限定):
数量 ( 1 到 5 之间 ): <input type="number" name="quantity" min="1" max="5">

Input 类型: range
range 类型用于应该包含一定范围内数字值的输入域。
range 类型显示为滑动条。
定义一个不需要非常精确的数值（类似于滑块控制）:
<input type="range" name="points" min="1" max="10">

定义一个搜索字段 (类似站点搜索或者Google搜索):
Search Google: <input type="search" name="googlesearch">

Input 类型: tel
定义输入电话号码字段:
电话号码: <input type="tel" name="usrtel">

Input 类型: time
time 类型允许你选择一个时间。
定义可输入时间控制器（无时区）：
选择时间: <input type="time" name="usr_time">

Input 类型: url
url 类型用于应该包含 URL 地址的输入域。
在提交表单时，会自动验证 url 域的值。
添加您的主页: <input type="url" name="homepage">

Input 类型: week
week 类型允许你选择周和年。
定义周和年 (无时区):
选择周: <input type="week" name="week_year">
```

## 表单新元素

* datalist

`<datalist>` 元素规定输入域的选项列表。

`<datalist>` 属性规定 form 或 input 域应该拥有自动完成功能。当用户在自动完成域中开始输入时，浏览器应该在该域中显示填写的选项：

使用 `<input>` 元素的列表属性与 `<datalist>` 元素绑定

```html
<input list="browsers">
 
<datalist id="browsers">
  <option value="Internet Explorer">
  <option value="Firefox">
  <option value="Chrome">
  <option value="Opera">
  <option value="Safari">
</datalist>
```

* keygen

`<keygen>` 元素的作用是提供一种验证用户的可靠方法。

`<keygen>`标签规定用于表单的密钥对生成器字段。

当提交表单时，会生成两个键，一个是私钥，一个公钥。

私钥（private key）存储于客户端，公钥（public key）则被发送到服务器。公钥可用于之后验证用户的客户端证书（client certificate）。

```html
<form action="demo_keygen.asp" method="get">
用户名: <input type="text" name="usr_name">
加密: <keygen name="security">
<input type="submit">
</form>
```

* output

<output> 元素用于不同类型的输出，比如计算或脚本输出：

```html
<!DOCTYPE html>
<html>
<head> 
<meta charset="utf-8"> 
<title>菜鸟教程(runoob.com)</title> 
</head>
<body>

<form oninput="x.value=parseInt(a.value)+parseInt(b.value)">0
<input type="range" id="a" value="50">100
+<input type="number" id="b" value="50">
=<output name="x" for="a b"></output>
</form>

<p><strong>注意:</strong> Edge 12及更早 IE 版本的浏览器不支持 output 元素。</p>

</body>
</html>
```

## websocket

WebSocket 协议本质上是一个基于 TCP 的协议。

为了建立一个 WebSocket 连接，客户端浏览器首先要向服务器发起一个 HTTP 请求，这个请求和通常的 HTTP 请求不同，包含了一些附加头信息，其中附加头信息"Upgrade: WebSocket"表明这是一个申请协议升级的 HTTP 请求，服务器端解析这些附加的头信息然后产生应答信息返回给客户端，客户端和服务器端的 WebSocket 连接就建立起来了，双方就可以通过这个连接通道自由的传递信息，并且这个连接会持续存在直到客户端或者服务器端的某一方主动的关闭连接。

```html
<!DOCTYPE HTML>
<html>
   <head>
   <meta charset="utf-8">
   <title>菜鸟教程(runoob.com)</title>
    
      <script type="text/javascript">
         function WebSocketTest()
         {
            if ("WebSocket" in window)
            {
               alert("您的浏览器支持 WebSocket!");
               
               // 打开一个 web socket
               var ws = new WebSocket("ws://localhost:9998/echo");
                
               ws.onopen = function()
               {
                  // Web Socket 已连接上，使用 send() 方法发送数据
                  ws.send("发送数据");
                  alert("数据发送中...");
               };
                
               ws.onmessage = function (evt) 
               { 
                  var received_msg = evt.data;
                  alert("数据已接收...");
               };
                
               ws.onclose = function()
               { 
                  // 关闭 websocket
                  alert("连接已关闭..."); 
               };
            }
            
            else
            {
               // 浏览器不支持 WebSocket
               alert("您的浏览器不支持 WebSocket!");
            }
         }
      </script>
        
   </head>
   <body>
   
      <div id="sse">
         <a href="javascript:WebSocketTest()">运行 WebSocket</a>
      </div>
      
   </body>
</html>
```

