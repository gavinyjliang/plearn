# Pandas可视化指南：从零教你绘制数据图表

<a id="profileBt"></a><a id="js_name"></a>DataScience *2021-12-09 08:18*

![Image](../../../_resources/640_wx_fmt_gif_wxfrom_5_wx_lazy__b1d379b68c034cbea.gif)

![](../../../_resources/0_wx_fmt_png_71a39fbc0cf9428a8a2781a8cd0bd15b.png)

**DataScience**

数据科学--\> DataScience，这是一个世界边缘的佛系公号，建于21世纪初，长期维护并保持低频更新。覆盖数据领域技能、经验、实战等。

<a id="js_profile_article"></a>408篇原创内容

Official Account

##### 来源：量子位

数据可视化本来是一个非常复杂的过程，但随着Pandas数据帧plot()函数的出现，使得创建可视化图形变得很容易。

在数据帧上进行操作的plot()函数只是**matplotlib**中plt.plot()函数的一个简单包装 ，可以帮助你在绘图过程中省去那些长长的matplotlib代码。

最近，一位来自印度的小哥以2019年世界幸福指数的数据为例，详细讲述了在Pandas中plot()函数的各种参数设置的小技巧，熟练掌握这些技巧后，你也能绘制出丰富多彩的可视化图表。

## 导入数据

在绘制图形前，我们首先需要导入csv文件：

```
`import pandas as pd``df=pd.read_csv(‘./world-happiness-report-2019.csv’)``df.head(3)`
```

这个csv图标的内容是各个国家按照不同维度评价的幸福指数（数据下载地址见文末）：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

数据帧中一些列的名称比较冗长，可以重命名使其更加简洁：

```
`df.rename(columns={“Country (region)”: “Country”, “Log of GDP\nper capita”: “Log_GDP_per_capita”, “Healthy life\nexpectancy”:”Health_life_expect”},inplace=True)``df.columns`
```

## 绘制柱状图、散点图等常见图形

从最近简单的**柱状图**开始，只统计腐败程度、自由度、宽容度、社会支持等几个维度

```
`%matplotlib tk``df1=df[:5]``df1.plot(‘Country’,[‘Corruption’,’Freedom’,’Generosity’,’Social support’],kind = ‘bar’)`
```

嫌直接写名称太麻烦？没关系，我们也可以用所在列的数字来绘制，比如上述4个列分别为7、6、8、5：

```
`%matplotlib tk``df1=df[:5]``df1.plot(‘Country’,[7,6,8,5],kind = ‘bar’)`
```

<img width="677" height="375" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__17ec86a6c72b4e54a.png"/>

在上面的代码中kind = ‘bar’，所以绘制的图形是柱状图，如果我们把参数改成kind = ‘line’，画出的就是**线状图**。

```
`df1=df[:5]``df1.plot(‘Country’,[‘Corruption’,’Freedom’,’Generosity’,’Social support’],kind = ‘line’)`
```

<img width="677" height="523" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__84635dcabbd94fb28.png"/>

同样的，如果把参数改成kind = ‘line’，还能绘制出箱形图：

```
df[:5].plot(x=’Country’,kind=’box’)
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

对于散点图，设置**kind=’scatter’**，绘制出腐败程度与自由度之间的关系，用color=’R’将点定义为红色：

```
df.plot(x=’Corruption’,y=’Freedom’,kind=’scatter’,color=’R’)
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

此外，Pandas中还有一个辅助函数pandas.plotting.table，它创建一个来自数据帧的表格，并将其添加到matplotlib Axes实例中。

```
`from pandas.plotting import table``df1=df[:5]``df1=df.loc[:5,[‘Country (region)’,’Corruption’,’Freedom’,’Generosity’,’Social support’]]``ax=df1.plot(‘Country (region)’,[‘Corruption’,’Freedom’,’Generosity’,’Social support’], kind = ‘bar’, title =’Bar Plot’,legend=None)``table(ax, np.round(df1.describe(), 2),loc=’upper right’)`
```

<img width="677" height="374" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__1254b3a7c4f94c23a.png"/>

