# 归档和解档
oc归档解档实质就是对象序列化，类似mfc序列化，google的protobuf等东东  
下面看下具体使用
##归档
###第一种形式：归档对象  
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
###对多个对象进行归档到一个文件
```
    //第二种方式  
    //第一种方式的缺陷是一个对象归档成一个文件  
    //但是第二种方式，多个对象可以归档成一个文件  
     NSArray *array = [NSArray arrayWithObjects:@"zhangsan",@"lisi", nil nil];  
     NSMutableData *data = [NSMutableData data];  
     NSKeyedArchiver *archiver = [[NSKeyedArchiver alloc] initForWritingWithMutableData:data];  
     //编码  
     [archiver encodeObject:array forKey:@"array"];  
     [archiver encodeInt:100 forKey:@"scope"];  
     [archiver encodeObject:@"jack" forKey:@"name"];  
       
     //完成编码，将上面的归档数据填充到data中，此时data中已经存储了归档对象的数据  
     [archiver finishEncoding];  
     [archiver release];  
       
     NSString *filePath = [NSHomeDirectory() stringByAppendingPathComponent:@"array.src"];  
     BOOL success = [data writeToFile:filePath atomically:YES];  
     if(success){  
     NSLog(@"归档成功");  
     }  
```
多个对象归档的话，这里要用到一个类：NSMutableData和NSData，他们两的区别很简单，一个是可变的，一个是不可变的。然后这里还创建了一个归档器：NSKeyedArchiver,这个类负责进行指定类型的编码操作，然后将数据填充到NSMutableData类。归档的时候对每个类型对象用一个key进行对应，这个NSData和NSDirctionary很类似了。
##解档

