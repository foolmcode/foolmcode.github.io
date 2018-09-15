---
title:  "合并静态库.a文件"
---

在网上看了一些文章，因为命令写法有误导致不能正常执行合并。现记录如下。

```
lipo -create "/Users/galileio/Desktop/SDK/libLLTestSDK.a" "/Users/galileio/Desktop/SDK/device/libLLTestSDK.a" -output "/Users/galileio/Desktop/libLLTestSDK.a"
```
   

