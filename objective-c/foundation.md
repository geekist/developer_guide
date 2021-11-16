
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



























 