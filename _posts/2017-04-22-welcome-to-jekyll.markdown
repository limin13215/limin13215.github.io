---
layout: post
title:  "博客开篇：使用jekyll从零开始创建自己的博客"
date:   2017-04-22 23:15:08 +0800
categories: jekyll update
---

## 1.只需几秒钟就能让网站跑起来。
在cmd ,powershell,git bash或者mingw等命令行工具窗口输入以下命令：
{% highlight jekyll linenos%}
~$gem install jekyll
~$jekyll new my-awesome-site
~$cd my-awesome-site
~my-awesome-site$ jekyll serve
#在浏览器中输入地址： http://127.0.0.1:4000/ 就可以预览你的博客了，去看看吧
{% endhighlight %}

## 2.图片插入的方法
在根目录下创建images文件夹，在images文件夹中放入要引用的图片
使用下面的语法插入：
```
![](/images/图片名称.后缀格式)
```
 
## 3.jekyll目录结构
![](/images/20170422_jekyll_structure.png)
具体的查看官方文档：[jekyll目录结构](http://jekyll.com.cn/docs/structure/)

## 4.如果你想使用你某篇文章的链接，标签 'post_url' 可以满足你的需求。

![](/images/2017023_post_url.png)

当使用post_url标签时，不需要写文件后缀名。

配合markdown语法，使用下面的命令：
![](/images/2017023_post_url_git.png)

对应如下效果，点击链接即可访问这篇文章：

[《论Git高级用法本地仓库到远程仓库协同操作(一)》]({% post_url 2016-09-02-git_remote_first %})

## 5.表格测试
|水果名称|价格（元/500克）| 
|:-------:|-----:| 
|苹果|3.2| 
|香蕉|4.5|

