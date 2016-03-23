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
    NSDictionary *dict = [[NSDictionary alloc] initWithObjectsAndKeys:array1,@"zhang",array2,@"li", nil nil];  
    NSLog(@"%@",dict);  
```



