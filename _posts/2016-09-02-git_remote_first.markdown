---
layout: post
title:  "论Git高级用法本地仓库到远程仓库协同操作(一)"
page.date:   2016-09-02 13:06 +0800
categories: jekyll update
---

# 一，前言
![image](http://img.blog.csdn.net/20160831162122935)
   git里有四种对象：commit对象、tree对象、blob对象和tag对象
   
   working tree就是手工更改的源代码
   
   index file 为git建立的索引(git add 命令之后的快照)
   
   仓库代码 就是git保存起来的代码。
# 二，Git建立本地项目仓库
### 1.初始化Git仓库

> **$ git init**  
Git会打印：Initialized empty Git repository in $PROJECT/.git/
    
     (上述操作的结果是会在当前目录下创建了一个 .git 隐藏目录,它就是所谓的 Git 仓库,
     不过现在它还是空的。另外当前目录也不再是普通的文档目录了,今后我们将其称为工作树。)
### 2.完善仓库基本信息

> **$ git config --global user.name "Your Name"**

> **$ git config --global user.email you@yourdomain.example.com**

### 3.忽略提交文件
如果有一些部件我们不想纳入Git版本控制，也不想在每次"**git status**"时看到这些文件的提示，或者很
多时候我们为了方便会使用"**git add .**"添加所有修改的文件，这时就会添加上一些我们不想添加的文件，
怎么忽略这些文件呢？
    
 GIT当然提供了方法，只需在主目录下建立"**.gitignore**"文件，此文件有如下规则：

  > - 所有以#开头的行会被忽略
  > - 可以使用glob模式匹配
  > - 匹配模式后跟反斜杠（/）表示要忽略的是目录
  > - 如果不要忽略某模式的文件在模式前加"!"
  
 
| 标识符           | 备注                             |
| --------         | -----                            |
| **#**            | 这是注释行符号，这行将被git忽略  |
| ***.img**        | 忽略所有.img结尾的文件           |
| **!auto.img**    | auto.img除外，其他的忽略         |
| **/TODO**        | 忽略项目根目录下的TODO文件，不包括subdir/TODO文件 |
| **out/**         | 忽略out/ 目录下的所有文件        |
| **doc/\*.txt**   | 会忽略 doc/notes.txt 但不包括 doc/server/arch.txt |

### 4.提交修改到本地仓库
> **$ git commit -m "提交信息"**

提交之后，如果我们发现几个文件漏了提交或者想修改一下之前的提交信息， 可以这样提交：

> **$ git commit --amend**

# 三，与远程仓库协同工作
## 1.创建github仓库
之前我们git操作的都是在本地上，git远程协作才是重点，工作中经常会用到。一般我们在GitHub创建仓库作为我们的远程仓库，其实你也可以在csdn托管代码，还有很多其他的网站都提供了代码托管服务，我们演示在GitHub创建远程仓库，按下图操作：
![通过这三步就创建好了远程仓库](http://img.blog.csdn.net/20160831172137377)
> **上图中的Repository name仓库名自己定义，我的是studygit,点击绿色的Create repository按钮就会创建git仓库：https://github.com/limin13215/studygit.git ，这样仓库就建好了。如下图示，github还提示了如何添加关联远程库的操作命令：**

![这里写图片描述](http://img.blog.csdn.net/20160902130210353)

## 2.添加远程仓库
接下来我们要做的就是把本地仓库与之关联，然后，把本地仓库的内容推送到GitHub仓库。
给本地仓库添加远程仓库，根据上图的提示内容，运行命令：
> ####$ git remote add *origin* https://github.com/limin13215/studygit.git 
>> 其中origin，是Git默认生成的，可以自己修改。origin表示这个*studygit.git* 仓库地址的别名。

下一步，就是把本地仓库的内容推送到远程仓库：

> #### $ git push -u *origin* master
>> 这里的origin，就是远程仓库别名，git push具体的以后再讲。

由于远程库studygit是空的，我们第一次推送master分支时，加上了 **-u** 参数，Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令，不用-u参数了。
> 推送成功的话，github上跟你本地仓库有相同的内容了。 没有成功的话，考虑两点，第一点是不是两个仓库内容提交有冲突，第二点看是不是需要把SSH公钥添加到github，我没提交公钥也成功了。

## 3.删除远程仓库
你想换个服务器或者换个github账号托管代码，反正，没道理，你现在就是要删除一个远程仓库。
- 3.1.查看有哪些远程库:
>  **$ git remote -v**

![image](http://img.blog.csdn.net/20160902113805706)

- 3.2.删除远程仓库

> **$ git remote rm *远程仓库名***

之前，我把本地仓库的代码也同步到了CSDN代码片库，仓库地址：https://code.csdn.net/limin13215/testgit.git 。用*gitremote-v* 时可以看到有两个远程库，一个*cdsngit*，一个*origin*。现在我要删除CSDN上远程仓库:

> $ git remote rm *csdngit*

哈哈，我不是有意的啊，演示而已，再次*git remote -v*查看远程库，已经不显示*csdngit* 信息了。  