
## 一、Foundation框架简介

其实Foundation框架就是一个Foundation.h文件。和其它.h文件不同的是，Foundation.h文件中包含的全是头文件(接口)。如果要在编程过程中使用这个类，那么首先要在程序开始之前引入这个类的.h文件，引入之后，才可以对这个类进行操作。
 
Foundation.h文件全是其他类的.h文件。那么就应该明白，其实Foundation框架就是通过一个.h文件将苹果官方工程师们写的各个类的接口文件包裹起来，全部包含在一个.h文件中，给这个文件起了个名字，叫Foundation.h文件，又由于它符合框架的定义，所以Foundation.h文件又叫做Foundation框架。

## 二、Foundation框架涉及的内容

Foundation框架涉及的领域
 
* 提供了OC专有基本数据类型


<Foundation/NSArray.h>

 <Foundation/NSData.h>

<Foundation/NSDate.h>

<Foundation/NSString.h>

<Foundation/NSDictionary.h>

 
* 提供了和网络连接相关功能的接口类


<Foundation/NSURLConnection.h>

<Foundation/NSURLRequest.h>

<Foundation/NSURL.h>

 
* 提供了关于本地数据存储相关功能的接口类


<Foundation/NSUserDefaults.h>

 
* 提供了对文件操作相关的接口类，例如移动文件等

<Foundation/NSFileManager.h>

 
* 提供了和多线程相关的接口类

<Foundation/NSThread.h>


## 三、Foundation类介绍

### 3.1 NSString和NSMutableString

NSString类型相当于C语言中的char *，代表的是字符串类型。在OC中，字符串的表示方式和C语言略有不同。在C语言中，字符串的指示标志是“”，而在OC中，字符串的指示标志为@“”。
 
NSString这个数据类型在Foundation框架中的NSString.h文件中，大家可以进入文件中查看。

* NSString的创建
 
在NSString类中提供了创建对象的几种方式：

```c

+ (instancetype)stringWithString:(NSString *)string;
 

+ (instancetype)stringWithFormat:(NSString *)format


- (instancetype)initWithString:(NSString *)aString;


```
 
（提示：instancetype和id的意义是一样的，但是这不用id的原因，是instancetype能够返回本类的对象，而如果使用id的话，就是任意类型的对象。对于这两个的使用：在类中需要返回本类对象的时候就使用instancetype，在其他地方就使用id）。
 
当运用这两个方法初始化对象时，Xcode会提示让我们直接使用：NSString * str=@"";这种形式。

可见，NSString这个类型是可以直接赋值的，但是它还是遵循我们的原则的，必须对对象进行分配空间初始化。
 
+ (instancetype)stringWithFormat:(NSString *)format

这个类方法非常的强大，我们可以使用它来将一些外界的变量或常量与任意字符串进行组合，将组合成的新的字符串赋值给新声明的字符串对象，如：

```c


#import <Foundation/Foundation.h>

int main() {

    int count=5;

    NSString * str=[NSString stringWithFormat:@"The count is :%d",count];

    NSLog(@"%@",str);

    return 0;
}


```


* 判断字符串是否相等
 
NSString类中，提供了一个比较字符串是否相等的方法：

```c

- (BOOL)isEqualToString:(NSString *)aString;

```
 
这个方法返回的是一个BOOL类型的值，可以赋值给一个整形变量，如果相等，返回1；反之，返回0。

```c
#import <Foundation/Foundation.h>
int main() {
       NSString * str1=@"abc";
       NSString * str2=@"ABC";
       int equal=[str1 isEqualToString:str2];
       NSLog(@"%d",equal);
       return 0;
}
```
输出结果为：0
          
从这个程序中，可以得到这样的信息：这个方法默认是区分大小写比较的。
 
* 字符串比较
 
除了判断是否相等以外，NSString类还提供了两个字符串进行大小比较的方法：

```c

- (NSComparisonResult)compare:(NSString *)string;

```

注意：这个方法返回的是一个枚举类型的数值，这个枚举的定义是这样的：

```c

typedef NS_ENUM(NSInteger, NSComparisonResult)
{NSOrderedAscending = -1L, NSOrderedSame, NSOrderedDescending};
 
NSOrderedAscending  = -1,   左边小于右边
NSOrderedSame        = 0,   两边相等
NSOrderedDescending  =1     左边大于右边

```
 
方法使用实例：

```c

#import <Foundation/Foundation.h>
int main() {
       NSString * str1=@"abc";
       NSString * str2=@"ABC";
       int equal=[str1 compare:str2];
       NSLog(@"%d",equal);
       return 0;
}

```
输出结果：1
 
可以看出，这个方法默认也是区分大小写的。在NSString类中还有更多的比较的方法（例如：可以自己选择比较的条件）。
 
* 判断字符串内是否包含另一字符串
 
NSString类中还提供了判断字符串之间是否相符包含的方法：

```c

- (BOOL)containsString:(NSString *)str

```
 
例子：
#import <Foundation/Foundation.h>
int main(int argc, const char * argv[]) {
    NSString * str1=@"a10b9c";
    NSString * str2=@"b9c";
    int i=[str1 containsString:str2];
    NSLog(@"%d",i);
    return 0;
}

输出结果：1
 
（注意：NSString是不可变的，但是这并不意味着你不能操作他们。不可变的意思是一旦NSString对象被创建了，就不能对原有字符串进行增删改的操作。除了对原有字符串操作的限制外，例如：生成新的字符串，比较字符串等操作都可以做。）
 
* NSMutableString的使用
 
虽然NSString是不可变的，但是Foundation框架还给我们提供了一类可变的字符串类型：NSMutableString。
 