## 坐标轴的设置

### 取值范围

使用xlim和ylim两个参数可设置x和y轴的范围。在折线图中，我们要将x轴设置为0到20，y限制为从0到100。

```
`df1=df[:20]``df1[‘Freedom’].plot(kind=’line’,xlim=(0,20),ylim=(0,100))`
```

<img width="677" height="519" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__58a5224933a4429e9.png"/>

### x、y轴刻度

有时候坐标轴上的刻度并不理想，我们希望在上面标上我们喜欢的数值。

比如对于x轴，我们想要标上0、10、15和20几个值；对于y轴，我们想要标上0、50、70、100几个值，可以在**xticks**和**yticks**参数中悉数列出。

```
df[:20][‘Freedom’].plot(kind=’line’,xlim=(0,20),ylim=(0,100),color=’red’,xticks=([0,10,15,20]),yticks=([0,50,70,100]), title = ‘xticks’)
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

但是用列表来制定坐标刻度的方法，在数值太多的时候就比较麻烦了，因此我们还能通过指定刻度间隔的方法来绘制坐标轴，比如指定x轴间隔是1，y轴间隔是10：

```
df[:20][‘Freedom’].plot(kind=’line’,xlim=(0,20),ylim=(0,100),color=’red’,xticks=([w1 for w in range(20)]),yticks=([w10 for w in range(40)]))
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

如果我们不希望在坐标轴上看到数字，而是想要设置标签。我们还可以将x轴标签更改为文本标签“低、中、高”这种样式。

```
`ax=df[:20][‘Freedom’].plot(kind=’line’,xlim=(0,20),ylim=(0,100),color=’red’,xticks=([0,10,20]),yticks=([w*30 for w in range(40)]))``ax.set_xticklabels([‘Low’,’Med’,’High’])`
```

<img width="677" height="504" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__ba6606edcec9439b9.png"/>

### 对数坐标

如果数据的跨度范围非常大，横跨好几个数量级，那么用线性坐标就无法很好地展示数据。这时候我们需要用到对数坐标，设置方法是将**logx**或者**logy**的值设置为**Ture**。

如果我们只想设置x轴为对数坐标，y轴仍保持线性坐标，那么

```
df[:20][‘Freedom’].plot(kind=’line’,xlim=(0,1000),ylim=(0,100),color=’red’,logx=True)
```

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__0d5269aa376d4902b.png)

## 其他高阶用法

可以使用**stacked**参数来绘制带有条形图的**堆叠图**。在这里，我们绘制堆叠的水平条，stacked设置为True。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

将grid参数设置为True，可以给图表加入网格。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

有了subplot参数还可以绘制子图，根据需要指定行数和列数以及绘图的数量。

<img width="677" height="346" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__7edf4002214541348.jpg"/>

在上面的子图中，我们没有给子图添加标题。当subplot 设置为True 时，在设置一组title的值，即可在列表上方加入标题。

表格下载地址：
https://www.kaggle.com/PromptCloudHQ/world-happiness-report-2019/version/1

\- THE END-

关注+星标，不迷路。分享干货

免费学知识+0元领新书▼

<img width="243" height="432" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_a5f636383d5d43e09.jpg"/>

请长按扫码加小编，回复：数据可视化

**进群一起学习交流吧**
<img width="151" height="146" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_37b99e5d947b4bca8.jpg"/>

▲长按扫

**-今日互动-**

你get到了吗？欢迎文章下方留言互动<img width="20" height="20" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__f7cfc362519d4802b.png"/>

如果感觉对你有帮助的话

来个「转发朋友圈」「在看」，是对我们最大的支持！

People who liked this content also liked

详解 seaborn，快速实现统计数据可视化

大邓和他的Python

不看的原因

- 内容质量低
- 不看此公众号

使用SPSS进行元回归分析：有操作步骤哦！！！

元分析

不看的原因

- 内容质量低
- 不看此公众号

详解数据分析最常用的14个Excel函数

爱数据LoveData

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___df607179eba149d89.bmp"/>

Scan to Follow