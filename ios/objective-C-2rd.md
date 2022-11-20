

# 第一章 预备知识

## 1.1 基本术语

* **Objective-C语言**

Objective-C是在20世纪80年代，由Brad Cox设计出来的，融合了流行的C语言和优雅的SmallTalk语言的优势设计出来的。

* **Cocoa**

是Mac OS操作系统的开发包，

* **Cocoa Touch** 

是ios系统的开发包。

* **框架**

框架是一种把头文件、库、突破、声音等内容聚集在一个独立单元中的集合体。每个框架都是一个重要的技术集合，通常包含数十个甚至上百个头文件。每个框架都有一个主头文件，包含了框架内的所有头文件。通过在主文件中使用#import,可以访问框架内的所有内容。

Foundation框架 主要是NSXXX对象，比如NSString、NSArray、NSEnumerator、NSNumber等100多个类

CoreFoundation框架 CoreFoundation是纯C语言编写的，函数或变量以CF开头。 Foundation框架是以CoreFoundation框架为基础创建的，几乎都有CoreFoundation的NS版本。

CoreGraphics框架 用于处理集合图形，以CG开头的类。

AppKit AppKit是基于OS X平台（Mac），比如NSColor

UIKit UIKit是基于IOS平台（iPhone），比如UIColor

* **\#import语句**

#import和c 语言中的#include一样，用来通知编译器查询头文件中相应的定义代码。 在Objective-C中，import可以保证头文件只被包含一次。

* **@**

@符号是Objective-C在标准C语言上添加的特性之一。

object-c中，@符号是表示对C语言的扩展，以@开头的说明是扩展功能，比如大部分的面向对象特性，@”字符串”等

* **@""**

@"字符串" 符号意味着引号内的字符串应作为Cocoa的NSString元素来处理。

* **%@**
  


* **NSLog**

类似于C语言的printf函数，接受一个字符串作为第一个参数，该参数可以包含格式说明符，其他参数与说明符相匹配。和printf相比，添加了时间戳、日期戳和自动换行符等。

* **布尔类型BOOL**

bool类型是C语言的数据类型，它拥有false和true两个值。

BOOL类型是Objective-C中的布尔类型，它拥有YES和NO两个值。

尽量使用BOOL类型。

* **id**

id（identifier)是一种泛型，用来引用任何类型的对象。

```c
id shape = shapes[5];
```

* **[]方法定义**

Objective-C中，方括号[]表示通知某个对象去执行某个操作。第一项是对象，第二项是需要对象执行的操作。称为发送消息。即向对象发送一个消息。

实际上，是类对象的方法调用。

* **方法名中的中缀符**

冒号是方法名称中的重要部分，如果方法使用参数，则使用冒号，否则不使用冒号。

* **对象实例化**

对象实例化为对象分配内存，将内存初始化并保存为有用的默认值。

实例化方法：

向类发送创建对象实例的消息：

```c
[Object new];
或
[[Object alloc]init];
```
[className new]基本等同于[[className alloc] init]；区别在于alloc分配内存的时候使用了zone.在给对象分配内存的时候，把关联的对象分配到一个相邻的内存区域内，以便于调用时消耗很少的代价，提升了程序处理速度；同时使用alloc的init消息可以自定义自己的初始化方法。

## 1.2 Objective C 文件结构（类定义）

OC中的类定义分为接口文件和实现文件两个部分。

接口文件声明了类的成员变量和方法。用于定义类的公共接口。

实现文件中对变量初始化以及实现方法。


### 1.2.1 类接口文件

接口文件声明了类的成员变量和方法。用于定义类的公共接口。

* 接口文件声明

`@interface Circle : NSObject`

>@inteface是编译器指令，告诉编译器，这是新类Circle的接口。

>：告诉编译器，类Circle继承于NSObject

* 类的实例变量定义

用大括号引用起来的变量定义成为类的实例变量。

```objective-c
{
ShapeColor fillColor;
ShapeRect bounds;
}
```

* 类的方法声明

类的方法声明列出了每个方法的名称、方法返回值的类型和参数。

```objective-c
- (void) setFillColor: (ShapeColor) fillColor;

- (void) setBounds:(ShapeRect) bounds;

- (void) draw;
```

>\- 表示这是Objective-C方法的声明，这是区分类的方法声明和函数的一种方式。函数原型中没有短线。
>
>() 括号和括号中的类型组成了类方法的返回类型。(void)表示无返回值。Objective-C可以返回标准类型（整形、浮点型、字符串等）指针、对象和结构体。
>
>：中缀符号。方法中冒号是名称的一部分，它告诉编译器，后面出现的是类方法的参数。参数类型在圆括号中指定。
>
>@end 告诉编译器，对Circle类的声明完成。

实例如下：

```objective-c
@interface Circle: NSObject 
{
ShapeColor fillColor;
ShapeRect bounds;
}

- (void) setFillColor: (ShapeColor) fillColor;

- (void) setBounds:(ShapeRect) bounds;

- (void) draw;

@end
```


### 1.2.2 类实现文件

```objective-c
@implementation Circle

-(void) setFillColor: (ShapeColor) c {
  fillColor = c;
}

- (void) setBounds: (ShapeREct) b {
  bounds = b;
}
```

@implementation 是一个编译器指令，表明即将为某个类提供代码。

方法的定义在实现文件中。

类实现文件中还可以定义在@interface中没有声明过的方法。可以看做是类的私有方法，只在该类中使用。


### 1.2.3 类的实例化

类的实例化为类分配内存，然后将这些内存初始化并保存默认值。内存分配和初始化工作完成后，类的对象实例就可以使用了。 

```objective-c
int main (int argc, const char * argv[]) {
  Circle *circle = [[Circle alloc]init];
  [circle setFillColor: kRedColor];
}
```




# 第二章 对C的扩展

# 第三章 面向对象编程的基础知识

# 第四章 继承

# 第五章 复合

# 第六章 源文件组织

# 第七章 深入了解xcode

# 第八章 Foundation Kit介绍

# 第九章 内存管理

# 第十章  对象初始化

# 第十一章  属性

# 第十二章 类别

# 第十三章 协议

# 第十四章 代码块和并发性

# 第十五章 AppKit简介

# 第十六章 UIKIt简介

# 第十七章 文件加载与保存

# 第十八章 键值对编码

# 第十九章 使用静态分析器

# 第二十章 NSPredicate

