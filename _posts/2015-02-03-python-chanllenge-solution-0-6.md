---
layout: post
title: Python Challenge 0-6通关攻略
category: technology
tags: 技术
keywords: Python Challenge,攻略
description:
---
Python Challenge是一个网页闯关游戏，通过提示找出下一关的网页地址。值得注意的是，它是专门为程序员设计的，因为大多数关卡都是需要编程来计算。总共33关，感觉很有趣，闲来无事玩一玩，能学到Python很多有用的知识和技巧，共享每关攻略在此。
----------

* 目录
{:toc}

### 0

进入第零关：[http://www.pythonchallenge.com/pc/def/0.html](http://www.pythonchallenge.com/pc/def/0.html)

图片给出238三个数，把URL末尾地址改成238.html,得到提示`No... the 38 is a little bit above the 2...`，38在2的上面，猜测是2的38次方，计算结果得：274877906944。

``` python
pow(2,38)
```

第一关URL：[http://www.pythonchallenge.com/pc/def/274877906944.html](http://www.pythonchallenge.com/pc/def/274877906944.html)

### 1

第一关URL重定向后，末尾变成了map.html，细看图片，像是键值对，同时底下给出了一个看似杂乱无章的字符串，联想到URL里的map关键词，猜测这是一段加密后的文字，而解码规则就是图中的键值对，规律是字符在字母表中的位置后移两位（标点符号和空格除外）。解码后的字符串是`i hope you didnt translate it by hand. thats what computers are for. doing it in by hand is inefficient and that's why this text is so long. using string.maketrans() is recommended. now apply on the url. `，大意是对URL进行同样的解码，于是得到ocr，过关。

``` python
s = "g fmnc wms bgblr rpylqjyrc gr zw fylb. rfyrq ufyr amknsrcpq ypc dmp. bmgle gr gl zw fylb gq glcddgagclr ylb rfyr'q ufw rfgq rcvr gq qm jmle. sqgle qrpgle.kyicrpylq() gq pcamkkclbcb. lmu ynnjw ml rfc spj."
trans = string.maketrans(string.ascii_lowercase, string.ascii_lowercase[2:] + string.ascii_lowercase[0:2]);
print(s.translate(trans));
print('map'.translate(trans))
```

第二关URL：[http://www.pythonchallenge.com/pc/def/ocr.html](http://www.pythonchallenge.com/pc/def/ocr.html)

### 2

第二关，URL中有个关键词，ocr(Optical Character Recognition)，但明显无法从图片辨别出来，在网页源码中有段注释，提示找出最少的字母。用python代码统计后，得到单词`equality`。这就是答案。

``` python
with open('text','r') as fileReader:
    text = fileReader.read()
    print(''.join([x for x in text if x.isalpha()]))
fileReader.close()
```

第三关URL：[http://www.pythonchallenge.com/pc/def/equality.html](http://www.pythonchallenge.com/pc/def/equality.html)

### 3

第三关，网页title为re，提示可能跟正则表达式有关。题目的意思是，一个小写字母，两边各有不多不少的三个大写字母。还是看网页源码有段注释，用正则表达式处理，得到linkedlist。这就是答案。

``` python
import re
with open('text','r') as fileReader:
    text = fileReader.read()
    l = re.findall("[a-z][A-Z]{3}([a-z])[A-Z]{3}[a-z]", text)
    print(''.join([x for x in l]))
fileReader.close()
```

第四关URL：[http://www.pythonchallenge.com/pc/def/linkedlist.html](http://www.pythonchallenge.com/pc/def/linkedlist.html)

### 4

第四关，发现只有一行`linkedlist.php`，替换掉URL中的`linkedlist.html`后，发现这才是真正的第四关，网页源码提示`urllib may help. DON'T TRY ALL NOTHINGS, since it will never end. 400 times is more than enough.`。而网页title提醒`follow the chain`。发现图片是可以点击的，进入`http://www.pythonchallenge.com/pc/def/linkedlist.php?nothing=12345`。得到提示`and the next nothing is 44827`。于是懂了，nothing是循环得到的，要循环甚至到400次才能有最后结果。循环过程中会发现中间有一次是`Yes. Divide by two and keep going.`，于是有下面代码，最后得到`peak.html`，这就是答案。

``` python
import urllib.request
import re
url = 'http://www.pythonchallenge.com/pc/def/linkedlist.php?nothing='
nothing = '12345'
def next(nothing):
    text = urllib.request.urlopen(url + nothing).read()
    print(str(text))
    if 'Divide' in str(text):
        return next(str(int(nothing[0])/2))
    else:
        nothing = re.findall('([0-9]+)', str(text))
        if len(nothing) != 0:
            return next(nothing[0])
        else:
            pass
if __name__ == '__main__':
    next(nothing)
```

第五关URL：[http://www.pythonchallenge.com/pc/def/peak.html](http://www.pythonchallenge.com/pc/def/peak.html)

### 5

第五关，进去是一张图，网页title为`peak hell`，源码内有注释提醒`peak hell sounds familiar ?`，实在不懂。。网上有前辈说是`pickle`，python的序列化模块（又学到了一个知识点，捏哈哈）。源码中有个链接`banner.p`用pickle处理得到一个list，（windows 64位下，python3，一直有一个错误：`UnpicklingError: the STRING opcode argument must be quoted`，只好又装了python2.7，:(）还是经网上前人指点，做如下处理即可，得到#打印组成的`channel`单词，这就是答案。

``` python
import pickle as p
with open('text','r') as f:
    storedlist = p.load(f)
    for line in storedlist:
        print(''.join(t[0] * t[1] for t in line))
    f.close()
```

第六关URL：[http://www.pythonchallenge.com/pc/def/channel.html](http://www.pythonchallenge.com/pc/def/channel.html)

### 6

第六关，按照惯例先看网页源码，有堆注释是呼吁大家对python challenge项目捐款的。奇怪的是顶部有个zip的提示，把URL中的channel改成zip，得打提示`yes. find the zip.`。猜测是要找到一个zip文件，尝试把html改成zip，果然下载了一个zip文件，其中readme文件给出提示，`welcome to my zipped list. hint1: start from 90052. hint2: answer is inside the zip`。唉。。还是跟第四关差不多，由于对zip操作不熟悉，可耻的看了网上的攻略，发现这一关还真有些复杂！第一段代码得到提示`Collect the comments.`。还是根据前辈提示，利用ZipInfo。得到一堆字符。辨别出应该是`hockey`，但是应用到URL中时，得到提示`it's in the air. look at the letters.`。于是发现，这幅图是有若干小字组成，得到`oxygen`。这就是答案。（太坑爹了。。。）

``` python
import re, zipfile
z = zipfile.ZipFile('channel.zip', mode = 'r')
number = '90052'
while True:
    text = z.read(number + '.txt')
    print text
    number = re.findall('([0-9]+)', text)
    print number
    try:
        number = number[0]
    except:
        break
```

``` python
import re, zipfile
z = zipfile.ZipFile('channel.zip', mode = 'r')
number = '90052'
comments = []
while True:
    text = z.read(number + '.txt')
    number = re.findall('([0-9]+)', text)
    print number
    try:
        number = number[0]
        comments.append(z.getinfo(number + '.txt').comment)
    except:
        break
print ''.join(comments)
```

```
 **************************************************************
****************************************************************
**                                                            **
**   OO    OO    XX      YYYY    GG    GG  EEEEEE NN      NN  **
**   OO    OO  XXXXXX   YYYYYY   GG   GG   EEEEEE  NN    NN   **
**   OO    OO XXX  XXX YYY   YY  GG GG     EE       NN  NN    **
**   OOOOOOOO XX    XX YY        GGG       EEEEE     NNNN     **
**   OOOOOOOO XX    XX YY        GGG       EEEEE      NN      **
**   OO    OO XXX  XXX YYY   YY  GG GG     EE         NN      **
**   OO    OO  XXXXXX   YYYYYY   GG   GG   EEEEEE     NN      **
**   OO    OO    XX      YYYY    GG    GG  EEEEEE     NN      **
**                                                            **
****************************************************************
 **************************************************************
```

第七关URL：[http://www.pythonchallenge.com/pc/def/oxygen.html](http://www.pythonchallenge.com/pc/def/oxygen.html)
