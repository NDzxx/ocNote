# 闭包和selector
代理模式内有使用selctor
##SEL
SEL是一种数据类型，代表方法签名，通过它就找到这个对象上方法   

个人感觉看上去就是c++的bind和function这类东西，或者理解为函数指针一类的东西   
使用  

1.定义变量 SEL 变量名称  
2.通过@selector(方法名称) 快速生成一个SEL   
3.作为方法的实参   
4.作为方法的形参  
5.作为方法返回值  
oc方法在内存中也是使用sel存储的
![OC存储方式](sel.png)

###常用方法
```
- (id)performSelector:(SEL)aSelector;
- (id)performSelector:(SEL)aSelector withObject:(id)object;
- (id)performSelector:(SEL)aSelector withObject:(id)object1 withObject:(id)object2;
//注：最多支持两个参数，参数必须是对象，可以用指针表示


-(BOOL) isKindOfClass: classObj 用来判断是否是某个类或其子类的实例
-(BOOL) isMemberOfClass: classObj 用来判断是否是某个类的实例
-(BOOL) respondsToSelector: selector 用来判断是否有以某个名字命名的方法(被封装在一个selector的对象里传递)
+(BOOL) instancesRespondToSelector: selector 用来判断实例是否有以某个名字命名的方法. 和上面一个不同之处在于, 前面这个方法可以用在实例和类上，而此方法只能用在类上.
```
SEL和NSDictionary联用可以产生如下效果
```
NSDictionary *dic = [NSDictionary dictionaryWithObjectsAndKeys:  
                                @selector(say),@"say",
                                @selector(eat),@"eat", 
                                 @selector(sleep),@"sleep",nil];  
SEL say_sel = [dic objectForKey:@"say"];
 [self performSelector: say_sel]; 
```
###sel多参数方法
有没有觉得很无语？为什么参数必须是对象？为什么最多只支持2个参数？

好在selector本身也不在乎参数类型，所以传个不是对象的玩意也行：
```
NSLog(@"%@", [test performSelector:@selector(intToString:) withObject:(id)123]);
```
可是double和struct就不能这样传递了，因为它们占的字节数和指针不一样。如果非要用performSelector的话，就只能修改参数类型为指针了：
```
- (NSString *)doubleToString:(double *)number {
return [NSString stringWithFormat:@"%f", *number];
}

double number = 123.456;
NSLog(@"%@", [test performSelector:@selector(doubleToString:) withObject:(id)(&number)]);
```

参数类型算是搞定了，可是要支持多个参数，还得费番气力。理想状态下，我们应该可以实现这2个方法：
```
@interface NSObject (extend)

- (id)performSelector:(SEL)aSelector withObjects:(NSArray *)objects;
- (id)performSelector:(SEL)aSelector withParameters:(void *)firstParameter, ...;

@end
```
先看看前者，NSArray要求所有的元素都必须是对象，并且不能为nil，所以适用的范围仍然有限。不过你可别小看它，因为你会发现根本没法用objc_msgSend来实现，因为你在写代码时没法预知参数个数。
这时候就轮到NSInvocation登场了：
```
@implementation NSObject (extend)

- (id)performSelector:(SEL)aSelector withObjects:(NSArray *)objects {
NSMethodSignature *signature = [self methodSignatureForSelector:aSelector];
NSInvocation *invocation = [NSInvocation invocationWithMethodSignature:signature];
[invocation setTarget:self];
[invocation setSelector:aSelector];

NSUInteger i = 1;
for (id object in objects) {
[invocation setArgument:&object atIndex:++i];
}
[invocation invoke];

if ([signature methodReturnLength]) {
id data;
[invocation getReturnValue:&data];
return data;
}
return nil;
}

@end


NSLog(@"%@", [test performSelector:@selector(combineString:withSecond:withThird:) withObjects:[NSArray arrayWithObjects:@"1", @"2", @"3", nil]]);
```

这里有3点要注意的：
1. 因为方法调用有self（调用对象）和_cmd（选择器）这2个隐含参数，因此设置参数时，索引应该从2开始。  
2. 因为参数是对象，所以必须传递指针，即&object。  
3. methodReturnLength为0时，表明返回类型是void，因此不需要获取返回值。返回值是对象的情况下，不需要我们来创建buffer。但如果是C风格的字符串、数组等类型，就需要自行malloc，并释放内存了。  

