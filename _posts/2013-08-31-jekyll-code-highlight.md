---
layout: post
title: jekyll代码高亮
summary: 作为一个技术类博客，在博客里粘贴代码是必不可少的事情，代码若没有高亮，可读性真没法说，所以我们需要给自己的博客增加代码高亮的功能。幸运的是，Jekyll提供里代码高亮的功能，我们只需要完成一小部分工作即可。
categories:
- else
tags:
- blog
- jekyll
- code highlight
---

{{ page.title }}
=================
作为一个技术类博客，在博客里粘贴代码是必不可少的事情，代码若没有高亮，可读性真没法说，所以我们需要给自己的博客增加代码高亮的功能。幸运的是，Jekyll提供里代码高亮的功能，我们只需要完成一小部分工作即可。
###搭建环境   
- Python
- easy_install
- Pygments

jek的代码高亮是使用[Pygments][2]来完成的，它是一款语法高亮的[Python][1]包，所以我们首先需要下载并安装Python。可用如下命令检测是否有Python环境：

	python --version   
	\# Python 2.7.3

安装[Pygments][2]时需要借助[EasyInstall][3]这个工具，这个工具和Pthon的关系，就像gems和ruby，或者apt-get和ubuntu的关系一样，它可以让你很方便的自动下载、编译、安装和管理[Python][1]包。在[Python][1]的较高版本中，该工具已经自动附带，比如我的版本是2.7.3，easy_install命令已经可以直接使用了，不同平台和环境，可能有所差异(比如Ubuntu上，需要先安装python-setuptools)。若你发现不能使用easy_install命令，记得先安装它。安装好[EasyInstall][3]之后，执行如下命令安装[Pygments][2]：

	easy_install Pygments

到此，需要的环境已经没有问题了，接下来如何在博客中使用代码高亮？
###代码高亮

首先，我们需要生成一个高亮代码的CSS文件，并引入到我们的博客中，生成方式如下：

	pygmentize -S fruity -f html > syntax.css

然后，在博客中使用代码高亮，高亮代码的模板是这样的：

	{% highlight 词法分析器 %}
	需要高亮的代码
	{% endhighlight %}
