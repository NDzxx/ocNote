#自动释放池
　自动释放池（Autorelease pool）是OC的一种内存自动回收机制，可以将一些临时变量通过自动释放池来回收统一释放。
 
自动释放池的原理： 

1.系统中存在一个自动释放池栈， 当遇到 @autoreleasepool { 把这个自动释放池压入栈中
但遇到与之对应 } 自动释放池，出栈，它出栈的时候，会对池中所有对象进行一次release操作  
2.自动释放池栈中，只有栈顶自动释放池是活动的，其他的都在休眠  
3.当你调用autorelease的时候，它会把这个对象放在栈顶的自动释放池中 

使用例子
```
#import <Foundation/Foundation.h>
#import "Person.h"

int main(int argc, const char * argv[]) {

    /*
    Person *p = [[Person alloc] init];
    @autoreleasepool {
//        Person *p = [[[Person alloc] init] autorelease];
//        [p run];
        
        // 2.在自动释放池中创建了对象, 一定要调用autorelease,才会将对象放入自动释放池中
//        Person *p = [[Person alloc] init];
//        [p run];
        
        // 3.只要在自动释放池中调用autorelease, 就会将对象放入自动释放池
        p = [p autorelease];
        [p run];
    }
    // 1.一定要在自动释放池中调用autorelease, 才会将对象放入自动释放池中
//    Person *p = [[[Person alloc] init] autorelease];
     */
    
    /*
    // 4.一个程序中可以创建N个自动释放池, 并且自动释放池还可以嵌套
    // 如果存在多个自动释放池的时候, 自动释放池是以 "栈" 的形式存储的
    // 栈的特点: 先进后出
    
    // 给一个对象方法发送一条autorelease消息, 永远会将对象放到栈顶的自动释放池
    @autoreleasepool { // 创建第一个释放池
        @autoreleasepool { // 创建第二个释放池
            @autoreleasepool { // 创建第三个释放池
                Person *p = [[[Person alloc] init] autorelease];
                [p run];
            } // 第三个释放池销毁
            
            Person *p = [[[Person alloc] init] autorelease];
            
        }// 第二个释放池销毁
    }// 第一个释放池销毁
    */
    
    @autoreleasepool {
        // 千万不要写多次autorelease
        // 一个alloc, new对应一个autorelease
//        Person *p = [[[[Person alloc] init] autorelease] autorelease];
        
        // 如果写了autorelease就不要写release
        // 总之记住: 一个alloc/new对应一个autorelease或者release
        Person *p = [[[Person alloc] init] autorelease];
        [p release];
    }
    
    return 0;
}

```


