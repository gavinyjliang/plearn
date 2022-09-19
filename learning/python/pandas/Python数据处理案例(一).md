# Python数据处理案例(一)

<a id="copyright_logo"></a>Original <a id="js_author_name"></a>付学威 <a id="profileBt"></a><a id="js_name"></a>大气化学python笔记 *2021-10-03 11:00*

收录于话题

<a id="js_article_tag_name__2070277671715930115"></a>#python <a id="js_article_tag_num__2070277671715930115"></a>29 <a id="js_article_tag_tips__2070277671715930115"></a>个

<a id="js_article_tag_name__2073532319717457922"></a>#pandas <a id="js_article_tag_num__2073532319717457922"></a>17 <a id="js_article_tag_tips__2073532319717457922"></a>个

<a id="js_article_tag_name__2073532320086556673"></a>#数据透视 <a id="js_article_tag_num__2073532320086556673"></a>1 <a id="js_article_tag_tips__2073532320086556673"></a>个

<a id="js_article_tag_name__2073532320120111104"></a>#resample <a id="js_article_tag_num__2073532320120111104"></a>2 <a id="js_article_tag_tips__2073532320120111104"></a>个

<a id="js_article_tag_name__2111129921791000576"></a>#数据分析案例 <a id="js_article_tag_num__2111129921791000576"></a>13 <a id="js_article_tag_tips__2111129921791000576"></a>个

**“** 分享用python帮朋友处理科研数据的案例——pandas数据透视+重采样**”**

01

—

**需求分析**

<img width="613" height="1395" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_ac6aa6a233e049c8b.jpg"/>

朋友的原始数据格式如下图所示，每天的每个小时，都有PM2.5、SO2、O3、和CO等五项空气污染物的浓度数据。

![Image](../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_79176e461ba6474c8.jpg)

朋友的需求是求每个污染物浓度数据的日平均(24h均值)，得到处理好的目标数据结果应如下表所示：

![Image](../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_b79f0933f8594e2d8.jpg)

总结需求，相当于是要对原始数据进行数据透视，然后对得到的1h时间分辨率的PM2.5、SO2、O3、和CO数据求24h平均。

02

—

**Pandas数据透视+重采样**

Pandas库提供了pivot\_table()函数用于数据透视。pivot\_table()函数中参数分别为原始数据data，以及原始数据中用于做数据透视索引的列\['date'\]，以及用于数据透视的五类污染物的数据列\['type'\]。

```
`import pandas as pd``data = pd.read_excel('南京20191111-20191112.xlsx')``df = pd.pivot(data,index='date',columns='type')``df.columns=['CO','NO2','O3','PM2.5','SO2']``print(df)`
```

原始数据经过上述数据透视后，数据结构变为：

![Image](../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_9757e1437e114f129.jpg)

接下来是对1h时间分辨率的数据求24h平均，使用的是pandas中的resample()函数，函数中参数'D'表示每日时间尺度，mean()为求平均，所以df.resample('D').mean()表示对原始数据df求日平均(24h平均)。这里需要注意的是，使用resample()函数的前提是数据中的时间序列能被pandas识别，如本文数据中的date列能直接被pandas识别，若不能被识别，则需要使用代码df\['date'\] = pd.to_datetime(df\['date'\])，将其变为pandas可识别的时间序列。

```
`# 时间重采样，求24h平均``data_finally = df.resample('D').mean()``data_finally.to_excel('24h平均.xlsx')`
```

运行完上述程序，将最终数据保存为如下结果的excel表格，完成需求：

<img width="657" height="58" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_a3831421c83144ad8.jpg"/>

本案例的数据及代码点击文末阅读原文即可获得。希望能对读者今后处理类似数据提供思路，最后请大家多多点赞支持! 有问题可在公众号聊天窗口留言，我会及时回复。

<img width="558" height="314" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_77aadb5df4c54a2ca.jpg"/>

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

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___fa4588ff87a5421a9.bmp"/>

Scan to Follow