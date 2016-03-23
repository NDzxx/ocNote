# KVC和KVO

##KVC
OC中的KVC操作就和Java中使用反射机制去访问类的private权限的变量，很暴力的，这样做就会破坏类的封装性，本来类中的的private权限就是不希望外界去访问的，但是我们这样去操作，就会反其道而行，但是我们有时候真的需要去这样做，哎。所以说有些事不是都是顺其自然的，而是需要的时候自然就诞生了。
dog.h
```      
    #import <Foundation/Foundation.h>  
      
    @interface Dog : NSObject  
      
    @end  
```
dog.m
```
  
#import "Dog.h"  
  
@implementation Dog  
  
@end  
```
person.h  
```
#import <Foundation/Foundation.h>  
#import "Dog.h"  
  
@interface Person : NSObject{  
@private  
    NSString *_name;  
    NSDog *_dog;  
      
    NSInteger *age;  
}  
  
@end 
```
person.m
```
#import "Person.h"  
  
@implementation Person  
  
- (NSString *)description{  
    NSLog(@"%@",_name);  
    return _name;  
}  
  
@end

```
Person类中我们定义了两个属性，但是这两个属性对外是不可访问的，而且也没有对应的get/set方法。我们也实现了description方法，用于打印结果
main.m

```
  
#import <Foundation/Foundation.h>  
#import "Person.h"  
#import "Dog.h"  
  
//KVC：很暴力，及时一个类的属性是私有的，而且也没有get/set方法，同样可以读写  
//相当于Java中的反射，破坏类的封装性  
int main(int argc, const charchar * argv[]) {  
    @autoreleasepool {  
          
        Person *p = [[Person alloc] init];  
          
        //设置值  
        //这里setValue方法：第一个参数是value,第二个参数是key(就是类的属性名称)  
        [p setValue:@"jiangwei" forKey:@"name"];  
          
        Dog *dog = [[Dog alloc] init];  
        [p setValue:dog forKey:@"dog"];  
          
        //KVC设置值时，如果属性有set方法，则优先调用set方法，如果没有则直接设置上去，get方法类似  
          
        //读取值  
        NSString *name = [p valueForKey:@"name"];  
          
        //设置基本数据类型  
        //这里需要将基本类型转化成NSNumber  
        //在设置值的时候，会有自动解包的过程，NSNumber会解包赋值给age  
        [p setValue:@22 forKey:@"age"];  
          
        NSLog(@"%@",p);  
          
        return 0;  
    }  
    return 0;  
}  
```
##KVO操作

KVO操作在OC中也是经常会用到的，而且这种机制在java中不存在的。

它的作用就是用来监听类中属性值的变化，实现原理是观察者模式，当然我们也可以使用观察者模式在Java中实现这样的机制
children.h
```      
    #import <Foundation/Foundation.h>  
      
    @interface Children : NSObject  
      
    @property NSInteger *hapyValue;  
    @property NSInteger *hurryValue;  
      
      
    @end  
```