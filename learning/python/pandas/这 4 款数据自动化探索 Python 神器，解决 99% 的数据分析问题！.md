# 这 4 款数据自动化探索 Python 神器，解决 99% 的数据分析问题！

<a id="profileBt"></a><a id="js_name"></a>数据分析与开发 *2022-05-08 12:10*

The following article is from Python学习与数据挖掘 Author 喜欢就关注呀

<a id="copyright_info"></a>[![](../../../_resources/0_fe0479a8afcb4366b235b2078548eafd.jpg)<br>**Python学习与数据挖掘** .<br>点我关注，持续获取Python/数据分析/数据挖掘原创干货！](#)

探索性数据分析是一种非常重要的数据探索技术，用于了解数据的各个方面，这是执行任何机器学习或深度学习任务之前最重要的步骤之一。

探索性数据分析可以帮助识别明显的错误，区分数据集中的异常，发现重要元素，发现内部信息的设计并提供新的知识。![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__8fc81b102c9e431cb.png)

#### 背景

在任何机器学习项目的生命周期中，我们在数据分析、特征选择、特征工程等环节耗费时间占整个项目的 60% 的以上，一方面它是数据科学项目中最重要的部分，另一方面它是必须要进行的，比如清理数据、处理缺失值、处理异常值、处理不平衡的数据集、等等，高效完成数据探索任务势在必行。

#### 自动化探索性数据分析

今天给大家分享4款自动化探索数据分析的顶级 Python 库，列表如下：

- dtale
    
- pandas profiling
    
- sweetviz
    
- autoviz
    

##### 1、D-tale

<img width="677" height="333" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__6aaed31e2df44412b.png"/>D-tale 是一个在 2020 年 2 月推出的库，可让我们轻松可视化 pandas 数据框。它具有许多功能，对于探索性数据分析非常方便、支持交互式绘图、3d 绘图、热图、特征之间的相关性、构建自定义列等等。

安装

```
`pip install dtale
`
```

首先，我们分享一个 d-tale 的案例

```
`import dtale
import pandas as pd
df = pd.read_csv("data.csv")
d = dtale.show(df)
d.open_browser()
`
```

上述代码的输出如下所示：<img width="675" height="615" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__b912005f22af40eab.png"/>它提供许多选项，例如对数据进行排序、描述数据集、列分析等等，也可以自行查看此功能。<img width="574" height="554" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__ba5facd97f8a448b9.png"/>如果单击"Describe"，则会显示所选列的统计分析，例如平均值、中位数、最大值、最小值方差、标准差、四分位数等等。<img width="675" height="333" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__3a3eafff71de411c8.png"/>也可以自行尝试其他功能，例如列分析、格式、过滤器。![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__92ac443a57a544afb.png)如何相互关联呢？<img width="677" height="639" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__322e5073bb2c495c9.png"/>图表 \- 建立自定义图表，如折线图、条形图、饼图、堆叠图、散点图、地质图等。<img width="677" height="431" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__4b973861db864e0e8.png"/>这个工具非常方便，与使用传统的机器学习库（如 pandas、matplotlib 等）相比，它探索性数据分析更快。

##### 2、Pandas Profiling

<img width="677" height="357" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__28449b8a93314b28b.png"/>它是一个用 python 编写的开源库，生成交互式 HTML 报告并描述数据集的各个方面。关键功能包括处理缺失值、数据集的统计数据（如平均值、众数、中位数、偏度、标准差等），以及直方图和相关性等图表。

安装

```
`pip install pandas-profiling
`
```

让我们深入研究使用这个库的探索性数据分析。使用示例数据集从 pandas 分析开始：

```
`#importing required packages
import pandas as pd
import pandas_profiling
import numpy as np
#importing the data
df = pd.read_csv('sample.csv')
#descriptive statistics
pandas_profiling.ProfileReport(df)
`
```

下面是上述代码输出![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

这是一个数据分析报告，它返回数据集中的变量数量、行数、数据集中缺失的单元格、缺失单元格的百分比、重复行的数量和百分比。缺失和重复的单元格数据对于我们的分析非常重要，因为它描述了数据集的更广泛情况。该报告还显示内存的总大小。

变量部分显示特定列的分析。例如对于分类变量，将出现以下输出![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)它提供对数值变量的深入分析，例如分位数、均值、中位数和、方差、单调性、范围、峰度、四分位间距等等。

描述变量如何相互关联，这些数据对于数据科学家来说是非常必要的。![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

##### 3、Sweetviz

Sweetviz 是一个开源的 Python 库，用于获得可视化效果，只需几行代码即可用于探索性数据分析。该库可用于可视化变量和比较数据集。![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

安装

```
`pip install sweetviz
`
```

让我们深入研究使用这个库的探索性数据分析，使用示例数据集开始

```
`import sweetviz
import pandas as pd
df = pd.read_csv('sample.csv')
my_report  = sweetviz.analyze([df,'Train'], target_feat='SalePrice')
my_report.show_html('FinalReport.html')
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

##### 4、Autoviz

Autoviz 代表自动可视化，只需几行代码，就可以使用任意大小的数据集进行可视化。![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)安装

```
`pip install autoviz
`
```

可视化

```
`from autoviz.AutoViz_Class import AutoViz_Class
AV = AutoViz_Class()
df = AV.AutoViz('sample.csv')
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

\- EOF -

推荐阅读  点击标题可跳转

<ins>1、[我学习 Python 的三个神级网站](http://mp.weixin.qq.com/s?__biz=MzA5ODM5MDU3MA==&mid=2650883906&idx=1&sn=a9b6ee2ad3bd96a37969c7f2d273a285&chksm=8b67a007bc102911da0b7591857786a1699b406391c16c513279baf21c5cad154d9c6052cc41&scene=21#wechat_redirect)</ins>

<ins>2、[16 个动态图！一款好用到爆的 Python 可视化利器](http://mp.weixin.qq.com/s?__biz=MzA5ODM5MDU3MA==&mid=2650880873&idx=2&sn=d85bcf2a744cb8ea32475aab8bd41014&chksm=8b67dc2cbc10553a9a5cb9df1ea613d90f26553945e9eeb03369ff0fbcff38e95c8223df73e4&scene=21#wechat_redirect)</ins>

<ins>3、[这个插件竟打通了 Python 和 Excel ，还能自动生成代码！](http://mp.weixin.qq.com/s?__biz=MzA5ODM5MDU3MA==&mid=2650884017&idx=2&sn=8643b5d79f48c76b7018c4da38840122&chksm=8b67a0f4bc1029e2bda4478936b07d5afc6f1cf2115fef13b12a58eb84ffd185a02097a0ebb4&scene=21#wechat_redirect)</ins>

看完本文有收获？请转发分享给更多人

**推荐关注「数据分析与开发」，提升数据技能**

![](../../../_resources/0_wx_fmt_png_10dba079da7649218875829aaae9b6d9.png)

**数据分析与开发**

「数据分析与开发」分享数据分析与开发相关技术文章、教程、工具

<a id="js_profile_article"></a>55篇原创内容

Official Account

点赞和在看就是最大的支持❤️

Posted on 浙江

People who liked this content also liked

SQL常用函数系列——8个常用的字符函数

...

爱数据LoveData

不看的原因

- 内容质量低
- 不看此公众号

Github开发大神教你玩转数据库编程

...

新智元

不看的原因

- 内容质量低
- 不看此公众号

Anaconda 发布 PyScript：在网页中嵌入 Python 代码

...

21CTO

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___a35909d9063b4a119.bmp"/>

Scan to Follow