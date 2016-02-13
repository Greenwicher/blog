title: 毕设开题不会写，怎么破？快试试BiblioPy吧！
toc: true
date: 2014-12-04 17:12:04
categories: [如何做科研]
tags: [文献分析,BiblioPy,开源项目]
description: 文献话题挖掘工具BiblioPy介绍
feature: imgs/cover/BiblioPy-Intro.png
---

春花秋月何时了，毕设开题开不了。导师昨夜又催稿，学渣不堪回首发呆中。学长论文应犹在，只是用不了。问君能有几多愁？恰似一江春水向东流。怎么破！怎么破！快来试试[**BiblioPy**](https://github.com/Greenwicher/BiblioPy)吧！

<!--more-->

# BiblioPy简介

本科时我的毕业论文题目是『生产与运作管理研究趋势分析』，也就是对研究本身的研究啦，隶属文献计量学与自然语言处理范畴。**主要目的是为生产与运作管理领域的研究者提供该领域的核心研究话题群以及话题演变动态模式。核心研究话题群可以为初涉该领域的人员提供宏观的鸟瞰，话题演变动态模式可以为久扎某个领域的人员提供新的研究方向。**本人对Sebastian Grauwin的[BiblioTools2](http://www.sebastian-grauwin.com/?page_id=492)进行了部分修改重写，使之能按照文献计量学的主流方法co-citation来进行分析，目前只完成了核心研究话题群的识别。因为整套方法论基本适用于所有学科的分析，因此希望自己写的这个小脚本对大家学术研究入门之路能有所帮助，也希望大家能给我提些建议。

# 工作原理

因为牵涉到一些专业术语，我就简单的说明一下[**BiblioPy**](https://github.com/Greenwicher/BiblioPy)的工作原理。

## step1 文献数据获取

据相应搜索条件从web of science核心数据库上下载文献数据（包括文章的标题、关键词、摘要、引文等等），文献数据格式为制表符分割的UTF8纯文本文件。因为web of science一次最多下载500条记录，如果记录过多的话，只能多次下载获得。

![](http://greenwicher.qiniudn.com/20141204-BiblioPy-Intro/wos-txt.jpg)

## step2 建立文章-引文矩阵

文章-引文矩阵的行标代表所有文章标号，列标代表所有引文标号。矩阵中的元素表示该行标所代表的文章是否引用了该列标所代表的引文，取值0或1，1表示二者存在引用关系。

## step3 建立同被引网络

根据上述的文章-引文矩阵，以每篇引文作为网络中的结点，网络中边的权重表示相应结点的相似性（即都引用了这两篇引文的文章的数目），建立同被引网络（co-citation networks）。

![](http://greenwicher.qiniudn.com/20141204-BiblioPy-Intro/co-citation-network.jpg)

## step4 对网络进行聚类

其实在step2结束之后，可以利用PCA或MDS进行降维处理，但是为了体现网络的结构特性，这里用了网络聚类。就是指利用网络的拓扑结构将网络中的结点分成不同的几个群体，不同的群体有不同的研究话题。

![](http://greenwicher.qiniudn.com/20141204-BiblioPy-Intro/co-citation-network-cluster.jpg)

## step5 发现话题

目前使用的方法是自然语言处理中的TextRank，TF-IDF算法来对每一聚类中的关键词代表性进行排序。这里除了给出每个聚类的top关键词外，还给出了top reference和top author等，并利用LaTex生成了相应的聚类分析报告，如下图所示。

![](http://greenwicher.qiniudn.com/20141204-BiblioPy-Intro/report.jpg)

![](http://greenwicher.qiniudn.com/20141204-BiblioPy-Intro/graph.jpg)

# 立刻尝试

文献综述的撰写可以根据上述报告中的Top Keyword和Top Reference部分综合得出，Keyword体现了该话题下的研究人员们之前都做了什么（相当于综述啦），而Reference部分则体现了他们是怎么做的（大概搜下文献，稍微看下abstract就可以了）。也欢迎对学术有所兴趣，但不知道自己所学专业重要方向的同学来试试[**BiblioPy**](https://github.com/Greenwicher/BiblioPy)~ 目前的脚本只是个demo，并且web of science的下载受到IP限制，后续有时间和精力的话，会把完善的[**BiblioPy**](https://github.com/Greenwicher/BiblioPy)以网页版呈现并开发动态性话题变迁以及趋势发现的功能。因此目前，只能人工从web of science下载数据，然后自己运行脚本和LaTeX来分析和生成报告了。

对文献计量软件感兴趣的同学也可以试试类似的软件：

BiblioTools2.x: http://www.sebastian-grauwin.com/?page_id=492

CiteSpace: http://cluster.cis.drexel.edu/~cchen/citespace/

HistCite: http://histcite.com
