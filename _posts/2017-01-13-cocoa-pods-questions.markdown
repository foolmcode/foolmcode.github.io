---
title:  "cocoapods Abort trap: 6问题解决"
---

### 环境
```
OSX:10.12.2    
Xcode:8.2  
```

### 问题描述
执行`pod install`下载完第三方库之后终端比平常多了一行日志`Abort trap: 6`并且没有生成.lock文件  

### 解决方案
* 首先找到了stackoverflow里面这个链接<http://stackoverflow.com/questions/39980096/xcode8-cocoapods-abort-trap6>    
 依次尝试没能解决。（其实这里给出的解决方案相当关键。我没有成功的原因是因为ruby版本过低，没能成功安装.  `sudo gem uninstall cocoapods`    
`sudo gem install cocoapods --pre` ）
* 然后在这个链接下<https://github.com/CocoaPods/CocoaPods/issues/6371>找到如下评论进行第二次尝试

>  According to this <http://stackoverflow.com/questions/39980096/xcode8-cocoapods-abort-trap6>
upgraded to 1.2.0.beta.3 is one way to fix it. But to upgrade to 1.2.0.beta.3, I need to upgrade ruby from 2.0 to 2.6.7.  
It did fix my problem.

主体思想是把cocoapod升级到1.2.0.beta.3。但是需要同时把ruby从2.0升到2.6.7(但是随后我尝试的时候升级到2.3.3也可以)。
查看了下我本机pod的版本号，并且尝试使用 果然只能升级到1.1.1 。所以尝试安装ruby。

#### 1. 安装ruby
 然后找安装ruby的方法<http://blog.csdn.net/qq_33391441/article/details/50723198>  
 
 总结上个链接安装ruby大致指令如下方便以后使用(注意！如果无法安装请自行使用vpn，我购买了shadowsocks，以下安装指令在公司运行正常，回到家里就死活不行。最后找了一个可以每天免费3次每次使用20分钟的vpn。挂上之后以下指令军正常运行)
  
* 安装 RVM`$ curl -L https://get.rvm.io | bash -s stable` 
* 载入 RVM 环境`$source ~/.rvm/scripts/rvm`（或者重新开启终端）
* 检查版本`$ rvm -v `
* 列出已知的ruby版本`$ rvm list known`
* 安装ruby `$ rvm install 2.3`
* 查询已经安装的ruby`$ rvm list`
* 卸载一个已安装版本 `$ rvm remove 1.9.2`
* 设置 Ruby 版本`$ rvm 2.3 --default`

安装ruby遇到如下问题   

* 文中提到`$ source ~/.rvm/scripts/rvm`是载入环境，不过我自己试了没成功。出现了`-bash: rvm : command not found`不过重新开启终端就可以了。  

* 执行安装`rvm install 2.3`终端有如下报错 
 
> Somehow it happened there is no executable 'openssl',
run 'brew doctor' and make sure latest '' is installed properly.
RVM autolibs is now configured with mode '4' =>
  'Allow RVM to use package manager if found, install missing dependencies, install package manager (only OS X).',
please run `rvm autolibs enable` to let RVM do its job or run and read `rvm autolibs [help]`
or visit https://rvm.io/rvm/autolibs for more information.
Requirements installation failed with status: 12. 
 
然后搜索在此链接<https://github.com/rvm/rvm/issues/3724>下找到解决方案 
 
>  After deleting the empty folder under /usr/local/Cellar/openssl/ and reinstalling openssl with `brew install openssl`, the problem got solved.  

此方法是先删除/usr/local/Cellar/openssl/文件夹下的文件，然后执行`brew install openssl`重新安装建议先备份

至此 ruby安装成功
#### 2. 再次安装cocoapod预览版
再次尝试安装cocoapod `sudo gem install cocoapods --pre` 成功把cocoapod安装到1.2.0.beta.3

#### 3. 重新执行`pod install`

此时，问题已解决。不会再出现报错。也正常生成了.lock 文件


