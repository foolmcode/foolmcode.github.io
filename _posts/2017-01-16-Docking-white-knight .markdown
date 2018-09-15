---
title:  "iOS白骑士对接问题总结"
---

文档是公司洽谈，白骑士方给的。无法在官网下载。包括对接文档，还有相应的sdk文件。

* 问题1: 通过tokenkey去调用查询设备信息query，无法获取设备信息。     
调用getTokenKey正常生成tokenKey，但是通过tokenKey去调用查询设备信息query，一直是`"resultCode":"BQS000"` 。按照文档`BQS000`是查询成功的code。但是没有设备信息，所以怀疑上传时候出了问题。  
但是上传是白骑士官方自己进行的上传，所以准备用工具抓包。  
这里是https链接，所以要进行https抓包。  
抓包后发现，上传信息时候不断调用uploadAppDFInfo.遇到报错，然后不停的重试。如下图， 基本上是无限重试。。。这个比较坑。换了4g网也不行。但是安卓是ok的。  

![](/assets/image/0003.png) 
 
点击uploadAppDFInfo接口返回信息如下。
 
![](/assets/image/0001.png)

 怀疑是环境配置问题。随后询问安卓环境env参数配置，安卓说没设置。但是iOS是必填。我这里填了dev，所以改为prd试了下上传不载报错。  
 接下来uploadAppDFInfo上传成功。接口返回值信息如下  
 
 ![](/assets/image/0002.png)
 
 接着根据tokenKey去查询设备信息,也查询成功。
 