---
title:  "iOS各种小问题解决方法记录"
---

#### 1.`-[UIDeviceRGBColor length]: unrecognized selector sent to instance 0x79053e00`并且闪退。
解决方法。更改下图中标红选项。  
![](/assets/image/0015.png) 
   

#### 2.error: WatchKit App doesn't contain any WatchKit Extensions. Verify that the value of NSExtensionPointIdentifier in your WatchKit Extension's Info.plist is set to com.apple.watchkit.  
环境 Xcode 8.2.1  
场景 调试AFNetworking项目时候遇到这个报错。
解决方案  

```
在Targets里选中 watchOS app -> Build Phases -> Embed App Extensions
移除 WatchKit app Extensions (如果存在的话) 然后从弹出的弹框选择重新添加Products文件夹里面的.appex
```
