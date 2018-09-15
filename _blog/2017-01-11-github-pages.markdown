---
title:  "mac搭建jekyll博客遇到的问题"
---

1. 执行`gem install jekyll`安装好jekyll后执行`jekyll serve`有如下报错

> Dependency Error: Yikes! It looks like you don't have jekyll-paginate or one of its dependencies installed. In order to use Jekyll as currently configured, you'll need to install this gem. The full error message from Ruby is: 'cannot load such file -- jekyll-paginate' If you run into trouble, you can find helpful resources at http://jekyllrb.com/help/! 

只要进行jekyll-paginate的安装就可以了`gem install jekyll-paginate`


