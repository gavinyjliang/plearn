# Python数据处理案例(三)

<a id="copyright_logo"></a>Original <a id="js_author_name"></a>付学威 <a id="profileBt"></a><a id="js_name"></a>大气化学python笔记 *2021-10-09 12:00*

收录于话题

<a id="js_article_tag_name__2070277671715930115"></a>#python <a id="js_article_tag_num__2070277671715930115"></a>29 <a id="js_article_tag_tips__2070277671715930115"></a>个

<a id="js_article_tag_name__2073532319717457922"></a>#pandas <a id="js_article_tag_num__2073532319717457922"></a>17 <a id="js_article_tag_tips__2073532319717457922"></a>个

<a id="js_article_tag_name__2073532320120111104"></a>#resample <a id="js_article_tag_num__2073532320120111104"></a>2 <a id="js_article_tag_tips__2073532320120111104"></a>个

<a id="js_article_tag_name__2082367141265080322"></a>#数据分析 <a id="js_article_tag_num__2082367141265080322"></a>14 <a id="js_article_tag_tips__2082367141265080322"></a>个

<a id="js_article_tag_name__2111129921791000576"></a>#数据分析案例 <a id="js_article_tag_num__2111129921791000576"></a>13 <a id="js_article_tag_tips__2111129921791000576"></a>个

**“** 用python处理科研数据的案例——pandas时间序列数据重采样**”**

重采样：指的是将时间序列从一个频率转化为另一个频率进行处理的过程，将高频率数据转化为低频率数据为降采样，低频率转化为高频率为升采样。

pandas提供了一个resample的方法来帮助我们实现频率转化。如下所示生成一个时间序列数据，从2021/01/01往后共100天，用numpy的random函数生成100个随机数与之一一对应：

```
`import pandas as pd``import numpy as np``t = pd.DataFrame(np.random.uniform(10,50,(100,1)),index=pd.date_range('20210101',periods=100))``t.columns =['random']``print(t)`
```

<img width="545" height="372" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__80d31bef670247adb.png"/>

resample()函数最常用的用法如下：

- t.resample('time').mean() 表示求time时间尺度的所有数据对应的均值；
    
- t.resample('time').sum() 表示求time时间尺度的所有数据对应的和；
    
- t.resample('time').std() 表示求time时间尺度的所有数据对应的标准偏差。
    

'time'是resample()函数中rule的输入，规定了重采样的偏移量(可以理解为重采样的时间尺度)，其包括: 整数n+D  / H /(T or min)/ S / M / Q)，分别代表着n天，n小时，n分钟，n秒，n月，n季度的时间尺度。

如下所展示代码为求10天数据总和，20天数据的均值，月数据的均值：

```
`print(t.resample('10D').sum())``print('--'*20)``print(t.resample('20D').mean())``print('--'*20)``print(t.resample('M').mean())`
```

<img width="657" height="516" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__625bd73c03a14db2b.png"/>

01

—

**重采样报错**

我的实验数据(4437行×2列)如下表所示。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

我的需求为将实验数据改为1min的均值：

重采样代码如下：

```
`import pandas as pd``data = pd.read_excel('cpc.xlsx')``# 当data['time']不能被转换为pd.datatime()时，会报错<class 'datetime.time'> is not convertible to datetime``# 解决办法，将excel中的时间列选中，数据-分列-下一步-下一步-点选‘文本’-完成``data['time'] = pd.to_datetime(data['time'])``data.set_index('time',inplace=True)``df = data.resample('1min').mean()``df.to_excel('cpc1min.xlsx')`
```

遇到如下报错：

```
TypeError: <class ‘datetime.time’> is not convertible to datetime
```

报错的大概意思就是无法将excel中的时间序列转换为pandas可以识别的时间序列，从而不能使用resample()进行重采样。

02

—

**报错解决方法**

将excel中时间列转换为文本格式，再次导入pandas中进行识别，详细步骤如下：

1.在Excel里选取需要转换的时间列，先点击数据，然后选择分列；

<img width="657" height="454" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__902dabf8dee84cd2a.png"/>

2.分隔符号选Tab键，然后点击下一步。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

3.列数据格式选择文本格式，然后点击完成，重新运行代码无报错。

<img width="657" height="357" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__ee841577e71e4a419.png"/>

最终获得的1min均值结果如下：

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__211d92d5e4ef4faaa.png)

以上便是本文全部内容，本案例的数据及代码点击文末的阅读原文即可获取。希望本文能对读者今后处理类似数据提供思路，最后请大家多多点赞支持，扫描下方二维码关注我，我后续将会分享更多python的实用案例！有问题可在公众号聊天窗口留言，我会及时回复。

<img width="558" height="314" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_7580f52e0c7f484db.jpg"/>

<a id="js_view_source"></a>Read more

People who liked this content also liked

Python提取并整理txt文档中目标数据

大气化学python笔记

不看的原因

- 内容质量低
- 不看此公众号

python数据可视化--matplotlib绘制直方图

AI小白笔记

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___941aa0d67fdb4054a.bmp"/>

Scan to Follow