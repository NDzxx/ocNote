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
###单个对象解档
```
    //解归档  
    NSString *filePath = [NSHomeDirectory() stringByAppendingPathComponent:@"array.src"];  
    id array = [NSKeyedUnarchiver unarchiveObjectWithFile:filePath];  
    NSLog(@"%@",array);  
```
###对多个对象进行解档操作
```
    NSString *filePath = [NSHomeDirectory() stringByAppendingPathComponent:@"array.src"];  
    //读取归档数据  
    NSData *data = [[NSData alloc] initWithContentsOfFile:filePath];  
      
    //创建解归档对象，对data中的数据进行解归档  
    NSKeyedUnarchiver *unarchiver = [[NSKeyedUnarchiver alloc] initForReadingWithData:data];  
      
    //解归档  
    NSArray *array = [unarchiver decodeObjectForKey:@"array"];  
    NSLog(@"%@",array);  
      
    int value = [unarchiver decodeObjectForKey:@"scope"];  
    NSLog(@"%d",value);  
```
##自定义类型的归档和解档
自定义的类型可以进行归档和解档的话，必须实现一个协议：NSCoding  
看例子  
person.h
```
#import <Foundation/Foundation.h>  
  
//类只有实现NSCoding协议才能归档  
@interface Person : NSObject<NSCoding>  
  
@property(nonatomic,copy)NSString *name;  
@property(nonatomic,assign)NSInteger age;  
@property(nonatomic,retain)NSArray *apples;  
  
- (NSString *)description;  
  
@end
```
person.m
```

#import "Person.h"

@implementation Person

//解归档的时候调用
//也是一个初始化方法
- (id)initWithCoder:(NSCoder *)aDecoder{
    NSLog(@"initWithCoder");
    self = [super init];
    if(self != nil){
        /*
        _name = [aDecoder decodeObjectForKey:@"name"];
        _age = [aDecoder decodeObjectForKey:@"age"];
        _apples = [aDecoder decodeObjectForKey:@"apples"];
         */
        //一般我们将key定义成宏，这样就不会出错
        _name = [[aDecoder decodeObjectForKey:@"name"] copy];
        self.age = [aDecoder decodeObjectForKey:@"age"];
        self.apples = [aDecoder decodeObjectForKey:@"apples"];
        
    }
    return self;
}

//归档时调用此方法
- (void)encodeWithCoder:(NSCoder *)aCoder{
    NSLog(@"encodeWithCoder");
    [aCoder encodeObject:_name forKey:@"name"];//一般key和属性名是取一样的
    [aCoder encodeInteger:_age forKey:@"age"];
    [aCoder encodeObject:_apples forKey:@"apples"];
}

- (NSString *)description{
    NSString *string = [NSString stringWithFormat:@"name=%@,age=%d,apples=%@",_name,_age,_apples];
    return string;
}

@end

```
在Person.m文件中，我们需要实现协议中的两个方法：

initWithCoder (解档调用)

encodeWithCoder (归档调用)

main.m
```

#import <Foundation/Foundation.h>

#import "Person.h"
int main(int argc, const char * argv[]) {
    @autoreleasepool {
        
        Person *p = [[Person alloc] init];
        p.name = @"张三";
        p.age = 20;
        p.apples = @[@"iphone",@"ipad"];
        
        //归档
        NSString *filePath = [NSHomeDirectory() stringByAppendingPathComponent:@"person.archiver"];
        BOOL success = [NSKeyedArchiver archiveRootObject:p toFile:filePath];
        if(success){
            NSLog(@"归档成功");
        }
        
        //解归档
        Person *person = [NSKeyedUnarchiver unarchiveObjectWithFile:filePath];
        NSLog(@"%@",person);
        
        
    }
    return 0;
}

```
