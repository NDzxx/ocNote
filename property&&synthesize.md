# @property和@synthesize

##@property关键字  

编译器指令，当我们在类的声明中使用 @property，它会自动生成标准的setter和getter方法的声明 
@synthesize：   

1、用类的实现中，可以生成setter和getter方法的实现   

2、生成一个私有成员变量,这个私有的成员变量与@synthesize后面的名字一样   
在xcode4.4后面，不需要再写@synthesize了，只写@property编译器就会对应实现setter和getter  

##格式和参数
声明property的语法为：  

@property (参数1,参数2) 类型 名字;  


