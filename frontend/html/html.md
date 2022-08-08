
# 一、HTML介绍

HTML（HyperText Markup Language，超文本标记语言） 是一种描述语言，用来定义网页结构。

1990 年，由于对 Web 未来发展的远见，Tim Berners-Lee 提出了 超文本 的概念，并在第二年在 SGML (en-US) 的基础上将其正式定义为一个标记语言。IETF (en-US) 于 1993 年正式开始制定 HTML 规范，并在经历了几个版本的草案后于 1995 年发布了 HTML 2.0 版本。1994 年，Berners-Lee 为了 Web 发展而成立了 W3C (en-US)。1996 年，W3C 接管了 HTML 的标准化工作，并在 1 年后发布了 HTML 3.2 推荐标准。1999 年，HTML 4.0 发布，并在 2000 年成为 ISO (en-US) 标准。

自那以后，W3C 几乎放弃了 HTML 而转向 XHTML，并于 2004 年成立了另一个独立的小组 WHATWG。幸运的是，WHATWG 后来又转回来参与了 HTML5 标准的制定，两个组织（译注：即 W3C 和 WHATWG）在 2008 年发布了第一份草案，并在 2014 年发布了 HTML5 标准的最终版。

# 二、HTML基本概念：

## 2.1 HTML文档

一个简单的html文档样式如下：

![](./assets/../../assets/html_0.jpg)

HTML 文档是包含多个 HTML 元素 的文本文档。每个元素都用一对开始和结束标签包裹。每个标签以尖括号（<>）开始和结束。也有一部分标签中不需要包含文本，这些标签称为空标签，如 <img>。

HTML 文件通常会以 .htm 或 .html 为扩展名。用户可以从 Web 服务器 中下载，并使用任一 Web 浏览器 来解析和显示。

## 2.2 标签

HTML 标记标签通常被称为 HTML 标签 (HTML tag)。

HTML 标签是由尖括号包围的关键词，比如 <html> 标签对中的第一个标签是开始标签，第二个标签是结束标签

HTML 标签通常是成对出现的，比如 <b> 和 </b>.

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

## 2.4 元素 

元素指标签和标签包含的内容、属性等。

![](./assets/../../assets/html_1.png)

开始标签（Opening tag）：包含元素的名称（本例为 p），被左、右角括号所包围。表示元素从这里开始或者开始起作用 —— 在本例中即段落由此开始。

结束标签（Closing tag）：与开始标签相似，只是其在元素名之前包含了一个斜杠。这表示着元素的结尾 —— 在本例中即段落在此结束。初学者常常会犯忘记包含结束标签的错误，这可能会产生一些奇怪的结果。

内容（Content）：元素的内容，本例中就是所输入的文本本身。

元素（Element）：开始标签、结束标签与内容相结合，便是一个完整的元素。



# 三、HTML元素

## 标题

标题（Heading）是通过 `<h1> - <h6>` 标签进行定义的。`<h1>` 定义最大的标题。 `<h6>` 定义最小的标题。搜索引擎使用标题为您的网页的结构和内容编制索引。浏览器会自动地在标题的前后添加空行。

```html
<h1>这是一个标题。</h1>
<h2>这是一个标题。</h2>
<h3>这是一个标题。</h3>
```

## 水平线

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

## 段落

段落是通过 `<p>` 标签定义的,可以将文档分割为若干段落。浏览器会自动地在段落的前后添加空行

```html
<p>这是一个段落 </p>
<p>这是另一个段落</p>
```
## 换行

如果您希望在不产生一个新段落的情况下进行换行（新行），请使用 `<br>` 标签： `<br />` 元素是一个空的 HTML 元素。由于关闭标签没有任何意义，因此它没有结束标签。

```html
<p>这个<br>段落<br>演示了分行的效果</p>
```

## 文本格式化标签

HTML 文本格式化标签

| 标签	| 描述 |
| ---- | ---- |
|`<b>`	|定义粗体文本|
|`<em>`	|定义着重文字|
|`<i>`	|定义斜体字|
|`<small>`	|定义小号字|
|`<strong>`	|定义加重语气|
|`<sub>`	|定义下标字|
|`<sup>`	|定义上标字|
|`<ins>`	|定义插入字|
|`<del>`	|定义删除字|

HTML "计算机输出" 标签

|标签	| 描述|
| ---- | ---- |
|`<code>`	|定义计算机代码|
|`<kbd>`	|定义键盘码|
|`<samp>`	|定义计算机代码样本|
|`<var>`	|定义变量|
|`<pre>`	|定义预格式文本|

HTML 引文, 引用, 及标签定义

|标签	|描述|
| ---- | ---- |
|`<abbr>`	|定义缩写|
|`<address>`	|定义地址|
|`<bdo>`	|定义文字方向|
|`<blockquote>`	|定义长的引用|
|`<q>`	|定义短的引用语|
|`<cite>`|	定义引用、引证|
|`<dfn>`	|定义一个定义项目。|

## 超链接

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

target 属性定义被链接的文档在何处显示。

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

## head

`<head>` 元素包含了所有的头部标签元素。在 `<head>`元素可以插入脚本（scripts）, 样式文件（CSS），及各种meta信息。

可以添加在头部区域的元素标签为: `<title>, <style>, <meta>, <link>, <script>, <noscript> 和 <base>`。

|标签	|描述 |
| ---- | ---- |
|`<head>`	|定义了文档的信息|
|`<title>`	|定义了文档的标题|
|`<base>`	|定义了页面链接标签的默认链接地址|
|`<link>`	|定义了一个文档和外部资源之间的关系|
|`<meta>`	|定义了HTML文档中的元数据|
|`<script>`	|定义了客户端的脚本文件|
|`<style>`	|定义了HTML文档的样式文件|


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

## 图像

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

## 表格

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

|标签	|描述|
|----|----|
|`<table>`|定义表格|
|`<th>`	|定义表格的表头|
|`<tr>`	|定义表格的行|
|`<td>`	|定义表格单元|
|`<caption>`	|定义表格标题|
|`<colgroup>`	|定义表格列的组|
|`<col>`	|定义用于表格列的属性|
|`<thead>`	|定义表格的页眉|
|`<tbody>`	|定义表格的主体|
|`<tfoot>`	|定义表格的页脚|

## 列表

HTML 支持有序、无序和定义列表:

|标签|	描述|
| ---- | ---- |
|`<ol>`	|定义有序列表|
|`<ul>`	|定义无序列表|
|`<li>`	|定义列表项|
|`<dl>`	|定义列表|
|`<dt>`	|自定义列表项目|
|`<dd>`	|定义自定列表项的描述|

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

## 内联元素span

span元素是内联元素，可用作文本的容器。span元素也没有特定的含义。当与 CSS 一同使用时，<span> 元素可用于为部分文本设置样式属性。

##




