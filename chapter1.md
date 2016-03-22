# 类的声明和实现


##声明和实现例子
```
@interface Person:Nsobject 声明
{ //大括号中声明成员变量
}
@end

@implementation Persion 定义

@end 
```

##oc方法声明格式

-（返回值类型） 方法名称；


__在OC中没有命名空间，也不支持包名


__在OC定义类、枚举、结构体的时候需要给它们加上前缀__

//函数的多个参数写法，

```
- (int) sum:(int) num1 and:(int) num2;  
```
## #import和#include的区别

当我们在代码中使用两次#include的时候会报错：因为#include相当于拷贝头文件中的声明内容，所以会报重复定义的错误
但是使用两次#import的话，不会报错，所以他可以解决重复导入的问题，他会做一次判断，如果已经导入一次就不导入了
