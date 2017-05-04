---
layout: post
title:  "Linux通过shell脚本实现JDK版本之间的快速切换"
date:   2017-05-04 14:20:28 +0800
categories: jekyll update
---

> 在[android官网](https://source.android.com/source/requirements)中,关于JDK的部分有一下说明，对于不同的Android SDK版本，需要通过的合适的jdk版本才能正常编译。
- The master branch of Android in AOSP: Ubuntu - [OpenJDK 8](http://openjdk.java.net/install/), Mac OS - jdk 8u45 or newer
- Android 5.x (Lollipop) - Android 6.0 (Marshmallow): Ubuntu - [OpenJDK 7](http://openjdk.java.net/install/), Mac OS - jdk-7u71-macosx-x64.dmg
- Android 2.3.x (Gingerbread) - Android 4.4.x (KitKat): Ubuntu - [Java JDK 6](http://www.oracle.com/technetwork/java/javase/archive-139210.html), Mac OS - Java JDK 6
- Android 1.5 (Cupcake) - Android 2.2.x (Froyo): Ubuntu - [Java JDK 5](http://www.oracle.com/technetwork/java/javase/archive-139210.html)

## 一.JDK的安装
这个不说了，网上有很多教程，我以前也总结过一篇关于jdk的安装：[《ubuntu下配置安装jdk1.6实用简单方法详解》](http://blog.csdn.net/limin2928/article/details/17138163)。

不过，追本溯源，官方的才是最正宗的，点击上面提到的JDK版本链接，就可以跳转到官方提供的JDK安装步骤。

## 二.实现shell脚本切换JDK的步骤
> 这里以我虚拟机安装的Ubuntu16.04桌面版系统为例；


### 1.确定多个JDK版本的安装路径


我电脑里安装了jdk1.8 和 jdk1.6 两个版本，安装路径都在`/usr/lib/jvm`目录下:

`/usr/lib/jvm/java-8-openjdk-amd64/`
`/usr/lib/jvm/jdk1.6.0_45/`


### 2.根据上面的jdk路径，新建脚本文件 change_jdk, 脚本的内容：
```
if [ x$1 == x ]; then
	echo default jdk1.6
	exit 0
fi

if [ x$1 == x1.6 ]; then
	echo change jdk to 1.6
	export JAVA_HOME=/usr/lib/jvm/jdk1.6.0_45
	export JRE_HOME=$JAVA_HOME/jre
	export CLASSPATH=.:$JAVA_HOME/lib:$JRE_HOME/lib:$CLASSPATH
	export PATH=$JAVA_HOME/bin:$JRE_HOME/bin:$PATH
	java -version
fi

if [ x$1 == x1.8 ]; then
	echo change jdk to 1.8
	export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
	export JRE_HOME=$JAVA_HOME/jre
	export CLASSPATH=.:$JAVA_HOME/lib:$JRE_HOME/lib:$CLASSPATH
	export PATH=$JAVA_HOME/bin:$JRE_HOME/bin:$PATH
	java -version
fi
```
> 这个脚本也托管在csdn code服务器，链接地址：https://code.csdn.net/limin13215/change_jdk/tree/master

### 3.设置通过脚本名称可以在任意目录执行脚本
我的脚本文件放在`~/work/tools`目录下，在系统环境管理文件 ~/.bashrc 或者~/etc/profile 添加一行代码：
```
export PATH=~/work/tools:$PATH
```

### 4.打开终端，在任意目录执行以下命令都可以切换jdk版本
- 若切换到jdk 1.8 ,执行命令:
```
source change_jdk 1.8
```
- 若切换到jdk 1.6 ,执行命令:
```
 source change_jdk 1.6
```