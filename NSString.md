# NSString对象和NSMutableString对象


NSString 类似C++ 的string，是oc的字符串
##NSString对象
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
###比较两个字符串的大小方法
```
//4.---------------------compare方法  
//比较两个字符串的大小  
result = [string15 compare:string16];  
if(result == NSOrderedAscending){  
    //升序  
}else if(result == NSOrderedDescending){  
    //降序  
} 
```
###取字符串的长度的方法

```
//5.---------------------length方法
NSString *string17 = @"abc";
NSInteger len = [string17 length];
NSLog(@"len=%ld",len);
```
###转化字符串的大小写方法
```
    //6.---------------------uppercaseString/lowercaseString  
    //转成大写  
    NSString *string18 = @"aEc";  
    NSString *string19 = [string18 uppercaseString];  
    string19 = [string18 lowercaseString];  
    //首字符大写,其他的变成小写  
    [string18 capitalizedString];  
```
###数值转化的方法
```
//7.---------------------TypeXXXValue
//数值转化方法
NSString *string20 = @"3.14";
float value = [string20 floatValue];
string20 = @"1";
BOOL values = [string20 boolValue];
```
###字符串的截取方法
```
//8.---------------------subStringToIndex/subStringFromIndex/subStringWithRange  
//字符串的截取  
NSString *string21 = @"abcdefg";  
//从开始位置截取到下标为3的字串(不包括3)  
NSString *stringnews = [string21 substringToIndex:3];  
//从哪个index开始截取到尾部(包括3)  
stringnews = [string21 substringFromIndex:3];  
//截取1~(3+1)部分的字串  
NSRange range = {1,3};  
stringnews = [string21 substringWithRange:range]; 
```
###字符串的追加方法
```
    //9.----------------------stringByAppendingString/stringByAppendingFormat  
    //字符串的追加  
    NSString *string22 = @"Android";  
    NSString *stringnew = [string22 stringByAppendingString:@"IOS"];  
    stringnew = [string22 stringByAppendingFormat:@"%@",@"IOS"];  
```
这里需要注意的是，每次追加完一个字符串之后，是返回一个新的字符串，因为NSString是不可变类。和Java中的String类很类似，如果用+去连接字符串都会重新创建一个字符串对象，Java中可以使用StringBuilder对象去解决这样的问题，在OC中也是有解决的，后面会说到一个可变字符串类

###字符串的查找方法
```
    //10.---------------------rangeOfString  
    //字符串的查找  
    NSString *string23 = @"www.iphonetrain.com/ios.html";  
    NSRange rang = [string23 rangeOfString:@"ios"];//返回一个范围  
    if(rang.location == NSNotFound){  
        //没找到  
    }else{  
        //找到了  
    }  
```
###取出字符串中指定的char
```
//11.----------------------characterAtIndex  
//取出字符串中指定的char  
NSString *string24 = @"abcdef";  
unichar c = [string24 characterAtIndex:3]; 
```
##NSMutableString对象
和NSString类不同的地方  
###创建方法
```
 //1.--------------------创建字符串  
        //和NSString的创建方法相同  
        NSMutableString *string1 = [[NSMutableString alloc] initWithString:@"hello"];  
          
        //但是不能使用以下方式创建  
        //因为以下方式创建的字符串是不可变的，其值是放到常量池中的，是不可变得  
        //NSMutableString *string1 = @"hello";  
```
NSMutableString类的创建方式和NSString的创建方式差不多，只是有一点需要注意，NSMutableString不能使用直接方式去创建，因为直接方式创建的字符串都是在常量池中，而常量池中的值都是不可变的，所以不能创建的，同时initWithString创建出来的字符串也是不在常量池中的。这点需要和NSString区分一下


###在字符串中插入一个字串
```
//2.---------------------insertString
//在源字符串中指定的index中插入字符串，不会产生新的对象
[string1 insertString:@"可变" atIndex:0];
```
##字符串的追加
```
//3.---------------------appendString  
//和NSString的不同是追加字符串,不会产生新的对象  
[string1 appendString:@"可变"]; 
```

##删除字串
```
    //4.---------------------deleteCharactersInRange  
    //删除字符串中指定范围的字串内容  
    NSMutableString *string2 = [NSMutableString stringWithString:@"hello"];  
    NSRange range = {1,3};  
    [string2 deleteCharactersInRange:range];  
```
##替换字符串  
```
    //5.---------------------replaceCharactersInRange  
    //替换字符串内容  
    NSMutableString *string3 = [NSMutableString stringWithString:@"hello"];  
    NSRange ranges = [string3 rangeOfString:@"ll"];//先查找出需要替换字符串的范围  
    [string3 replaceCharactersInRange:ranges withString:@"ee"];  
```