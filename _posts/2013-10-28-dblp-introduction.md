---
layout: post
title: DBLP数据集简介及简单用法
tags:
- DBLP
- 数据集
---

前一段时间利用大名鼎鼎的DBLP数据集做关于论文合作关系推荐的实验，感觉确实是一个非常不错的数据集，可挖掘的东西很多很多，在此对DBLP及其用法做一个简单介绍。
###简介###
**DBLP**——**Digital Bibliography & Library Project**的缩写。这里是[DBLP的主页](http://www.informatik.uni-trier.de/~ley/db/)   
DBLP是计算机领域内对研究的成果以作者为核心的一个计算机类英文文献的集成数据库系统，按年代列出了作者的科研成果。包括国际期刊和会议等公开发表的论文。DBLP没有提供对中文文献的收录和检索功能，国内类似的权威期刊及重要会议论文集成检索系统有C-DBLP。   
这个项目是德国特里尔大学的Michael Ley负责开发和维护。它提供计算机领域科学文献的搜索服务，但只储存这些文献的相关元数据，如标题，作者，发表日期等。和一般流行的情况不同，DBLP并没有使用数据库而是使用XML存储元数据。   
DBLP在学术界声誉很高，而且很多论文及实验都是基于DBLP的。所收录的期刊和会议论文质量较高，也比较全面。（我刚才看了下，已经大致集成2.1 million的文章，也包含了很多计算机科学研究者的主页连接。）文献更新速度很快（现在是2013年10月28日，系统显示最后一次更新是2013年10月27日。可见更新之快），能很好地反应了国外学术研究的前沿方向。   
官网发了[这篇文章](http://dblp.uni-trier.de/xml/docu/dblpxml.pdf)，对DBLP做了详细解释。   
另外，DBLP数据开放免费，版权和许可[在这](http://www.informatik.uni-trier.de/~ley/db/copyright.html)。 
###提供的服务 
DBLP的支持团队基于DBLP数据做了很多工作。提供各种搜索、统计等服务，并提供了API和可下载数据集。[这里](http://dblps.uni-trier.de/~mwagner/statistics/)有些有意思的统计数据，并用google chart tool做了可视化处理。例如：出版物类型分布，每个期刊或会议的作者数，每年论文数目，每个作者的合作者数目，并且每年、每月都会做些全局数据统计。
###DBLP的API 
DBLP数据文件1G多。假如不需要所有数据或者对获取数据速度没较高要求的话，可使用它的API。[这里](http://dblp.uni-trier.de/xml/docu/dblpxmlreq.pdf)官网发了篇文章，里面解释了BDLP的基础API以及用法，还有几个例子（java、C）。有个是经典的“两个作者在合作网络上的最短路径”。   
**我这里列几个比较可能会用到的**   

- 列出所有合作者：`http://dblp.uni-trier.de/pers/xc/t/Tang:Jie`（不好意思，这里是唐杰老师的名字。）    
请求格式：倒数第二个参数是姓的首字母。倒数第一个参数是姓：名。（老外也是，Harry Potter,请求参数为`p/Potter:Harry`）
返回格式：xml文件，两级节点。

{ % highlight html % }
<coauthors author="Jie Tang" urlpt="t/Tang:Jie">
	<author urlpt="a/Abbeel:Pieter" count="4">Pieter Abbeel</author>
	<author urlpt="a/Aberer:Karl" count="1">Karl Aberer</author>
	<author urlpt="a/Anderson:Steven_J=" count="1">Steven J. Anderson</author>
</coauthors>
{ % endhighlight % }

{% highlight java %}
public class Hello {
    public static void main(String[] args) {
        System.out.println("Hello World!");
    }
}
{% endhighlight %}

注意：urlpt是DBLP用来标识唯一作者的。第二级节点包括合作者的urlpt和合作数目。