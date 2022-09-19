# 再见 VBA！神器工具统一 Excel 和 Python

<a id="profileBt"></a><a id="js_name"></a>数据分析与开发 *2021-01-20 11:45*

The following article is from Python数据科学 Author 东哥起飞

<a id="copyright_info"></a>[![](../../../_resources/0_48211ec9d108474b915cfec9190bb452.jpg)<br>**Python数据科学** .<br>以Python为核心语言，专攻于「数据科学」领域，文章涵盖数据分析，数据挖掘，机器学习等干货内容，分享大量数据挖掘实战项目分析和讲解，以及海量的学习资源。](#)

<img width="558" height="293" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_f61230e8ab23415db.jpg"/>

`Excel`和`Jupyter Notebok`都是我每天必用的工具，而且两个工具经常协同工作，一直以来工作效率也还算不错。但说实在，毕竟是两个工具，使用的时候肯定会有一些切换的成本。

最近，在逛GitHub突然发现了一款神器「PyXLL-Jupyter」，它可以完美将`Jupyter Notebook`嵌入到Excel中！是的，你没听错，使用它我们就可在`Excel`中运行`Jupyter Notebook`，调用Python函数，实现数据共享。

## 一、安装

首先，想要在Excel中运行Python代码，需要安装`PyXLL`插件。`PyXLL`可以将Python集成到Excel中，用`Python`替代`VBA`。

先用 pip 安装 `PyXLL`。

```
pip install pyxll

```

然后再用`PyXLL`独特的命令行工具安装Excel插件。

```
>> pyxll install

```

安装好了`PyXLL`在 Excel中的插件，下一步就是安装`pyxll-jupyter`软件包了。使用pip安装`pyxll-jupyter`软件包：

```
pip install pyxll-jupyter

```

安装完毕后，启动Excel，将在`PyXLL`选项卡中看到一个新的`Jupyter`按钮。

<img width="657" height="323" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__ee421a8f3d2144d4a.png"/>

单击此按钮可在Excel工作簿的侧面板中打开Jupyter Notebook。该面板是Excel界面的一部分，可以通过拖放操作取消停靠或停靠在其他位置。

在Jupyter面板中，你可以选择一个现有的Notebook或创建一个新的Notebook。创建一个新的Notebook，选择新建按钮，然后选择`Python 3`。

<img width="677" height="578" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__a58fa6f37cd749e9a.png"/>

## 二、使用方法

这样做有什么用处呢？

### 1、Excel和Python共享数据

比如，我们要将数据从Excel导入Python。

由于Excel和Python已经在同一进程中运行了，所以在Python中访问Excel数据以及在Python和Excel之间切换非常快。

更牛X的是，`pyxll-jupyter`还单独附带了一些`IPython`魔法函数，输入后一键即可完成同步。

```
%xl_get
```

<img width="655" height="560" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__5dec6cda0abc46e29.png"/>

**将Python中的数据移到Excel，也是同理，非常简单。**

无论是使用Python先加载数据集，再传输到Excel，还是其它形式，从Python复制数据到Excel非常容易。

```
%xl_set

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

当然，`%xl_get`和`%xl_set`都附带参数选项可以自定义导入导出规则。

### 2\. 在Excel中使用Python绘图

`PyXLL`的另一大用处就是它集成了几乎所有主流的可视化包，因此我们可以在Excel中利用这些可视化包随意绘图，包括`matplotlib`、`plotly`、`bokeh`和`altair`等。

```
%xl_plot

```

<img width="677" height="576" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__1a88e402571b4049b.png"/>

同样，使用魔法函数`％xl_plot`在Excel中可以绘制任何的Python图。任何一个受支持的可视化包也可进行绘图然后传递图形对象到Excel中，比如上图中使用pandas的绘图效果就很好。

```
%xl_plot df.plot(kind='scatter')

```

### 3\. 从Excel调用Python函数

使用Excel离不开函数，而当我们需要一些复杂功能时，自带函数未必能满足我们的需求。

通过`PyXLL`，我们可以直接在`Excel`中调用`Python`函数，并对其进行实时测试。这就避免了Excel和Jupyter之间的来回切换成本，有点像dataframe的`apply`用法，写个函数直接与`Excel`完美融合。

函数写好后，还可将其添加到`PyXLL Python`项目中。这样以后每次都可以复用实现相同功能，简直不要太香！

```
from pyxll import xl_func
@xl_func
def test_func(a, b, c):
    return (a * b) + c

```

比如，输入以上代码在`Jupyter`中运行后，Python函数将立即可被Excel工作簿调用。

不只是简单的函数，还可以将整个数据作为`pandas`的`DataFrames`传给函数，并返回任何的Python类型，比如`numpy array`、`DataFrames`，甚至还可以通过给`@xl_func`装饰器一个签名字符串来告诉PyXLL输出什么类型。例如，以下函数：

```
from pyxll import xl_func
# 装饰器签名告诉 PyXLL 如何转换函数参数和返回的值
@xl_func("dataframe df: dataframe<index=True>", auto_resize=True)
def df_describe(df):
    # df 是一个从数据集里创建的 pandas DataFrame 传递给函数
    desc = df.describe()
    # desc 是新的 DataFrame（PyXLL转换为一组值并返回给Excel所创建的）
    return desc

```

现在可以编写复杂的Python函数来进行数据转换和分析，但是可以协调在Excel中如何调用或排序这些函数。更改输入会导致调用函数，并且计算出的输出会实时更新，这与我们期望的一样。

### 4\. 替代VBA

VBA脚本所需的功能函数，在Python中均有相同的API。这对于熟悉Python但不熟悉VBA的同学绝对是个好消息。

官网还给出了和VBA功能一样的API说明文档。

> https://www.pyxll.com/docs/userguide/vba.html

`Jupyter Notebook`在Excel中运行，整个Excel对象都可用，所有操作就像在`VBA`编辑器中编写`Excel`脚本一模一样。

由于`PyXLL`在Excel进程内运行Python ，因此从Python调用Excel不会对性能造成任何影响。当然，也可以从外部Python进程调用Excel，但这通常要慢很多。在Excel中运行`Jupyter Notebook`，一切变得就不一样了！

使用`PyXLL`的`xl_app`函数获取`Excel.Application`对象，该对象等效于`VBA`中的`Application`对象。弄清楚如何使用Excel对象模型进行操作的一种好方法是记录`VBA`宏，然后将该宏转换为`Python`。

下图中尝试将当前选择单元格更改颜色。

<img width="677" height="576" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__07e7f4740e114bc6b.png"/>

## 三、总结

## `PyXLL`将完美融合`Python`和`Excel`，实现了以下功能，为表格数据处理提升一个全新的高度。

- Excel和Python共享数据
    
- 在Excel中使用Python绘图
    
- 从Excel调用Python函数
    
- 替代VBA脚本
    

不得不说这个工具是真的香，喜爱Python的同学可以不用学习VBA了，Python脚本打天下。

\- EOF -

推荐阅读  点击标题可跳转

<ins>1、[2W 字图解 Redis，扫盲必备！](http://mp.weixin.qq.com/s?__biz=MzA5ODM5MDU3MA==&mid=2650870470&idx=1&sn=4dfce4fd6c396ecb6908ad45bce57887&chksm=8b67f583bc107c950d9c762f0394b1fda2b4f962e28fd9bda9e62b56d8dbd452b57f03ba4469&scene=21#wechat_redirect)</ins>

<ins>2、[一文从0到1掌握用户画像知识体系](http://mp.weixin.qq.com/s?__biz=MzA5ODM5MDU3MA==&mid=2650870188&idx=2&sn=6521a4b0d9113e425b2244a451c12e88&chksm=8b67f6e9bc107fff277e77e0a32f3de9b3b2d8ecf0b58a2035c261cc92f84369b9a87ff026ca&scene=21#wechat_redirect)</ins>

<ins>3、[太强了！60 种可视化图表制作工具和使用场景（推荐收藏）](http://mp.weixin.qq.com/s?__biz=MzA5ODM5MDU3MA==&mid=2650870115&idx=1&sn=464e4388d991db711a307367d1293434&chksm=8b67f626bc107f30f8eda9e21a23b5d2953de873e64a30958384f140e555b602ef074b551fc5&scene=21#wechat_redirect)</ins>

看完本文有收获？请转发分享给更多人

**推荐关注「数据分析与开发」，提升数据技能**

![Image](../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_38152eea32d146888.jpg)

点赞和在看就是最大的支持❤️

People who liked this content also liked

我学习 Python 的三个神级网站

数据分析与开发

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___04610376ce9a420b8.bmp"/>

Scan to Follow