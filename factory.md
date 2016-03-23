# 类工厂方法

什么是类工厂方法:  

 用于快速创建对象的类方法, 我们称之为类工厂方法  
 
 类工厂方法中主要用于 给对象分配存储空间和初始化这块存储空间
 
 规范:  
 1.一定是类方法 +  
 2.方法名称以类的名称开头, 首字母小写  
 3.一定有返回值, 返回值是id/instancetype  
 person.h
 ```
#import <Foundation/Foundation.h>

@interface Person : NSObject

@property int age;

+ (instancetype)person;

+ (instancetype)personWithAge:(int)age;
@end
 ```
 person.m
 ```
 
#import "Person.h"

@implementation Person

/*
+ (instancetype)person
{
    Person *p = [Person alloc];
    Person *p1 = [p init];
    return p1;
}
 */
+ (instancetype)person
{
    return [[Person alloc] init];
}

+ (instancetype)personWithAge:(int)age
{
    Person *p = [[Person alloc] init];
    p.age = age;
    return p;
}

@end
 ```
 
 