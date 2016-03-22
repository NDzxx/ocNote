# 对象的拷贝

##系统类对象的拷贝

C++中有拷贝构造函数，Java中需要实现Cloneable接口，在clone方法中进行操作。  
但是不过OC更偏向于Java这种方式，OC中如果一个对象需要被拷贝，他需要实现协议：
<NSCopying>、<NSMutableCopying>

```
int main(int argc, const charchar * argv[]) {  
    @autoreleasepool {  
                    
        //copy方法返回的是一个不可变对象，mutableCopy方法返回的是一个可变对象  
          
        /* 
        NSMutableArray *array1 = [NSMutableArray arrayWithObjects:@"one",@"two",nil]; 
        NSMutableArray *array2 = [array1 retain]; 
        //retain只是引用计数+1，没有创建新的对象 
        //array1与array2指针相同，指向同一个对象 
        if(array1 == array2){ 
            NSLog(@"array1 == array2"); 
            NSLog(@"array1的引用计数:%ld",array1.retainCount); 
        } 
         */  
          
        NSMutableArray *array1 = [NSMutableArray arrayWithObjects:@"one",@"two",nil];  
        //复制对象，创建一个新的副本对象  
        //这里使用copy方法复制，返回的是一个不可变数组，但是用一个可变数组来声明，但是我们关心的是指针的的内容，而不是类型  
        //所以array2的真实类型还是不可变类型的  
        NSMutableArray *array2 = [array1 copy];//array2计数为:1,因为是新创建出来的对象  
          
        //使用mutableCopy方法，返回的就是可变数组  
        //当然这种方法只针对于那些有可变对象之分有用，对于其他的对象这个方法和copy方法的效果是一样的  
        NSMutableArray *array3 = [array1 mutableCopy];  
          
        if(array1 != array2){  
            NSLog(@"array1 != array2");  
            NSLog(@"array1的引用计数:%ld",array1.retainCount);  
            NSLog(@"array2的引用计数:%ld",array2.retainCount);  
        }  
        [array2 release];  
        [array1 release];  
          
          
    }  
    return 0;  
}  
```

###copy方法和mutableCopy方法的区别

这两个方法的区别只在于那些有可变对象和不可变对象之分的对象上，对于没有这种区分的对象来说，这两个方法的效果是一样的。

[不可变对象 copy]是假拷贝，等价于[不可变对象 retain]

[不可变对象 mutableCopy是真拷贝

##对象的拷贝
同C++一样，oc对象拷贝也有深复制和浅复制之分

浅拷贝：只拷贝所有属性对象的指针  

深拷贝：拷贝属性对象的内容

person.h
```
    //  
    //  Person.h  
    //  31_DeepCopy  
    //  
    //  Created by jiangwei on 14-10-13.  
    //  Copyright (c) 2014年 jiangwei. All rights reserved.  
    //  
      
    #import <Foundation/Foundation.h>  
      
    @interface Person : NSObject <NSCopying>  //实现协议
      
    @property(nonatomic,retain)NSMutableArray *apples;  
    @property(nonatomic)int age;  
      
    @end  
```
person.m
```
//
//  Person.m
//  31_DeepCopy
//
//  Created by jiangwei on 14-10-13.
//  Copyright (c) 2014年 jiangwei. All rights reserved.
//

#import "Person.h"

@implementation Person

- (id)copyWithZone:(NSZone *)zone{
    //创建一个新的副本对象
    //这个方法是会被继承的，所以这里还是不用
    //[Person allocWithZone:<#(struct _NSZone *)#>];
    Person * p = [[self class] allocWithZone:zone];
    //p.apples = _apples;//是指针赋值，所以还是浅拷贝
    //深拷贝
    //拷贝之后引用计数会+1，需要release以下
    p.apples = [_apples mutableCopy];
    p.age = _age;
    
    [p.apples release];
    
    //但是如果我们使用->语法就不需要了，因为我们没有使用set方法，引用计数没有操作
    //但是这种方式我们不采用
    //p->_apples = [_apples mutableCopy];
    
    return p;
}

@end

```
我们看到，Person实现了NSCopying协议，然后需要实现一个方法：copyWithZone

在这个方法中我们开始进行拷贝操作：

Person类中有一个属性类型是数组

这里我们需要生成一个Person对象，然后进行属性的拷贝，最后在返回这个对象
