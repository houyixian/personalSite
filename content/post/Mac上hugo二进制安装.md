---
title: "Mac上hugo二进制安装"
date: 2020-01-12T10:53:14+08:00
categories: ["个人博客搭建"]
draft: false
---

## 写在前面的话

最近一时兴起，想着搭建一个个人博客，搜索了一些资料后，发现目前github上hugo的stars最高，hugo又是用go语言编写的，就选择了使用hugo来搭建静态博客，在安装hugo的过程中，遇到了一些坑，在此记录一下，如果能帮到同样安装hugo的人，那就最好不过啦。

### hugo安装方法

安装方法官网上有好几种，最简单的方式应该是通过homebrew来安装，只需要一条命令，如下所示：

`brew install hugo`

但是似乎是由于一些资源在国外，导致了很多奇奇怪怪的网络问题，一直没成功，最终选择了二进制方式来安装，只需要从官网下载一个二进制包即可。

### 二进制hugo下载

[hugo二进制下载](https://github.com/gohugoio/hugo/releases)

hugo二进制目前支持很多系统，包括macOS、Windows、Linux、OpenBSD、FreeBSD。选择自己系统对应的二进制文件下载即可。

### 二进制hugo安装

我这里下载的文件是hugo_0.62.2_macOS-64bit.tar。首先，先去用户级别的bin目录里，创建一个hugo文件夹，用来存放我们的hugo二进制包。

`cd /usr/local/bin`

然后创建hugo文件夹

`mkdir hugo`

然后去到hugo_0.62.2_macOS-64bit.tar所在的根目录

`cd ~/Downloads`

把hugo_0.62.2_macOS-64bit.tar复制到刚刚创建的hugo文件夹里

`cp hugo_0.62.2_macOS-64bit.tar /usr/local/bin/hugo`

然后进入到刚刚创建的hugo文件夹里

`cd /usr/local/bin/hugo`

解压hugo二进制压缩包到当前目录里

`tar -zxvf hugo_0.62.2_macOS-64bit.tar`

至此，我们的hugo已经安装完成了，但是当我们在终端执行hugo version时并没有效果，因为系统还不知道去哪里找hugo。下面，我们来添加环境变量，让系统可以找到hugo。首先，打开用户级别的.bash_profile文件

`vim ~/.bash_profile`

增加一条环境变量配置

`export PATH=$PATH:/usr/local/bin/hugo`

然后再进入到.zshrc文件中

`vim ~/.zshrc`

增加一条环境变量

`export PATH="$PATH:/usr/local/bin/hugo" # Add hugo to PATH for scripting`

然后退出terminal重新登陆或者执行

`source ~/.bash_profile`

现在，再在终端执行hugo version。如果你看到了和下面类似的话，就代表你安装成功啦，恭喜哦。如果你还是看到了错误的提示，那就再仔细看看这篇安装过程或者查查其他资料呀，good luck！

`Hugo Static Site Generator v0.62.2-83E50184 darwin/amd64 BuildDate: 2020-01-05T18:50:42Z`

希望我的文章对你有帮助，如果有问题的话，欢迎加我的微信一起讨论呀。



