---
layout: post
title: Python Challenge 7-13通关攻略
category: technology
tags: 技术
keywords: Python Challenge,攻略
description:
---

### 7

第七关，这个图片有点奇怪，中间的黑白条像是蕴含了一些信息，应该是用到了图像处理的东西。依旧是窃取前人指点，Python有一个第三方库PIL（python image library），其中的Image可以用在这处理图像。下载oxygen.png这个图像，用GIMP打开可以发现黑白条的坐标起止，同时每个像素格宽度为7。由`im.mode`知道，这个图片模式是RGBA的。RGBA的值是(x,x,x,255)的形式，取R, B, G值都行，这里用的R值。换算换成ASCII得到`smart guy, you made it. the next level is [105, 110, 116, 101, 103, 114, 105, 116, 121]`，再将这里的数字换成ASCII得到`integrity`。这就是答案。

``` python
import Image,re
x_begin, x_end = 0, 609
y_begin, y_end = 43, 53
step = 7
im = Image.open('oxygen.png')
msg = ''.join([chr(im.getpixel((i, y_begin))[1]) for i in xrange(x_begin, x_end, step)])
print msg
print ''.join([chr(int(x)) for x in re.findall(r'\d{3}',msg)])
```

第八关URL：[http://www.pythonchallenge.com/pc/def/integrity.html](http://www.pythonchallenge.com/pc/def/integrity.html)

### 8

第八关，点击图片中的蜜蜂，需要用户名和密码，网页源码中有一堆mess，`un: 'BZh91AY&SYA\xaf\x82\r\x00\x00\x01\x01\x80\x02\xc0\x02\x00 \x00!\x9ah3M\x07<]\xc9\x14\xe1BA\x06\xbe\x084' pw: 'BZh91AY&SY\x94$|\x0e\x00\x00\x00\x81\x00\x03$ \x00!\x9ah3M\x13<]\xc9\x14\xe1BBP\x91\xf08' `，un和pw应该分别是username和password了，网上查到，BZh91开头的余Bzip2压缩有关，用python的bz2模块解压一下，得到`huge`和`file`，这就是答案。

``` python
import bz2
un = 'BZh91AY&SYA\xaf\x82\r\x00\x00\x01\x01\x80\x02\xc0\x02\x00 \x00!\x9ah3M\x07<]\xc9\x14\xe1BA\x06\xbe\x084'
pw = 'BZh91AY&SY\x94$|\x0e\x00\x00\x00\x81\x00\x03$ \x00!\x9ah3M\x13<]\xc9\x14\xe1BBP\x91\xf08'
print(bz2.decompress(un))
print(bz2.decompress(pw))
```

第九关URL：[http://www.pythonchallenge.com/pc/return/good.html](http://www.pythonchallenge.com/pc/return/good.html)

### 9

第九关，网页title是`connect the dots`，网页源码中有一堆数字，猜测是需要连线，而数字是坐标，还是用到了PIL库，最后连线画出来像是一头牛。在URL中输入cow，给出提示`hmm. it's a male.`，所以得到`bull`，这就是答案。

``` python
import Image
import ImageDraw
first=[...] #注释中fist的内容
second=[...] #注释中second的内容
img = Image.open('good.jpg')
newImg = Image.new(img.mode, img.size) #创建一张新的图片，背景默认为black
draw = ImageDraw.Draw(newImg)
draw.line(first, fill='#fff') # 将线条设置成白色
draw.line(second, fill='#fff')
del draw
newImg.save('result.png')
```

第十关URL：[http://www.pythonchallenge.com/pc/return/bull.html](http://www.pythonchallenge.com/pc/return/bull.html)

### 10

第十关，显示让计算`len(a[30]) = ?`，点击图中的bull发现`a = [1, 11, 21, 1211, 111221, `，传说中的[look-and-sat sequence](http://en.wikipedia.org/wiki/Look-and-say_sequence)。即求第30个的字符串长度，网上看到一个非常精妙的方法，正则表达式，代码如下。其中正则部分，`\<number>`表示引用编号为`<number>`的分组匹配到的字符串，例如`(\d)abc\1`可以匹配`1abc1`，`5abc5`等等。结果是`5808`，这就是答案。

``` python
import re
x="1"
for each in range(30):
    x="".join([str(len(i+j))+i for i,j in re.findall(r"(\d)(\1*)", x)])
print len(x)
```

第十一关URL：[http://www.pythonchallenge.com/pc/return/5808.html](http://www.pythonchallenge.com/pc/return/5808.html)

### 11

第十一关，有是图像处理的题目，网页的title提示odd和even，而图像看上去存在叠影，猜测可能跟奇偶像素点有关系，还是看网上提示，把奇偶像素点分开即可。得到`evil`，这就是答案。

``` python
import Image
img = Image.open('cave.jpg')
w = img.size[0]
h = img.size[1]
odd = even = Image.new(img.mode, (w/2, h/2))
for x in range(w):
    for y in range(h):
        pixel = img.getpixel((x, y))
        if x % 2 == 0 and y % 2 == 0:
            odd.putpixel((x/2, y/2), pixel)
        elif x % 2 == 1 and y % 2 == 0:
            even.putpixel(((x-1)/2, y/2), pixel)
        elif x % 2 == 0 and y % 2 == 1:
            even.putpixel((x/2, (y-1)/2), pixel)
        elif x % 2 == 1 and y % 2 == 1:
            odd.putpixel(((x-1)/2, (y-1)/2), pixel)
odd.save('odd.png')
even.save('even.png')
```

第十二关URL：[http://www.pythonchallenge.com/pc/return/evil.html](http://www.pythonchallenge.com/pc/return/evil.html)

### 12

第十二关，完全没思路，上网查后，被这道题的想象力给惊呆了！网页中，图片地址是`evil1.jpg`，猜测还有`evil2.jpg`，地址改成`evil2.jpg`后给出提示，后缀应该是`gfx`，下载下来一个`gfx`文件。而一开始的分牌的图，表示这个文件分成五份，用二进制的方式`rb`读取。得到`"dis","pro","port","ional","ity"`，但最后一个单词被划去了，因此得到单词`disproportional`。这就是答案。忍不住吐槽，本人这智商完全被碾压！

``` python
f = open('evil2.gfx', 'rb')
data = f.read()
for i in range(5):
    file = open('evil_%d.jpg' % i, 'wb')
    file.write(data[i::5])
    file.close()
f.close()
```

第十三关URL：[http://www.pythonchallenge.com/pc/return/disproportional.html](http://www.pythonchallenge.com/pc/return/disproportional.html)

### 13

第十三关，是个电话，底下提示;phone that evil。点击5进入另一个页面，出来一个xml文件，并无头绪。求助谷歌度娘，前辈们提到一个东西：xmlrpc，XML-RPC的全称是XML Remote Procedure Call，即XML远程方法调用。它是一套允许运行在不同操作系统、不同环境的程序实现基于Internet过程调用的规范和一系列的实现。这种远程过程调用使用http作为传输协议，XML作为传送信息的编码格式。Xml-Rpc的定义尽可能的保持了简单，但同时能够传送、处理、返回复杂的数据结构。XML-RPC是工作在Internet上的远程过程调用协议。一个XML-RPC消息就是一个请求体为xml的http-post请求，被调用的方法在服务器端执行并将执行结果以xml格式编码后返回。有一个库xmlrpclib，用listmethod方法进行测试发现这真的是一个标准的XML-RPC服务器，其中有个phone方法，是返回一个人的电话。在这里试一下提示`He is not evil.`。那么，到底谁是evil?经过上一题最后一个不是图片的evil4得知，Bert是evil，得到`555-ITALY`，`ITALY`就是答案。

``` python
import xmlrpclib
xmlrpc = xmlrpclib.ServerProxy('http://www.pythonchallenge.com/pc/phonebook.php')
print xmlrpc.system.listMethods()
print xmlrpc.system.methodHelp('phone')
print xmlrpc.phone('evil')
print xmlrpc.phone('Bert')
```

```
['phone', 'system.listMethods', 'system.methodHelp', 'system.methodSignature', 'system.multicall', 'system.getCapabilities']
Returns the phone of a person
He is not the evil
555-ITALY
```

第十四关URL：[http://www.pythonchallenge.com/pc/return/italy.html](http://www.pythonchallenge.com/pc/return/italy.html)
