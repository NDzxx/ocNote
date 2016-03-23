# 文件操作详解

##NSString类操作文件路径名的方法
```  
#import <Foundation/Foundation.h>  
  
int main(int argc, const charchar * argv[]) {  
    @autoreleasepool {  
          
        //演示路径  
        NSString *path = @"/Users/jiangwei/file.text";  
          
        //1.返回路径的组成部分  
        //返回的一个数组，将路径分割了，但是需要注意的是，第一个是"/"，这个也算是路径的一部分。
        NSArray *array = [path pathComponents];  
        NSLog(@"%@",array);  
          
        //2.返回路径最后组成部分  
        NSString *str = [path lastPathComponent];  
        NSLog(@"%@",str);  
          
        //3.在原有的路径后面追加子目录  
        //这种方式有一点不好，就是需要添加/  
        [path stringByAppendingString:@"/appFile.text"];  
        //不需要添加/  
        [path stringByAppendingPathComponent:@"appFile.text"];  
          
        //4.删除最后的组成部分  
        str = [path stringByDeletingLastPathComponent];  
        //删除扩展名(.text被删除了)  
        str = [path stringByDeletingPathExtension];  
          
        //5.获取扩展名  
        str = [path pathExtension];  
          
        //6.添加扩展名  
        [path stringByAppendingPathExtension:@".jpg"];  
          
    }  
    return 0;  
}
```
##沙盒文件

在IOS中，每个应用程序都是一个沙盒，他们有自己的文件目录，这个目录对其他程序是不可见的，和Android中的/data/data/包名/中的内容差不多，当然IOS中的沙盒目录有三种：

Documents：存放长期使用的文件(一般存放目录)

Library：系统存放文件

tmp：临时文件，app重启时，该目录下得文件清空

```
#import <UIKit/UIKit.h>  
#import "AppDelegate.h"  
  
int main(int argc, charchar * argv[]) {  
    @autoreleasepool {  
        //获取沙盒目录  
        //默认有三个文件夹  
        //Documents：存放长期使用的文件(一般存放目录)  
        //Library：系统存放文件  
        //tmp：临时文件，app重启时，该目录下得文件清空  
          
        //第一种获取方式，我们得到Home目录之后，手动的去拼接Document/Library  
        NSString *homePath = NSHomeDirectory();  
        NSLog(@"沙盒目录:%@",homePath);  
          
        //第二种方式，直接使用Api获取  
        //NSDocumentDirectory:Documents文件夹  
        //NSLibraryDirectory:Library文件夹  
        NSArray *array = NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask, YES);  
        NSLog(@"%@",array);  
          
        NSArray *array1 = NSSearchPathForDirectoriesInDomains(NSLibraryDirectory, NSUserDomainMask, YES);  
        NSLog(@"%@",array1);  
          
        return UIApplicationMain(argc, argv, nil, NSStringFromClass([AppDelegate class]));  
    }  
} 
```
##NSFileManager
这个类的主要功能是对文件进行操作：创建，复制，剪切，删除等
