# 简单好用，分享 4 款 Pandas 自动数据分析神器！

<a id="profileBt"></a><a id="js_name"></a>数据分析与开发 *2022-02-21 20:00*

The following article is from 渡码 Author 渡码

<a id="copyright_info"></a>[![](../../../_resources/0_cc4ca34f9d3949db870c31e3e4221df9.jpg)<br>**渡码** .<br>持续输出 Python 全栈优质文章](#)

我们做数据分析，在第一次拿到数据集的时候，一般会用统计学或可视化方法来了解原始数据。

了解列数、行数、取值分布、缺失值、列之间的相关关系等等，这个过程叫做 `EDA`（Exploratory Data Analysis，探索性数据分析）。

如果你现在做`EDA`还在用`pandas`一行行写代码，那么福音来了！

目前已经有很多`EDA`工具可以自动产出基础的统计数据和图表，能为我们节省大量时间。

本文会对比介绍 4 款常用的`EDA`工具，最后一款绝了，完全是抛弃代码的节奏。

正式介绍这些工具之前，先来加载数据集

```
import numpy as np
import pandas as pd
iris = pd.read_csv('iris.csv')
iris

```

<img width="661" height="259" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__b9ba743e2d3c4e6bb.png"/>

`iris`是下面用到的数据集，是一个`150行 * 4列`的 DataFrame。

### 1\. PandasGUI

`PandasGUI`提供数据预览、筛选、统计、多种图表展示以及数据转换。

```
# 安装
# pip install pandasgui
from pandasgui import show
show(iris)

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

PandasGUI操作界面

`PandasGUI`更侧重数据展示，提供了10多种图表，通过可视的方式配置。

但数据统计做的比较简单，没有提供缺失值、相关系数等指标，数据转换部分也只开放了一小部分接口。

### 2\. Pandas Profiling

`Pandas Profiling` 提供了整体数据概况、每列的详情、列之间的关图、列之间的相关系数。

```
# 安装：
# pip install -U pandas-profiling
# jupyter nbextension enable --py widgetsnbextension
from pandas_profiling import ProfileReport
profile = ProfileReport(iris, title='iris Pandas Profiling Report', explorative=True)
profile

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

Pandas Profiling操作界面

每列的详情包括：缺失值统计、去重计数、最值、平均值等统计指标和取值分布的柱状图。

列之间的相关系数支持Spearman、Pearson、Kendall 和 Phik 4 种相关系数算法。

与 `PandasGUI` 相反，`Pandas Profiling`没有丰富的图表，但提供了非常多的统计指标以及相关系数。

### 3\. Sweetviz

`Sweetviz`与`Pandas Profiling`类似，提供了每列详细的统计指标、取值分布、缺失值统计以及列之间的相关系数。

```
# 安装
# pip install sweetviz
import sweetviz as sv
sv_report = sv.analyze(iris)
sv_report.show_html()

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

Sweetviz操作界面

`Sweetviz`还有有一个非常好的特性是支持不同数据集的对比，如：训练数据集和测试数据集的对比。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

Sweetviz数据集对比

蓝色和橙色代表不同的数据集，通过对比可以清晰发现数据集之前的差异。

### 4\. dtale

最后重磅介绍`dtale`，它不仅提供丰富图表展示数据，还提供了很多交互式的接口，对数据进行操作、转换。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

dtale操作界面

`dtale`的功能主要分为三部分：**数据操作**、**数据可视化**、**高亮显示**。

#### 4.1 数据操作（Actions）

`dtale`将`pandas`的函数包装成可视化接口，可以让我们通过图形界面方式来操作数据。

```
# pip install dtale
import dtale
d = dtale.show(iris)
d.open_browser()

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

Actions

右半部分图是左边图的中文翻译，用的是 Chrome 自动翻译，有些不是很准确。

举一个**数据操作**的例子。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

Summarize Data

上图是**Actions**菜单中**Summarize Data**的功能，它提供了对数据集汇总操作的接口。

上图我们选择按照`species`列分组，计算`sepal_width`列的平均值，同时可以看到左下角`dtale`已经自动为该操作生成了`pandas`代码。

#### 4.2 数据可视化（Visualize）

提供比较丰富的图表，对每列数据概况、重复行、缺失值、相关系数进行统计和展示。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

Visualize

举一个**数据可视化**的例子。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

Describe

上图是Visualize菜单中Describe的功能，它可以统计每列的最值、均值、标准差等指标，并提供图表展示。

右侧的`Code Export`可以查看生成这些数据的代码。

#### 4.3 高亮显示（Highlight）

对缺失值、异常值做高亮显示，方便我们快速定位到异常的数据。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

Highlight

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

上图显示了将`sepal_width`字段的异常值。

`dtale`非常强大，功能也非常多，大家可以多多探索、挖掘。

最后，简单总结一下。如果探索的数据集侧重数据展示，可以选`PandasGUI`；如果只是简单了解基本统计指标，可以选择`Pandas Profiling`和`Sweetviz`；如果需要做深度的数据探索，那就选择`dtale`。

\- EOF -

推荐阅读  点击标题可跳转

<ins>1、[8000 字，Pandas 必知必会 50 例](http://mp.weixin.qq.com/s?__biz=MzA5ODM5MDU3MA==&mid=2650877876&idx=2&sn=7dfc677948b5ed0b7822d296cd2c451e&chksm=8b67c8f1bc1041e745bee78f96ad436b6552fd8e06500cb8c8fb3e8fc21b18709f93ebe1a585&scene=21#wechat_redirect)</ins>

<ins>2、[安利 3 个 pandas 数据探索分析神器！](http://mp.weixin.qq.com/s?__biz=MzA5ODM5MDU3MA==&mid=2650875999&idx=2&sn=3a975505aa0f912d063a0a2cddc30d43&chksm=8b67cf1abc10460cdc9e812db9bf840fc8666813ea427404a8666d2b7662b7c97cfb490a3e86&scene=21#wechat_redirect)</ins>

<ins>3、[吹爆这个 pandas GUI 神器，自动转代码！](http://mp.weixin.qq.com/s?__biz=MzA5ODM5MDU3MA==&mid=2650875939&idx=2&sn=7fb6e503f956dea7cf77dd69cd81780d&chksm=8b67cf66bc104670d72d6732e99bfed63121376405cdb1a415045fd6816a6ad7ac43824cae20&scene=21#wechat_redirect)</ins>

看完本文有收获？请转发分享给更多人

**推荐关注「数据分析与开发」，提升数据技能**

![](../../../_resources/0_wx_fmt_png_95987cd50a0348d99c607b7c33c50311.png)

**数据分析与开发**

「数据分析与开发」分享数据分析与开发相关技术文章、教程、工具

<a id="js_profile_article"></a>55篇原创内容

Official Account

点赞和在看就是最大的支持❤️

People who liked this content also liked

这些 Shell 分析服务器日志命令集锦，优秀！

高效运维

不看的原因

- 内容质量低
- 不看此公众号

Linux 性能优化的全景指南，可能都在这里了，建议收藏~

高效运维

不看的原因

- 内容质量低
- 不看此公众号

Apache 架构师总结的 30 条架构原则

架构师

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___e9cfdb11e0904a44a.bmp"/>

Scan to Follow