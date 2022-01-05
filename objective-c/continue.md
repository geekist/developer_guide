
objective C 中遍历数组的四种方法：

```c
#import <Foundation/Foundation.h>

@interface Tire : NSObject

@end
@implementation Tire

-(NSString *)description
{
    return @"I am a Tire";
}

@end

int main(int argc, const char * argv[]) {
    @autoreleasepool {
        // insert code here...

        NSMutableArray *array = [NSMutableArray arrayWithCapacity:40];
        for(NSInteger i = 0;i < 4;i++)
        {
            Tire *tire = [Tire new];//创建类对象tire
            [array addObject:tire];//为可变数组array添加元素
        }
        //1、通过索引遍历数组
        for(NSInteger i = 0;i < [array count];i++)
        {
            NSLog(@"index %lu has %@",(long)i,[array objectAtIndex:i]);//结合计数和取值功能来输出数组内容（使用方法：-(id)objectAtIndex:(NSUInteger)index）
        }
        [array removeObjectAtIndex:1];//删除数组中索引为1的元素
        for(NSInteger i = 0;i < [array count];i++)
        {
            NSLog(@"index %lu has %@",(long)i,array[i]);//使用数组字面量语法来获取数组元素
        }
        //2、使用NSEnumerator遍历数组
        NSEnumerator *enumerator = [array objectEnumerator];
        id thingie;
        while(thingie = [enumerator nextObject])
        {
            NSLog(@"I found %@",thingie);
        }
        //3、使用快速枚举遍历数组
        NSString *object;
        for(object in array)
        {
            NSLog(@"%@",object);
        }
        //4、通过代码块方法遍历数组（-(void)enumerateObjectsUsingBlock:(void(^)(id obj,NSUInteger idx,BOOL *stop))block）
        [array enumerateObjectsUsingBlock:^(NSString *string,NSUInteger index,BOOL *stop){
            NSLog(@"You found %@",string);
        }];

    }
    return 0;

}


```