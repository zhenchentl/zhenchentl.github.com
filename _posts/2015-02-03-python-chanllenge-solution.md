---
layout: post
title: Python Challenge 通关攻略
category: technology
tags:
- Python Challenge
- 攻略
keywords: Python Challenge,攻略
description:
---
Python Challenge是一个网页闯关游戏，通过提示找出下一关的网页地址。值得注意的是，它是专门为程序员设计的，因为大多数关卡都是需要编程来计算。总共33关，感觉很有趣，闲来无事玩一玩，共享攻略在此。

###0
进入第一关：[http://www.pythonchallenge.com/pc/def/0.html](http://www.pythonchallenge.com/pc/def/0.html)

图片给出238三个数，把URL末尾地址改成238.html,得到提示`No... the 38 is a little bit above the 2...`，38在2的上面，猜测是2的38次方，计算结果得：274877906944。
{% highlight python %}

    pow(2,38)
{% endhighlight %}
第二关URL：[http://www.pythonchallenge.com/pc/def/274877906944.html](http://www.pythonchallenge.com/pc/def/274877906944.html)

###1
第二关URL重定向后，末尾变成了map.html，细看图片，像是键值对，同时底下给出了一个看似杂乱无章的字符串，联想到URL里的map关键词，猜测这是一段加密后的文字，而解码规则就是图中的键值对，规律是字符在字母表中的位置后移两位（标点符号和空格除外）。解码后的字符串是`i hope you didnt translate it by hand. thats what computers are for. doing it in by hand is inefficient and that's why this text is so long. using string.maketrans() is recommended. now apply on the url. `，大意是对URL进行同样的解码，于是得到ocr，过关。
{% highlight python %}

    s = "g fmnc wms bgblr rpylqjyrc gr zw fylb. rfyrq ufyr amknsrcpq ypc dmp. bmgle gr gl zw fylb gq glcddgagclr ylb rfyr'q ufw rfgq rcvr gq qm jmle. sqgle qrpgle.kyicrpylq() gq pcamkkclbcb. lmu ynnjw ml rfc spj."
    trans = string.maketrans(string.ascii_lowercase, string.ascii_lowercase[2:] + string.ascii_lowercase[0:2]);
    print(s.translate(trans));
    print('map'.translate(trans))
{% endhighlight %}
第三关URL：[http://www.pythonchallenge.com/pc/def/ocr.html](http://www.pythonchallenge.com/pc/def/ocr.html)
