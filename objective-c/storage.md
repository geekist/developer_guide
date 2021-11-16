

## 一、Foundation类存储到文件中

* 哪些Foundation类可以存储到文件中

NSString、NSArray、NSDictionary、NSData 可以直接保存到文件中。

NSNumber和NSDate 可以存储到plist文件中，但是没有提供直接和文件操作相关的方法。

如果要存储其它类型的数据，首先要将数据转换为这几种中的一种，然后在存储到文件中。

* Foundation类数据保存到文件中的方法：

下面通过一个例子介绍一下将以上这些基本类型保存到一个文件中的具体流程：（拿NSString类型举例）：
在main.m文件中编写以下代码：

```c

#import <Foundation/Foundation.h>

int main(int argc, const char * argv[]) {

    NSString * path=@"/Users/xieyang/Desktop/a.txt";

    NSString * string=@"c.biancheng.net";

    NSError * error=nil;

    [string writeToFile:path atomically:YES encoding:NSUTF8StringEncoding error:&error];
    return 0;
}

```
（提示：这个程序中的path字符串是文件的路径，在运行程序时，首先要确保这个路径是正确的）
 
运行后，去这个路径下找到a.txt文件，会发现成功的把string字符串添加到文件中（无论文件中之前有什么，运行之后就只有这个字符串）
 
下面我们来分析一下这段程序的思路：

因为我们是将字符串保存到一个文件中，所以首先我们要获取这个文件的绝对路径。（获取一个文件的路径一个非常简单的方法：打开命行，然后把这个文件拖进去，松开鼠标，你就会获取这个文件的路径）。

 
获取到路径后，我们需要知道保存的对象，也就是字符串；
          
两项工作做完后，可以通过：

```c

-(BOOL)writeToFile:(NSString*)path
atomically:(BOOL)useAuxiliaryFile encoding:(NSStringEncoding)enc error:(NSError **)error;

```

这个方法（NSString类接口中的方法）将字符串保存到path路径中的a.txt文件中。
 
在程序中还定义了一个NSError类的对象，这是因为在保存方法中有一个参数，需要这个对象的地址，所以我们需要提前声明，然后将这个对象的地址传给上面的方法。
 
以上就是将字符串保存到本地文件的过程。对于数组，字典等同样是这个过程。下面再给大家一个数组的例子：

在main.m文件中编写以下代码：

```c
#import <Foundation/Foundation.h>

int main(int argc, const char * argv[]) {

    NSString * path=@"/Users/xieyang/Desktop/a.txt";

    NSArray * array=[NSArray arrayWithObjects:@"1",@"2",@"3",@"4",nil];

    [array writeToFile:path atomically:YES];

    return 0;
}

```
将路径更改为自己本机的路径后运行，就可以看到已经把数据存进去了
 

* 从文件中读取Foundation类


通过上面的两个例子，我们就可以往其他类上扩展，能够向本地文件中保存数据，还要学会从文件中取数据，下面来学习一下怎么取数据（拿上边存进去的数组为例）：

```c

#import <Foundation/Foundation.h>

int main(int argc, const char * argv[]) {

    NSString * path=@"/Users/xieyang/Desktop/a.txt";

    NSArray * array=[NSArray arrayWithContentsOfFile:path];

    NSLog(@"%@",array);

    return 0;
}
```
运行结果：(
    1,
    2,
    3,
    4
)
我们可以看到，只要知道文件的路径，告诉文件中数据的类型，就可以将文件中的数据取出来。其它类型也是这样，不再做举例。


## 二、自定义类存储到文件中：NSKeyedArchiver和NSKeyedUnarchiver
  
本节我们讲学习如何将创建的对象存储到本地文件中。存储对象，Foundation框架中提供了一个专门实现这个功能的类：NSKeyedArchiver和NSKeyedUnarchiver。

通过这两个类，我们就能够轻松地实现对对象进行存储和读取的功能，OC中对于对象的本地存储和读取有另外的称呼：


* 归档和解档

将对象存储到本地文件中被称作：归档。

从本地文件中读取数据的操作被称为：解档或者恢复。

* NSCoding协议

服从NSCoding协议：Foundation框架规定，对于每一个要实现将自定义对象存储到本地文件的类，都要服从NSCoding协议。
 
既然服从了NSCoding协议，就要实现协议中的用@required修饰的方法(简单的理解，就是如果要将一个对象进行归档或者解档，那么这个对象中的属性也必须要进行归档或者解档)，所以这个协议中需要进行实现的只有两个方法；

```c

-(void)encodeWithCoder:(NSCoder *)aCoder ：从方法的命名就可以看出，这是归档的方法。

-(instancetype)initWithCoder:(NSCoder *)aDecoder ：这是解档用到的方法。

```

