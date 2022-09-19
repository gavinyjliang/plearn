# pandas|数据清洗(处理缺失值及异常值）

<a id="copyright_logo"></a>Original <a id="js_author_name"></a>付学威 <a id="profileBt"></a><a id="js_name"></a>大气化学python笔记 *2021-11-17 12:00*

收录于话题

<a id="js_article_tag_name__2138722879104942086"></a>#数据清洗 <a id="js_article_tag_num__2138722879104942086"></a>1 <a id="js_article_tag_tips__2138722879104942086"></a>个

<a id="js_article_tag_name__2073532319717457922"></a>#pandas <a id="js_article_tag_num__2073532319717457922"></a>17 <a id="js_article_tag_tips__2073532319717457922"></a>个

<a id="js_article_tag_name__2137135026105286658"></a>#数据处理 <a id="js_article_tag_num__2137135026105286658"></a>2 <a id="js_article_tag_tips__2137135026105286658"></a>个

<a id="js_article_tag_name__2101124600976703488"></a>#pandas学习笔记 <a id="js_article_tag_num__2101124600976703488"></a>6 <a id="js_article_tag_tips__2101124600976703488"></a>个

<a id="js_article_tag_name__2070277671715930115"></a>#python <a id="js_article_tag_num__2070277671715930115"></a>29 <a id="js_article_tag_tips__2070277671715930115"></a>个

<img width="613" height="204" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__177253a91ad048309.png"/>

**Python数据分析系列之**

**Panda学习笔记(五)**

——数据清洗(处理缺失值及异常值)

本文将介绍如何在pandas的二维数据结构DataFrame中查看与处理缺失值、重复值及异常值等。数据清洗是在进行数据分析前的必备工作，处理缺失值、重复值及异常值，能降低它们给数据分析带来的不利影响，有益于更好的挖掘数据中包含的真实信息。

**目录：**

1.  检测与处理缺失值
    
    1.1 查看数据概况
    
    1.2 判断数据是否存在缺失值
    
    1.3处理缺失值
    
       1.3.1 删除缺失值
    
       1.3.2 填充缺失值
    
2.  处理重复值
    
3.  异常值检测与处理
    

<img width="36" height="36" src="../../../_resources/640_wx_fmt_gif_wxfrom_5_wx_lazy__e21f6f5988c34d97a.gif"/>

**01**

**检测与处理缺失值**

**1.1**

**查看数据概况**

首先以学生成绩为例，构建一个8行3列的DataFrame对象：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

其中 小赵1 和 小李1 是插入的两行重复数据，分别和小赵、小李两行的数据内容相同。小赵2 和 小赵3 是包含缺失值的数据行。

**1.2**

**判断数据是否存在缺失值**

使用DataFrame中的info()函数，查看数据信息：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

从输出信息可以看出，该数据是一个8行3列的DataFrame，3个列分别为语文、数学和英语。语文列有7个非空值1个缺失值，数学列有6个非空值2个缺失值，英语列有7个非空值1个缺失值，数据类型均为浮点型。

使用isnull()和notnull()函数，可获得data中数据是否为缺失值的布尔索引，data.isnull()中缺失值返回True，而data.notnull()中缺失值返回False：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

**1.3**

**处理缺失值**

**1.3.1 删除缺失值**

删除缺失值主要使用dropna()函数，dropna()中how的参数默认为any，表示只要存在缺失值就删除整行：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

当how='all'，表示只删除整行为缺失值的行：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

dropna()中aixs参数默认为0，其表示按行删除数据，即删除含有缺失值的行。当传入axis=1时，则表示按列删除数据，即删除含有缺失值的列：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

当我们实际处理数据时，可能只关注某一列数据是否有缺失值，并想把有缺失的行单独取出来，则可使用布尔索引条件判断的方法，取出数学成绩有缺失值和无缺失值的行：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

**1.3.2 填充缺失值：**

将缺失值填充为0：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

向上填充缺失值，即使用上一行的值填充缺失值:

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

向下填充缺失值，即使用后一行的值填充缺失值：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

使用每列数据的均值填充缺失值：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

使用每列数据的中位数填充缺失值：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

**02**

**处理重复值**

使用drop_duplicates()去除data中全部重复的数据：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

去除指定列重复的数据，如下去除了data\['语文'\]该列中的重复数据：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

**03**

**异常值检测与处理**

在数据分析中，异常值是指超出或低于正常范围的值。可以通过给定的数据范围进行判断，不在范围内的数据视为异常值；也可以根据箱线图来判断，在箱线图中任何高于上限或者低于下限的数据都可以视为异常值。

这里为了用于演示，我们规定学生的成绩为100分制，插入一行小王的成绩，其中-10和120不在范围内，属于异常值。可以使用以下条件判断方法，判断数据是否在指定范围内，不在范围内视为异常值，自动赋值为NaN：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

也可以用箱线图的方法判断异常值数据，其中标红的点高于上限值，为异常值：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

绘制上述箱线图详细的代码及注释如下：

```
`import matplotlib.pyplot as plt
import pandas as pd
df=pd.read_excel('tips.xlsx')
plt.boxplot(x = df['总消费'],
            whis = 1.5, # 指定1.5倍的四分位差
            widths = 0.3, #指定箱线图中箱子的宽度为0.3
            patch_artist = True, #填充箱子颜色
            showmeans = True, #显示均值
            boxprops = {'facecolor':'RoyalBlue'}, # 指定箱子的填充色为宝蓝色
            flierprops = {'markerfacecolor':'red', 'markeredgecolor':'red', 'markersize':3}, # 指定异常值的填充色、边框色和大小
            meanprops = {'marker':'h','markerfacecolor':'black', 'markersize':8},# 指定均值点的标记符号（六边形）、填充色和大小
            medianprops = {'linestyle':'--','color':'orange'}, # 指定中位数的标记符号（虚线）和颜色
            labels = ['']) # 去除x轴刻度值 # 指定绘制箱线图的数据  
plt.show()
# 计算下四分位数和上四分位
Q1 = df['总消费'].quantile(q = 0.25)
Q3 = df['总消费'].quantile(q = 0.75)
# 基于1.5倍的四分位差计算上下限对应的值
low_limit = Q1 - 1.5*(Q3 - Q1)
up_limit = Q3 + 1.5*(Q3 - Q1)
# 查找异常值
val=df['总消费'][(df['总消费'] > up_limit) | (df['总消费'] < low_limit)]
# 或者 val=df[(df['总消费'] > up_limit) | (df['总消费'] < low_limit)]['总消费'] 也可以
print('异常值如下：')
print(val)
`
```

运行后还会打印输出异常值及其索引位置：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

总结

本文主要介绍了如何使用pandas清洗数据的方法，包括了对缺失值、重复值和异常值的检测与处理，若需要本文演示所用的文件和数据，可在公众号对话框点击**与我联系**，备注来意即可。后续《pandas学习笔记》系列将继续更新如何对清洗好的数据进行索引设置、排序和排名等等操作。请大家多多分享、点赞、点击看一看支持，谢谢！

1

**END**

1

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

**球分享**

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

**球点赞**

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

**球在看**

People who liked this content also liked

使用pandas实现Excel中vlookup功能

大气化学python笔记

不看的原因

- 内容质量低
- 不看此公众号

Pandas | 设置数据索引

大气化学python笔记

不看的原因

- 内容质量低
- 不看此公众号

一文精通mysql的索引优化

程序JAVA圈

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___3e110be061614c15b.bmp"/>

Scan to Follow