NSMutbaleString这个类中，提供了在原有字符串基础上进行增删改的方法。
   
在原有字符串上添加一个字符串：

```c

- (void)appendString:(NSString *)aString;

- (void)appendFormat:(NSString *)format, ...
 
-(void)insertString:(NSString*)aString atIndex:(NSUInteger)loc;

```
 
Foundation框架中有很多方法都是可以从英文角度翻译出它的大概功能的，例如，这个方法的意思是：在loc索引处插入astring字符串。(索引：简单的理解，就是对字符串进行一对一标记，从0开始，例如：@“abc”这个字符串，a的索引就是0，b的索引是1，c的索引是2)
 
例子：

```c

#import <Foundation/Foundation.h>
int main() {
    NSMutableString * str=[NSMutableString stringWithString:@"c.net"];
    [str insertString:@"biancheng." atIndex:2];
    NSLog(@"%@",str);
    return 0;
}

```
输出结果：c.biancheng.net
 
注意：可变字符串不能直接赋值。必须给其分配空间。
 
* 在原有字符串上删除特定部分的字符串：

```c
- (void)deleteCharactersInRange:(NSRange)range;
```
 
补充：在这里用到NSRange这个类型，NSRange是Foundation给我们提供的创建的结构体，它的结构是这样的：

```c
typedef struct _NSRange {

NSUInteger location;

NSUInteger length;

} NSRange;
```
 
可以看到，只含有两个无符号长整形变量，location是当前位置，length是长度。
 
在使用这个方法时，我们首先要创建一个NSRange的变量，将之作为参数传递进去，
例子：

```c

#import <Foundation/Foundation.h>

int main() {
    NSMutableString * str=[NSMutableString stringWithString:@"c.biancheng.net"];
    NSRange range;
    range.location=2;
    range.length=9;
    [str deleteCharactersInRange:range];
    NSLog(@"%@",str);
    return 0;
}

```
输出结果为：c..net


### 3.2 NSArray和NSMutableArray


* 创建NSArray

```c

+ (instancetype)arrayWithObjects:(ObjectType)firstObj, ...

- (instancetype)initWithObjects:(ObjectType)firstObj, ...


```

使用方法：

```c


#import <Foundation/Foundation.h>
int main() {
       NSArray * array1=[NSArray arrayWithObjects:@"0",@"1",@"2",@"3", nil];

       NSLog(@"array1 are :%@",array1);

       NSArray * array2=[[NSArray alloc] initWithObjects:@"00",@"11",@"22",@"33", nil];

       NSLog(@"array2 are :%@",array2);

       return 0;
}     

```

* NSArray的使用注意事项：

1、数组中存储的必须是对象(OC中的任意对象都可以，但是对于C语言中的int、double、float等不允许存储到NSArray数组里)。
 
2、数组中不允许存储nil。(因为NSArray存储数据结束的标志就是nil，如果在数组中存储了nil，就会产生冲突，从而丢失数据)。

* count属性和count方法

```c

#import <Foundation/Foundation.h>

int main() {

       NSArray * array1=[NSArray arrayWithObjects:@"0",@"1",@"2",@"3", nil];

       NSInteger count=[array1 count];//也可以用array1.count

       NSLog(@"array1 are :%@ andTheCount is :%ld",array1,count);

       return 0;
}

```

* 获取特定索引处的对象

```c

- (ObjectType)objectAtIndex:(NSUInteger)index;

```

例子：

```c

#import <Foundation/Foundation.h>

int main() {

       NSArray * array1=[NSArray arrayWithObjects:@"0",@"1",@"2",@"3", nil];

       NSString * str=[array1 objectAtIndex:1];

       NSLog(@"%@",str);

       return 0;
}
```
输出结果为：1
 
分析：对于OC中NSArray的数组，和C语言中数组的索引是一样的，都是从0开始。所以，对于array1这个数组来说，索引为1的对象是@“1”。
 
注意：在使用索引时，一定要注意索引是否存在，如果索引不存在，就会产生越界的情况。例如对于array1这个数组来说，他的索引范围是0-3(nil只是作为结束标注，本身并不包括在数组中)，如果你引入的索引是4，就会出错。所以，使用索引时一定要注意索引是否有意义。


* 判断一个对象是否包含在一个数组中

```c

- (BOOL)containsObject:(ObjectType)anObject;
```

```c

#import <Foundation/Foundation.h>

int main() {

       NSArray * array1=[NSArray arrayWithObjects:@"0",@"1",@"2",@"3", nil];

       NSLog(@"%d",[array1 containsObject:@"1"]);

       return 0;

}
```

输出结果：1
 
分析：由于该方法返回的是一个BOOL类型的值，之前我们介绍过BOOL类型的返回值如果是1，说明是真的；反之，是假的。
 
除了以上为大家介绍的方法，NSArray.h文件中还提供了实现各种功能的方法。我们大致可分为这么几类：

1、排序方法。

2、知道对象，获得该对象的索引的方法

3、获取某文件中的数组的方法。

……

这些方法我们会在后边的学习中都接触到。有兴趣的朋友可以进入NSArray.h文件中去尝试使用这些方法。
 
* NSMutableArray的使用
 
和NSString一样，除了有不可变的NSArray，Foundation还提供了可变的数组NSMutableArray。可变数组比不可变数组增加了对数组中对象的增删改的功能：
 
* 向数组中增加对象
 
增加一个对象的方法：

```c

- (void)addObject:(ObjectType)anObject;

```

例子：

```c
#import <Foundation/Foundation.h>

int main() {

    NSMutableArray * array=[NSMutableArray arrayWithObjects:@"1",@"2",@"3", nil];

    [array addObject:@"4"];

    NSLog(@"%@",array);
   
    return 0;
}
```


 
* 一次增加多个对象的方法：
 
