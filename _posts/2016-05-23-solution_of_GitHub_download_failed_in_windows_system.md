---
layout: post
title:  "GitHub Windows版本下载失败的解决方法"
page.date:   2016-05-23 16:11:48 +0800
categories: jekyll update
---

 GitHub官方下载地址 https://desktop.github.com/ ,但是从这里下载的文件并不是最终的安装文件，而是一个下载种子GitHubSetup.exe, 双击就会进入到正式的下载安装，如下图示：
 ![点击“安装”进入下一步](http://img.blog.csdn.net/20160523154109962)
 ![等待下载完成](http://img.blog.csdn.net/20160523154209865)

问题来了，当下载提示到100%，正当满心欢喜的时候，一个晴天霹雳袭来：
![这里写图片描述](http://img.blog.csdn.net/20160523154421694)

但是别怕，有问题出现找到原因即可，最怕不知道哪里出错，点击上图的“详细信息(D)...”按钮，打开了一个文本，仔细查看可以看到这样几句话：`错误摘要
	以下是错误摘要，这些错误的详细信息列在该日志的后面。
	* 激活 http://github-windows.s3.amazonaws.com/GitHub.application 导致异常。 检测到下列失败消息:
		+ 下载 http://github-windows.s3.amazonaws.com/Application Files/GitHub_3_1_1_4/lfs-x86.7z.deploy 未成功。
		+ 无法从传输连接中读取数据: 远程主机强迫关闭了一个现有的连接。。
		+ 远程主机强迫关闭了一个现有的连接。`


通过上面的信息知道， 是激活 http://github-windows.s3.amazonaws.com/GitHub.application 导致异常。难道是要翻墙才能激活吗？

google了一番， 发现有网友提供了如下操作：
 1.“打开控制面板→ Internet 选项→“安全”选项卡。
 2.  选择“受信任的站点”→点击“站点”按钮。
 3.  弹出的窗口中的文本框中输入点击“添加” https://github-windows.s3.amazonaws.com/ ；或者去除复选框“对该区域中的所有站点要求服务器验证(https:)”的钩，直接加入 github-windows.s3.amazonaws.com 。
 
 这个步骤有没有不得而知了，因为我操作了这步之后，又按文章开头操作到此,,,然并卵,,,,,,,,,,,,,,,,,,,,,,,,,,,,,

直接将 http://github-windows.s3.amazonaws.com/GitHub.application 复制到微软自带的IE浏览器打开，它就会成功下载GitHub.application，之后下载fg757p.zip
![这里写图片描述](http://img.blog.csdn.net/20160523160700576)
这个是典型的自由门翻墙的包啊，666，居然是墙出来的，之后就能安装成功了，你将欣赏到GitHub Desktop真面目：
![这里写图片描述](http://img.blog.csdn.net/20160523161027065)