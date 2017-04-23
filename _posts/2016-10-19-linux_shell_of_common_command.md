---
layout: post
title:  "(一)Linux shell常见命令操作以及问题解决"
page.date:   2016-10-19 18:17:23 +0800
categories: jekyll update
---

#### 1.  输入```ll``` 命令, 提示 ```ll: command not found```
```
mli@mli-HP:~/test/testmk$ ll
ll: command not found
```
解决方法：
编辑 /etc/bash.bashrc ， 在最后一行加上：
>  **alias ll='ls -l --color=tty'**

保存之后，需要重启，不重启的话，可以输入source命令:
> zdd@mli-HP:~$ source /etc/bash.bashrc

再次运行``ll`` 命令 ，成功：
```
mli@mli-HP:~/test/testmk$ ll
total 128
-rw-rw-r-- 1 mli mli 32372 10月 17 16:50 jielc.log
-rw-rw-r-- 1 mli mli 32372 10月 17 16:49 SF_3GR.c
-rw-rw-r-- 1 mli mli 32372 10月 17 16:49 SF_3GR-phone-r55dw.dts
-r-xr-xr-- 1 mli mli 32372 10月 17 16:50 zhibo.sh
```
#### 2. vim编辑工具的一些设置
![这里写图片描述](http://img.blog.csdn.net/20161109161531176)

> - 1.设置行号栏的背景颜色
highlight LineNr ctermfg=gray
highlight LineNr ctermbg=black

> - 2.vim设置快键键显示文件夹目录
> "NERDTree快捷键
    nmap  ``<F3> ``:NERDTree  ``<CR>``

>- 3.设置vim 鼠标点击插入“输入光标”
set mouse=a
> - 4.设置搜索高亮
set hlsearch
> - 5.设置括号匹配
set showmatch