```c

- (void)addObjectsFromArray:(NSArray<ObjectType> *)otherArray;

```
例子：

```c

#import <Foundation/Foundation.h>
int main() {
    NSMutableArray * array=[NSMutableArray arrayWithObjects:@"1",@"2",@"3", nil];
    NSArray * addArray=[NSArray arrayWithObjects:@"4",@"5",@"6", nil];
    [array addObjectsFromArray:addArray];
    NSLog(@"%@",array);
    return 0;
}
```

输出结果：
(     
       1,
       2,
       3,
       4,
       5,
       6
)
 

* 删除数组中的对象

```c

- (void)removeAllObjects;  //删除所有对象

- (void)removeObject:(ObjectType)anObject; //删除某个对象：


```

例子：

```c
#import <Foundation/Foundation.h>
int main() {
       NSMutableArray * array=[NSMutableArray arrayWithObjects:@"1",@"2",@"3",@"4",@"5",@"6", nil];
    [array removeObject:@"2"];
    NSLog(@"the array is :%@",array);
    [array removeAllObjects];
    NSLog(@"the last array is :%@",array);
    return 0;
}
```

输出结果：
the array is :(
                  1,
                  3,
                  4,
                  5,
                  6
              )
the last array is :(
                     )
 
* 对对象进行更改：

```c

 replaceObjectAtIndex:2 withObject:@"7"

```
 
```c

#import <Foundation/Foundation.h>
int main() {

    NSMutableArray * array=[NSMutableArray arrayWithObjects:@"1",@"2",@"3",@"4",@"5",@"6", nil];

    [array replaceObjectAtIndex:2 withObject:@"7"];

    NSLog(@"%@",array);

    return 0;
}

```

### 3.3 NSDictory和NSMutableDictory

* 创建NSDictory

```c

+(instancetype)dictionaryWithObject:(ObjectType)object forKey:(KeyType <NSCopying>)key;//向字典中存储一个键值对

+(instancetype)dictionaryWithObjects:(NSArray<ObjectType> *)objects forKeys:(NSArray<KeyType <NSCopying>> *)keys;//向字典中存储多个键值对

+(instancetype)dictionaryWithObjectsAndKeys:(id)firstObject, ...

- (instancetype)initWithObjectsAndKeys:(id)firstObject, ...


```

例子：

```c

#import <Foundation/Foundation.h>
int main() {

    NSDictionary * dic1=[NSDictionary dictionaryWithObject:@"object1" forKey:@"key1"];

    NSLog(@"dic1 has :%@",dic1);
   
    NSDictionary * dic2=[NSDictionary dictionaryWithObjects:[NSArray arrayWithObjects:@"object1",@"object2",@"object3", nil] forKeys:[NSArray arrayWithObjects:@"key1",@"key2",@"key3", nil]];
    
NSLog(@"dic2 has :%@",dic2);
   
    NSDictionary * dic3=[NSDictionary dictionaryWithObjectsAndKeys:@"object1",@"key1",@"object2",@"key2",@"object3",@"key3", nil];
    
NSLog(@"dic3 has :%@",dic3);
   
    NSDictionary * dic4=[[NSDictionary alloc] initWithObjectsAndKeys:@"object1",@"key1",@"object2",@"key2",@"object3",@"key3", nil];
    
NSLog(@"dic4 has :%@",dic4);
    
return 0;

}

```

* count属性和count

可以用来获取字典对象中键值对的数量(和NSArray中count的用法相同，不再举例)。

* 比较两个字典是否相等：

```c
          
- (BOOL)isEqualToDictionary:(NSDictionary<KeyType, ObjectType> *)otherDictionary;

```
 
例子：

```c

#import <Foundation/Foundation.h>
int main() {

NSDictionary * dic2=[NSDictionary dictionaryWithObjects:[NSArray arrayWithObjects:@"object1",@"object2",@"object3", nil] forKeys:[NSArray arrayWithObjects:@"key1",@"key2",@"key3", nil]];

    NSDictionary * dic3=[NSDictionary dictionaryWithObjectsAndKeys:@"object1",@"key1",@"object2",@"key2",@"object3",@"key3", nil];
    
int i=[dic2 isEqualToDictionary:dic3];
    
NSLog(@"%d",i);
    
return 0;

}

```
输出结果：1

* 从字典中根据键找出对应的值；

```c

#import <Foundation/Foundation.h>

int main() {
    NSDictionary * dic2=[NSDictionary dictionaryWithObjects:[NSArray arrayWithObjects:@"object1",@"object2",@"object3", nil] forKeys:[NSArray arrayWithObjects:@"key1",@"key2",@"key3", nil]];

    NSString * str=[dic2 objectForKey:@"key1"];

    NSLog(@"%@",str);

    return 0;
}
```
输出结果：object1

* NSDictionary还提供了allKeys和allValues两个属性，可以通过点语法调用这两个属性来获取字典中的所有键和所有值。这两个属性返回的都是一个数组，所以需要用数组去获取。

 
例子：

```c

#import <Foundation/Foundation.h>
int main() {
    NSDictionary * dic2=[NSDictionary dictionaryWithObjects:[NSArray arrayWithObjects:@"object1",@"object2",@"object3", nil] forKeys:[NSArray arrayWithObjects:@"key1",@"key2",@"key3", nil]];
    NSArray * keys=dic2.allKeys;
    NSArray * objects=dic2.allValues;
   
    NSLog(@"all keys are :%@",keys);
    NSLog(@"all values are :%@",objects);
    return 0;
}
```

输出结果为：
all keys are :(
    key1,
    key3,
    key2
) all values are :(
    object1,
    object3,
    object2
)
 