* 定义一个可以存储的类

Person.h中的代码：

```c

#import <Foundation/Foundation.h>

@interface Person : NSObject<NSCoding>

@property (nonatomic,copy)NSString * name;

@property (nonatomic,assign)int age;

@property (nonatomic,assign)BOOL sex;

@end

```

Person.m中的代码：

```c

#import "Person.h"

@implementation Person

-(void)encodeWithCoder:(NSCoder *)aCoder{

//在这个方法中要对类中声明的每个属性都要进行归档。
    [aCoder encodeObject:self.name forKey:@"name"];
    [aCoder encodeBool:self.sex forKey:@"sex"];
[aCoder encodeInt:self.age forKey:@"age"];
}


-(instancetype)initWithCoder:(NSCoder *)aDecoder{
    self.name=[aDecoder decodeObjectForKey:@"name"];

    self.sex=[aDecoder decodeBoolForKey:@"sex"];

    self.age=[aDecoder decodeIntForKey:@"age"];

//通过查找键找出对应的属性值，而这个键值就是在归档时候设置的值。
return self;

}
@end

```
* 归档一个对象

通过代码中解释，对于NSCoding协议应该有了一个基本的认识。通过上面的工作，Person类的代码已经编写完成了，下面我们通过一个例子来看一下怎么通过归档的方式将Person类的对象保存到本地文件中：
 
在main.m文件中编写以下代码：

```c

#import <Foundation/Foundation.h>

#import "Person.h"

int main(int argc, const char * argv[]) {

    Person * person1=[[Person alloc] init];

    person1.name=@"ZhangSan";

    person1.sex=1;

    person1.age=10;

    NSString * path=@"/Users/xieyang/Desktop/a.txt";

    [NSKeyedArchiver archiveRootObject:person1 toFile:path];//这行代码将person1对象归档到path这个路径下的文件中

    return 0;
}
```

（提示：在mac下，如果你想知道某个文件的绝对路径，有一个简便方法：打开命令行，将文件直接拖到命令行下，你就可以直接得到这个文件的绝对路径）


* 解档一个对象
 
通过运行上边的代码，完成了将person1对象存储到a.txt文件中，下面来读取一下，尝试从a.txt文件中得到person1对象：
 
在main.m文件中编写以下代码：

```c
#import <Foundation/Foundation.h>

#import "Person.h"

int main(int argc, const char * argv[]) {

    NSString * path=@"/Users/xieyang/Desktop/a.txt";

    Person * person=[NSKeyedUnarchiver unarchiveObjectWithFile:path];//在解档的时候，我们首先要找到文件，然后通过这个方法直接提取文件中的的对象

    NSLog(@"the person's name is :%@,sex is :%d,age is :%d",person.name,person.sex,person.age);
    
    return 0;
}

```

输出结果：the person's name is :ZhangSan,sex is :1,age is :10
 
* 归档多个对象
 
通过以上内容的学习，掌握了关于如何存储一个对象，那么如何储存多个对象呢，我们来继续学习一下：
 
在main.m文件中编写以下代码：

```c

#import <Foundation/Foundation.h>

#import "Person.h"

int main(int argc, const char * argv[]) {

    Person * person1=[[Person alloc] init];
    person1.name=@"ZhangSan";
    person1.sex=1;
    person1.age=10;
   
    Person * person2=[[Person alloc] init];
    person2.name=@"LiSi";
    person2.sex=1;
    person2.age=20;

    //存储文件的路径
    NSString * path=@"/Users/xieyang/Desktop/a.txt";
    NSMutableData * data=[NSMutableData data];
    NSKeyedArchiver * archiver=[[NSKeyedArchiver alloc] initForWritingWithMutableData:data];
    [archiver encodeObject:person1 forKey:@"person1"];
    [archiver encodeObject:person2 forKey:@"person2"];
    [archiver finishEncoding];
    [data writeToFile:path atomically:YES];
    return 0;
}

```

代码分析
既然要练习存储多个对象，首先要建立至少两个对象person1和person2。
然后要确定存储对象的文件的路径path。
NSMutableData * data=[NSMutableData data];    NSMutableData是NSData类型的可变类型，通过看这个类提供的方法，我们很容易看到data类方法，它返回的是本类的对象（实际上是一个单例方法）。
 
