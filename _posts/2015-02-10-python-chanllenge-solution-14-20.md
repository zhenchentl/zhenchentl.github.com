---
layout: post
title: Python Challenge 14-20通关攻略
category: technology
tags: 技术
keywords: Python Challenge,攻略
description:
---

###14
第十四关，面包圈应该是没什么用的，寓意在网页title已经显示了，`walk around`，第二个图下载下来居然是一个10000\*1的像素条，源码中给出了提示`remember: 100*100 = (100+99+99+98) + (...`，100\*100和10000应该是有关系的。联想到刚才那个面包圈，想着先横着100，再竖着99，再向左横着99，向上竖98，刚好就是一圈，这样一圈一圈下去就能把这个 10000\*1 的图给围成 100\*100的了。网上有有段比较精巧的代码来处理，只有一个循环，根据边界条件来改变坐标。得到的图片是一直猫，改URL为cat，进入新的页面得到猫的原图和提示`and its name is uzi. you'll hear from him later. `。于是得到`uzi`，这就是答案。同时，`you'll hear from him later`貌似给后面的题埋下了伏笔。
{% highlight python %}
import Image
def spiral(source):
     target = Image.new(source.mode, (100, 100))
     left, top, right, bottom = (0, 0, 99, 99)
     x, y = 0, 0
     dirx, diry = 1, 0
     for i in xrange(10000):
         target.putpixel((x, y), source.getpixel((i, 0)))
         if dirx == 1 and x == right:
             dirx, diry = 0, 1
             top += 1
         elif dirx == -1 and x == left:
             dirx, diry = 0, -1
             bottom -= 1
         elif diry == 1 and y == bottom:
             dirx, diry = -1, 0
             right -= 1
         elif diry == -1 and y == top:
             dirx, diry = 1, 0
             left += 1
         x += dirx
         y += diry
     return target
