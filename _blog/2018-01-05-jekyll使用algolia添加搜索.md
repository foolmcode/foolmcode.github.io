---
title:  "jekyll使用algolia添加搜索"
---

# 无法搜索自己添加文章？？记录下解决问题过程
## 解决方案  
在使用```jekyll```模板```Minimal Mistakes```的demo的时候，发现项目现有的测试markdown 文章都可以被搜索到，但是自己添加的就无法被搜索。做了以下几个尝试。  
1.我自己的markdown文章扩展名为.markdown,项目里现有为 .md 尝试改扩展名，但未能解决问题。
2.查看可以被索引到的markdown文章头部的配置，在尝试使用相同配置后，自己的文章还是不能被搜索。
3.在```_config.yml``` 里面搜索相关```search```的配置.如下

```
search                   : true # true, false (default)
search_full_content      : true # true, false (default)
search_provider          : "algolia"
algolia:
  application_id         : "0BPDKBYGEE"
  index_name             : "foolmcodeIndex"
  search_only_api_key    : "3a0db82fc1d63b07640d021de1ab2d59"
  powered_by             : true

```
可以看到search已经开启。包括内容搜索也已经开启。作者也添加了algolia的相关配置，更加想不通为何自己添加的文章不可以。  
4.意识到自己对```algolia```一无所知，所以决定去官网了解一下相关使用方法。  

## 正确操作
在参考了官方的几篇文档之后得出以下结论：  
1.文章需要通过执行命令
```
ALGOLIA_API_KEY='自己注册的新Admin API Key' bundle exec jekyll algolia
```
执行以上命令相当于给项目里的所有文章添加索引。

2._config.yml里面添加的几项algolia的配置没有Admin API Key一项。所以需要自己去[官网](https://www.algolia.com)注册。

## 参考链接
[参考这篇文章注册algolia获取相关配置](https://zouzeir.xyz/2017/01/16/Hexo集成Algolia搜索插件/)  
[algolia如何工作](https://community.algolia.com/jekyll-algolia/how-it-works.html)  
[algolia设置](https://community.algolia.com/jekyll-algolia/options.html#settings)  
[algolia命令使用方法](https://community.algolia.com/jekyll-algolia/getting-started.html#usage)


## 总结
我自己其实陷入了思维定式，认为只要加入模板demo里的文章都应该被搜索到。其实demo里的文章可以被搜索是因为模板作者之前已经通过命令添加过索引了。 

在缺乏相关知识的情况下，就不要胡乱猜测和想象。这个时候多看比多想重要。因为要收集到足够有用的知识和信息才有利于自己的决策。
