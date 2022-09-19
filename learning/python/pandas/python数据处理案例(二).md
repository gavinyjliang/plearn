# python数据处理案例(二)

<a id="copyright_logo"></a>Original <a id="js_author_name"></a>付学威 <a id="profileBt"></a><a id="js_name"></a>大气化学python笔记 *2021-10-05 21:52*

收录于话题

<a id="js_article_tag_name__2070277671715930115"></a>#python <a id="js_article_tag_num__2070277671715930115"></a>29 <a id="js_article_tag_tips__2070277671715930115"></a>个

<a id="js_article_tag_name__2077847786154164225"></a>#excel <a id="js_article_tag_num__2077847786154164225"></a>2 <a id="js_article_tag_tips__2077847786154164225"></a>个

<a id="js_article_tag_name__2073532319717457922"></a>#pandas <a id="js_article_tag_num__2073532319717457922"></a>17 <a id="js_article_tag_tips__2073532319717457922"></a>个

<a id="js_article_tag_name__2077847785751511043"></a>#glob <a id="js_article_tag_num__2077847785751511043"></a>2 <a id="js_article_tag_tips__2077847785751511043"></a>个

<a id="js_article_tag_name__2111129921791000576"></a>#数据分析案例 <a id="js_article_tag_num__2111129921791000576"></a>13 <a id="js_article_tag_tips__2111129921791000576"></a>个

**“** 用python帮朋友处理科研数据的案例——glob+pandas对多个excel文件数据提取及合并**”**

01

—

**需求分析**

<img width="520" height="1430" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_547d8a150543465d9.jpg"/>

朋友需要处理的是某种大气气溶胶SOA模型输出结果的数据，据朋友介绍，模型每次模拟输出的excel文件多达180+(为了缩短代码运行时间，提高效率，本文仅仅截取所有文件中的前56个excel数据作为案例)，需要从每个excel的名称(如：model\_out\_run\_8.1e+08\_293_1.17e+04.xlsx)、和excel文件内容中(193行×5列)提取指定的数据信息，并汇总成单个表格。手动复制粘贴近两百个excel表格数据，将耗费大量的时间和精力，而且还容易出错，所以更好的办法是编写程序，让计算机自动帮我们处理。

总结朋友的需求，主要有以下两点：

1\. 提取每个excel文件名中温度与OM的数据；

2\. 从所有不同温度和OM的excel文件中提取倒数第二行的O2C和yield数据。

02

—

**glob+pandas对多个excel文件数据提取及合并**

实现该需求第一步是运用glob库对所有待处理的excel进行信息提取，获得所有excel名中的温度及OM的数据，第二步是运用pandas库提取汇总不同温度、OM的excel文件中的目标数据。整体的思路是先运用glob库获得所有excel文件名，通过字符串的切片和索引获得各个excel文件的温度和OM数据，然后用pandas构建两个空的以温度和OM为列名和索引的DataFrame数据结构，最后构建循环，根据不同的温度和OM对空的DataFrame进行赋值。

整体代码如下，各块代码含义我均已添加详细备注，供大家参考学习：

```
`import pandas as pd``import glob``files = glob.glob(r'DODECANE/*.xlsx') # 获取DODECANE文件夹下所有待处理的excel路径``OM = [] # 构建用于存储excel文件名中OM和温度T的空列表``OM1 = []``T = []``T1 = []``for file in files: # 逐一对所有excel文件名进行切片提取OM和温度T的数据信息，分别储存在OM和T列表中` `OM.append(eval(file[-13:-5]))` `T.append(eval(file[-17:-14]))``#去对OM和T中的数据去重，存放在新的OM1和T1中``for m in T:` `if m not in T1:` `T1.append(m)``for n in OM:` `if n not in OM1:` `OM1.append(n)``# 构建两个以OM为索引，温度为列的空DataFrame对象。``df1 = pd.DataFrame(index=OM1,columns=T1)  # O2C``df2 = pd.DataFrame(index=OM1,columns=T1)  # 产率``# 根据不同温度和OM，构建循环对两个空的DataFrame对象进行赋值，df1中存储O2C的数据，df2中存储yield的数据``for file in files:` `for i in df1.index:` `for j in df1.columns:` `if (eval(file[-17:-14])==j) and (eval(file[-13:-5])==i):` `data_temp = pd.read_excel(file)` `df1.loc[[i],[j]] = data_temp.iloc[-2,-2]` `df2.loc[[i],[j]] = data_temp.iloc[-2,-1]``# 将O2C的数据及yield的数据保存为excel文件。``df1.to_excel('finally_O2C.xlsx')``df2.to_excel('finally_Yiled.xlsx')`
```

最终经过上述python程序整理好的数据结果如下表所示，达成需求。

1\. 不同温度(横轴)和OM(纵轴)条件下O2C的数据结果：

![Image](../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_e23b53b9d2644f6d9.jpg)

2\. 不同温度(横轴)和OM(纵轴)条件下yield的数据结果：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

以上便是本文全部内容，本案例的数据及代码点击文末的阅读原文即可获取。希望本文能对读者今后处理类似数据提供思路，最后请大家多多点赞支持，扫描下方二维码关注我，我后续将会分享更多python的实用案例！有问题可在公众号聊天窗口留言，我会及时回复。

<img width="558" height="314" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_db7c2767841f48259.jpg"/>

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

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___2c22b838d7124776a.bmp"/>

Scan to Follow