* NSMutableDictionary的使用
 
NSDictionary也有可变的类型：NSMutableDictionary。用可变字典可以随意添加、删除或者修改键值对。


* 向可变字典中添加键值对

```c

- (void)setObject:(ObjectType)anObject forKey:(KeyType <NSCopying>)aKey;

```
例子：

```c

#import <Foundation/Foundation.h>

int main() {

    NSMutableDictionary * dic=[[NSMutableDictionary alloc] initWithObjectsAndKeys:@"object1",@"key1",@"object2",@"key2", nil];

    [dic setObject:@"object3" forKey:@"key3"];

    NSLog(@"%@",dic);

    return 0;
}
```
输出结果：
{
       key1 = object1;
       key2 = object2;
       key3 = object3;
}
 
* 删除可变字典中的键值对

```c

- (void)removeObjectForKey:(KeyType)aKey;

```

例子：

```c

#import <Foundation/Foundation.h>
int main() {

    NSMutableDictionary * dic=[[NSMutableDictionary alloc] initWithObjectsAndKeys:@"object1",@"key1",@"object2",@"key2", nil];
    [dic removeObjectForKey:@"key1"];
    
NSLog(@"%@",dic);
    
return 0;

}

```

输出结果：
{
    key2 = object2;
}

### 3.4 NSDate


NSDate类是用来获取时间的一个类，在我们实际开发中有时会需要获取当前的时间，这时就需要使用这个类。

* 创建NSDate类

```c

+ (instancetype)date;


```

```c

#import <Foundation/Foundation.h>
int main() {
       NSDate * date=[NSDate date];
       NSLog(@"%@",date);
       return 0;
}

```

* NSDate和NSDateDormatter配合使用获取各种格式的时间

```c

#import <Foundation/Foundation.h>
int main(int argc, const char * argv[]) {

    NSDate * date=[NSDate date];

    NSDateFormatter * formatter=[[NSDateFormatter alloc]init];

    [formatter setDateFormat:@"YYYY-MM--dd"];

    NSString * string=[formatter stringFromDate:date];
NSLog(@"%@",string);

[formatter setDateFormat:@"YYYY-MM--dd hh:mm"];

    string=[formatter stringFromDate:date];

    NSLog(@"%@",string);

    return 0;

}

```

### 3.5 NSData


* 应用领域

NSData类，功能是操作二进制数据。

 在某些情况下，需要将字符串转换成二进制进行存储或者做其他用处时，需要使用NSData和NSString之间进行互换。

 这种情况是最常见的：当从网上下载数据（例如图片）时，通常返回的是二进制数据，那么就需要通过NSData类生成的对象去接，然后通过NSData类中提供的方法进行适当的类型转换，才能算真正的获取到数据。

在开发过程中，有时需要对二进制文件进行数据的读取，这时也会用到NSData类。

```c

#import <Foundation/Foundation.h>

int main() {

    NSString * str=@"http://c.biancheng.net";

    NSData * data=[str dataUsingEncoding:NSUTF8StringEncoding];

    NSLog(@"the data is :%@",data);

    NSString * strCopy=[[NSString alloc]initWithData:data encoding:NSUTF8StringEncoding];

    NSLog(@"the new string is :%@",strCopy);

    return 0;
}

```

代码分析：
首先进行的是将字符串转换成二进制数据：
 
程序开头创建了一个字符串：@http://c.biancheng.net，通过dataUsingEncoding：就可以直接将字符串转换成二进制数据（方法中的参数默认是NSUTF8StringEncoding）。
 
然后给大家演示的是将二进制数据再转换成字符串：
在NSString类接口文件中，可以找到这个initWithData:对象方法，通过它，就可以直接将二进制数据转换成字符串（参数同样默认是：NSUTF8StringEncoding）。
 
NSData类还有一个可变类型：NSMutableData类，比NSData对象多的功能就是可以更新对象中的二进制数据（添加、更改）。这里不再举例，编程中不经常使用。会在文件操作的章节中看到它的出现。

## 四、c和OC的数据转化--包裹类

### 4.1 NSNumber


NSNumber类实现的功能是C语言中的基本数据类型和OC对象相互转换。
 
 
下面先来学习一下NSNumber的具体使用：

```c

#import <Foundation/Foundation.h>

int main() {   

    int number=5;

    NSNumber * intNum=[[NSNumber alloc] initWithInt:number];

    //将对象5存储到数组中。
    NSArray * array=[NSArray arrayWithObjects:intNum, nil];

    //输出array数组中的对象
    NSLog(@"the array is :%@",array);

    //从数组中得到对象5，由于数组中只有一个对象5，所以索引是0
    NSNumber * getNum=[array objectAtIndex:0];

   //重新得到int类型的值
    int getNumber=[getNum intValue];

    NSLog(@"the number is :%d",getNumber);

    return 0;
}

```

### 4.2 NSValue

NSValue类实现的功能是C语言声明的结构体和OC对象相互转换。

对于NSValue来说，它的作用是可以将C语言中的结构体转换成对象，反之，将结构体对象转换成结构体也可以。
 
目前我们接触过的关于Foundation框架中的结构体有:

NSRange

NSPoint(记录某个点的坐标的结构体，包含X和Y两个浮点型的变量)

NSSize(包含width和height两个浮点型变量)

NSRect(包含NSPoint类型的变量origin和NSSize类型的变量size，一般定义四边形的时候用)。
 
以上四个结构体，我们目前只需掌握NSRange即可，其他三个以后碰到会讲。
 
例子：

