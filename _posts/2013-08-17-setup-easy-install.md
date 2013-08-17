---
layout: post
title: Windows32环境下在python3安装easy_install
summary: 官方页面引导的easy_install为Windows准备的安装包只有2.x版本的。那么，在python3下如何安装easy_install呢？
categories:
- Python
tags:
- python
- easy_install
---

{{ page.title }}
=================
经常接触 Python 的同学可能会注意到，当需要安装第三方 python 包时，可能会用到 easy_install 命令。easy_install是由PEAK(Python Enterprise Application Kit)开发的setuptools包里带的一个命令，所以使用easy_install实际上是在调用setuptools来完成安装模块的工作.

官方页面引导的easy_install为Windows准备的安装包只有2.x版本的。那么，在python3下如何安装easy_install呢？   

在[这里下载](https://pypi.python.org/pypi/setuptools)最新的easy_install的tar.gz的源码

然后解压进到其目录里

`C:\python32\python setup.py install`

因为多版本的问题 我没有将python3放入PATH

如果放入了就不用输入前边的路径了

然后就有easy_install了

    {% highlight python linenos %}
    def foo
      puts 'foo'
    end
    {% endhighlight %}