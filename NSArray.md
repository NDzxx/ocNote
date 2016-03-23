# NSArray类和NSMutableArray类
NSArray类(类似C++数组)和NSMutableArray类(类似c++ vector)


##NSArray类  
###创建方法
```
//1.---------------------数组的创建  
        NSString *s1 = @"zhangsan";  
        NSString *s2 = @"lisi";  
        NSString *s3 = @"wangwu";  
        //最后的nil相当于结束的标记  
        NSArray *array1 = [[NSArray alloc] initWithObjects:s1,s2,s3, nil nil];  
        //打印的时候会调用description方法  
        //相当于:array1.description  
        NSLog(@"%@",array1);  
          
        //使用类方法创建  
        array1 = [NSArray arrayWithObjects:s1,s2,s3, nil nil];  
          
        //创建一个数组，将源数组中的数据拿出来放进去  
        NSArray *array2 = [NSArray arrayWithArray:array1];  
```
###使用下标访问元素
```
//2.----------------------objectAtIndex  
//访问数组中的数据，数组中存放的是对象的指针  
NSString *str1 = [array1 objectAtIndex:0];
```
###获取数组的大小
```
    //3.----------------------count  
    //获取数组元素中的个数  
    NSUInteger count = [array1 count];//返回的是一个无符号数值  
```