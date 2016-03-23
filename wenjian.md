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
```

#import <UIKit/UIKit.h>
#import "AppDelegate.h"

//NSFileManager:对文件进行操作的，包括复制，粘贴，删除，剪切等操作
int main(int argc, char * argv[]) {
    @autoreleasepool {
        
        //------------------创建文件/文件夹
        //获取沙盒目录
        NSString *homePath = NSHomeDirectory();
        //在沙盒目录中创建一个文件file.text
        NSString *filePath = [homePath stringByAppendingPathComponent:@"Documents/file.text"];
        //NSFileManager是单例模式,所以不能使用alloc+init创建
        NSFileManager *manager = [NSFileManager defaultManager];
        NSString *str = @"无线互联";
        NSData *data = [str dataUsingEncoding:NSUTF8StringEncoding];
        //参数：文件路径、文件内容、文件的属性
        BOOL sucess = [manager createFileAtPath:filePath contents:data attributes:nil];
        if(sucess){
            NSLog(@"文件创建成功");
        }else{
            NSLog(@"文件创建失败");
        }
        
        //创建文件夹
        NSString *filePaths = [homePath stringByAppendingPathComponent:@"Documents/file"];
        NSError *error;
        //需要传递一个创建失败的指针对象，记录创建失败的信息
        BOOL success1 = [manager createDirectoryAtPath:filePaths withIntermediateDirectories:YES attributes:nil error:&error];
        if(!success1){
            NSLog(@"创建成功");
        }else{
            NSLog(@"创建失败");
        }
        
        
        //--------------------读取文件
        //根据路径读取文件内容
        NSData *datas = [manager contentsAtPath:filePath];
        NSString *s = [[NSString alloc] initWithData:data encoding:NSUTF8StringEncoding];
        NSLog(@"%@",s);
        
        
        //--------------------移动文件/剪切文件
        //NSFileManager中没有提供重命名的方法，所以我们可以借助移动的api进行操作
        //把filePath移动到targetPath目录中
        NSString *targetPath = [homePath stringByAppendingPathComponent:@"Documents/file/file2.text"];
        BOOL sucess2 = [manager moveItemAtPath:filePath toPath:targetPath error:nil];
        if(sucess2) {
            NSLog(@"移动成功");
        }else{
            NSLog(@"移动失败");
        }
        
        
        //--------------------复制文件
        BOOL sucess3 = [manager copyItemAtPath:filePath toPath:targetPath error:nil];
        if(sucess3){
            //复制成功
        }else{
            //复制失败
        }
        
        
        //--------------------删除文件
        //删除之前需要判断这个文件是否存在
        BOOL isExist = [manager fileExistsAtPath:filePath];//判断文件是否存在
        if(isExist){
            BOOL sucess4 = [manager removeItemAtPath:filePath error:nil];
            if(sucess4){
                //删除成功
            }else{
                //删除失败
            }
        }
        
        
        //--------------------获取文件的属性
        NSDictionary *dic = [manager attributesOfItemAtPath:filePath error:nil];
        NSLog(@"%@",dic);//通过打印我们就可以查看文件属性的一些key
        
        return UIApplicationMain(argc, argv, nil, NSStringFromClass([AppDelegate class]));
    }
}

```