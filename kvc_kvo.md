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

children.m
```
#import "Children.h"  
  
@implementation Children  
  
- (id) init{  
    self = [super init];  
    if(self != nil){  
        //启动定时器  
        [NSTimer scheduledTimerWithTimeInterval:1 target:self selector:@selector(timerAction) userInfo:nil repeats:YES];  
        self.hapyValue= 100;  
    }  
    return self;  
}  
  
- (void) timerAction:(NSTimer *) timer{  
    //使用kvc set方法修改属性值，才能触发KVO  
      
    int value = _hapyValue;  
    [self setHapyValue:--value];  
      
    int values = _hurryValue;  
    [self setHurryValue:--values];  
}  
  
  
@end  
```

Nure.h
```
      
    #import <Foundation/Foundation.h>  
      
    @class Children;  
    @interface Nure : NSObject{  
        Children *_children;  
    }  
      
    - (id) initWithChildren:(Children *)children;  
      
    @end  
```

Nurse.m
```
#import "Nure.h"  
#import "Children.h"  
  
@implementation Nure  
  
- (id) initWithChildren:(Children *)children{  
    self = [super init];  
    if(self != nil){  
        _children = children;  
          
        //观察小孩的hapyValue  
        //使用KVO为_children对象添加一个观察者，用于观察监听hapyValue属性值是否被修改  
        /*第一个参数：监听者，这里是Nure,所以可以直接传递self

        第二个参数：监听对象的属性名

        第三个参数：监听这个属性的状态：这里可以使用|进行多种组合操作，属性的新值和旧值

        第四个参数：传递内容给监听方法*/
        [_children addObserver:self forKeyPath:@"hapyValue" options:NSKeyValueObservingOptionNew |NSKeyValueObservingOptionOld context:@"context"];  
          
        //观察小孩的hurryValue  
        [_children addObserver:self forKeyPath:@"hurryValue" options:NSKeyValueObservingOptionNew |NSKeyValueObservingOptionOld context:@"context"];  
    }  
    return self;  
}  
  
//触发方法  
- (void)observeValueForKeyPath:(NSString *)keyPath ofObject:(id)object change:(NSDictionary *)change context:(voidvoid *)context{  
    NSLog(@"%@",change);  
    //通过打印change，我们可以看到对应的key  
      
    //通过keyPath来判断不同属性的观察者  
    if([keyPath isEqualToString:@"hapyValue"]){  
        //这里change中有old和new的值是因为我们在调用addObserver方法时，用到了  
        //NSKeyValueObservingOptionNew | NSKeyValueObservingOptionOld；想要哪一个就用哪一个  
        //[change objectForKey:@"old"]是修改前的值  
        NSNumber *hapyValue = [change objectForKey:@"new"];//修改之后的最新值  
          
        NSInteger *value = [hapyValue integerValue];  
          
        if(value < 90){  
            //do something...  
        }  
    }else if([keyPath isEqualToString:@"hurryValue"]){  
        //这里change中有old和new的值是因为我们在调用addObserver方法时，用到了  
        //NSKeyValueObservingOptionNew | NSKeyValueObservingOptionOld；想要哪一个就用哪一个  
        //[change objectForKey:@"old"]是修改前的值  
        NSNumber *hurryValue = [change objectForKey:@"new"];//修改之后的最新值  
          
        NSInteger *value = [hurryValue integerValue];  
          
        if(value < 90){  
            //do something...  
        }  
    }  
      
    NSLog(@"%@",context);//打印的就是addObserver方法的context参数  
      
      
      
    //使用KVC去修改属性的值，也会触发事件  
}  
  
- (void)dealloc{  
      
    //移除观察者  
    [_children removeObserver:self forKeyPath:@"hapyValue"];  
    [_children removeObserver:self forKeyPath:@"hurryValue"];  
      
}  
  
@end  
```