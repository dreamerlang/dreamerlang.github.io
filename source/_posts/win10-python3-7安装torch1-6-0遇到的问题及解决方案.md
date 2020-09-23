---
title: win10+python3.7安装torch1.6.0遇到的问题及解决方案
date: 2020-09-23 20:31:15
tags:
- 教程
---

win10+python3.7安装torch1.6.0遇到的问题及解决方案

<!-- more -->

1.首先去[这个网址](https://www.lfd.uci.edu/~gohlke/pythonlibs/#pytorch)找到py和win10对应的版本对torch进行离线下载：

![](/img/pytorch离线下载包.png)

2.然后执行：

pip install +下载文件的目录+文件名，例如：pip install c:\downloads\torch-1.6.0-cp37-cp37m-win_amd64.whl

3.执行后我遇到了如下错误

OSError: [WinError 126] 找不到指定的模块 caffe2_detectron_ops.dll，然后上网搜索了一番，找到了解决方案：

**pip install intel-openmp**

----

问题到此解决。

参考博客：

https://blog.csdn.net/qq_35482604/article/details/103035881

https://github.com/pytorch/pytorch/issues/35803