NSKeyedArchiver * archiver=[[NSKeyedArchiver alloc] initForWritingWithMutableData:data];这行代码的作用是将归档对象和data可变类型相连接，这样的好处就是在后边归档的时候，数据都会保存在data对象中（这也是为什么使用二进制类型的可变类型的原因，归档一个，就添加一部分数据）。
[archiver encodeObject:person1 forKey:@"person1"];
[archiver encodeObject:person2 forKey:@"person2"];
上边两行分别对person1和person2两个对象进行归档。
[archiver finishEncoding];这行代码是结束归档的标志，当你归档完对象之后，一定要调用一下这个方法，否则会出错。
最后将归档完成获得的data存入文件中。

* 解档多个对象

通过上边的代码，我们可以将多个Person类的实例对象保存到本地文件中，那么我们再怎么读取呢？
 
在main.m文件中编写以下代码：

```c

#import <Foundation/Foundation.h>
#import "Person.h"
int main(int argc, const char * argv[]) {

    NSString * path=@"/Users/xieyang/Desktop/a.txt";

    NSData * theData=[NSData dataWithContentsOfFile:path];

    NSKeyedUnarchiver * unarchiver=[[NSKeyedUnarchiver alloc]initForReadingWithData:theData];
   
    Person * thePerson1=[unarchiver decodeObjectForKey:@"person1"];

    NSLog(@"the person1's name is :%@,sex is :%d,age is :
%d",thePerson1.name,thePerson1.sex,thePerson1.age);
   
    Person * thePerson2=[unarchiver decodeObjectForKey:@"person2"];
   
    NSLog(@"the person2's name is :%@,sex is :%d,age is :%d",thePerson2.name,thePerson2.sex,thePerson2.age);
  
    return 0;
}

```
输出结果：the person1's name is :ZhangSan,sex is :1,age is :10
       the person2's name is :LiSi,sex is :1,age is :20
 
解档代码分析
首先我们要知道解档文件的路径path，而且确定这个文件中的数据是通过上面的方式归档的；
由于采用上边的方式归档的是一个NSMutableData类型的数据，所以我们可以用NSData类型的对象去接。
获得二进制数据之后，由于我们无法直接对二进制数据进行操作，所以还得先将数据解档才行。
解档之后我们就可以通过这个decodeObjectForKey方法根据键来查找对应的对象
 
如果归档对象过多时，可以考虑将这些自定义对象保存到数组中，然后通过保存数组的方式存储过多的对象；当需要提取出这些数据时，可以将采用遍历数组的方式将自定义对象提取出来。
 
通过本节内容的学习之后，我们就可以对数据(包括基本数据和对象数据)进行永久的保存，同时，也能够实现对文件中的数据进行读取。

## 三、Plist文件

在实际开发过程中，对于存取小的数据而言，最常用的就是plist文件，它的全称是：Property List。

* plist文件保存的数据类型：

NSString、

NSArray、

NSData、

NSDate、

NSDictionary、

BOOL、

NSNumber

这几种数据类型。
 
* plist文件特点和优势
 
plist文件经常用作保存用户的登录注册信息，或者程序的配置信息，总体来说保存的是一些小数据。
      
plist文件中的数据还有一个特点，那就是里边的数据都要用字典或者是数组包裹起来。
 
* 创建plist文件
 
首先我们来创建一个plist文件：

1、右键点击Demo文件夹，选择“New File”，弹出创建文件的对话框：


2、按上图选择后，“Next”，给新创建的plist文件起一个名字，例如test。
 
做完上述步骤后，你会发现功能框架中多了一个test.plist文件。
 
通过本章对数据本地存储的学习，完全可以通过代码向test.plst文件中添加数据，前提是这个数据本身必须是一个数组或者是字典(通过观察新创建的plist文件，就会发现它是以字典或数组开头)。
 
下面举一个例子，在main.m文件中编写以下代码：

```c

#import <Foundation/Foundation.h>

int main(int argc, const char * argv[]) {

    NSArray * array=[NSArray arrayWithObjects:@"1",@"2",@"3",@"4",@"5", nil];

    NSString * path=@"/Users/xieyang/Documents/Demo/Demo/test.plist";

    [array writeToFile:path atomically:YES];

    return 0;
}
```
结果你会发现新创建的plist文件中出现了数组中的数据：

同样，还可以通过代码将plist文件中的数据获取到一个数组中。
 
编写代码创建并使用plist文件
 
以上是我们自己创建plist文件的方法，同样，还可以通过代码的方式，添加plist文件，由于添加文件有很多方式，所以这里只给大家介绍一种最方便且常用的方法，看下面的例子：