再来实现第2个方法：
```
- (id)performSelector:(SEL)aSelector withParameters:(void *)firstParameter, ... {
NSMethodSignature *signature = [self methodSignatureForSelector:aSelector];
NSUInteger length = [signature numberOfArguments];
NSInvocation *invocation = [NSInvocation invocationWithMethodSignature:signature];
[invocation setTarget:self];
[invocation setSelector:aSelector];

[invocation setArgument:&firstParameter atIndex:2];
va_list arg_ptr;
va_start(arg_ptr, firstParameter);
for (NSUInteger i = 3; i < length; ++i) {
void *parameter = va_arg(arg_ptr, void *);
[invocation setArgument:&parameter atIndex:i];
}
va_end(arg_ptr);

[invocation invoke];

if ([signature methodReturnLength]) {
id data;
[invocation getReturnValue:&data];
return data;
}
return nil;
}

NSLog(@"%@", [test performSelector:@selector(combineString:withSecond:withThird:) withParameters:@"1", @"2", @"3"]);

NSInteger number1 = 1, number2 = 2, number3 = 3;
NSLog(@"%@", [test performSelector:@selector(intsToString:second:third:) withParameters:number1, number2, number3]);
```
和前面的实现差不多，不过由于参数长度是未知的，所以用到了[signature numberOfArguments]。当然也可以把SEL转成字符串（可用NSStringFromSelector()），然后查找:的数量。
处理可变参数时用到了va_start、va_arg和va_end，熟悉C语言的一看就明白了。
不过由于不知道参数的类型，所以只能设为void *。而这个程序也报出了警告，说void *和NSInteger类型不兼容。而如果把参数换成double，那就直接报错了。遗憾的是我也不知道怎么判别一个void *指针究竟是指向C数据类型，还是指向一个Objective-C对象，所以最好是封装成Objective-C对象。如果只需要兼容C类型的话，倒是可 以将setArgument的参数的&去掉，然后直接传指针进去：
```
NSInteger number1 = 1, number2 = 2, number3 = 3;
NSLog(@"%@", [test performSelector:@selector(intsToString:second:third:) withParameters:&number1, &number2, &number3]);

double number4 = 1.0, number5 = 2.0, number6 = 3.0;
NSLog(@"%@", [test performSelector:@selector(doublesToString:second:third:) withParameters:&number4, &number5, &number6]);
[test release];
```
至于NSObject类添加的performSelector:withObject:afterDelay:等方法，也可以用这种方式来支持多个参数。  、

引用来源：
参考文章：http://www.cnblogs.com/ygm900/archive/2013/01/16/2862676.html  

参考文章:http://blog.csdn.net/jjwwmlp456/article/details/41844055(:简单使用 )   

参考文章:http://www.cnblogs.com/buro79xxd/archive/2012/04/10/2440074.html(进阶)   
 
参考文章2：http://blog.csdn.net/libaineu2004/article/details/43733827（原理和总结）  

##闭包(block)

block 是一种数据类型，封装代码,本质上就是代码块，类似js的闭包，c++的lamda
(区别在于是可以自动捕获外部变量的匿名函数) 
函数不能在方法内部或函数内部，但是block可以   
作用： 
Block可以作为函数参数或者函数的返回值，而本身又可以带输入参数或返回值 
block，在多线程、异步任务，集合遍历，集合排序、动画专场用得多 

定义block类型的变量的格式   

返回值类型 (^block变量名称)(形参列表);

实现格式   

^ 返回值类型 (形参列表){ 语句….. };
例子：
```
 void(^myblock)() = ^ {

        NSLog(@"---------");

        NSLog(@"---------");

    };//经常是这样写，直接带实习，类似lamda
    
    //利用block变量调用block内部的代码

    myblock(); //运行block
    
    //定以一个block返回值为int函数的参数为int int

   int(^sumblock)(int ,int ) = ^(int a,int b){

       return a + b;

    };

   int sum = sumblock(10,11);

   NSLog(@"sum = %d",sum);
```
注意：在block 中可以访问局部变量的   

默认在block内部不能修改局部变量的值  


如果想修改局部变量的值，要再定义这个变量的时候，在前面加上__block  
```
 // 1.block中可以访问外面的变量
    /*
    int a = 10;
    void (^myBlock)() = ^{
        NSLog(@"a = %i", a);
    };
    myBlock();
     
     int a=10;
     void( ^myBlock)()=^{
       Nslog(@"a= %i",a);
     }
     myBlock();
    */
    
    // 2.block中可以定义和外界同名的变量, 并且如果在block中定义了和外界同名的变量, 在block中访问的是block中的变量
    /*
    int a = 10;
    void (^myBlock)() = ^{
        int a  = 20;
        NSLog(@"a = %i", a);
    };
    myBlock();
     */
    
    // 3.默认情况下, 不可以在block中修改外界变量的值
    // 因为block中的变量和外界的变量并不是同一个变量
    // 如果block中访问到了外界的变量, block会将外界的变量拷贝一份到堆内存中
    // 因为block中使用的外界变量是copy的, 所以在调用之前修改外界变量的值, 不会影响到block中copy的值
    /*
    int a = 10;
    NSLog(@"&a = %p", &a);
    void (^myBlock)() = ^{
//        a = 50;
        NSLog(@"&a = %p", &a);
        NSLog(@"a = %i", a);
    };
    a = 20;
    myBlock();
     */
    
    /*
    // 如果想在block中修改外界变量的值, 必须在外界变量前面加上__block
    // 如果在block中修改了外界变量的值, 会影响到外界变量的值
    __block int a = 10;
    NSLog(@"&a = %p", &a);
    void (^myBlock)() = ^{
        a = 50;
        NSLog(@"&a = %p", &a);
        NSLog(@"a = %i", a);
    };
    myBlock();
    NSLog(@"a = %i", a);
     */
```