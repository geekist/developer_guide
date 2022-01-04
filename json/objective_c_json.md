

## Objective-C 中Json解析

Objective-C 操作JSON 主要使用的是NSJSONSerialization 这个类。NSJSONSerialization 包含了以下五个类函数


* 1.  + (BOOL)isValidJSONObject:(id)obj;

判断 该实例(obj)是否为JSONObject

需满足下面三个条件

    （1）.obj 是NSArray 或 NSDictionay 以及他们派生出来的子类

    （2）.obj 包含的所有对象是NSString,NSNumber,NSArray,NSDictionary 或NSNull

    （3）.NSNumber的对象不能是空或无穷大


* 2. + (NSData *)dataWithJSONObject:(id)obj options:(NSJSONWritingOptions)opt error:(NSError **)error;

将JSONObject的实例转成NSData


* 3. + (id)JSONObjectWithData:(NSData *)data options:(NSJSONReadingOptions)opt error:(NSError **)error;

 将NSData类型的实例转成JSONObject

* 4. + (NSInteger)writeJSONObject:(id)obj toStream:(NSOutputStream *)stream options:(NSJSONWritingOptions)opt error:(NSError **)error;


 将一个JSONObject的实例写入到一个输出流中 返回写入的长度


* 5. + (id)JSONObjectWithStream:(NSInputStream *)stream options:(NSJSONReadingOptions)opt error:(NSError **)error;

从输入流中读取成JSONObject 并返回

* 6. 根据服务端返回的数据类型接收 解析服务端返回的json格式数据。

如返回的是NSDictionary就使用字典接收 如果返回的是数组就用数组接收。

## Json和Foundation数据结构的转化

Json To Fundation

使用 JSONObjectWithData 可以将 Json 转化为 Foundation。Json的顶层可以是{} 或 []因此可以有 NSDictionary 和 NSArray 两种格式。读取使用 ObjectForKey 返回对应的对象。

```c
NSString* items = @"{"items":["item0","item1","item2"]}";
 
NSData *data= [items dataUsingEncoding:NSUTF8StringEncoding];
 
NSError *error = nil;
 
id jsonObject = [NSJSONSerialization JSONObjectWithData:data 
          options:NSJSONReadingAllowFragments 
          error:&amp;error];
 
if ([jsonObject isKindOfClass:[NSDictionary class]]){
 
  NSDictionary *dictionary = (NSDictionary *)jsonObject;
 
  NSLog(@"Dersialized JSON Dictionary = %@", dictionary);
 
}else if ([jsonObject isKindOfClass:[NSArray class]]){
 
  NSArray *nsArray = (NSArray *)jsonObject;
 
  NSLog(@"Dersialized JSON Array = %@", nsArray);
 
} else {
 
  NSLog(@"An error happened while deserializing the JSON data.");
 
}
 
NSDictionary *dict = (NSDictionary *)jsonObject;
 
NSArray* arr = [dict objectForKey:@"items"];
NSLog(@"list is %@",arr);


```


## Foundation转化为JsonObject

使用 dataWithJsonObject 可以将 Fundation 转换为 Json。其中 options:NSJSONWritingPrettyPrinted 是分行输出json ,无空格输出使用 option:kNilOptions。

下面这段代码是IOS内购获取商品列表。获取后，将内容添加到Json中。

```c

NSArray *myProduct = response.products;
NSDictionary *myDict;
NSMutableDictionary *dict = [NSMutableDictionary 
                dictionaryWithCapacity: 4];
 
for(int i = 0;i&lt;myProduct.count;++i)
{
 
  //NSLog(@"----------------------");
  //NSLog(@"Product title: %@" ,[myProduct[i] localizedTitle]);
  //NSLog(@"Product description: %@" ,[myProduct[i] localizedDescription]);
  //NSLog(@"Product price: %@" ,[myProduct[i] price]);
  //NSLog(@"Product id: %@" ,[myProduct[i] productIdentifier]);
 
  myDict = [NSDictionary dictionaryWithObjectsAndKeys:
          [myProduct[i] localizedTitle], @"title",
          [myProduct[i] localizedDescription], @"desc",
          [myProduct[i] price], @"price",
          [myProduct[i] productIdentifier], @"product", nil];
 
  [dict setValue: myDict forKey: [myProduct[i] productIdentifier]];
}
if([NSJSONSerialization isValidJSONObject:dict])
{
  NSError* error;
  NSData *str = [NSJSONSerialization dataWithJSONObject:dict 
            options:kNilOptions error:&amp;error];
  NSLog(@"Result: %@",[[NSString alloc]initWithData:str 
              encoding:NSUTF8StringEncoding]);
}
else
{
  NSLog(@"An error happened while serializing the JSON data.");
}    


```
