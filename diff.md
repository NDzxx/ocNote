# 代理通知和kvo的不同
首先来看共同点，这三种模式都是一个对象传递事件给另外一个对象，并且不要他们有耦合。  
现在看看不同点  
##代理

代理属于1对1，一个对象只有一个代理。当被代理的对象需要做某事时候，通过函数调用代理人进行相关操作，c++和java也有对应的实现，只是oc采用协议进行了实现。
##kvo操作

kvo是用来监听类中属性值的变化的，不可监视方法等内容
KVO操作讲解参考
注意：kvo是可以一对多的，也就是说对象属性发生变化后，可以通知多个类的对象，类似广播  
##通知

通知是另外一种一对多模型，

和kvo的区别是两者都是观察者模式，不同的是，KVO是被观察者直接发送消息给观察者，是对象间的交互，而通知则是观察者和被观察者通过通知中心对象之间进行**交互**，即消息由被观察者发送到通知中心对象，再由中心对象发给观察者，两者之间并不进行直接的交互。
而且通知可以并不仅仅只是监视属性值的变化，可以发送任务内容。

