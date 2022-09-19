# 用Python自动生成Excel数据报表！

<a id="profileBt"></a><a id="js_name"></a>数据分析与开发 *2021-05-20 12:10*

The following article is from 法纳斯特 Author 小F

<a id="copyright_info"></a>[![](../../../_resources/0_bb5ff45564324ab099e5760527750f4e.jpg)<br>**法纳斯特** .<br>分享学习Python爬虫、数据分析、数据挖掘的点滴。](#)

今天带大家来实战一波，使用Python自动化生成数据报表！

从一条条的数据中，创建出一张数据报表，得出你想要的东西，提高效率。

主要使用到pandas、xlwings以及matplotlib这几个库。

先来看一下动态的GIF，都是程序自动生成。

<img width="660" height="403" src="../../../_resources/640_wx_fmt_gif_wxfrom_5_wx_lazy__201d07c8333a4d50b.gif"/>

下面我们就来看看这个案例吧，水果蔬菜销售报表。

原始数据如下，主要有水果蔬菜名称、销售日期、销售数量、平均价格、平均成本、总收入、总成本、总利润等。

<img width="660" height="535" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__341e5f52c2444493b.png"/>

先导入相关库，使用pandas读取原始数据。

```


import pandas as pd
import xlwings as xw
import matplotlib.pyplot as plt
# 对齐数据
pd.set_option('display.unicode.ambiguous_as_wide', True)
pd.set_option('display.unicode.east_asian_width', True)
# 读取数据
df = pd.read_csv(r"fruit_and_veg_sales.csv")
print(df)


```

结果如下。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

一共是有1000行的销售数据。

使用xlwings库创建一个Excel工作簿，在工作簿中创建一个表，表名为fruit\_and\_veg_sales，然后将原始数据复制进去。

```


# 创建原始数据表并复制数据
wb = xw.Book()
sht = wb.sheets["Sheet1"]
sht.name = "fruit_and_veg_sales"
sht.range("A1").options(index=False).value = d



```

关于xlwings库的使用，推荐两个文档地址

中文版：

*https://www.kancloud.cn/gnefnuy/xlwings-docs/1127455*

英文版：

*https://docs.xlwings.org/en/stable/index.html*

推荐使用中文版，可以降低学习难度...

<img width="660" height="392" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__a7d556e355324466b.png"/>

当然关于Excel的VBA操作，也可以看看微软的文档。

<img width="660" height="434" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__4c46d58a3d0e40269.png"/>

地址：

*https://docs.microsoft.com/zh-cn/office/vba/api/overview/excel*

将原始数据取过来后，再在工作簿中创建一个可视化表，即Dashboard表。

```


# 创建表
wb.sheets.add('Dashboard')
sht_dashboard = wb.sheets('Dashboard')



```

现在，我们有了一个包含两个工作表的Excel工作簿。fruit\_and\_veg_sales表有我们的数据，Dashboard表则是空白的。

下面使用pandas来处理数据，生成Dashboard表的数据信息。

DashBoard表的头两个表格，一个是产品的利润表格，一个是产品的销售数量表格。

使用到了pandas的数据透视表函数。

```


# 总利润透视表
pv_total_profit = pd.pivot_table(df, index='类别', values='总利润(美元)', aggfunc='sum')
print(pv_total_profit)
# 销售数量透视表
pv_quantity_sold = pd.pivot_table(df, index='类别', values='销售数量', aggfunc='sum')
print(pv_quantity_sold)



```

得到数据如下。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

稍后会将数据放置到Excel的表中去。

下面对月份进行分组汇总，得出每个月的销售情况。

```


# 查看每列的数据类型
print(df.dtypes)
df["销售日期"] = pd.to_datetime(df["销售日期"])
# 每日的数据情况
gb_date_sold = df.groupby(df["销售日期"].dt.to_period('m')).sum()[["销售数量", '总收入(美元)', '总成本(美元)', "总利润(美元)"]]
gb_date_sold.index = gb_date_sold.index.to_series().astype(str)
print(gb_date_sold)



```

得到结果如下。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

这里先对数据进行了查询，发现日期列为object，是不能进行分组汇总的。

所以使用了pd.to_datetime()对其进行了格式转换，而后根据时间进行分组汇总，得到每个月的数据情况。

最后一个groupby将为Dashboard表提供第四个数据信息。

```


# 总收入前8的日期数据
gb_top_revenue = (df.groupby(df["销售日期"])
    .sum()
    .sort_values('总收入(美元)', ascending=False)
    .head(8)
    )[["销售数量", '总收入(美元)', '总成本(美元)', "总利润(美元)"]]
print(gb_top_revenue)



```

总收入前8的日期，得到结果如下。

<img width="661" height="176" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__43f29b0bf8ca403b9.png"/>

现在我们有了4份数据，可以将其附加到Excel中。

```


# 设置背景颜色, 从A1单元格到Z1000单元格的矩形区域
sht_dashboard.range('A1:Z1000').color = (198, 224, 180)
# A、B列的列宽
sht_dashboard.range('A:B').column_width = 2.22
print(sht_dashboard.range('B2').api.font_object.properties.get())
# B2单元格, 文字内容、字体、字号、粗体、颜色、行高(主标题)
sht_dashboard.range('B2').value = '销售数据报表'
sht_dashboard.range('B2').api.font_object.name.set('黑体')
sht_dashboard.range('B2').api.font_object.font_size.set(48)
sht_dashboard.range('B2').api.font_object.bold.set(True)
sht_dashboard.range('B2').api.font_object.color.set([0, 0, 0])
sht_dashboard.range('B2').row_height = 61.2
# B2单元格到W2单元格的矩形区域, 下边框的粗细及颜色
sht_dashboard.range('B2:W2').api.get_border(which_border=9).weight.set(4)
sht_dashboard.range('B2:W2').api.get_border(which_border=9).color.set([0, 176, 80])
# 不同产品总的收益情况图表名称、字体、字号、粗体、颜色(副标题)
sht_dashboard.range('M2').value = '每种产品的收益情况'
sht_dashboard.range('M2').api.font_object.name.set('黑体')
sht_dashboard.range('M2').api.font_object.font_size.set(20)
sht_dashboard.range('M2').api.font_object.bold.set(True)
sht_dashboard.range('M2').api.font_object.color.set([0, 0, 0])
# 主标题和副标题的分割线, 粗细、颜色、线型
sht_dashboard.range('L2').api.get_border(which_border=7).weight.set(3)
sht_dashboard.range('L2').api.get_border(which_border=7).color.set([0, 176, 80])
sht_dashboard.range('L2').api.get_border(which_border=7).line_style.set(-4115)


```

先配置一些基本内容，比如文字，颜色背景，边框线等，如下图。

<img width="660" height="370" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__cbfab4b99f4146c08.png"/>

使用函数，批量生成四个表格的格式。

```


# 表格生成函数.
def create_formatted_summary(header_cell, title, df_summary, color):
    """
    Parameters
    ----------
    header_cell : Str
        左上角单元格位置, 放置数据
    title : Str
        当前表格的标题
    df_summary : DataFrame
        表格的数据
    color : Str
        表格填充色
    """
    # 可选择的表格填充色
    colors = {"purple": [(112, 48, 160), (161, 98, 208)],
              "blue": [(0, 112, 192), (155, 194, 230)],
              "green": [(0, 176, 80), (169, 208, 142)],
              "yellow": [(255, 192, 0), (255, 217, 102)]}
    # 设置表格标题的列宽
    sht_dashboard.range(header_cell).column_width = 1.5
    # 获取单元格的行列数
    row, col = sht_dashboard.range(header_cell).row, sht_dashboard.range(header_cell).column
    # 设置表格的标题及相关信息, 如：字号、行高、向左居中对齐、颜色、粗体、表格的背景颜色等
    summary_title_range = sht_dashboard.range((row, col))
    summary_title_range.value = title
    summary_title_range.api.font_object.font_size.set(14)
    summary_title_range.row_height = 32.5
    # 垂直对齐方式
    summary_title_range.api.verticalalignment = xw.constants.HAlign.xlHAlignCenter
    summary_title_range.api.font_object.color.set([255, 255, 255])
    summary_title_range.api.font_object.bold.set(True)
    sht_dashboard.range((row, col),
                        (row, col + len(df_summary.columns) + 1)).color = colors[color][0]  # Darker color
    # 设置表格内容、起始单元格、数据填充、字体大小、粗体、颜色填充
    summary_header_range = sht_dashboard.range((row + 1, col + 1))
    summary_header_range.value = df_summary
    summary_header_range = summary_header_range.expand('right')
    summary_header_range.api.font_object.font_size.set(11)
    summary_header_range.api.font_object.bold.set(True)
    sht_dashboard.range((row + 1, col),
                        (row + 1, col + len(df_summary.columns) + 1)).color = colors[color][1]  # Darker color
    sht_dashboard.range((row + 1, col + 1),
                        (row + len(df_summary), col + len(df_summary.columns) + 1)).autofit()
    for num in range(1, len(df_summary) + 2, 2):
        sht_dashboard.range((row + num, col),
                            (row + num, col + len(df_summary.columns) + 1)).color = colors[color][1]
    # 找到表格的最后一行
    last_row = sht_dashboard.range((row + 1, col + 1)).expand('down').last_cell.row
    side_border_range = sht_dashboard.range((row + 1, col), (last_row, col))
    # 给表格左边添加带颜色的边框
    side_border_range.api.get_border(which_border=7).weight.set(3)
    side_border_range.api.get_border(which_border=7).color.set(colors[color][1])
    side_border_range.api.get_border(which_border=7).line_style.set(-4115)
# 生成4个表格
create_formatted_summary('B5', '每种产品的收益情况', pv_total_profit, 'green')
create_formatted_summary('B17', '每种产品的售出情况', pv_quantity_sold, 'purple')
create_formatted_summary('F17', '每月的销售情况', gb_date_sold, 'blue')
create_formatted_summary('F5', '每日总收入排名Top8 ', gb_top_revenue, 'yellow')



```

得到结果如下。

<img width="660" height="486" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__2ca192ccb53e429c9.png"/>

可以看到，一行行的数据经过Python的处理，变为一目了然的表格。

最后再绘制一个matplotlib图表，添加一张logo图片，并保存Excel文件。

```


# 中文显示
plt.rcParams['font.sans-serif']=['Songti SC']
# 使用Matplotlib绘制可视化图表, 饼图
fig, ax = plt.subplots(figsize=(6, 3))
pv_total_profit.plot(color='g', kind='bar', ax=ax)
# 添加图表到Excel
sht_dashboard.pictures.add(fig, name='ItemsChart',
                           left=sht_dashboard.range("M5").left,
                           top=sht_dashboard.range("M5").top,
                           update=True)
# 添加logo到Excel
logo = sht_dashboard.pictures.add(image="pie_logo.png",
                           name='PC_3',
                           left=sht_dashboard.range("J2").left,
                           top=sht_dashboard.range("J2").top+5,
                           update=True)
# 设置logo的大小
logo.width = 54
logo.height = 54
# 保存Excel文件
wb.save(rf"水果蔬菜销售报表.xlsx")



```

此处需设置一下中文显示，否则会显示不了中文，只有一个个方框。

得到最终的水果蔬菜销售报表。

<img width="660" height="419" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__552df3b4eb5b4c5fa.png"/>

本文的示例代码，可以在Mac+Excel2016中运行的，与Windows还是会有一些区别，API函数的调用(pywin32 or appscript)。

比如表格文字的字体设置。

```


# Windows
sht_dashboard.range('B2').api.font.name = '黑体'
# Mac
sht_dashboard.range('B2').api.font_object.name.set('黑体')


```

关于Windows版本的，作者也提供了相关的程序文件，在公众号回复「**excel报表**」，即可获取代码及相关数据。

感兴趣的小伙伴，可以动手尝试一下。无需太多的代码，就能轻松的创建一个Excel报表出来～

\- EOF -

推荐阅读  点击标题可跳转

<ins>1、[推荐两个高效处理 Excel 的 Python 开源库](http://mp.weixin.qq.com/s?__biz=MzA5ODM5MDU3MA==&mid=2650871885&idx=1&sn=16d49356b92481c2b7b84d75ddddf3f8&chksm=8b67ff08bc10761e6d07b91d30ef2d7cb78f5f077214509f0aaaee5e1e274a2a52b7f20586af&scene=21#wechat_redirect)</ins>

<ins>2、[可能是全网最完整的 Python 操作 Excel 库总结！](http://mp.weixin.qq.com/s?__biz=MzA5ODM5MDU3MA==&mid=2650871420&idx=1&sn=42413126c7d10ec8e165d7382182d5bb&chksm=8b67f139bc10782f7570c1e566843b6ec9f78c4b701ab5fff70f8b929b8809f036bce7497052&scene=21#wechat_redirect)</ins>

<ins>3、[又一个 Jupyter 神器，操作 Excel 自动生成 Python 代码！](http://mp.weixin.qq.com/s?__biz=MzA5ODM5MDU3MA==&mid=2650871873&idx=1&sn=33b3a20f66f48c7cf6ccd06d236c6ec5&chksm=8b67ff04bc107612e094c672e16a255063a358bf5ce31fed07dac866320f4e02914b7ca742c8&scene=21#wechat_redirect)</ins>

看完本文有收获？请转发分享给更多人

**推荐关注「数据分析与开发」，提升数据技能**

![](../../../_resources/0_wx_fmt_png_aac66385ad054fe2a92d0b8a0612793e.png)

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

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___7bfccc335cde470da.bmp"/>

Scan to Follow