```c
#import <Foundation/Foundation.h>
int main() {
   
    //定义一个NSRange类型的变量
    NSRange range;
    range.length=2;
    range.location=1;

    //将range转换成对象
    NSValue * value=[NSValue valueWithRange:range];
   
    //将NSRange对象转换成NSRange变量
    NSRange theRange=[value rangeValue];

    //输出这个结构体中的数据，(unsigned long)是强制类型转换，和C语言类似
    NSLog(@"%lu,%lu",(unsigned long)theRange.length,(unsigned long)theRange.location);
   
    return 0;
}

```

输出结果：2,1

## 五、OC中的通知--NSNotificationCenter

 
OC语言为了在编程中实现一对多通信的模式，采取了通知中心这样一种机制。简单的理解就是，首先希望取得通过的对象提前到通知中心注册想要获取的通知。这样当某对象将想要发出的通知传递到通知中心时，通知中心就会立刻根据之前注册过的信息将通知发给想要这个通知的所有人。
 
在OC的通知中规定：等待通知下达的被称作“观察者”。
 
首先要清楚的是，使用通知时，要先有消息的观察者，后有消息的发送者(如果反过来，由于OC中消息的发送和接受是立刻执行的，观察者将接受不到通知，发送消息也就变得没有任何意义)
 
明白了通知是什么以及通知需要注意的几点之后，我们通过一个小例子来学习一下通知。
 
这个例子是说老板发出一个通知，内容是让他的工人去生产小汽车，工人们收到这个消息后，开始工作，具体实现如下：
 
首先，我们新建一个工程，例如：Demo3；
在工程中，我们新建一个Worker类，代表工人。
 
Worker.h文件中没有添加新的代码。


Worker.m文件中：

```c

#import "Worker.h"

@implementation Worker

-(instancetype)init{
    if (self=[super init]) {
        [[NSNotificationCenter defaultCenter]addObserver:self selector:@selector(makeCar) name:@"canMake" object:nil];
    }  

    return self;
}

-(void)makeCar{

    NSLog(@"Let's begin to make car,gogogo");
}

@end

```
代码讲解：

4-9行代码：是init方法，大家在之前的学习中可能没见过，但是大家每次对类对象进行初始化时都会用到它。在编写代码时，如果你不写它，在对象初始化时也会调用它。这个方法是继承自NSObject父类的。
 
第5行代码：涉及到了类之间的继承。由于每个类都含有父类，所以每个类中，都包含有父类的属性和方法。在进行初始化时，同样也需要对父类中的属性进行初始化。所以，这里通过调用super 的init方法，首先对父类进行初始化，然后如果本类属性需要进行初始化，再做设置。
 
第6行代码：大家要知道这是一段用于监听的代码。在这做监听，是因为，在创建每一个工人对象时，该工人都要等待老板的通知。所以在初始化中做这个工作最适合不过了。
 
知道为何在init方法中进行对通知的监听后，分析一下监听的代码写法：
 
NSNotificationCenter是一个类，用于通知的收集和发送。defaultCenter是一个类方法，返回的是这个类的对象，而且每次调用这个类方法的时候，返回的都是这个对象(单例)。后边的addObserver…，是生成观察者的方法，其中的self参数是监听者对象，@selector是当创建的观察者接收到通知后要执行的方法，name参数表示要监听的消息的名称，object是通知发送的时候传递过来的参数对象，一般为nil。
 
return self;返回本类的对象。
 
main.m文件中的代码：

```c

#import <Foundation/Foundation.h>

#import "Worker.h"

int main(int argc, const char * argv[]) {

    Worker * worker=[[Worker alloc] init];

    [[NSNotificationCenter defaultCenter]postNotificationName:@"canMake" object:nil];

    [[NSNotificationCenter defaultCenter]removeObserver:worker name:@"canMake" object:nil];

    return 0;
}
```

输出结果：Let's begin to make car,gogogo
 
代码讲解：
 
第5行代码：首先先实例化了一个worker类，但是别忘了，在初始化init方法中增加了监听者。
 
第6行代码：这行代码是发送消息的代码，[NSNotificationCenter defaultCenter]同样是通过调用defaultCenter类方法来获得唯一的那个对象，然后调用post的方法将名称为canMake的消息发送出去，object同样是nil。
 
注意：由于前面提到的通知的用法是先有观察者，后有消息的发送者，所以如果将5、6代码的顺序调整，运行程序虽然不会报错，但是不会输出内容。所以在通知的使用时一定要注意观察者和消息的发送者之间的顺序关系。
 
第8行代码：在使用通知的时候，一定要注意及时将观察者移除，通过调用removeObserver方法就可以将worker这个对象中的监听者移除。
 
提示：在使用通知的时候，要注意将创建的监听者手动移除（使用后马上移除，避免程序出错），养成一个良好的习惯。

## 六、key_value_coding 键值编码

KVC不同于点语法，它是一种间接调取对象属性的方法。它的实现方式是通过字符串来自动找到要更改的对象属性。看一个例子就懂了：
 
首先创建一个工程，在工程中添加一个类，例如：Person类：
Person.h：

```c
#import <Foundation/Foundation.h>

@interface Person : NSObject

@property (nonatomic,copy)NSString * name;

@property (nonatomic,assign)int age;

@property (nonatomic,assign)int sex;

@end

```
Person.m中没有添加代码。
 
如何通过键值编码的方式间接初始化一个Person类的对象：
main.m：

```c

#import <Foundation/Foundation.h>

#import "Person.h"

int main(int argc, const char * argv[]) {

    Person * person=[[Person alloc] init];

    [person setValue:@"ZhangSan" forKey:@"name"];

    [person setValue:[NSNumber numberWithInt:10] forKey:@"age"];

    [person setValue:[NSNumber numberWithBool:YES] forKey:@"sex"];

    NSLog(@"The person's name is :%@,and age is :%d, sex is :%d",person.name,person.age,person.sex);

    return 0;
}

```
输出结果：The person's name is :ZhangSan,and age is :10, sex is :1
 
