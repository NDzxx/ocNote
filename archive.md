# 归档和解档
oc归档解档实质就是对象序列化，类似mfc序列化，google的protobuf等东东  
下面看下具体使用
##归档
```
    //第一种形式：归档对象  
    //对象----》文件  
     NSArray *array = [NSArray arrayWithObjects:@"zhang",@"wangwu",@"lisi",nil];  
     //NSHomeDirectory()返回当前用户路径
     NSString *filePath = [NSHomeDirectory()
                           stringByAppendingPathComponent:@"array.src"];  
       
     BOOL success = [NSKeyedArchiver archiveRootObject:array toFile:filePath];  
     if(success){  
         NSLog(@"保存成功");  
     }  
```
