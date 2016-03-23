# NSString对象和NSMutableString对象

NSString 类似C++ 的string，是oc的字符串
###创建的方法
```
//1.--------------字符串的创建方式
NSString *string1 = @"hello";
NSLog(@"%@",string1);
//和上面的方式是等价的
string1 = [[NSString alloc] initWithString:@"hello1"];
//使用utf8格式
//NSString str = [[NSString alloc] initWithUTF8String:”C语言字符串”] 
NSLog(@"%@",string1);
//占位符，可以实现拼接字符串
//[string1 stringByAppendingFormat:<#(NSString *), ...#>]这个方法也是可以实现字符串拼接的
string1 = [[NSString alloc] initWithFormat:@"hello %@",string1];
NSLog(@"%@",string1);
//使用类方法创建字符串
string1 = [NSString stringWithString:@"hello1"];
```
###比较字符串的值
```
//2.--------------字符串的比较isEqualToString
//比较字符串的值
NSString *string2 = @"abcd";
NSString *string3 = @"8888";
BOOL isEquals = [string2 isEqualToString:string3];
if(isEquals){
}else{
}
```
**注意判断字符串不要用==**  
原因是在使用常量创建字符串和使用initWithString创建的时候，值都是保存在常量池里面的
这个时候
```
    NSString *string8 = @"abc";  
    NSString *string9 = @"abc";  
    //这里内存做了优化，这里string8和string9是相同的,"abc"在常量区中分配的  
    //string8和string9是局部变量，在栈中分配内存的  
    if(string8 == string9){  
        NSLog(@"string8 == string9");  
    }else{  
        NSLog(@"string8 != string9");  
    }  
//下面的string11和string12还是在栈中分配内存的，结果相等
NSString *string11 = [[NSString alloc] initWithString:@"abc"];
NSString *string12 = [[NSString alloc] initWithString:@"abc"];
if(string11 == string12){
    NSLog(@"string11 == string12");
}else{
    NSLog(@"string11 != string12");
}

```
但是使用
```
//下面的string13和string14是在堆中分配内存的，所以string13和string14不相等了  
NSString *string13 = [[NSString alloc] initWithFormat:@"abc%@",@"def"];  
NSString *string14 = [[NSString alloc] initWithFormat:@"abc%@",@"def"];  
if(string13 == string14){  
    NSLog(@"string13 == string14");  
}else{  
    NSLog(@"string13 != string14");  
} 
```
进行创建，==判断是不相等的，所以应该使用isEqualToString判断，就不会出现错误