if __name__ == '__main__':
    source = Image.open('wire.png')
    spiral(source).save('result14.png')
{% endhighlight %}
第十五关URL：[http://www.pythonchallenge.com/pc/return/uzi.html](http://www.pythonchallenge.com/pc/return/uzi.html)

###15
第十五关，是一张日历，圈出了1月26日这天，并且源码中提示`todo: buy flowers for tomorrow`，因此重点在1月27日，网页title提示是whom，猜测是与一个人有关，右下角显示2月有29天，因此这一年还是闰年，且年份开头为1，结尾为6。计算可得有`1976 1756 1576 1356 1176`，根据提示`he ain't the youngest, he is the second`得知应该是1756年，百度下1756年1月27日是奥地利音乐大师莫扎特的生日，得到`mozart`，这就是答案。
{% highlight python %}
from datetime import date
for year in xrange(1996,1000,-20):
    if date(year,1,1).isoweekday() == 4:
        print year,
{% endhighlight %}
第十六关URL：[http://www.pythonchallenge.com/pc/return/mozart.html](http://www.pythonchallenge.com/pc/return/mozart.html)

###16
第十六关，还需要登陆，试了下用户名和密码分别还是`huge`和`file`。得到的提示图片充满噪点，而中间有看上去等长的红线，仔细观察发现，每一像素行只有一条红线，由网页title的提示`let me get this straight`猜测，应该是把红线对其成一条线。由第一段代码获取红线的颜色值为195，最后得到的图中有`romance`，这就是答案。
{% highlight python %}
import Image
im = Image.open("mozart.gif")
w,h,ct,last = im.size,0,0
for x in range(w):
    color = im.getpixel((x,0))
    if last == color:
        ct += 1
        if ct > 2:
            print last
    else:
        ct = 0
    last = color
print last
{% endhighlight %}
{% highlight python %}
import Image
im = Image.open("mozart.gif")
w,h = im.size
for y in range(h):
    colors = [im.getpixel((x,y)) for x in range(w)]
    start = colors.index(195)#找到195第一次出现的横坐标
    colors = colors[start:] + colors[:start]#重组colors
    for x,color in enumerate(colors):
        im.putpixel((x,y),color)
im.save('result.png')
{% endhighlight %}
第十七关URL：[http://www.pythonchallenge.com/pc/return/romance.html](http://www.pythonchallenge.com/pc/return/romance.html)

###17
第十七关，这一关实在复杂，根据图片提示看到cookie后，`info=you+should+have+followed+busynothing…`，剩下的完全查网上攻略了。左下角图片曾经出现在第四关，发现第四关的cookie也是busynothing这句话，把url改一下http://www.pythonchallenge.com/pc/def/linkedlist.php?busynothing=12345 看看发生了什么。Response cookie出现了值，是一个字母，然后当你继续访问的时候，字母在变，于是收集这些值。得到`BZh91AY%26SY%94%3A%E2I%00%00%21%19%80P%81%11%00%AFg%9E%A0+%00hE%3DM%B5%23%D0%D4%D1%E2%8D%06%A9%FA%26S%D4%D3%21%A1%EAi7h%9B%9A%2B%BF%60%22%C5WX%E1%ADL%80%E8V%3C%C6%A8%DBH%2632%18%A8x%01%08%21%8DS%0B%C8%AF%96KO%CA2%B0%F1%BD%1Du%A0%86%05%92s%B0%92%C4Bc%F1w%24S%85%09%09C%AE%24%90`，开头Bzh9看上去还需要bz2的decompress，得到提示`is it the 26th already? call his father and inform him that “the flowers are on their way”. he’ll understand.`，26th和flower指的是前面第十五关的Mozart，他的父亲是Leopold，重点是，`call his father`，又回到了打电话那关，得到电话`555-VIOLIN`，然而改网址的时候提示，先是提示`no! i mean yes! but ../stuff/violin.php.`，改后缀为php后，得到`it’s me. what do you want?`，在回头发现`inform him that "the flowers are on their way"`，也就是访问的时候还要构造cookie，带上信息为the flowers are on their way。于是得到一段html代码，里面`balloons`，这就是答案。
{% highlight html %}
<html>
<head>
  <title>it's me. what do you want?</title>
  <link rel="stylesheet" type="text/css" href="../style.css">
</head>
<body>
    <br><br>
    <center><font color="gold">
    <img src="leopold.jpg" border="0"/>
<br><br>
oh well, don't you dare to forget the balloons.</font>
</body>
</html>
{% endhighlight %}
{% highlight python %}
import urllib, urllib2, cookielib, xmlrpclib,bz2
jar = cookielib.CookieJar()
handler = urllib2.HTTPCookieProcessor(jar)
opener = urllib2.build_opener(handler)
baseurl = 'http://www.pythonchallenge.com/pc/def/linkedlist.php?busynothing='
num = '12345'
info = list()
for _ in xrange(118):
    content = opener.open(baseurl + num).read()
    num = content.split()[-1]
    info.append(jar._cookies.values()[0]['/']['info'].value)
msg = ''.join(info)
print msg
msg = urllib.unquote_plus(msg)
print msg
msg = bz2.decompress(msg)
print msg
proxy = xmlrpclib.ServerProxy('http://www.pythonchallenge.com/pc/phonebook.php')
print proxy.phone('Leopold')
jar._cookies.values()[0]['/']['info'].value = 'the+flowers+are+on+their+way'
print opener.open('http://www.pythonchallenge.com/pc/stuff/violin.php').read()
{% endhighlight%}
第十八关URL：[http://www.pythonchallenge.com/pc/return/balloons.html](http://www.pythonchallenge.com/pc/return/balloons.html)

###18
第十八关，寻找两幅图的不同，源码提示`it is more obvious that what you might think`，最明显的不同是什么？亮度，URL改成brightness，发现源码提示有`maybe consider deltas.gz`，又是一个压缩包，[下载下来](http://www.pythonchallenge.com/pc/return/deltas.gz)是一个txt文件，里面一堆16进制数，分成两列。而且左右两侧大部分相同，只有个别行是左边有右边没有，或者右边有左边没有。网上找到一个库，difflib，这个库的作用就是用来找差异的。输入两个片段，然后逐行比较，将差异分为三个类型，两个片段都有，只有第一个片段有和只有第二个片段有。文件的开始是`89 50 4e 47 0d 0a 1a 0a 00`，这里存储的是图片格式，即png格式的，因此，思路是，把deltas文件分成三份，把16进制转换成二进制，每份表示一个图片，得到`../hex/bin.html`，`butter`，`fly`，这就是答案。
{% highlight python%}
import difflib, binascii
f = open('delta.txt','rb')
src = f.read().splitlines()
f.close()
text1 = [line[:53] for line in src]
text2 = [line[56:] for line in src]
d=difflib.Differ()
re = list(d.compare(text1, text2))
temp1 = temp2 = temp3 = ''
for line in re:
    if line[:2] == '  ':
        temp1 += line[2:].replace(" ",'')
    elif line[:2] == '+ ':
        temp2 += line[2:].replace(" ",'')
    elif line[:2] == '- ':
        temp3 += line[2:].replace(" ",'')
    # break
pic1 = open('pic1.png','wb')
pic1.write(binascii.unhexlify(temp1))
pic1.close()
pic2 = open('pic2.png','wb')
pic2.write(binascii.unhexlify(temp2))
pic2.close()
pic3 = open('pic3.png','wb')
pic3.write(binascii.unhexlify(temp3))
pic3.close()
{% endhighlight %}
第十九关URL：[http://www.pythonchallenge.com/pc/hex/bin.html](http://www.pythonchallenge.com/pc/hex/bin.html)，这关的登陆用户名为butter，密码fly。

###19
第十九关，源码注释给出了一封邮件，附件名称是indian.wav，因此，附件应该是base64编码过的wav音频文件，解码即可。但是得到的音频里说sorry，用sorry替换URL得到提示`what are you apologizing for?`。考虑到提示图片大海和陆地颜色反转了，尝试将音频每一帧反转，得到一句话：`you are an idiot`，idiot就是答案。
{% highlight python%}
import base64
import wave
text = open('base64_text.txt','r').read()
indian = open('indian.wav','wb')
wav = base64.b64decode(text)
indian.write(wav)
indian.close()
wi = wave.open('indian.wav','rb')
wo = wave.open('indian_out.wav','wb')
wo.setparams(wi.getparams())
for i in range(wi.getnframes()):
    wo.writeframes(wi.readframes(1)[::-1])
wi.close()
wo.close()
{% endhighlight%}
第二十关URL：[http://www.pythonchallenge.com/pc/hex/idiot.html](http://www.pythonchallenge.com/pc/hex/idiot.html)

###20
第二十关，完全无头绪，按照网上前辈攻略来。图片的响应头信息有这么一段`Content-Range bytes 0-30202/2123456789`，在headers加上Range，改变请求范围试试，从30203开始，返回的content-range有改变，继续循环，最后得到一系列输出。如下：
```python
import urllib2,base64,re
url = 'http://www.pythonchallenge.com/pc/hex/unreal.jpg'
start = 30203
end   = start + 1
range_obj = re.compile('''bytes \d+-(\d+)/\d+''')
while 1:
    req = urllib2.Request(url = url, headers={'Range': 'bytes=%d-%d' % (int(start),int(end)),'Authorization': 'Basic ' + base64.b64encode('butter:fly')})
    rsp = urllib2.urlopen(req)
    print rsp.read()
    print rsp.info().dict['content-range']
    objs = range_obj.findall(rsp.info().dict['content-range'])
    if objs:
        start = int(objs[0]) + 1
        end   = start + 1
        print start,end
    else:
        break
```

    Why don't you respect my privacy?
    bytes 30203-30236/2123456789
    30237 30238
    we can go on in this way for really long time.
    bytes 30237-30283/2123456789
    30284 30285
    stop this!
    bytes 30284-30294/2123456789
    30295 30296
    invader! invader!
    bytes 30295-30312/2123456789
    30313 30314
    ok, invader. you are inside now.

又试试从2123456789开始，打印出`esrever ni emankcin wen ruoy si drowssap eht`，其实需要反过来读，`print 'esrever ni emankcin wen ruoy si drowssap eht'[::-1]`得到`the password is your new nickname in reverse`。那么，invader是我的nickname？密码就是redavni。从2123456789开始总是得到`2123456744-2123456788/2123456789`，又看到and it is hiding at 1152983631 .跟之前的信息不一样了，手动把start改为1152983631试试，打印出pk etx eot的字样，放狗搜索得知这是zip file的格式，也想明白了那个密码是zipfile的密码。从1152983631到2123456789是不成功的，while循环打印出bytes 1152983631-1153223363/2123456789，于是要从1152983631读到1153223363。

``` python
import urllib2,base64
url = 'http://www.pythonchallenge.com/pc/hex/unreal.jpg'
start = 1152983631
end   = 2123456789
while 1:
    req = urllib2.Request(url = url, headers={'Range': 'bytes=%d-%d' % (int(start),int(end)),'Authorization': 'Basic ' + base64.b64encode('butter:fly')})
    rsp = urllib2.urlopen(req)
    content = rsp.read()
    print rsp.info().dict['content-range']
    with open('Challenge20.zip','wb') as f:
        f.write(content)
    break
```

获得zipfile，输入redavni，读到文本内容.

```
Yes! This is really level 21 in here.
And yes, After you solve it, you'll be in level 22!
Now for the level:
* We used to play this game when we were kids
* When I had no idea what to do, I looked backwards.
```

第二十一关，没有看到第21关的url，看来就是要解决package.pack了。
