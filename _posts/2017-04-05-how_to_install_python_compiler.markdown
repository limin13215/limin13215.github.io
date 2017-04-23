---
layout: post
title:  "Python，PyCharm2017安装教程，包含注册码"
page.date:   2017-04-22 23:15:08 +0800
categories: jekyll update
---
> **[导语]：**很久以前就知道Python是一门很牛逼的脚本语言，一直没深入了解过，直到最近看到一些大侠用python语言写的爬虫软件之后，心生膜拜，不由得自己也想玩玩Python；看到很多大侠都在推荐由JetBrains开发的PyCharm工具，那就直接用它吧， 不过安装过程还是有几个坑，搞了我一个多小时，谨记于此。

## 一，安装PyCharm
### 1.下载PyCharm
进入https://www.jetbrains.com/pycharm/download/#section=windows官网下载页面，可以到到PyCharm有两个版本，一个专业版，一个自由版本；
![这里写图片描述](http://img.blog.csdn.net/20170405172209584?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGltaW4yOTI4/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

建议下载专业版本，点击download按钮下载professional版本， 注册码的事情后面搞定。
### 2.安装，注册码激活
正常安装，需要输入activation code时， 进入http://idea.lanyus.com/ 这个页面获取注册码，不知道是哪位提供的，非常感谢。注册码为正版注册码，无需打补丁，有效期为2017年01月31日至2018年01月30日。
激活成功，就会进入这个界面：
![这里写图片描述](http://img.blog.csdn.net/20170423121629481?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGltaW4yOTI4/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

### 3.新建项目入坑
我们这些首次安装PyCharm的菜鸟，兴致冲冲的点击2中的“Create New Project”，以为就可以开始编辑Python代码， 一个红色的闪电给你当头棒喝，哈哈，***No Python interpreter selected**这就是坑啊。
![这里写图片描述](http://img.blog.csdn.net/20170423121706528?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGltaW4yOTI4/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

不用急，凭我CET-4级的水平，这个警告的意思就是“没有选择Python解释器”。
> - interpret栏里应该选中python.exe，看来安装Pycharm的时候，并没有帮我把Python一起安装好，需要自己下载。
> - Pycharm跟eclipse有点像，不自带编译/解释器，像用eclipse开发 android应用时需要另外安装插件ADT，虽然有点麻烦，但拓展性很好，而visual studio之类的IDE则是自带编译功能，一个安装包里什么都有了，非常方便，但相对的，体积非常大而且不能很方便地更新某些组件。

### 4.首次安装Python解释器
#### 4.1下载python
在官网https://www.python.org/downloads/windows/列出了python的所有版本，根据自己的需要下载，我下载了最新的win 64位版本：https://www.python.org/ftp/python/3.6.1/python-3.6.1-amd64.exe 。

#### 4.2安装python
勾选Add Python 3.6 to PATH，按我的理解是，能把python注册到系统环境中，这样PyCharm也能自动识别出python.exe的路径。
![这里写图片描述](http://img.blog.csdn.net/20170423121803824?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGltaW4yOTI4/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

点击上图的2，可以自己修改设置安装，点击后进入下面的界面，勾选前6项，安装路径会自动变成：C:\Program Files\Python36，就安装在这了。
![这里写图片描述](http://img.blog.csdn.net/20170423121844638?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGltaW4yOTI4/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

点击安装，就完成了Python的安装。

### 5.重新启动PyCharm，新建项目
重新启动PyCharm -> 新建项目，稍等一会，我们就发现，python解释器已经自动识别了。如下图：
![这里写图片描述](http://img.blog.csdn.net/20170423121937670?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGltaW4yOTI4/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

接下来，就可以开始进入Python的世界.......