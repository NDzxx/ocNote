# 其他类(NSNumber,NSDate,NSExcetion)

##NSNumber

这个类主要是用来封装基本类型的，说到这里，就不得不说一下了：

OC中的集合是不允许存入基本类型的，所以NSNumber类就诞生了，需要将基本类型封装一下，然后存进去，这个类似于Java中的自动装箱和拆箱，Java中的集合其实也是不允许存入基本类型的，但是我们任然看可以那么操作，就是因为有自动装箱功能，如果你将Eclipse的JDK改成5.0之前的，你看看还能操作吗。。

**注：那为什么基本类型不能存入到集合中呢？**

原因是在操作集合的时候会操作集合元素中的方法，但是基本类型是没有方法的。

从这一点上我们就可以看出NSNumber是很重要的，后面一定会经常用到。
```
#import <Foundation/Foundation.h>  
  
//包装基本数据类型  
int main(int argc, const charchar * argv[]) {  
    @autoreleasepool {  
        //1.----------------创建NSNumber  
        //封包  
        NSNumber *intNumber = [NSNumber numberWithInt:3];  
        NSNumber *floatNumber = [NSNumber numberWithFloat:9.8f];  
        NSArray *array = @[intNumber,floatNumber];  
          
        //解包  
        int value = [intNumber intValue];  
        float values = [floatNumber floatValue];  
        NSString *str = [intNumber stringValue];  
          
        //优化语法  
        NSNumber *intNumbers = @12;  
          
          
        //如何将结构体存入到数组中  
        //使用NSValue对结构体进行封装  
        //NSValue是NSNumber的父类  
        //封包  
        NSRange rang = {1,3};  
        NSValue *v = [NSValue valueWithRange:rang];  
          
        //解包  
        rang = [v rangeValue];  
          
          
        //对自定义的结构体进行封包  
        struct WXPoint{  
            float x;  
            float y;  
        };  
          
        struct WXPoint p = {50,100};  
        //第一个参数是结构体的变量地址，第二个参数是类型  
        NSValue *v1 = [NSValue value:&p withObjCType:@encode(struct WXPoint)];  
          
        //解包  
        struct WXPoint p1;  
        [v1 getValue:&p1];  
          
          
        //NSNULL的使用  
        //数组中是没有办法存放一个空对象的nil  
        //NSArray *nilArray = @[nil,nil,nil];//报错的  
        //有时候我们需要在数组中存入一个空对象的需求  
        NSNull *n1 = [NSNull null];  
        NSNull *n2 = [NSNull null];  
        NSArray *nullArray = @[n1,n2];  
        NSLog(@"nullArray =%@",nullArray);  
          
          
    }  
    return 0;  
}  
```
##NSDate 日期类
