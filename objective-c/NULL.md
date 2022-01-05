
Objective-C 中 NULL、nil、Nil、NSNull 的定义及不同


在 C 语言中用 0 来作为“不存在”的原始值，而用 NULL 作为指针空值。在 Objective－C 中，则有几种不同的方式来表示“不存在”，分别有：NULL、nil、Nil、NSNull。下面我们来看看这几种空值的定义以及使用上的不同。

注：以下各种空值定义的源码摘自 iOS 10.0 SDK 中的相关头文件。

## NULL

NULL 定义在 usr/include/sys/_types/_null.h 文件里：

```c
#ifndef NULL 
#define NULL  __DARWIN_NULL
#endif  /* NULL */
```

其中 __DARWIN_NULL 的定义在 usr/include/sys/__types.h 文件里，如下：

```c
#ifdef __cplusplus
#  ifdef __GNUG__
#    define __DARWIN_NULL __null
#  else /* ! __GNUG__ */
#    ifdef __LP64__
#      define __DARWIN_NULL (0L)
#    else /* !__LP64__ */
#      define __DARWIN_NULL 0
#    endif /* __LP64__ */
#  endif /* __GNUG__ */
#else /* ! __cplusplus */
#  define __DARWIN_NULL ((void *)0)
#endif /* __cplusplus */
```

上述代码首先定义在 C++ 环境下不同编译器的 __DARWIN_NULL 的取值，然后定了其他环境下 __DARWIN_NULL 的值，因此在 Objective-C 中 NULL 的最终定义为：

>#define NULL ((void*)0)

即 NULL 本质上是：(void*)0。

***使用惯例：***

NULL 一般用于表示 C 指针空值，例如：

```c
int *pointerToInt = NULL;
char *pointerToChar = NULL;
struct TreeNode *rootNode = NULL;
```

## nil

nil 定义在 usr/include/objc/objc.h 文件里：

```c
#ifndef nil
#  if __has_feature(cxx_nullptr)
#    define nil nullptr
#  else
#    define nil __DARWIN_NULL
#  endif
#endif
```

其中 __has_feature(cxx_nullptr) 用于判断当前环境是否有 C++ 的 nullptr 特性，如果有，nil 定义为 nullptr，否则 nil 定义为 __DARWIN_NULL，所以在 Objective-C 中 nil 的最终定义为：

>#define nil ((void*)0)

也就是说，nil 本质上也是：(void *)0，与 NULL 一致。

***使用惯例：***

nil 用于表示指向 Objective-C 对象（id 类型的对象，或者使用 @interface 声明的 OC 对象）的指针为空，例如：

```c
NSString *someString = nil;
NSURL *someURL = nil;
id someObject = nil;

if (anotherObject == nil) // do something
```

## Nil

Nil 定义在 usr/include/objc/objc.h 文件里：

```c
#ifndef Nil
#  if __has_feature(cxx_nullptr)
#    define Nil nullptr
#  else
#    define Nil __DARWIN_NULL
#  endif
#endif
```

与上述 nil 一致，Nil 本质上也是：(void *)0。

***使用惯例：***

Nil 用于表示指向 Objective-C 类（Class）类型的指针为空，例如：

```c
Class someClass = Nil;
Class anotherClass = [NSString class];
```

## NSNull

NSNull 定义在 NSNull.h 文件里：

```c
#import <Foundation/NSObject.h>

NS_ASSUME_NONNULL_BEGIN

@interface NSNull : NSObject <NSCopying, NSSecureCoding>


+ (NSNull *)null;

@end


NS_ASSUME_NONNULL_END


```

从上述定义中，我们可知 NSNull 是一个 Objective-C 对象，是一个用于表示空值的类，而且它只有一个单例方法：+[NSNull null]，一般用于在集合对象中保存一个空的占位对象。

使用惯例：在 Foundation 集合对象（NSArray、NSDictionary、NSSet 等）中， nil 通常被用于表示集合对象结束的标志，因此无法用 nil 来存储一个空值，所以一般用 [NSNull null] 空对象来存储。另外，在 NSDictionary 的 -objectForKey: 方法中，如果当前字典中 key 对应的值不存在时，该方法会返回 nil，表明当前 key 在字典中未添加，但是如果我们想明确表示某一 key 已经在字典中添加，但是它没有值，这时候就可以用 [NSNull null] 来赋值表示。

// 当 NSArray 里遇到 nil 时，就说明这个数组对象的元素截止了，即 NSArray 只关注 nil 之前的对象，nil 之后的对象会被抛弃。

NSArray *array = [NSArray arrayWithObjects:@"one", @"two", nil];
 
// 错误的使用

```c
NSMutableDictionary *dict = [NSMutableDictionary dictionary];

[dict setObject:nil forKey:@"someKey"];
```
 
// 正确的使用

```c

NSMutableDictionary *dict = [NSMutableDictionary dictionary];

[dict setObject:[NSNull null] forKey:@"someKey"];
```
