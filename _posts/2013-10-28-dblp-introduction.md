---
layout: post
title: DBLP数据集简介及简单用法
tags:
- DBLP
- 数据集
---

前一段时间利用大名鼎鼎的DBLP数据集做关于论文合作关系推荐的实验，感觉确实是一个非常不错的数据集，可挖掘的东西很多很多，在此对DBLP及其用法做一个简单介绍。

###简介

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

- **列出所有合作者**：`http://dblp.uni-trier.de/pers/xc/t/Tang:Jie`（不好意思，这里是唐杰老师的名字。）     
**请求格式**：倒数第二个参数是姓的首字母。倒数第一个参数是姓：名。（老外名字也是，Harry Potter,请求参数为`p/Potter:Harry`）   
**返回格式**：   

		<coauthors author="Jie Tang" urlpt="t/Tang:Jie">
			<author urlpt="a/Abbeel:Pieter" count="4">Pieter Abbeel</author>
			<author urlpt="a/Aberer:Karl" count="1">Karl Aberer</author>
			<author urlpt="a/Anderson:Steven_J=" count="1">Steven J. Anderson</author>
			...
		</coauthors>
注意：urlpt是DBLP用来标识唯一作者的。第二级节点包括合作者的urlpt和合作数目。

- **获取论文详细信息**：`http://dblp.uni-trier.de/rec/bibtex/key.xml`   
**请求格式**：最后一个参数key为论文在DBLP中的唯一标识，例如：
`http://dblp.uni-trier.de/rec/bibtex/journals/dke/TangCKW13.xml`（不好意思又是唐老师大作）。   
**返回格式**：

		<dblp>
			<article key="journals/dke/TangCKW13" mdate="2013-10-17">
				<author>Jie Tang</author>
				<author>Ling Chen</author>
				<author>Irwin King</author>
				<author>Jianyong Wang</author>
				<title>
					Introduction to Special section on Large-scale Data Mining.
				</title>
				<pages>355-356</pages>
				<year>2013</year>
				<volume>87</volume>
				<journal>Data Knowl. Eng.</journal>
				<ee>http://dx.doi.org/10.1016/j.datak.2013.05.001</ee>
				<url>db/journals/dke/dke87.html#TangCKW13</url>
			</article>
		</dblp>
注意：`article`的key唯一标识该文章，`journals`表示期刊，`dke`表示期刊名，（一般是期刊名首字母）   
其他字段很明显了，不解释。

- **搜索作者**：`http://dblp.uni-trier.de/search/author?author=Tang`（HTML格式）   `http://dblp.uni-trier.de/search/author?xauthor=Tang`(XML格式)    
**请求格式**：author或者xauthor后面加搜索内容    
**返回格式**：模糊匹配到的作者列表    

		<authors>
			<author urlpt="=/=Fayuan=:Kuo=Ming_Tang">Kuo-Ming Tang (Fayuan)</author>
			<author urlpt="b/Basar:Tang=uuml=l_=Uuml==">Tangül Ü. Basar</author>
			<author urlpt="b/Berre:Tanguy_Le">Tanguy Le Berre</author>
			<author urlpt="b/Boespflug=Tanguy:Odile">Odile Boespflug-Tanguy</author>
			<author urlpt="b/Bohnuud:Tanggis">Tanggis Bohnuud</author>
			<author urlpt="b/Boyland:John_Tang">John Tang Boyland</author>
			...
		</authors>

- **搜索某作者所有论文**：`http://dblp.uni-trier.de/pers/xk/urlpt`        
**请求格式**：`http://dblp.uni-trier.de/pers/xk/t/Tang:Jie`   
**返回格式**：文章列表。给出的是`dblpkey`，即论文在DBLP中的唯一标识。   

		<dblpperson name="Jie Tang">
			<dblpkey type="person record">homepages/t/JieTang</dblpkey>
			<dblpkey>journals/dke/TangCKW13</dblpkey>
			<dblpkey>journals/icl/BournakaTLL13</dblpkey>
			<dblpkey>journals/jcst/TangTLLGG13</dblpkey>
			<dblpkey>journals/joi/HeDTRB13</dblpkey>
			<dblpkey>journals/kais/WangTFCTY13</dblpkey>
			<dblpkey>journals/kbs/WangLZST13</dblpkey>
			...
		</dblpperson>

###DBLP可下载数据集

下载地址[在这](http://dblp.uni-trier.de/xml/) 。   

其中：**dblp.xml**是我们需要的数据集。**dblp.dtd**是格式说明文件。解析的时候和前者放在一
起。   

**dblp.xml文件格式**：除<dblp>标签外的一级标签有：**Article**，**inproceedings**，  **proceedings**，  **book**，  **incollection**， **phdthesis**， **mastersthesis** ，**www**

###我对DBLP的处理

**语言**：Python    

**工具**：SAX解析包（专门处理xml文档。Python的一个类库）  
  
**SAX解析代码**：   

sax是一个基于事件流的xml解析工具，边读文件边解析，好处是无内存限制，可以解析处理较大的xml文件。其代码如下。   


{% highlight python %}
from xml.sax import handler, make_parser
paperTag = ('article','inproceedings','proceedings','book',
                   'incollection','phdthesis','mastersthesis','www')
class mHandler(handler.ContentHandler):

   	def __init__(self):
   	
   	def startDocument(self):
   	    print 'Document Start'
   	    
   	def endDocument(self):
   	    print 'Document End'
   	    
   	def startElement(self, name, attrs):
		print 'Element start'
		
   	def endElement(self, name):
		print 'Element end'
		
   	def characters(self, content):
		print content

def parserDblpXml():
    handler = mHandler()
    parser = make_parser()
    parser.setContentHandler(handler)
    f = open(DBLP_XML_PATH,'r')
    parser.parse(f)
    f.close()
	if __name__ == '__name__':
    parserDblpXml()
{% endhighlight %}

###简单总结

总的来说，DBLP集成元素不多，只有最基本的论文题目，时间，作者，发表类型及期刊或会议名称等等。可能很多人想要的标签、关键词都没有。但是，基于DBLP数据集这些基本的元素，可以挖掘、利用的也是很多。例如官网给出的统计信息，就能引申出很多东西。    

涉及到DBLP，我能一下想到的关键词：经典的复杂网络，小世界，无标度，合作关系网，关系推荐，聚类，连接预测，随机游走，中心作者分析，作者影响力分析，研究热点发展等等，非常多。因此，DBLP是个很丰富宝贵的资源。    

**整理的有点乱，也不全面。仅作参考。欢迎补充、共享。**