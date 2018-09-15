---
title:  "iOS 自定义键盘"
---

之前自定义键盘在github上找了一个第三方[Numberpad](https://github.com/lnafziger/Numberpad)。在项目测试阶段，无奈最后发现bug。几番折腾没能找到原因。因为我只是在我自己出现问题的项目里找问题。耗费了较长时间。  
最后我使用UITextField+系统自带键盘再次尝试实现我们的需求，发现这个bug还是存在。看来锅是自己的。  
需要实现的效果如下图所示  

![](/assets/image/0016.gif) 

一开始我的实现思路是  

- 创建 `¥` label ,textfield 和 他们的视图容器view。 
- 动态计算textfield的宽度，并实时改变textfield的宽度还有`¥`的位置。（通过监听UITextFieldTextDidChangeNotification实现）
- 因为一开始textfield宽度为0，所以需要在view里放入一个透明button来触发弹起键盘。

按照上面效果实现了但是有两个bug  

1. 在删除字符时候有如下光标先左移，后右移。如下图所示  
![](/assets/image/0017.gif) 
2. 在不收键盘的情况下push到一个新界面的时候再回来，会出现如下图所示情况    
![](/assets/image/0018.gif) 
为了解决这两个bug各种折腾。不过感觉这中折腾浪费了时间才是重要的。而且经过折腾并不能完全解决问题。

所以准备换个思路。或者改进一下。

- 创建 `¥` label ,宽度固定的textfield，把label添加到textfield里。 
- 计算textfield的宽度，并实时改变textfield的宽度还有`¥`的位置。
然后就实现了这种效果。并且无上述问题存在。  
[demo的地址LLKeyboardTest](https://github.com/galileioo/LLKeyboardTest)  
收获：1.当遇到棘手的bug时候。需要把需求的场景抽离成为尽可能简单的demo，然后去找问题。
2.当一种实现有很大缺陷或者bug很多时候不如换换思路。

