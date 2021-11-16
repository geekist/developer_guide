
## 基本术语和用法



#import <Foundation/Foundation.h>



## import命令

用来引入头文件。与C语言中的#include相比，#import 能够保证头文件只被引入一次，也就是说，只有第一次引入有效，后续对同一文件的引入都会被忽略。


## Foundation 

是OC语言的基础框架，它提供了OC专有的基本数据类型（如字符串、数组、时间日期等），提供了多线程、网络连接、文件操作、本地数据存储等常用功能。Foundation.h 就像C语言中的 stdio.h，一般都需要引入。



## main() 

是程序的入口函数，这点和C语言一样。一个OC程序有且只能有一个 main() 函数。更多信息请查看《分析第一个C语言程序》。


## NSLog() 

是OC中的格式化输出函数，相当于C语言中的 printf() 的升级版，不仅可以用来输出C语言中的数据，还可以输出OC中的数据。

## @符号

没有实际含义，只是用来作为OC字符串的特有标志。在OC中，字符串前面都要加@，如果不加，就变成了C语言中的字符串，就要使用字符指针或字符数组来定义，这在后面会详细讲解。

```c

#import <Foundation/Foundation.h>

int main() {
    int a = 100;
    float b = 39.52;
    char *str1 = "Objective-C";  //C语言中的字符串
    NSString *str2 = @"http://c.biancheng.net";  //OC中的字符串
    NSLog(@"a: %d\nb: %f\nstr1: %s\n str2: %@\n", a, b, str1, str2);

    return 0;
}

```

## objective-c中类和对象的定义

OC中的类分为声明和实现两个部分。OC中对类的声明和实现通常是在不同文件中写的：

### 类的声明

类的声明部分写在.h文件中，h文件中的属性和方法，外界可以访问

类的声明：OC中用关键字@interface来声明一个类，用@end结束类的声明


e.g  student.h类声明

```c
#import <Foundation/Foundation.h>

//声明类Student
@interface Student : NSObject  //通过@interface关键字来声明类

//类所包含的变量
@property NSString *name;
@property int age;
@property float score;

//类所包含的函数
-(void)display;


@end  //使用@end关键字结束类的声明


```

### 类的实现


对类的实现写在.m文件中，在.m文件声明的属性或方法外界无法访问。

类的实现： OC中用关键字@implementation来实现类中的函数


```c

//实现在类中声明的函数

@implementation Student  //通过@implementation关键字来实现类中的函数

-(void)display{
    NSLog(@"%@的年龄是 %d，成绩是 %f", self.name, self.age, self.score);
}

@end  //使用@end关键字结束类中函数的实现


```

### 属性 



### 方法 


### 成员

在OC中，类所包含的变量和函数都有特定的称呼，变量被称为属性（Property），函数被称为方法（Method），属性和方法统称为类的成员（Member）。

从上面的代码可以看出，声明类的属性要使用@property关键字，而声明类的方法不需要使用任何关键字。




























