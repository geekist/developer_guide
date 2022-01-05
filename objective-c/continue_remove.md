

我们在遍历可变数组时，最好不要做删除数组中元素的操作。

因为删除操作可能会引起数组容量的变化，导致数组越界等问题。

以前在使用for循环遍历的时候遇到过这个问题。

当时的做法是使用enumerateObjectsUsingBlock: ，但是这次又遇到这个问题时，顺便好好的测试了一下 for、for in、enumerateObjectsUsingBlock:。

***使用enumerateObjectsUsingBlock***

```c
NSMutableArray *array = [NSMutableArray arrayWithArray:@[@"1",@"2",@"3",@"4",@"5"]];
[array enumerateObjectsUsingBlock:^(id  _Nonnull obj, NSUInteger idx, BOOL * _Nonnull stop) {
    if ([obj isEqualToString:@"3"]) {
        [array removeObject:obj];
    }
}];
NSLog(@"%@",array);
```

// 输出结果:(1,2,4,5)

结果正常。

***使用for in :***

```c
NSMutableArray *array = [NSMutableArray arrayWithArray:@[@"1",@"2",@"3",@"4",@"5"]];
for (NSString *obj in array) {
    if ([obj isEqualToString:@"3"]) {
        [array removeObject:obj];
    }
}
NSLog(@"%@",array);
```

结果出现崩溃，信息如下：

2016-11-11 22:48:06.886 Solutions[15297:2231002] *** Terminating app due to uncaught exception 'NSGenericException', reason: '*** Collection <__NSArrayM: 0x1001060b0> was mutated while being enumerated.'
*** First throw call stack:
(
    0   CoreFoundation                      0x00007fff929c14f2 __exceptionPreprocess + 178
    1   libobjc.A.dylib                     0x00007fff8fcb2f7e objc_exception_throw + 48
    2   CoreFoundation                      0x00007fff92a2815c __NSFastEnumerationMutationHandler + 300
    3   Solutions                           0x0000000100000d1c main + 508
    4   libdyld.dylib                       0x00007fff90cd05ad start + 1
)
libc++abi.dylib: terminating with uncaught exception of type NSException

***使用for :***

```c
NSMutableArray *array = [NSMutableArray arrayWithArray:@[@"1",@"2",@"3",@"4",@"5"]];
for (int i = 0; i < array.count; i++) {
    NSString *obj = array[i];
    if ([obj isEqualToString:@"3"]) {
        [array removeObject:obj];
    }
}
NSLog(@"%@",array);
```
// 输出结果是：(1,2,4,5)

结果正常，可能是苹果对普通的for循环内部的处理做了修正，以前这样写是会报错的。

其实，最正确与稳妥的方案，是对数组逆序遍历，然后再删除元素就没有问题了。

例如 

***逆序遍历：***

```c
NSMutableArray *array = [NSMutableArray arrayWithArray:@[@"1",@"2",@"3",@"4",@"5"]];
for (NSString *obj in array.reverseObjectEnumerator) {
    if ([obj isEqualToString:@"3"]) {
        [array removeObject:obj];
    }
}
NSLog(@"%@",array);
// 输出结果：(1,2,4,5)
```
使用

```
NSMutableArray *array = [NSMutableArray arrayWithArray:@[@"1",@"2",@"3",@"4",@"5"]];
[array enumerateObjectsWithOptions:NSEnumerationReverse usingBlock:^(id  _Nonnull obj, NSUInteger idx, BOOL * _Nonnull stop) {
            if ([obj isEqualToString:@"3"]) {
                [array removeObject:obj];
            }
        }];

NSLog(@"%@",array);
```

// 输出结果：(1,2,4,5)

关于for 的逆序

```c
NSMutableArray *array = [NSMutableArray arrayWithArray:@[@"1",@"2",@"3",@"4",@"5"]];
for (int i = array.count - 1; i >= 0; i--) {
    NSString *obj = array[i];
    if ([obj isEqualToString:@"3"]) {
        [array removeObject:obj];
    }
}
NSLog(@"%@",array);
```
// 输出结果：(1,2,4,5)

以前做Java 开发的时候，也遇到过这种遍历删除元素的，那时候的做法是在遍历，删除元素后做一次i–操作。

现在想来，也可以逆序遍历然后直接删除即可。