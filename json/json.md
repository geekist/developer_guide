
## 一、Json介绍


Json 官方网站地址：https://www.json.org/json-zh.html

JSON(JavaScript Object Notation) 是一种轻量级的数据交换格式。 易于人阅读和编写。同时也易于机器解析和生成。 它基于JavaScript Programming Language, Standard ECMA-262 3rd Edition - December 1999的一个子集。 

JSON采用完全独立于语言的文本格式，但是也使用了类似于C语言家族的习惯（包括C, C++, C#, Java, JavaScript, Perl, Python等）。这些特性使JSON成为理想的数据交换语言。

## 二、Json的语法规则

JSON 语法是 JavaScript 语法的子集。

- 数据在名称/值对中
- 数据之间由逗号分隔
- 大括号 {} 保存对象
- 中括号 [] 保存数组，数组可以包含多个对象


JSON 数据的书写格式是：

 >key : value


***key:***

一个用双引号引起来的字符串，表示字段的名称


例如：
```json
"name" : "菜鸟教程"
```

***value***

JSON 值可以是：

- 数字（整数或浮点数）

JSON 数字可以是整型或者浮点型：

```json
{ "age":30 }
```

- 字符串（在双引号中）


```json

"url":"www.runoob.com"

```

- 逻辑值（true 或 false）


JSON 布尔值可以是 true 或者 false：

```
{ "flag":true }
```

- 数组（在中括号中）

```jaon
[
    { key1 : value1-1 , key2:value1-2 }, 
    { key1 : value2-1 , key2:value2-2 }, 
    { key1 : value3-1 , key2:value3-2 }, 
    ...
    { keyN : valueN-1 , keyN:valueN-2 }, 
]
{
    "sites": [
        { "name":"菜鸟教程" , "url":"www.runoob.com" }, 
        { "name":"google" , "url":"www.google.com" }, 
        { "name":"微博" , "url":"www.weibo.com" }
    ]
}
```


- 对象（在大括号中）

JSON 对象在大括号 {} 中书写：

{key1 : value1, key2 : value2, ... keyN : valueN }

对象可以包含多个名称/值对：

```json
{ "name":"菜鸟教程" , "url":"www.runoob.com" }
```

- null

JSON 可以设置 null 值：

```json
{ "runoob":null }
```

### 三、Json的两种结构：

* ***Json对象*** 

“名称/值”对的集合（A collection of name/value pairs）。

不同的语言中，它被理解为对象（object），纪录（record），结构（struct），字典（dictionary），哈希表（hash table），有键列表（keyed list），或者关联数组 （associative array）。

对象是一个无序的“‘名称/值’对”集合。一个对象以 {左括号 开始， }右括号 结束。每个“名称”后跟一个 :冒号 ；“‘名称/值’ 对”之间使用 ,逗号 分隔。

例如：

```json

{
    "success": "1",
    "result": {
        "timestamp": "1542456793",
        "datetime_1": "2018-11-17 20:13:13",
        "datetime_2": "2018年11月17日 20时13分13秒",
        "week_1": "6",
        "week_2": "星期六",
        "week_3": "周六",
        "week_4": "Saturday"
    }
}

```



* ***Json数组***

值的有序列表（An ordered list of values）。在大部分语言中，它被理解为数组（array）。

这些都是常见的数据结构。事实上大部分现代计算机语言都以某种形式支持它们。这使得一种数据格式在同样基于这些结构的编程语言之间交换成为可能。

JSON具有以下这些形式：

对象是一个无序的“‘名称/值’对”集合。一个对象以 {左括号 开始， }右括号 结束。每个“名称”后跟一个 :冒号 ；“‘名称/值’ 对”之间使用 ,逗号 分隔。

```jaon
[
    { key1 : value1-1 , key2:value1-2 }, 
    { key1 : value2-1 , key2:value2-2 }, 
    { key1 : value3-1 , key2:value3-2 }, 
    ...
    { keyN : valueN-1 , keyN:valueN-2 }, 
]
{
    "sites": [
        { "name":"菜鸟教程" , "url":"www.runoob.com" }, 
        { "name":"google" , "url":"www.google.com" }, 
        { "name":"微博" , "url":"www.weibo.com" }
    ]
}
```

