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
Xcode4.4之后新增了NSArray快速创建的一种方式
```
    //xcode4.4以后，编译器对数组的创建和访问语法做了优化  
    NSArray *array7 = @[s1,s2,s3];  
    NSString *s = array7[0];  
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
###是否包含某一个元素
```
    //4.----------------------containsObject  
    //判断数组中是否包含某一个对象,判断的是指针对象值，不是对象中的值  
    BOOL isContains = [array1 containsObject:@"zhangsan"];  
```
###找到一个元素在数组中的下标
```
//5.----------------------indexOfObject  
//返回一个对象在数组中的下标值  
NSUInteger index = [array1 indexOfObject:@"zhangsan"];  
if(index == NSNotFound){  
    //没有找到  
}else{  
    //找到了  
} 
```
###使用指定的连接符连接数组中所有的元素
```
    //6.----------------------componentsJoinedByString  
    //数组中存放的如果是字符串的话，可以使用连接符将其所有的元素进行连接  
    //注意数组中的元素必须都是字符串  
    NSString *content = [array1 componentsJoinedByString:@","];  
```
###在数组的尾部增加一个元素
```
    //8.----------------------arrayByAddingObject  
    //在原来的数组后面追加元素,返回一个新的数组对象，因为他是不可变的  
    NSArray *array3 = [array1 arrayByAddingObject:@"zhaoliu"];  
```
###数组的遍历  
```
//数组的遍历  
for(int i=0;i<array1.count;i++){  
    NSString *str = [array1 objectAtIndex:i];  
    NSLog(@"%@",str);  
}  
  
//快速遍历  
for(NSString *s in array1){  
    NSLog(@"%@",s);  
}
```
