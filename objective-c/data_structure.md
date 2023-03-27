
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

## 2.4 NSArray和NSMutableArray

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



