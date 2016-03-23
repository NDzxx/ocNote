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