可以看到，在对person对象初始化后，通过setValue:forKey:的方式给person对象的name、age、sex属性分别赋值。此方法的使用和字典中的setValue:forKey:方法类似，Value和Key必须是对象(两个方法完全没有关系，通过深入框架，可以看到，本节中使用的setValue:forKey:属于NSKeyValueCoding.h接口中提供的方法)。
 
在使用键值编码对声明的对象进行操作时，Key的值要求和对象中的属性名相同，只有这样才能对对象属性设置成功，否则程序会报告找不到你设置的key，程序出错。
 
和字典类似，NSKeyValueCoding.h不仅提供了setValue:forKey:的方法对对象中的属性进行设置，还可以通过键值编码的方式来间接地获取到对象中的方法值。可以使用valueForKey:的方法通过key的值来获取对应的属性值：
 
所以，上个例子中的输出语句，还可以改成：

```c

NSLog(@"The person's name is :%@,and age is :%d, sex is :%d",

[person valueForKey:@"name"],

[[person valueForKey:@"age"] intValue],

[[person valueForKey:@"sex"] boolValue]
);

```
(由于取出的value是对象，所以还需要转化一下)
        
除了上边介绍到的KVC提供的方法，NSKeyValueCoding.h接口中还提供了一个方法：

```c

-(void)setValuesForKeysWithDictionary:(NSDictionary<NSString *, id> *)keyedValues;

```
 
通过翻译这个方法“用字典通过key设置value”，了解到这同样是一个间接给对象赋值的方法，只不过这个方法需要传入的参数是一个字典。下面，将上面的例子改写一下；

```c

#import <Foundation/Foundation.h>

#import "Person.h"

int main(int argc, const char * argv[]) {
   
    Person * person=[[Person alloc] init];
   
    NSMutableDictionary * dic=[[NSMutableDictionary alloc]init];
   
    [dic setValue:@"ZhangSan" forKey:@"name"];
    [dic setValue:[NSNumber numberWithInt:10] forKey:@"age"];
    [dic setValue:[NSNumber numberWithBool:YES] forKey:@"sex"];
   
    [person setValuesForKeysWithDictionary:dic];
   
    NSLog(@"The person's name is :%@,and age is :%d, sex is :%d",
          [person valueForKey:@"name"],
          [[person valueForKey:@"age"] intValue],
          [[person valueForKey:@"sex"] boolValue]
          );
    return 0;
}

```
输出结果：The person's name is :ZhangSan,and age is :10, sex is :1
 
通过不同的方法，最终得到的是一个相同的结果。有两种方式可以实现间接对对象属性赋值的操纵，那么怎么做选择呢？
 
对于第一种方法，可以在平常方法中使用。而第二种方法，当你从外界文件中读入字典对对象属性进行赋值时，可以极大地减少代码的工作量，提高开发效率。

## 七、KVO：Key-Value-Observer，键值观察 或键值监听

 
和KVC长得很像的，还有一个KVO(全称：Key-Value-Observer，键值观察(或键值监听))
 
虽然它和KVC就差一个字母，但是它们完全没有关系，本节将它们放在一起，也只是为了初学者能够更好地区分它们。
 
KVO是干什么的呢？在编程过程中，有时需要对对象的某个或某些属性进行监听，通过对象属性的变化采取相应的对策。
 
下面来学习一下KVO的具体使用。通过一个例子来具体地感受一下KVO的强大功能

1、创建一个工程，例如Demo；
2、在工程中，创建一个类文件，例如Person

在Person.h文件：

```c

#import <Foundation/Foundation.h>

@interface Person : NSObject

@property (nonatomic,copy)NSString * name;

@property (nonatomic,assign)int age;

@end

```
在Person.m文件：

```c

#import "Person.h"

@implementation Person

-(void)observeValueForKeyPath:(NSString *)keyPath ofObject:(id)object change:(NSDictionary<NSString *,id> *)change context:(void *)context{
    NSLog(@"%@",change);
}
@end

```

可以发现，在Person.m文件中出现了一个类似和本类毫无关系的对象方法。先不管，先看main.m文件中的代码：

```c

#import <Foundation/Foundation.h>

#import "Person.h"

int main(int argc, const char * argv[]) {

        Person * person=[[Person alloc] init];

        person.name=@"ZhangSan";

        person.age=10;

        [person addObserver:person forKeyPath:@"name" options:NSKeyValueObservingOptionNew|NSKeyValueObservingOptionOld context:nil];

        [person addObserver:person forKeyPath:@"age" options:NSKeyValueObservingOptionNew|NSKeyValueObservingOptionOld context:nil];

        person.name=@"LiSi";

       person.age=20;

       [person removeObserver:person forKeyPath:@"name" context:nil];

       [person removeObserver:person forKeyPath:@"age" context:nil];

       return 0;
}

```
输出结果：
{
    kind = 1;
    new = LiSi;
    old = ZhangSan;
}
{
    kind = 1;
    new = 20;
    old = 10;
}
我们来分析一下main.m文件中代码：
                               
5-7行代码：初始化了一个Person类的对象，并对person对象的属性进行了初始化。
 
9、10行代码是KVO的重点，通过翻译，我们可以大概了解这个方法的作用，就是给person这个对象添加一个观察者，着个观察者就是自己。那自己观察自己什么呢？要时刻监听name属性值和age属性的变化。options参数的设置是要你选择一但监听的这个name属性值发生了变化，那么你要变化之前的值还是变化之后的值或者是其他类型的值(可以选择多种，可以进入枚举中查看),context可以用来传递数据，没有就设置为nil即可。
 
