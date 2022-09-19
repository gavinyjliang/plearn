# 聊聊 Pandas 的前世今生

<a id="profileBt"></a><a id="js_name"></a>数据分析与开发 *2021-06-22 12:15*

The following article is from Python大数据分析 Author 朱卫军

<a id="copyright_info"></a>[![](../../../_resources/0_d0e415cc352c4c35b050d5bf91377f57.jpg)<br>**Python大数据分析** .<br>分享python编程、可视化设计、大数据分析、机器学习等技术以及数据分析案例，包括但不限于pandas、numpy、spark、matplotlib、sklearn、tensorflow、keras、tableau等](#)

本文将从Python生态、Pandas历史背景、Pandas核心语法、Pandas学习资源四个方面去聊一聊Pandas，期望能带给大家一点启发。

<img width="677" height="251" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__cd6b7758be564d2fb.png"/>

## 一、Python生态里的Pandas

五月份TIOBE编程语言排行榜，Python追上Java又回到第二的位置。Python如此受欢迎一方面得益于它崇尚简洁的编程哲学，另一方面是因为强大的第三方库生态。<img width="677" height="160" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__7ab14d98c63e48e19.png"/>

要说杀手级的库，很难排出个先后顺序，因为python的明星库非常多，在各个领域都算得上出类拔萃。

比如web框架-Django、深度学习框架-TensorFlow、自然语言处理框架-NLTK、图像处理库-PIL、爬虫库-requests、图形界面框架-PyQt、可视化库-Matplotlib、科学计算库-Numpy、数据分析库-Pandas......![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

上面大部分库我都用过，用的最多也最顺手的是Pandas，可以说这是一个生态上最完整、功能上最强大、体验上最便捷的数据分析库，称为编程界的Excel也不为过。

Pandas在Python数据科学链条中起着关键作用，处理数据十分方便，且连接Python与其它核心库。

<img width="677" height="279" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_227fec1da35a4d719.jpg"/>

## 二、十项全能的Pandas

Pandas诞生于2008年，它的开发者是Wes McKinney，一个量化金融分析工程师。

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__0146eb8d514645248.png)

因为疲于应付繁杂的财务数据，Wes McKinney便自学Python，并开发了Pandas。

大神就是这么任性，没有，就创造。

为什么叫作Pandas，其实这是“Python data analysis”的简写，同时也衍生自计量经济学术语“panel data”（面板数据）。

![Image](../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_930079ecea9c41918.jpg)

所以说Pandas的诞生是为了分析金融财务数据，当然现在它已经应用在各个领域了。

> ❝
> 
> 2008: Pandas正式开发并发布 
> 
> 2009:Pandas成为开源项目 
> 
> 2012: 《利用Python进行数据分析》出版 
> 
> 2015: Pandas 成为 NumFOCUS 赞助的项目
> 
> ❞

**Pandas能做什么呢？**

它可以帮助你任意探索数据，对数据进行读取、导入、导出、连接、合并、分组、插入、拆分、透视、索引、切分、转换等，以及可视化展示、复杂统计、数据库交互、web爬取等。

同时Pandas还可以使用复杂的自定义函数处理数据，并与numpy、matplotlib、sklearn、pyspark、sklearn等众多科学计算库交互。

<img width="677" height="339" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_881833da6d6c4d398.jpg"/>

Pandas有一个伟大的目标，即成为任何语言中可用的最强大、最灵活的开源数据分析工具。

让我们期待下。

## 三、Pandas核心语法

#### 1\.  数据类型

Pandas的基本数据类型是dataframe和series两种，也就是行和列的形式，dataframe是多行多列，series是单列多行。

<img width="677" height="378" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_dd0cbaee90be41ef8.jpg"/>

如果在jupyter notebook里面使用pandas，那么数据展示的形式像excel表一样，有行字段和列字段，还有值。

<img width="677" height="282" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_281d3ab1b69845bd8.jpg"/>

#### 2\. 读取数据

pandas支持读取和输出多种数据类型，包括但不限于csv、txt、xlsx、json、html、sql、parquet、sas、spss、stata、hdf5

读取一般通过read_*函数实现，输出通过to_*函数实现。

<img width="677" height="198" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__894279dc326d45d4b.png"/>

<img width="677" height="430" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_5f4f68771f0f4de0a.jpg"/>

image

<img width="677" height="371" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_9332b9bbea9644e59.jpg"/>

image

#### 3\. 选择数据子集

导入数据后，一般要对数据进行清洗，我们会选择部分数据使用，也就是子集。

在pandas中选择数据子集非常简单，通过筛选行和列字段的值实现。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

具体实现如下：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

#### 4\. 数据可视化

不要以为pandas只是个数据处理工具，它还可以帮助你做可视化图表，而且能高度集成matplotlib。

你可以用pandas的plot方法绘制散点图、柱状图、折线图等各种主流图表。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

#### 5\. 创建新列

有时需要通过函数转化旧列创建一个新的字段列，pandas也能轻而易举的实现

<img width="677" height="174" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__08f82759999e4f0fb.png"/>

<img width="677" height="176" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__e5d5e246f0144eb18.png"/>

<img width="677" height="248" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_8dd479f6b6a14ca38.jpg"/>

image

#### 6\. 分组计算

在sql中会用到group by这个方法，用来对某个或多个列进行分组，计算其他列的统计值。

pandas也有这样的功能，而且和sql的用法类似。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

image

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

#### 7\. 数据合并

数据处理中经常会遇到将多个表合并成一个表的情况，很多人会打开多个excel表，然后手动复制粘贴，这样就很低效。

pandas提供了merge、join、concat等方法用来合并或连接多张表。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

#### 小结

pandas还有数以千计的强大函数，能实现各种骚操作。

python也还有数不胜数的宝藏库，等着大家去探索

## 三、Pandas学习资源

如果说学习Pandas最好的教程是什么，那毫无疑问是官方文档，从小白到高手，它都给你安排的妥妥的，这个后面详细介绍。

下面我会从入门、进阶、练习四个三面给你们推荐相应的教程和资源。

#### 1\. 入门教程

十分钟入门Pandas（英文版）<sup>\[1\]</sup>

这是Pandas官网专门为新手写的入门引导，大概就几千字，包括对Pandas的简要介绍，和一些基本的功能函数。

主要的内容有：数据的创建、查看、筛选、拼接、连接、分组、变形、可视化等等。

而且这个小册子包含了很多代码示例，如果你能完整过一遍，入门Pandas基本没啥问题。

中文版似乎也有，但翻译的准确性大家自己识别斟酌下。

十分钟入门 Pandas | Pandas 中文<sup>\[2\]</sup>

**利用Pandas进行数据分析<sup>\[3\]</sup>**

这本书不用了说了，可能是你入门python数据分析的第一本书，它的作者是Pandas库的核心开发者，也就是说这本书相当于是Pandas的官方出版教程。

<img width="677" height="267" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_80aa25a9022a48bcb.jpg"/>

image

为什么它适合入门pandas，因为整本书的编排是从数据分析的角度切入的，由浅入深将pandas对数据的处理讲的很透彻。

当然这本书也存在知识点过于零碎，翻译不到位的问题，但整体来说是本好书。

**w3schools pandas tutorial<sup>\[4\]</sup>**

w3school的pandas文档， 逻辑比较清晰，也是从数据分析角度去讲pandas。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

image

**Learn Pandas Tutorials<sup>\[5\]</sup>**

数据科学平台kaggle提供的pandas入门教程，共六大节涵盖了pandas数据处理各种方法。

<img width="677" height="324" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_23b1a1531f4246449.jpg"/>

image

**joyful-pandas<sup>\[6\]</sup>**

国内小伙伴写的Pandas笔记，挺详细的，大家可以去下载项目里的notebook，放到自己电脑里练习。

#### 2\. 进阶教程

**pandas用户指南<sup>\[7\]</sup>**

这是pandas官网的教程，非常详细，主要从数据处理的角度介绍相应的pandas函数，方便用户查阅。

如果你的英文还不错，也喜欢阅读技术文档，我是建议花时间把这份指南看一遍，配合练习。

我把整个pandas文档下载下来，发现足足有3000多页。

**pandas api检索<sup>\[8\]</sup>**

官网的pandas api集合，也就是pandas所有函数方法的使用规则，是字典式的教程，建议多查查。

**pandas-cookbook<sup>\[9\]</sup>**

这是一个开源文档，作者不光介绍了Pandas的基本语法，还给出了大量的数据案例，让你在分析数据的过程中熟悉pandas各种操作。

**Python Data Science Handbook<sup>\[10\]</sup>**

数据科学书册，不光有pandas，还有ipython、numpy、matplotlib、sklearn，这些都是深入学习pandas不可缺少的工具。

#### 3\. 练习资源

**Pandas练习集<sup>\[11\]</sup>**

github上一个练习项目，针对pandas每个功能都有对应的真实数据练习。

**101个Pandas练习<sup>\[12\]</sup>**

一位国外博主总结的100多个pandas练习题，非常全面。

**datacamp<sup>\[13\]</sup>**

数据科学教程网站，里面有大量pandas的练习题，还提供了详细的速查表。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

#### 小结

pandas的教程主要还是以英文为主，国内翻译的质量参差不齐，还是建议你在入门后多去看英文文档，这是第一手资料，也是最靠谱的。

### Reference

\[1\]

十分钟入门Pandas（英文版）:https://pandas.pydata.org/docs/user_guide/10min.html

\[2\]

十分钟入门 Pandas | Pandas 中文:http://www.pypandas.cn/docs/getting_started/10min.html

\[3\]

利用Pandas进行数据分析:https://github.com/wesm/pydata-book

\[4\]

w3schools pandas tutorial:https://www.w3schools.com/python/pandas/default.asp

\[5\]

Learn Pandas Tutorials:https://www.kaggle.com/learn/pandas

\[6\]

joyful-pandas:https://github.com/datawhalechina/joyful-pandas

\[7\]

pandas用户指南:https://pandas.pydata.org/docs/user_guide/index.html#user-guide

\[8\]

pandas api检索:https://pandas.pydata.org/docs/reference/index.html#api

\[9\]

pandas-cookbook:https://github.com/jvns/pandas-cookbook

\[10\]

Python Data Science Handbook:https://jakevdp.github.io/PythonDataScienceHandbook/

\[11\]

Pandas练习集:https://github.com/guipsamora/pandas_exercises

\[12\]

101个Pandas练习:https://www.machinelearningplus.com/python/101-pandas-exercises-python/

\[13\]

datacamp:https://www.datacamp.com/community/tutorials

\- EOF -

推荐阅读  点击标题可跳转

<ins>1、[总结了 67 个 pandas 函数，完美解决数据处理！](http://mp.weixin.qq.com/s?__biz=MzA5ODM5MDU3MA==&mid=2650872152&idx=1&sn=e578d7a627afaf19f800fdedfb5dd251&chksm=8b67fe1dbc10770b2b38c56064890b860b99faac262194d22ed2f1c24ea2c908114c97463af5&scene=21#wechat_redirect)</ins>

<ins>2、[对比 Excel，学习 pandas 数据透视表](http://mp.weixin.qq.com/s?__biz=MzA5ODM5MDU3MA==&mid=2650872461&idx=2&sn=1abdbe9d4254d96000dab11c38f9dd66&chksm=8b67fdc8bc1074de0e90dee66bf17c9fd851c13e724a7ac83e6f429d16916445fee2282dba61&scene=21#wechat_redirect)</ins>

<ins>3、[Seaborn + Pandas 带你玩转股市数据可视化分析](http://mp.weixin.qq.com/s?__biz=MzA5ODM5MDU3MA==&mid=2650872805&idx=2&sn=28260c5bfecf250c1cdd0ac5b35ed1f5&chksm=8b67fca0bc1075b67cb0fb7b6de2150af2ee987791ebb877ed33270542b61ec90dbff3141fd0&scene=21#wechat_redirect)</ins>

看完本文有收获？请转发分享给更多人

**推荐关注「数据分析与开发」，提升数据技能**

![](../../../_resources/0_wx_fmt_png_3c17223bdd0b4f50976b821e95f72d9a.png)

**数据分析与开发**

「数据分析与开发」分享数据分析与开发相关技术文章、教程、工具

<a id="js_profile_article"></a>55篇原创内容

Official Account

点赞和在看就是最大的支持❤️

People who liked this content also liked

我学习 Python 的三个神级网站

数据分析与开发

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___c1e0b9c97bd441f29.bmp"/>

Scan to Follow