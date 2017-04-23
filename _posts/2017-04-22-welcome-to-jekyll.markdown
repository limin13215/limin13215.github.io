---
layout: post
title:  "博客开篇：使用jekyll从零开始创建自己的博客"
page.date:   2017-04-22 23:15:08 +0800
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