12、13行代码：通过直接赋值的方式将person对象的属性的值做了改变。这一步的目的就是要判断监听者能不能监听到数据。
 
最后一定要移除观察者。（对移除的顺序不做要求）
 
当我们运行程序的时候打断点会发现，当person对象值发生改变的时候，程序会进入Person.m文件中的那个方法中执行输出语句，这是怎么回事呢？
 
这就是KVO，当你设置了谁作为监听者之后，监听者所在的类就要实现Perosn.m中的第5行代码，当监听的对象发生变化时，自动调用这个方法，并将变化的数据传递给这个方法的change字典，通过这个字典，我们就可以获取相应的数据。
 
KVO和通知的关系
相同点：
同属于监听，都能够传递数据。
在程序结束时，都需要移除观察者。
 
不同点：
KVO比通知更厉害的是，它不仅能够实现监听，同时还能监听对象属性的改变，这是使用通知办不到的。

## 八、过滤器 --NSPredicate

谓词NSPredicate-- 就像是一个筛子(过滤器)，帮助我们将需要的数据按照一定的条件挑选出来。所以本节将围绕NSPredicate来介绍一下怎样在一堆数据中获取到想要的数据。
 
首先，先来学习一下如何创建谓词，通过查看NSPredicate.h接口中的方法，我们可以找到这个方法：

```c

+(NSPredicate*)predicateWithFormat:(NSString*)predicateFormat,...;
   
```

首先，这个是一个类方法，它返回的是一个NSPredicate类的对象，方法中留给我们的是一个字符串，这个字符串用来填写过滤条件。过滤条件的书写下面只介绍几种常用的：

SELE：代表每个数据本身

=、==：是相同的，都是等于的意思。

BETWEEN：采用：表达式 BETWEEN{下限，上限}的格式

AND、&&、OR、NOT

BEGINSWITH：检查某个字符串是否以指定的字符串开头，如：BEGINSWITH’a’

CONTAINS：检查是否包含指定的字符串。

LIKE：用于筛选时都匹配指定的字符串格式，如：name LIKE ‘*ac*’表示name字符串中是否有ac字符串。

   
通过上面的方法以及参数的介绍，我们可以轻松创建一个谓词，那么怎么使用创建好的谓词呢？

在NSPredicate类中，还可以看到这么几个方法：

```c

-(NSArray<ObjectType>*)filteredArrayUsingPredicate:(NSPredicate *)predicate;
从名字上就知道用于过滤NSArray的。
 
-(void)filterUsingPredicate:(NSPredicate*)predicate;//用于过滤NSMutableArray数组的
 
-(NSSet<ObjectType>*)filteredSetUsingPredicate:(NSPredicate *)predicate;//用于过滤集合(NSSet，集合，和数组一样，都是存储数组的方法，区别就是集合中元素互异，使用的地方不多，和NSArray及其类似，非常简单，大家自学，这里不在过多解释。)

```
 
通过这些方法，我们可以轻松地得到我们想要的数据。给大家一个例子：

首先创建一个OC工程，在工程中创建一个Person类：

person.h中的代码：

```c

#import <Foundation/Foundation.h>

@interface Person : NSObject

@property (nonatomic,copy)NSString * name;

@property (nonatomic,assign)int age;

+(instancetype)personWithName:(NSString *)name age:(int)age;

@end

```
person.m中的代码：

```c
#import "Person.h"

@implementation Person

+(instancetype)personWithName:(NSString *)name age:(int)age{
    Person * person=[[Person alloc] init];
    person.name=name;
    person.age=age;
    return person;
}

@end

```
main.m文件中的代码：

```c
#import <Foundation/Foundation.h>

#import "Person.h"

int main(int argc, const char * argv[]) {

    NSArray * array=[NSArray arrayWithObjects:
                     [Person personWithName:@"ZhangSan" age:10],
                     [Person personWithName:@"LiSi" age:15],
                     [Person personWithName:@"WangWu" age:20],
                     [Person personWithName:@"WangLiu" age:25],
                     [Person personWithName:@"LiWu" age:30],
                     [Person personWithName:@"ZhangSi" age:35], nil];
   
    NSPredicate * predicate=[NSPredicate predicateWithFormat:@"age>20"];

   NSArray * myArray=[array filteredArrayUsingPredicate:predicate];

    for (Person * person in myArray) {
        NSLog(@"age>20 :%@",person.name);
    }

    return 0;
} 
```
输出结果：age>20 :WangLiu
age>20 :LiWu
age>20 :ZhangSi
(注意：在最后输出的时候，如果采用NSLog的方法，输出的是每个对象所占用的内存首地址，而不是具体的数据。大家可以上网搜索关于重写NSLog方法的代码，由于不属于OC的知识点，因此本文中不介绍)。
 
通过对谓词的学习，你是否看出使用谓词的弊端？通过谓词筛选的流程，我们可以看出，谓词过滤的是一些具有共性的数据，如果是杂乱没有规律的数据，我们如果使用谓词，反而会增加代码量。
                                                 
所以，在实际编程过程中，要根据具体的环境，合理地选择实现地方式。

## 九、代码块

这一节学习新的语法－block代码块。它是在iOS4.0和Mac OS X10.6以后的版本中才添加的，是对C语言的扩展。
          
简单的理解block代码块的作用就和C语言的函数的作用类似，就是将能够实现某种特定功能的一些代码包裹起来，留出必要的接口（就是参数），供外界调用。
          
