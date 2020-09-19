---
title: MAC OS下安装Jpype
date: 2020-09-19 15:25:48
tags:
- 教程
- Jpype
---

 macOs下安装Jpype。

Jpype可以实现Python 代码调用 Java 代码。

<!-- more -->

### 环境

Macos 10.15.6
Python 3.7 （venv环境）



### 步骤一：

到官方的[github](https://github.com/jpype-project/jpype/releases/tag/v1.0.2)地址下载对应的最新版本的安装包，比如macos+py3.7就选择

[JPype1-1.0.2-cp37-cp37m-macosx_10_9_x86_64.whl](https://github.com/jpype-project/jpype/releases/download/v1.0.2/JPype1-1.0.2-cp37-cp37m-macosx_10_9_x86_64.whl)



### 步骤二：

若没有wheel需要安装：

sudo pip install wheel

安装完成后在pycharm里cd到对应的whl文件目录下。

如在我的电脑上是：

cd ~/Downloads

然后执行：

sudo wheel install JPype1-1.0.2-cp37-cp37m-macosx_10_9_x86_64.whl 

Jpype的安装就到此完成。