---
title:  "JS和UIWebview通过JavaScriptCore无法执行iOS本地方法解决方案"
---
### 问题描述

html里面的script标签里面需要在网页加载的时候直接调用客户端的方法。无法调用成功。

### 问题分析

之前也做过js通过UIWebview调用iOS端代码。可以成功调用。但是为什么能够成功调用？
因为我之前的做法是当页面加载成功时候才去注入`wap_obj`,代码如下。

```
#pragma mark - UIWebViewDelegate
- (void)webViewDidFinishLoad:(UIWebView *)webView {
     self.title = [webView stringByEvaluatingJavaScriptFromString:@"document.title"];
    self.jsContext = [webView valueForKeyPath:@"documentView.webView.mainFrame.javaScriptContext"];
    self.jsContext[@"wap_obj"] = self;
}
```
以前的使用场景是点击webview上的按钮，触发js方法来调用iOS本地方法。因为点击按钮时候网页已经加载完了。所以这个场景是ok的。  
现在，新的场景是先加载js直接调用方法。调用方法在执行`webViewDidFinishLoad`之前。所以方法不触发就是理所当然。

### 解决方案

*  在网上搜索良久，终于找到了一篇文章。[JavaScript和Objective-C交互的那些事](http://www.jianshu.com/p/939db6215436).里面用到了一个第三方库。[UIWebView-TS_JavaScriptContext](https://github.com/TomSwift/UIWebView-TS_JavaScriptContext). 参考此项目的写法，将文件加入工程中之后就可以解决网页加载中无法成功调用本地方法的问题。但是看到文章后面的一些评论说是有被拒的风险。然后去github看了下[issue](https://github.com/TomSwift/UIWebView-TS_JavaScriptContext/issues/2)
有如下被拒的原因.

> I hope that we do not use this library, the bloody lessons.  
  2016年9月1日 上午1:22   
  发件人 Apple  
Performance - 2.5.1  
Your app uses or references the following non-public APIs:  
"parentFrame"  
The use of non-public APIs is not permitted on the App Store because it can lead to a poor user experience should these APIs change.

为了防止被拒。我还是放弃此种方法把。  

* 但是上一种方法给出了一种思路。就是使用`didCreateJavaScriptContext`。这个方法会多次回调。以获取最新的的JSContext。第一种方法只要不调用`parentFrame`方法不就好了？所以对之前方法加以修改就是改为发通知方式。方法代码如下

```
#import "NSObject+JSContextTracker.h"

@implementation NSObject (JSContextTracker)

- (void)webView:(id)unused didCreateJavaScriptContext:(JSContext *)ctx forFrame:(id)alsoUnused {
    if (!ctx)
        return;
     [[NSNotificationCenter defaultCenter] postNotificationName:@"LLCreatJSContex" object:ctx];
}

@end

```
在使用的地方添加观测

```
    [[NSNotificationCenter defaultCenter]addObserver:self selector:@selector(creatJSContex:) name:@"LLCreatJSContex" object:nil];
    
-(void)dealloc
{
    [[NSNotificationCenter defaultCenter] removeObserver:self name:@"LLCreatJSContex" object:nil];
}
-(void)creatJSContex:(NSNotification*)noti
{
    NSLog(@"%@",noti);
    //注意以下代码如果不在主线程调用会发生闪退。
    dispatch_async( dispatch_get_main_queue(), ^{
        
        self.jsContext = [_webView valueForKeyPath:@"documentView.webView.mainFrame.javaScriptContext"];
        self.jsContext[@"wap_obj"] = self;
        self.jsContext.exceptionHandler = ^(JSContext *context, JSValue *exceptionValue) {
            context.exception = exceptionValue;
            NSLog(@"异常信息：%@", exceptionValue);
        };
    });
   
}

```
运用这种思路。也可以实现成功调用。并且我们的应用已经通过审核。

###  参考
<http://www.jianshu.com/p/939db6215436>  
  
<https://github.com/TomSwift/UIWebView-TS_JavaScriptContext>
