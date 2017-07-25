---
title: CSDN迁移
date: 2016-05-20 23:00:12
tags: 
  - blog
  - csdn
category: 随笔
description: 
---


**博客迁移**：经过一些时间的处理，已经将csdn上的博文都成功的迁移过来了，效果感觉很好。本次将博文转移使用了github上的一个开源工具[csdn-blog-export](https://github.com/search?utf8=%E2%9C%93&q=csdn%20markdown)。
<!-- more -->
准备
--

要完成迁移，需要将csdn上的博客由html转化为markdown，使用csdn-blog-export能很好的进行抽取，而且使用非常便捷。
csdn-blog-export是基于python2.7写的，所以要想使用这个工具，那么必须有先安装：

 - python2.7 [下载链接](https://www.python.org/download/releases/2.7/)
 - beautifulsoup4 [下载链接](https://pypi.python.org/pypi/beautifulsoup4)
 - csdn-blog-export [下载链接](https://github.com/search?utf8=%E2%9C%93&q=csdn%20markdown)

 


安装
--

安装需要在linux下进行，python一般直接就集成了，beautifulsoup4 则下载完之后解压，进入文件夹，运行命令：

```
cd beautifulsoup4...(进入解压文件夹)
python setup.py install
(或者)sudo python setup.py install
```
对于csdn-blog-export 直接解压到文件夹就可以了

运行
--

解压之后，进入目录，直接运行一下命令就行了：

```
main.py -u <username> [-f <format>] [-p <page>] [-o <outputDirectory>]
    <format>： html | markdown，缺省为markdown
    <page>为导出特定页面的文章，缺省导出所有文章
    <outputDirectory>暂不可用
```
比如，我需要将我的csdn所有博文导出，则运行以下代码就可以：

```
./main.py -u phenixfate-f markdown
or
./main.py -u cecesjtu
```

如果需要输出格式为html，则命令为：
```
./main.py -u phenixfate -f html
```

最终获取到了所有博文的md文件，将md文件放入source/_post目录，然后构建，就可以将博文发布了。
需要注意的一点是，发布时要有title，date，flag等文章元素csdn-blog-export 工具构建出来的md文件是不包含的，需要自己加入进去，要不然上传的文章将显示不了标题，标签信息。