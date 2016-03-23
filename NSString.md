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
