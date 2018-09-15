---
title:  "github pages markdown有些语法和本地展示不一致问题解决"
---

突然发现在本地运行正常的markdown到了github pages上的时候就出现了解析错误。所以去github官方看了一下[github官方推荐使用Bundler](https://help.github.com/articles/setting-up-your-github-pages-site-locally-with-jekyll/#requirements)。  

## 1.安装`bundler` 
 
```
 $ sudo gem install bundler
``` 
此处出现报错
> ERROR:  Could not find a valid gem 'bundler' (>= 0), here is why:
          Unable to download data from http://ruby.taobao.org/ - bad response Not Found 404 (http://ruby.taobao.org/latest_specs.4.8.gz)  

原因是淘宝源已经改为https，所以进行如下替换

```
$ gem sources --remove https://ruby.taobao.org/
$ gem sources -a https://ruby.taobao.org/
```      
但是进入[淘宝源](https://ruby.taobao.org/)发现已经不维护了已经转到了[Ruby China 镜像](http://gems.ruby-china.org/)也就是<http://gems.ruby-china.org/>
所以添加[Ruby China 镜像](http://gems.ruby-china.org/)

```
gem sources -a http://gems.ruby-china.org/
```

        
## 2. 创建没有扩展名的`Gemfile`文件,并添加如下代码.
```
$ source 'https://rubygems.org'
$ gem 'github-pages', group: :jekyll_plugins
$ gem sources -l
```

 	

## 3. cd到Jekyll 网站仓库目录 执行`$ bundle install`

以下为报错和解决  

> Could not reach host index.rubygems.org. Check your network connection and try
again.

因为`Gemfile`文件配置的source并非<http://gems.ruby-china.org/>所以参照[Ruby China 镜像](http://gems.ruby-china.org/)给的示例可进行如下配置。

```
$ bundle config mirror.https://rubygems.org https://gems.ruby-china.org
```

> Could not find gem 'github-pages' in any of the gem sources listed in your
Gemfile or available on this machine.

## 4.安装成功后就可执行```$ bundle exec jekyll serve```,然后在本地浏览。

这样的话，解析markdown就和github的语法一致了。不会出现本地预览和github托管的网页效果不一致问题。





