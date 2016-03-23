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

```
