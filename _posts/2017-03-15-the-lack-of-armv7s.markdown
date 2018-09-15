---
title:  "armv7s问题"
---
### 问题描述
参考了制作[framework教程](http://www.cocoachina.com/ios/20150127/11022.html),发现生成的framework缺少armv7s指令集  
### 寻求原因
- 现有iOS设备指令集

 > arm64: iPhone 5s, iPhone 6(Plus), iPhone 6s(Plus), iPhone SE, iPhone 7(Plus), iPad Air(2), Retina iPad Mini(2,3)……   
armv7s: iPhone 5, iPhone 5c, iPad 4 
armv7: iPhone 3GS, iPhone 4, iPhone 4S, iPod 3G/4G/5G, iPad, iPad 2, iPad 3, iPad Mini   
armv6: iPhone, iPhone 3G, iPod 1G/2G

 armv7s 对应的机型: iPhone 5, iPhone 5c, iPad 4
然后下意识地去翻了一下现在我们已经上架appstore的项目的指令集配置但是我们现有项目的配置  
![](/assets/image/0019.png)  
从上图和创建的framework项目作对比并没有什么不同。
- 但是为什么没有添加armv7s但是还是可以正常在iPhone 5,  iPhone 5c, iPad 4 运行呢？  
然后在网上找到了[这篇文章里面有说一点向下兼容](http://www.cnblogs.com/cywin888/p/3229505.html)

> armv6, armv7, armv7s是ARM CPU的不同指令集，原则上是向下兼容的。如iPhone4S CPU支持armv7, 但它同时兼容armv6，只是使用armv6指令可能无法充分发挥它的特性。同理iPhone5 CPU支持armv7s，它虽然也兼容armv7，但是却无法进行相关的优化。  

在网上大致只搜到了Xcode 6 时候关于armv7s的一些问题。[Stack OverFlow 参考说明](http://stackoverflow.com/a/25398898/6531133)

###参考文章
<http://www.cocoachina.com/ios/20150127/11022.html>
<http://blog.csdn.net/wangyanchang21/article/details/52935609>
<http://www.itnose.net/detail/6207200.html>
<http://www.cnblogs.com/cywin888/p/3229505.html>


   