```c

#import <Foundation/Foundation.h>
int main(int argc, const char * argv[]) {

    NSString * path=@"/Users/xieyang/Documents/Demo/Demo";

    NSString * filePath=[path stringByAppendingPathComponent:@"data.plist"];

    NSLog(@"%@",filePath);

    NSArray * array=[NSArray arrayWithObjects:@"1",@"2",@"3",@"4", nil];

    [array writeToFile:filePath atomically:YES];

    return 0;
}

```
（这段代码的大概流程是：首先要知道创建plist文件的父目录，然后通过在父目录添加plist文件名（使用的方法是添加路径的方法，不是添加字符串）的方式创建这个文件，最后向这个plist文件添加数据）
 
注意
1、在父目录下通过上面的方法添加plist文件名，如果这个文件没有找到，会生成plist文件，如果存在，会直接拿来用。
2、还有一点，如果是用代码方法新创建的plist文件，如果不存入数据，plist文件不会显示。

## 四、用户默认设置-- NSUserDefaults

在实际开发过程中，有一种能够自行保存数据的方式，通过深入地了解，我们会发现这种保存数据的方式同样是将数据保存到plist文件中，通常管这种存储小型数据的方式：用户默认设置。
 
举一个例子，我们通常使用软件，第一次使用的时候，它会让我们输入登录信息。而以后使用的时候，它直接就帮我们登陆上了，不用每次使用都输入登录信息。这个功能，开发人员是怎么实现的呢？
 
通过下面的例子，模拟一下：

```c
#import <Foundation/Foundation.h>

int main(int argc, const char * argv[]) {

    if ([[[NSUserDefaults standardUserDefaults] objectForKey:@"logined"] isEqualToString:@"OK"]) {

        NSLog(@"您登陆过");

    }else{
        NSLog(@"你第一次登陆");

        NSUserDefaults * userDefaults=[NSUserDefaults standardUserDefaults];

        [userDefaults setObject:@"OK" forKey:@"logined"];

    }
       return 0;
}

```
第一次运行，输出结果：你第一次登陆
第二次运行，输出结果：您登陆过
第n次运行，输出结果：您登陆过
 
可以看到，问题很容易地就被解决了。我们来分析一下，这几行代码是怎么实现的：
首先，要学会几个知识点：

1、NSUserDefaults，这个类是专门用作存储用户默认设置的(从名字就可以了解到)，这个类，我们只需要知道有数的几个方法就可以。
2、standardUserDefaults是一个类方法，通过查询它，可以看到，他返回的本类的一个对象，这是一个单例(上一章讲过)。通过这个方法，我们能过获得唯一的一个对象。
3、此外，我们只需要知道这个类的两个方法，就能实现登录的功能：
      setObject:@"OK" forKey:@"logined":可以看到，用户默认设置是通过键值对的方式存储数据(和字典的使用类似)
      objectForKey:@"logined":通过键，得到相应的值
 
通过上面的讲解，模拟登录的例子应该就能看懂了，我给大家简单地梳理一下编程思路，大家可以跟着编程思路独立实现一下登录的功能：

1、判断用户是否登录过(使用NSUserDefaults类通过键查找对应的登陆信息，由于是自己设计的软件，所以登录信息的键值对是提前设计好的)

2、如果登录过，就跳过登录界面，直接进入软件主界面。

3、如果没登过，跳转到登陆界面，如果用户输入的登录信息正确，通过NSUserDefaults类提供的方法将信息进行保存
                                                                                                                                                                               
通过上面的学习，可能你会有疑问，NSUserDefaults类将登陆信息保存到哪了呢？
 
其实，当你通过这种方式存储数据的时候，代码底层帮你在某个地方创建了一个plist文件，对于你现在这台机器来说，每次当你运行这个程序时，都会去调取文件中的数据。
                               
下面带着大家一起来将NSUserDefaults创建的plist文件找出来：
          
首先，请大家先在main.m文件中原有代码下面拷贝一下这段代码：
 
```c
NSString * path=[NSSearchPathForDirectoriesInDomains(NSLibraryDirectory, NSUserDomainMask, YES) lastObject];

NSLog(@"%@",path);
```
 
这段代码是获取Library文件夹路径的代码，运行程序，看到path输出一个路径，我们通过这个路径找到响应的文件夹：


找到资源库之后，我们找到Preferences文件夹：


进入这个文件夹之后，我们通过搜索我们的工程名就能找到创建的plist文件：


进入plist文件就能看到我们存储的相关数据，例如，这个例子中的数据就储存在这个文件中：



通过亲身查找这个隐藏创建的plist文件，相信大家通过NSUserDefaults类保存用户数据有了更深入的认识。对于NSUserDefaults类来讲，只需要大家记住：

1、运用类中提供的单例，来获取唯一的NSUserDefaults类对象。
2、学会通过键值对的方式存取数据。


 






