知道block代码块的功能后，学习一下block的语法规则。
#import <Foundation/Foundation.h>
int main(int argc, const char * argv[]) {
void (^helloWorld)(void);
     helloWorld=^(void){
     NSLog(@"Hello World!");
     };
    helloWorld();
    return 0;
}
运行结果：Hello World!
 
可以看到，上边的例子通过使用block代码块实现了“Hello World”小程序。分解一下block代码块，看看到底是怎么实现的。
 
第4行代码：是block的声明，第一个void是代码块的返回值类型，(^helloWorld)：“^”是block代码块的标志，无实际意义。helloWorld是block代码块的名称，必须要用小括号扩起来。第二个void是代码块的参数，如果需要有参数，语法是：（int，……）或（int i，……）都可以；如果不需要参数，可以采用：（）或（void）但是不能连括号都不写。
      
6-8行代码：是block代码块的实现。首先在对代码块实现的时候，“^”是必须存在的，（void）中需要写的是当代码块的参数。有参数时，在这里必须写上具体的每个参数名，如果没有参数，可以写成: （void）或（）或干脆省略都可以。后边大括号中的代码表示对helloWorld代码块的具体实现代码。
      
（提示：代码块的声明和实现可以写在一起）
 
代码块的调用和C语言的函数调用相同。通过：“代码块名（参数）”的方式。
 
在使用代码块的时候，对于全局变量，在块内是完全可操作的。
但是对于局部变量来说，在块内只能使用不能更改。
 
如果试图在块内更改局部变量的值，程序会报错，解决的方案是在声明局部变量时添加__block关键字（注意是两个“_”）：
创建一个工程，在main.m文件中：
#import <Foundation/Foundation.h>
int number=10;
int main(int argc, const char * argv[]) {
   __block int i=10;
    void(^block)()=^{
        NSLog(@"The i is :%d",i);
        NSLog(@"The number is :%d",number);
        i++;
        number++;
    };
    block();
    NSLog(@"The i is :%d",i);
    NSLog(@"The number is :%d",number);
    return 0;
}
运行结果：The i is :10
           The number is :10
           The i is :11
           The number is :11
 
(对于Block代码块的使用，它也可以和C语言中的函数一样，写在main函数的外面，但是必须遵循“先声明，后使用”的原则)
 
代码块的具体使用
 
通过上面的学习，应该基本上可以使用代码块了。在实际开发过程中，代码块的使用都出现在那些地方呢？
 
1、我们在使用Foundation框架中的某些方法时，可能会看到代码块，这些代码块作为方法的参数。例如：对NSArray类的数组进行排序可以使用代码块排序：
#import <Foundation/Foundation.h>
int main(int argc, const char * argv[]) {
    NSArray * array=[NSArray arrayWithObjects:@"4",@"1",@"2",@"3",@"5", nil];
       array= [array sortedArrayUsingComparator:^NSComparisonResult(id obj1, id obj2) {
        int a1=[obj1 intValue];
        int a2=[obj2 intValue];
        return a1>=a2?-1:1;
    }];
       NSLog(@"%@",array);
       return 0;
}
运行结果：(
    5,
    4,
    3,
    2,
    1
)
通过上边对数组对象进行排序的例子，你会发现block代码块作为参数使用时，当使用这个排序方法时，代码块中的变量或对象是有值的。
 
通过将返回的数值进行比较，然后再返回给这个方法一个结果，就完成了对数组的排序。整个过程用到了block代码块的回调。
 
下面通过一个例子对这个过程进行一下模拟；
1、首先创建一个工程，例如：Demo；
2、创建一个Person类；

在Person.h中:
#import <Foundation/Foundation.h>
typedef void(^myBlock)(NSString * name,int age);
@interface Person : NSObject
-(void)exercise:(myBlock)block;
@end
其中，typedef时OC中的关键字，表示重命名，作用是通过使用typedef关键字，简化较为复杂的类型声明，在后边使用代码块的时候，直接用代码块的名字代表就可以了。
          
在Person.h中，首先声明了一个全局的block代码块，并将之作为方法的参数。         
在Person.m文件中：
#import "Person.h"
@implementation Person
-(void)exercise:(myBlock)block{
    NSString * theName=@"ZhangSan";
    int age=10;
    block(theName,age);
}
@end
在这个文件中，对.h声明的方法进行了实现，在实现过程中，我们调用了block代码块。直到现在我们还没有对block代码块做实现。
 
下面来看一下main.m文件中的代码：
#import <Foundation/Foundation.h>
#import "Person.h"
int main(int argc, const char * argv[]) {
    Person * person=[[Person alloc] init];
    [person exercise:^(NSString *name, int age) {
        NSLog(@"%@,%d",name,age);
    }];
    return 0;
}
在main.m文件中，我们看到，我们对block代码块做了实现，输出name值和age的值。通过给工程打断点，我们会看到这段小程序的运行过程：

首先，初始化一个person对象
向person对象发送exercise：消息
运行person.m文件中的exercise实现部分的代码
运行到block代码块时，运行代码块的实现部分，也就是main.m文件中的输出语句
block代码块的实现部分运行完后，程序继续运行exercise方法的实现部分。
exercise方法的实现部分运行完后，表明向person对象发送的消息就完成了
然后运行return 0结束程序。
 
通过跟踪程序的执行顺序，我们可以看到block代码块作为参数时的程序运行流程。正是利用block代码块的这种运行机制，我们可以利用block完成传值的功能。
      
block代码块，实际开发过程中，对于自定义的代码块很少用到，但是在Foundation框架中，block代码块作为方法参数的情况比较多。
 
而且，虽然协议、通知等都有传值的功能，但是如果你能够灵活运用block代码块，比起前两种方式编程将变得更简单。












 

























 