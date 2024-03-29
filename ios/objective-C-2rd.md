

- [一、基本术语](#一基本术语)
    - [**Objective-C语言**](#objective-c语言)
    - [**Cocoa**](#cocoa)
    - [**Cocoa Touch**](#cocoa-touch)
    - [**框架**](#框架)
    - [**#import语句**](#import语句)
    - [**@**](#)
    - [**@""**](#-1)
    - [**%@**](#-2)
    - [**NSLog**](#nslog)
    - [**布尔类型BOOL**](#布尔类型bool)
    - [**id**](#id)
    - [**\[\]方法定义**](#方法定义)
    - [**方法名中的中缀符**](#方法名中的中缀符)
- [二、 类定义和文件结构](#二-类定义和文件结构)
  - [2.1 类接口文件](#21-类接口文件)
    - [接口文件声明语法：](#接口文件声明语法)
    - [接口文件中也可以定义类的方法声明](#接口文件中也可以定义类的方法声明)
    - [成员变量](#成员变量)
    - [属性变量](#属性变量)
    - [ios属性的修饰符号](#ios属性的修饰符号)
  - [2.2 类实现文件](#22-类实现文件)
    - [**对象实例化**](#对象实例化)
    - [**类实现文件中的@interface定义**](#类实现文件中的interface定义)
- [三、类的继承--inherit](#三类的继承--inherit)
    - [**继承的工作机制--方法调度：**](#继承的工作机制--方法调度)
    - [**重写方法**](#重写方法)
    - [**self关键字**](#self关键字)
    - [**super关键字**](#super关键字)
- [四、类的聚合（composition)](#四类的聚合composition)
    - [**读取方法（assessors）**](#读取方法assessors)
    - [**@class关键字--向前引用**](#class关键字--向前引用)


# 一、基本术语

### **Objective-C语言**

Objective-C是在20世纪80年代，由Brad Cox设计出来的，融合了流行的C语言和优雅的SmallTalk语言的优势设计出来的。

### **Cocoa**

是Mac OS操作系统的开发包，

### **Cocoa Touch** 

是ios系统的开发包。

### **框架**

框架是一种把头文件、库、突破、声音等内容聚集在一个独立单元中的集合体。每个框架都是一个重要的技术集合，通常包含数十个甚至上百个头文件。每个框架都有一个主头文件，包含了框架内的所有头文件。通过在主文件中使用#import,可以访问框架内的所有内容。

Foundation框架 主要是NSXXX对象，比如NSString、NSArray、NSEnumerator、NSNumber等100多个类

CoreFoundation框架 CoreFoundation是纯C语言编写的，函数或变量以CF开头。 Foundation框架是以CoreFoundation框架为基础创建的，几乎都有CoreFoundation的NS版本。

CoreGraphics框架 用于处理集合图形，以CG开头的类。

AppKit AppKit是基于OS X平台（Mac），比如NSColor

UIKit UIKit是基于IOS平台（iPhone），比如UIColor

### **\#import语句**

#import和c 语言中的#include一样，用来通知编译器查询头文件中相应的定义代码。 在Objective-C中，import可以保证头文件只被包含一次。

### **@**

@符号是Objective-C在标准C语言上添加的特性之一。

object-c中，@符号是表示对C语言的扩展，以@开头的说明是扩展功能，比如大部分的面向对象特性，@”字符串”等

### **@""**

@"字符串" 符号意味着引号内的字符串应作为Cocoa的NSString元素来处理。

### **%@**
  


### **NSLog**

类似于C语言的printf函数，接受一个字符串作为第一个参数，该参数可以包含格式说明符，其他参数与说明符相匹配。和printf相比，添加了时间戳、日期戳和自动换行符等。

### **布尔类型BOOL**

bool类型是C语言的数据类型，它拥有false和true两个值。

BOOL类型是Objective-C中的布尔类型，它拥有YES和NO两个值。

尽量使用BOOL类型。

### **id**

id（identifier)是一种泛型，用来引用任何类型的对象。

```c
id shape = shapes[5];
```

### **[]方法定义**

Objective-C中，方括号[]表示通知某个对象去执行某个操作。第一项是对象，第二项是需要对象执行的操作。称为发送消息。即向对象发送一个消息。

实际上，是类对象的方法调用。

### **方法名中的中缀符**

冒号是方法名称中的重要部分，如果方法使用参数，则使用冒号，否则不使用冒号。

# 二、 类定义和文件结构

OC中的类定义分为接口文件和实现文件两个部分。

接口文件声明了类的成员变量和方法。用于定义类的公共接口。

实现文件中对变量初始化以及实现方法。


## 2.1 类接口文件

接口文件声明了类的成员变量和方法。用于定义类的公共接口。

### 接口文件声明语法：

```c
@interface Circle : NSObject
```

>@inteface是编译器指令，告诉编译器，这是新类Circle的接口。

>：告诉编译器，类Circle继承于NSObject

* 接口文件中可以定义类的实例变量

用大括号引用起来的变量定义成为类的实例变量。

```objective-c
{
ShapeColor fillColor;
ShapeRect bounds;
}
```

### 接口文件中也可以定义类的方法声明

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

### 成员变量

成员变量是在{}中声明的变量。如果成员变量的类型是一个类则称这个变量为实例变量。成员变量包括实例变量，所以可以通称为成员变量。


成员变量的定义

例如：

定义在h文件中

```
#import <Foundation/Foundation.h>
NS_ASSUME_NONNULL_BEGIN
@interface Persion : NSObject{
    NSString *name; //实例变量
    int age;//成员变量
}
@end
NS_ASSUME_NONNULL_END
```

定义在m文件中

```
#import "Persion.h"
@interface Persion ()
{
    NSString *gender;//实例变量
    int height;//成员变量
}
@end

@implementation Persion

@end
```

成员变量的使用；

成员变量创建后默认是@protected类型，就是受保护的，外部无法访问。

如果需要外部可以访问，在h文件中定义的变量，添加public修饰符；在m文件中定义的变量，添加publuc修饰符无效。

```
@interface Persion : NSObject
{
    @public NSString *name; //实例变量
    int age;//成员变量
}
@end
```

### 属性变量

使用@property声明的变量都叫做属性变量

在iOS5之后，苹果推出一个新机制，@property声明的属性默认会生成一个_类型的成员变量，同时也会生成setter/getter方法。，属性的使用不再需要sychronize

属性定义在h文件中

```
#import <Foundation/Foundation.h>
NS_ASSUME_NONNULL_BEGIN
@interface Persion : NSObject
@property (nonatomic, copy) NSString *info;//属性变量
@end
NS_ASSUME_NONNULL_END
```

属性定义在m文件中,在.m 中创建的属性变量只能在类内部使用，外部无法使用

```
#import "Persion.h"
@interface Persion ()

@property (nonatomic, copy) NSString *hobby;//属性变量
@end
```

属性变量的使用：

当我们创建一个属性变量时，系统会自动创建一个与之对应的下划线成员变量，并自动添加了getter和setter方法

```
#import "Persion.h"
@interface Persion ()
@property (nonatomic, copy) NSString *hobby;//属性变量
@end

@implementation Persion

- (instancetype)init {
    if (self = [super init]) {
        self->_hobby = @"爱美女";//使用成员变量赋值
    }
    return self;
}
```
可以用点操作符来使用属性

### ios属性的修饰符号

原子性： atomic，nonatomic
读写语义：readwrite，readonly，getter，setter
内存管理语义：assign，weak，unsafe_unretained，retain，strong，copy

**nonatomic** 表示非原子属性，并发访问性能高，但是访问不安全，它直接访问内存中的地址，不关心其他线程是否在改变这个值，并且中间没有死锁保护；所以可能拿不到完整的值。

**atomic** 表示原子属性，系统生成的 getter/setter 会保证 get、set 操作的完整性，不受其他线程影响。系统生成的 getter/setter 方法中，使用了@synchronized(self)如果一个线程正在执行 getter/setter，其他线程就得等待。但是如果有另一个线程同时在调 [property release]，那可能就会crash，因为 release 不受 getter/setter 操作的限制。也就是说，atomic 修饰的属性只能说是读/写安全的，但并不是线程安全的。因为别的线程还能进行读写之外的其他操作。线程安全需要开发者自己来保证。

**strong** 表示对对象的强引用，引用计数会 +1。给 strong 属性赋值时，setter 方法中会先 release 旧值再 retain 新值并赋值。两个对象之间相互强引用会造成循环引用，内存泄漏。

**weak** 表示对象的弱引用。弱引用时，不会使传入的对象计数+1；被其修饰的对象随时可能被系统销毁回收。当该对象的引用计数为 0，则会被回收，对象被释放以后，weak 指针会被自动设置为 nil。weak 多用于避免循环引用

**assign** 主要用于修饰基本数据类型。括OC基本数据类型（NSInteger，CGFloat）和C数据类型（int, float, double, char）基本数据类型存储在栈中，内存不用程序员管理。assign 也可以修饰对象，但是当对象被释放后，指针依然指向之前的内存地址。此时，访问被释放的地址就会 crash这个已经被释放了的对象被称为 僵尸对象

**copy** 建立一个和新对象内容相同且索引计数为1的对象，指针指向这个对象，然后释放指针之前指向的旧对象。NSString变量一般都用copy修饰，因为字符串常用于直接复制，而不是去引用某个字符串；





## 2.2 类实现文件

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

### **对象实例化**

对象实例化为对象分配内存，将内存初始化并保存为有用的默认值。

实例化方法：

向类发送创建对象实例的消息：

```c
[Object new];
或
[[Object alloc]init];
```
[className new]基本等同于[[className alloc] init]；区别在于alloc分配内存的时候使用了zone.在给对象分配内存的时候，把关联的对象分配到一个相邻的内存区域内，以便于调用时消耗很少的代价，提升了程序处理速度；同时使用alloc的init消息可以自定义自己的初始化方法。
```objective-c
int main (int argc, const char * argv[]) {
  Circle *circle = [[Circle alloc]init];
  [circle setFillColor: kRedColor];
}
```

### **类实现文件中的@interface定义**

.h里面的@interface，是典型的头文件，供其它Class调用。它的@property和functions，都能够被其它Class“看到”。

而.m里面的@interface，在OC里叫作Class Extension，是.h文件中@interface的补充。但是.m文件里的@interface，对外是不开放的，只在.m文件里可见。

因此，我们将对外开放的方法、变量放到.h文件中，而将不想要对外开放的变量放到.m文件中（.m文件的方法可以不声明，直接用）。

有的同学看到Class Extension，可能会想到OC里的@protocol。是的，它们都是对一个Class的扩展。不过它们的区别也很明显：

Class Extension只能用在能得到源代码的情况下，而@protocol在得不到源码的时候也可以使用。

因此@protocol一般用作对一些系统Class的扩展，常见的比如对NSString、UIView等。


# 三、类的继承--inherit

表明种类和种类中的某个具体对象之间的关系；

* 语法格式：

```objective-c
@interface Circle : NSObject
```
冒号后面的标识符是需要继承的类。如果使用cocoa框架，所有的类都继承自NSObject类。

继承实现举例：

```objective-c

//父类
@interface Shape : NSObject {
  ShapeColor fillColor;
  ShapeRect bounds;
}

- (void) setFillColor:(ShapeColor)fillColor;
- (void) setBounds:(ShapeRect)bounds;
- (void) draw;
@end

@implementation Shape
-(void)setFillColor:(ShapeColor)c {
  fillColor = c;
}

- (void) setBounds:(ShapeRect)b {
  bounds = b;
}
@end

//子类
@interface Circle : Shape
@end

@implementation Circle
-(void) draw {
    NSLog(@"");
}
@end

//子类
@interface Rectangle : Shape
@end

@implementation Rectangle

- (void) draw {
  NSLog(@"");
}
@end
```

### **继承的工作机制--方法调度：**

当实现类方法时，Objective-C的调度制度将在当前类中搜索响应的方法，如果无法在接受消息的对象的类中找到响应的方法，它就会在该对象的超类中进行寻找。必要时会在继承链的每一个类中重复执行此操作。如果在最顶层的NSObject类中也没有找到该方法，则会出现一个运行时错误。

### **重写方法**

可以定义与父类同名的方法，当向子类发送消息时，将运行重写后的方法，父类的方法会被忽略。

### **self关键字**

每个方法调用都有一个名为self的隐藏参数，该参数是一个指向接受消息的对象的指针。
```
[self draw];
```

### **super关键字**

为避免重写方法导致的父类方法被忽略，引入super关键字来调用父类的实现。

```
@implementation Circle

- (void) setFillColor: (ShapeColor)c {
  if(c == kRedColor) {
    c = kGreenColor;
  }

  [super setFillColor: c];
}

```

# 四、类的聚合（composition)

组合关系，表明一个对象和其组成部分之间的关系。

类的聚合例子：

```c

//Engine类
@interface Engine : NSObject
@end

@implement Engine

-(void) description {
    return @"";
}

//Tire类
@Interface Tire : NSObject
@end

@Implement Tire

-(void) description {
    return @"";
}
@end

//Car类
@Interface Car : NSObject
{

   Engine *engine;
   Tire *tires[4];

   -(void) print;

}
```
### **读取方法（assessors）**

进一步的拓展，通过IOC和DI方式实现依赖反转

```c
@interface Car: NSObject
{
    Engine *engine;
    Tire *tires[4];
}

-(Engine *) engine;

-(void) setEngine:(Engine *)engine;

-(Tire *) tireAtIndex:(NSInteger) index;


-(void) setTire:(Tire *) tire atIndex:(NSInteger) index;

```

Cocoa中，get和set方法可以用来设置变量的属性。存取方法的命名，cocoa有自己的惯例。
getter方法是以其返回的属性名称命名。
setter方法是set加上属性名称来命名。

### **@class关键字--向前引用**

为避免#import多次重复编译，引入关键字@class来告诉编译器，这是一个类，可以减少导入头文件的编译次数。

```c

@class Egine
@class Tire

@interface Car: NSObject

-(void) setEngine:(Engine *) engine;

......

```
具体的使用过程中，如在m文件中，还是需要导入m文件，以使

