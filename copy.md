# 对象的拷贝

同C++一样，oc对象拷贝也有深复制和浅复制之分

C++中有拷贝构造函数，Java中需要实现Cloneable接口，在clone方法中进行操作。  
但是不过OC更偏向于Java这种方式，OC中如果一个对象需要被拷贝，他需要实现协议：
<NSCopying>、<NSMutableCopying>

浅拷贝：只拷贝所有属性对象的指针  

深拷贝：拷贝属性对象的内容

