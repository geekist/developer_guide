
- [一、objective C中的基本数据类型](#一objective-c中的基本数据类型)
  - [1.1 OC中有如下基本数据类型：](#11-oc中有如下基本数据类型)
  - [1.2 常用占位符](#12-常用占位符)
- [二、objective-C 拓展数据类型](#二objective-c-拓展数据类型)
  - [2.1 NSString和NSMutableString](#21-nsstring和nsmutablestring)
  - [2.2 id类型 万能指针](#22-id类型-万能指针)
  - [2.3 NSRange](#23-nsrange)
  - [2.5 NSNumber](#25-nsnumber)
  - [2.6 NSValue](#26-nsvalue)
  - [2.7 NSNull空对象](#27-nsnull空对象)
  - [2.8 NSDate时间](#28-nsdate时间)
  - [2.9 NSObject](#29-nsobject)
  - [2.10 集合类型---NSArray和NSMutableArray](#210-集合类型---nsarray和nsmutablearray)
  - [2.11 NSDictionary和NSMutableDictionary](#211-nsdictionary和nsmutabledictionary)
  - [2.12 NSData](#212-nsdata)
    - [字符串与NSData相互转换](#字符串与nsdata相互转换)
    - [Byte与NSData](#byte与nsdata)
    - [NSData --\>UIImage](#nsdata---uiimage)
    - [NSData--NSMutableData](#nsdata--nsmutabledata)
    - [通过NSData 和 NSKeyedArchive 实现一个文件归档多个对象](#通过nsdata-和-nskeyedarchive-实现一个文件归档多个对象)



# 一、objective C中的基本数据类型

## 1.1 OC中有如下基本数据类型：

int：声明整型变量

double：声明双精度变量

float：声明浮点型变量

BOOL:声明布尔型变量

char：声明字符型变量

id：通用的指针类型

enum：声明枚举类型

long：声明长整型变量或函数

short：声明短整型变量或函数

signed：声明有符号类型变量

struct：声明结构体变量

union：声明共用体（联合）数据类型

unsigned：声明无符号类型变量

void：声明函数无返回值或无参

## 1.2 常用占位符

常用的一些占位符：

%@：字符串占位符

%d:整型

%ld:长整型

%f:浮点型

%c:char类型

%%：%的占位符

# 二、objective-C 拓展数据类型

Objective-C是在C语言基础上拓展出的新语言，所以它是完全兼容C语言代码的，C语言中的基本数据类型如int、float、double和char在Objective-C中是完全可以正常使用的。除此之外，Objective-C还拓展了一些新的数据。oc中类分为不可变类和可变（mutable）类，例如字符串类就有可以变和不可变，它们创建的对象也是，下面列出了Foundaiton框架中主要的可变类和不可变类：

## 2.1 NSString和NSMutableString

NSString 字符串

```c
创建常量字符串
NSString *str = @"Hello World!"; 

创建空字符串，进行赋值
NSString *str = [[NSString alloc] init];
str = @"Hello World!";


创建字符串创建字符串
NSString *str = [NSString stringWithFormat:@"My age is %i",18];


用标准C语言字符串创建字符串
(NSString*str)=[NSString(stringWithCString:scString)encoding:NSUTF8StringEncoding];
//char本cString“这是一个C语字符串”.  NSUTF8StringEncoding表示转码方式


从一个url读取字符
NSURL* url = [NSURL URLWithString:@"https://www.baidu.com"];

写入文件
NSString *path = @"/Users/AbsoluTely/Desktop/123.txt";
NsString*str = @"123456" :
NSError @error;
[str writeToFile:path atomically:YES encoding:NSUTF8StringEncoding error:&errorl;
if (error) {
NSLog(@"失败error = %@",[error localizedDescription]);
}

读取文件

NSString *path = [ [NSBundle mainBundle]pathForResource:@"123 txt*of Type:nil];
NsString*str = [NSString stringwithContentsofFile:path enconding:NSUTF8StringSEncoding error:&error];

NSString大小写处理

//全部转化为大写
 [str uppercaseString];

//全部转化为小写
 [str lowercasestring];

//首宇母大写，其它都是小写 
[str capitalizedtring];

NSSring中搜索字符串

//是否以@"a"开头
 [str hasPrefix:@"a"];

//是否以@"a"结尾
[str hasSuffix:@"a"];

//NSString的比较 比较两个字符串是否完全相同，返回Bool 
[str isEqualTostring:strl];

//比较大小
 [str compare:str1] ;

//忽略大小写进行比较
 [str caseInsensitiveCompare:strl];

//NSString的截取 从指定位置开始
[str substringFromIndex:1];

//从开头截图到指定位置，但不包括此位置 
[str substringToIndex:4];

//根据指定范围进行截图 
[str substringWithRange:NSMakeRange(1,3) ];

//用separator为分隔符截取字符串，返回一个装有所有子串
 [str componentsSeparatedByString:@","];

NSString其他用法

//返回长度 
[str length];
//返回指定位对应的字符
 [str characterAtIndex:3];
//转化为int
 [str intvalue];
//转化为float 
[str floatValue];
//转化为bool
 [str boolvalue]:
//转化为C语言字符本 [str UTF8String];
```

NSMutableString可变字符串


NSMutableString

NSString是不可变的，不能删除字符或者添加字符。NSString有一个子类NSMutableString,称为"可变字符串”.
可以用创建NSString的方法来创建NSMutableString,因为NSMutableString是NSString的子类，NSString能用的方法NSMutableString都能用
创建NSMutableString

创建的同时分配一个容量(只是一个最优值，便于系统提高性能。)

NSMutableString可变字符串

常用的方法

```c
//设置内容
 [mutableStr setString:@"我是可变字符串"];

//拼接一个字符串
 [mutableStr appendString:@" ..."];

//拼接一个格式 
[mutableStr appendFormat :@我有%d个朋友",3];

//替换字符串
[mutableStr replaceCharactersInRange : [mutableStr range of string @"age"]withString:@"Age"];

//插入字符串 [mutableStr insertString:@"..." atIndex:2];

//删除字符串 [mutableStr deleteCharactersinRange: [mutableStr range string @"age
```

## 2.2 id类型 万能指针

id类型是oc中独有的数据类型，它可以存储任何类型的对象,也叫万能指针，能指向任何OC对象，相当于NSObject*。

//注意 id后面不要加上 *
id p = [person new]

## 2.3 NSRange

NSRange结构体

```c
typedef struct_ NSRange {
NSUInteger location; //起始位置
NSUInteger length;//长度
} NSRange;
```

typedef NSRange *NSRangePointer;

这个结构体用来表示事物的一个范围，通常是字符串里的字符范围或者集合里的元素范围

location表示该范围的起始位置
length表示该范围内所含的元素个数
比如”1 love Objective-C"中的"Obj"可以用location为7，length的范围来表示

有3种方式创建一个NSRange变量

//直接给成员赋值
NSRange range；
range.location = 3;
range.length = 2;
应用C语言的聚合结构赋值机制NSRange range = {7,3}; 或者

NSRange range1 = {.location = 7..length = 3};
Foundation框架提供的一个快捷函数NSMakeRang {7,3};
NSRange range = NSMakeRange(7, 3);
CGPoint(表示平面中的一个点)
Struct CGPoint {
CGFloat x;};
CGFloat y；
typedef struct CGPoint CGPoint;
可以用NSMakePoint来创建 CGSize(存储宽度和高度)

struct cGsize (
CGFloat width;
CGFloat height;
typedef struct CGSize CGSize; 
可以用CGSizeMake来创建 CGRect(存储位置、宽度和高度)

struct CGRect 
CGPoint origin; CGSize size;
typedef struct CGRect CGRect CGRect；
可以用CGRectMake来创建



## 2.5 NSNumber

NSNumber的使用

NSNumber可以将基本数据类型包装成对象，这样就可以间 据类型存进NSArray、NSDictionary等集合中

常见的初始化方法:

```c
(NSNumber *) initWithChar: (char)value;
(NSNumber *)initWithShort: (short)value;
(NSNumber *) initWithInt:(int )value;
(NSNumber *)initWithUns ignedInt: (unsigned int)value;
(NSNumber *)initWithLong: (long )value ;
(NSNumber*)initWithUnsignedLong:(unsigned Long)value;
(NNumber *) initwithLongLong: (Long long)value;
(NSNumber*)initwithUnsignedLongLong:(unsigned Long long)value
(NSNumber*)initwithFloat:(float)value
(NSNumber *) initWithDouble: (double)value;
(NSNumber *) initWithBool: (B0OL)value;
(NSNumber *)initwithInteger: (NSInteger)value;
(NSNumber*) initWithUns ignedInteger: (NSUInteger)vatue,
```

常用属性和方法

```c
@property (readonly) char charValue;
@property (readonly) uns igned char unsignedCharValue;
@property ( readonly) short shortValue;
@property (readonly) unsigned short unsignedShortValue;
@property (readonly) int intValue;
@property ( readonly) unsigned int unsignedIntValue ;
@property ( readonly) long longValue;
@property (readonly) unsigned long unsignedLongValue;
@property ( readonly) long long long LongValue ;
@property(readonly)unsigned long long unsigned Long Long Value;
@property (readonly) float floatValue;
@property (readonly) double doubleValue;
@property (readonly) BOOL boolValue;
@property (readonly) NSInteger integerValue;
@property (readonly) NSUInteger unsignedIntegerValue;
```

## 2.6 NSValue


NSNumber时NSValue的子类，但NSNumber只能包装数字类型，

NSValue可以包装任意值，也就可以用NSValue包装结构体后加入集合中

创建NSValue的常用方法

```c
+(NSValue*)valueWithBytes:(const void*)value objCType:(const char *) type;
+ (NSValue *)value:(const void *)value withobjCType: (const char *)type;
```

NSValue常用方法

```c
+ (NSValue *)valueWi thPoint: (NSPoint )point;
+ (NSValue *)valueWithsize: (NSSize)size;
+ (NSValue*)valueWi thRect: (NSRect)rect;
+ (NSValue * )valueWi thEdgeInsets: (NSEdgeInsets)insets NS_ AVAILAB；
```

## 2.7 NSNull空对象

集合中是不能存放nil值的，因为nil在集合中有特殊含义，但有时确实需要存储一个表示“什么都没有"的值，那么就可以使用NSNull,它是NSObject的一个子类。

创建和获取NSNull的方法 NSNull *null = [NSNull null];

## 2.8 NSDate时间

NSDate的静态初始化

```c
//返回当前时间 NSDate *currentDate = [NSDate date];
//返回以当前时间为准，过了几秒的时间 NSDate*date1=[NSDatedatewithTime IntervalSinceNow:10];
//返回以2001/01/01 GMT为准，然后过了几秒的时间 NSDate*date2=[NSDatedatewithTimeIntervalSinceReferenceDate：10]；
//返回以1970/01/01 GMT为准，然后过了几秒的时间 NSDate*date3=[NSDatedateWithTimeIntervalSince1970:200]；
//返回很多年前的某一天 NSDate *date4 = [NSDate distantPast];
//返回很多年后的某一天 NSDate *date5 = [NSDate distantFuture];
```

NSDate取回时间间隔

```c
//返回两个时间的间隔 NSTimeInterval intervall = [date1 timeIntervalSinceDate :date2]；
//返回与现在的时间间隔 NSTimeInterval interval2 = [date1 timeIntervalSinceNow];
//返回与1970年的时间间隔 NSTimeInterval interval3 = [date1 timeIntervalsince1970];
```

NSDate的比较

```c
//返回两个日期中较早的一个 NSDate本earlierDate = [date1 earlierDate:date2];
//返回两个日期中较晚的一个 NSDate *laterDate = [date1 laterDate :date2];
```

## 2.9 NSObject

NSObject基类
NSObject类属于根类。根类在层级结构中处于最高级，除此之外没有更高层级

NSObject常用方法

```c
//比较两个对象是否相同 [obj1 isEqualTo:obj2 ];

//判断此对象是否属于某个类或者此类的子类[obj1 isKindOfClass:[NSString class]]

//判断此对象是否为此类(不包括子类)[obj1 isMemberOfClass:[NSString class]]

//判断此对象是否实现了某协议[obj1 conformsToProtocol :@protocol (StudentDelegate)

//判断此对象是否拥有此方法[obj1 responds ToSelector:@selector(clickAction)
```



## 2.10 集合类型---NSArray和NSMutableArray

NSArray 用来存储对象的有序列表，它是不可变的，也不能存储C语言中的基本数据类型，如int、float、enum、struct，也不能存储nil

常用的创建方法

```c
快速创建
NSArray *ar = @[@"1",@2",@"3"];

创建一个空字典
NSArray本arr1 = [NSArray array];

创建含有元素的字典
NSArray *arr2 = [NSArray arrayWithobjects:@"1" ,@"2"，@"3"，@"4"]；


从文件中读取
NSArray *arr3 = [NSArray arraywithContentsOfFile:path] ;
```

NSArray查询

```c
//获取数组元素个数 [arr count];

//是否包含某一元素 [arr containsObject:@"a"];

//返回第一个元素 [arr firstobject];

//返回最后一个元素 [arr lastobject]; 

//获得指定位置的对象 [arr objectAtIndex:2]; arr[2]；

//查找元素位置 [arr indexofobject:@"a"];

//在range范围内查找元素的位置 [arr indexOfobject:@"a" inRange:NSMakeRange(3, 4)];
```

NSArray其它方法

```c

//用"，"将字典元素依次拼接成一个字符串 [arr componentsJoinedByString：@“，”
//持久化 [arr writeToFile:path atomically：true]； //与在文件中读取一样，将数组存在文件中去；
```

NSArray排序

```c
Block排序

[arr sor tedArrayUsingComparator:
NSComparisonResult(id Nonnt obj1, id Nonnull obj2) {
return [obj1 compare:obj2] ;
```

NSMutableArray 可变数组

NSMutableArray的创建


可变的NSArray, NSArray的子类，可以随意的添加或者删除元素

创建NSMutableArray的方法

```c
NSMutableArray *mutableArr = [NSMutableArray arrayWithCapacity:5]; //表示这个数组可能有5个元素

NSMutableArray *mutableArrl = [[NSMutableArray alloclinwithCapacity:5];
```

也可以使用创建NSArray的方法来创建NSMutableArray.

当一个元素被加到集合中时，会执行一次retain操作; 让一个元素从集合中移除时，会执行一次release操作; 当集合被销毁时（调用dealloc),集合里的所有元素会执行一次release操作

NSMutableArray删除元素
```
//删除所有元素 [mutableArr removeAllobjects];

//删除某个范围的元素 [mutableArr removeObjectsInRange :NSMakeRange(3,2)；]

//删除某个指定元素 [mutableArr removeObject:@"a"];

//删除两个集合中都存在的元素 [mutableArr removeObjectsInArray:@[@"a" ,@"b" ,@"c“，@"d"]；
```


## 2.11 NSDictionary和NSMutableDictionary

NSDictionary

C++/STL中有一种容器叫做map，OC中的NSDictionary与map很类似，是一个拥有键值对的类/数据结构。这个键值对的要求是键必须遵守NSCopying协议（NSString就遵守这个协议），值需要满足是OC对象。NSDictionary的元素不可变，而NSMutableDictionary中元素可变。

1). 创建方法

```c
NSDictionary *dic1 = [NSDictionary new];

NSDictionary *dic2 = [[NSDictionary alloc] init];

NSDictionary *dic3 = [NSDictionary dictionary];//以上3种是没有意义的创建，无法更改，大小恒为0

NSDictionary *dic4 = [NSDictionary dictionaryWithObjectsAndKeys:@"jack",@"name",@"Xi'an",@"address", nil];//添加两个键值对，需要注意的是先写键值对中的值，再写键，最后以nil结尾。


//快速创建方法，用大括号
NSDictionary *dic5 = @{@"name":@"rose",@"age":@"18",@"address":@"Beijing"};
```
2). 相关方法

```c
//用%@就可以打印整个字典
NSLog(@"%@",dic5);

//用键来取值，有两种方法：下标或者自带的方法，需要注意的是如果没有指定的key，会返回nil
NSString *str1 = dic4[@"name"];
NSLog(@"%@",str1);
NSString *str2 = [dic4 objectForKey:@"address"];
NSLog(@"%@",str2);

//计算字典的大小
NSUInteger count = dic4.count;
NSLog(@"%lu",(unsigned long)count);

//遍历字典所以的键（值用allValues）：
for(NSString *str in dic4.allKeys)
    NSLog(@"%@",str);  

//遍历字典 使用for-in循环遍历出来的是字典中所有的键
for(id item in dic4)
    NSLog(@"%@ = %@",item,dic4[item]);

//遍历字典 使用block
 [dic4 enumerateKeysAndObjectsUsingBlock:^(id  _Nonnull key, id  _Nonnull obj, BOOL * _Nonnull stop) {
        NSLog(@"%@ = %@", key ,obj);
        //key为元素的键，obj为元素的值，stop为YES时，终止遍历
}];
```

最后再讨论一下字典在内存中的存放：在内存中，字典的元素先用键进行哈希函数运算，将计算所得的结果作为存储的位置；取值的时候也是先对键进行哈希运算，再定位查找值。与NSArray相比，NSDictionary的查找效率是很高的，不需要对整个字典进行遍历；而NSArray在存放的时候效率更高，不需要进行哈希运算。

——–NSMutableDictionary——–

NSMutableDictionary是从NSDictionary继承而来的。因此我们仅对其特性进行讨论，其他方法略去。

1). 创建方法

```c
NSMutableDictionary *dic6 = [NSMutableDictionary new];

NSMutableDictionary *dic7 = [[NSMutableDictionary alloc] init];

NSMutableDictionary *dic8 = [NSMutableDictionary dictionary];//以上3种都是有意义的创建，因为可以更改

NSMutableDictionary *dic9 = [NSMutableDictionary dictionaryWithObjectsAndKeys:@"jack",@"name",@"Xi'an",@"address", nil];

NSMutableDictionary *dic10 = @{@"name":@"jack"};//error,右值是NSDictionary对象,子类指针指向父类对象，危险！
```
2). 相关方法

```c
//新增与删除键值对
[dic9 setObject:@"19" forKey:@"age"];
[dic9 setObject:@"21" forKey:@"age"];//键重复了，此时保存的是后添加的，原有的被替换

[dic9 removeObjectForKey:@"age"];//删除键值为19的键值对
[dic9 removeAllObjects];//删除全部键值对

//字典数组信息持久化 读写磁盘
[dic9 writeToFile:@"Users/warwick/Desktop/abc.plist" atomically:NO];//将字典的内容写到磁盘上的文件中

NSDictionary *dic = [NSDictionary dictionaryWithContentsOfFile:@"Users/warwick/Desktop/abc.plist"];//从文件中读取字典

```

## 2.12 NSData

NSData是用来包装数据的，NSData存储的是二进制数据，屏蔽了数据之间的差异，文本、音频、图像等数据都可用NSData来存储。在多媒体开发时，比较常用，例如拼接音频、图片。

### 字符串与NSData相互转换

字符串 -->NSData

```c
 NSString *aString = @"http://www.baidu.com";
 NSData *aData = [aString dataUsingEncoding: NSUTF8StringEncoding];
```

NSData -->字符串

```c
NSString *aString = [[NSString alloc] initWithData:adata encoding:NSUTF8StringEncoding];
```

### Byte与NSData

NSData --> Byte

```c
 NSString *testString = @"1234567890";
 NSData *testData = [testString dataUsingEncoding: NSUTF8StringEncoding];　　
 Byte *testByte = (Byte *)[testData bytes];
 ```
Byte --> NSData

```c
Byte byte[] = {0,1,2,3,4,5,6,7};
NSData *adata = [[NSData alloc] initWithBytes:byte length:8];
```

### NSData -->UIImage


NSData --> UIImage

```c
 UIImage *aImage = [UIImage imageWithData: imageData];
```

UIImage --->NSData

```c
//例：从本地文件沙盒中取图片并转换为NSData
 NSString *path = [[NSBundle mainBundle] bundlePath];
 NSString *name = [NSString stringWithFormat:@"ceshi.png"];
 NSString *finalPath = [path stringByAppendingPathComponent:name];
 NSData *imageData = [NSData dataWithContentsOfFile: finalPath];
 UIImage *aimage = [UIImage imageWithData: imageData];
 
NSData *imageData = UIImageJPEGRepresentation(image, compressionQuality);
NSData *imageData = UIImagePNGRepresentation(aimae);
```
### NSData--NSMutableData

NSData与 NSMutableData

```c
NSData *data=[[NSData alloc]init];
NSMutableData *mData=[[NSMutableData alloc]init]; 
mData=[NSData dataWithData:data];
```

### 通过NSData 和 NSKeyedArchive 实现一个文件归档多个对象

使用archiveRootObject:toFile:方法可以将一个对象直接写入到一个文件中,

但有时候可能想将多个对象写入到同一个文件中,那么就要使用NSData来进行归档对象.

实现原理:

利用NSData为一些数据提供临时存储空间,以便随后写入文件，或者存放从磁盘读取的文件内容,可以使用[NSMutableData data]创建可变数据空间
屏幕快照 2016-06-08 下午3.42.15.png


实例操作:

归档（编码）

```c
// 新建一块可变数据区
NSMutableData *data = [NSMutableData data];

// 将数据区连接到一个NSKeyedArchiver对象
NSKeyedArchiver *archiver = [[[NSKeyedArchiver alloc] initForWritingWithMutableData:data] autorelease];

// 开始存档对象，存档的数据都会存储到NSMutableData中
[archiver encodeObject:person1 forKey:@"person1"];
[archiver encodeObject:person2 forKey:@"person2"];

// 存档完毕(一定要调用这个方法)
[archiver finishEncoding];

// 将存档的数据写入文件
[data writeToFile:path atomically:YES];
```

NSData-从同一文件中恢复2个Person对象

恢复（解码）

```c
// 从文件中读取数据
NSData *data = [NSData dataWithContentsOfFile:path];

// 根据数据，解析成一个NSKeyedUnarchiver对象
NSKeyedUnarchiver *unarchiver = [[NSKeyedUnarchiver alloc] initForReadingWithData:data];

Person *person1 = [unarchiver decodeObjectForKey:@"person1"];
Person *person2 = [unarchiver decodeObjectForKey:@"person2"];

// 恢复完毕
[unarchiver finishDecoding];

```c


