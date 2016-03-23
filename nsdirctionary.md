# NSDirctionary类以及NSMutableDirctionary类  
这个类似C++字典
## NSDirctionary类
###创建方法
```
    //1.---------------------字典的创建  
    NSArray *array1 = [NSArray arrayWithObjects:@"zhangsan",@"zhangfei", nil nil];  
    NSArray *array2 = [NSArray arrayWithObjects:@"lisi",@"liping", nil nil];  
      
    //第一个元素:key:@"zhang" value:array1  
    //第二个元素:key:@"li"  value:array2  
    NSDictionary *dict = [[NSDictionary alloc] 
                           initWithObjectsAndKeys:array1,@"zhang",
                           array2,@"li", nil nil];  
    NSLog(@"%@",dict);  
```
key-value的格式，需要注意的是，结尾是nil
###添加元素
```
    //存放一个元素  
    NSDictionary *dict2 = [NSDictionary dictionaryWithObject:array1 forKey:@"zhang"];  
```
###获取所有元素的key
```
    //2.---------------------获取所有的key  
    NSArray *allKeys = [dict allKeys];  
```
###获取所有元素的value
```
//3.---------------------获取所有的value  
//可能为二维数组  
NSArray *allValues = [dict allValues]; 
```
###通过key获取value
```
    //4.---------------------通过key获取value  
    NSArray *values = [dict objectForKey:@"zhang"];  
```
###NSDirctionary的快速创建方式和访问方式 (常用)
```
NSDictionary *dict3 = @{@"zhangsan":array1,@"lisi":array2};  
NSLog(@"%@",dict3);  
  
NSArray *array4 = dict3[@"zhang"]; 
```
##NSMutableDirctionary类
###创建方法
```
//1.-------------创建一个可变字典,大小为3  
NSMutableDictionary *md1 = [[NSMutableDictionary alloc] initWithCapacity:3];  
NSArray *array1 = [[NSArray alloc] initWithObjects:@"zhangsan",@"lis", nil nil]; 
```

###添加元素
```
    //2.-------------添加元素  
    [md1 setObject:array1 forKey:@"zhang"];  
      
    NSMutableDictionary *md2 = [[NSMutableDictionary alloc] initWithCapacity:3];  
    [md2 addEntriesFromDictionary:md1];  
```
###删除元素
```
//3.-------------删除元素  
[md1 removeObjectForKey:@"zhang"];//以key删除  
[md1 removeAllObjects];//删除所有元素  
[md1 removeObjectsForKeys:array1];//以value删除 
```

###遍历字典
```
    //4.-------------循环遍历字典  
    //快速遍历  
    for (NSString *key in md1) {  
        NSArray *values = [md1 objectForKey:key];  
        for(NSString *v in values){  
            NSLog(@"%@",v);  
        }  
    }  
      
    //普通遍历  
    NSArray *allKey = [md1 allKeys];  
    for(int i=0;i<allKey.count;i++){  
        NSArray *value = [md1 objectForKey:allKey[i]];  
    }  
```
###
