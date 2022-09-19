# python数据处理案例(五)

<a id="copyright_logo"></a>Original <a id="js_author_name"></a>付学威 <a id="profileBt"></a><a id="js_name"></a>大气化学python笔记 *2021-10-14 13:31*

收录于话题

<a id="js_article_tag_name__2070277671715930115"></a>#python <a id="js_article_tag_num__2070277671715930115"></a>29 <a id="js_article_tag_tips__2070277671715930115"></a>个

<a id="js_article_tag_name__2082367141265080322"></a>#数据分析 <a id="js_article_tag_num__2082367141265080322"></a>14 <a id="js_article_tag_tips__2082367141265080322"></a>个

<a id="js_article_tag_name__2090389542351470593"></a>#matplotlib <a id="js_article_tag_num__2090389542351470593"></a>2 <a id="js_article_tag_tips__2090389542351470593"></a>个

<a id="js_article_tag_name__2073532319717457922"></a>#pandas <a id="js_article_tag_num__2073532319717457922"></a>17 <a id="js_article_tag_tips__2073532319717457922"></a>个

<a id="js_article_tag_name__2111129921791000576"></a>#数据分析案例 <a id="js_article_tag_num__2111129921791000576"></a>13 <a id="js_article_tag_tips__2111129921791000576"></a>个

**“** 用python处理科研数据的案例——运用卷积函数(Numpy.convolve)求滑动平均。**”**

在处理连续性分布的数据时，数据波动是一个很常见的现象，如何降低这种波动是我们所关心的问题。滑动平均就是这样一种技术，其本质是借助历史记录来创造可以替代原始数据的数据。滑动平均(moving average)相当于低通滤波，其选定某一尺寸的窗口，将窗口内的所有数据值做算术平均，将所求的平均值作为窗口中心点的数据值，从而达到降低数据波动的目的。Python强大的数组计算、矩阵运算和科学计算的核心库numpy，提供了卷积函数convolve()用于快速地实现对数据求滑动平均。本文将介绍如何使用numpy.convolve()卷积函数，求多列数据的滑动平均，并运用matplotlib库绘图，对求滑动平均前后的数据进行可视化。

01

—

**数据组成**

本文所演示数据是某在线仪器测得的连续型数据(213行×3列)，时间分辨率为1min。数据组成如下表所示，a，b，c三列为待求滑动平均的原始数据列：

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__5e76ae5099114b34a.png)

02

—

**原始数据可视化**

对原始数据a,b,c三列数据进行可视化：

```
`import pandas as pd``import matplotlib.pyplot as plt``df = pd.read_excel('滑动平均案例数据.xlsx')``print(df) # 读入数据并打印输出数据结构``# 提取用于绘图的x，y数据``x = range(len(df['a']))``y1 = df['a']``y2 = df['b']``y3 = df['c']``# 绘制折线图``plt.plot(x,y1,label='a')``plt.plot(x,y2,label='b')``plt.plot(x,y3,label='c')``# 设置x，y轴的名称``plt.xlabel('x')``plt.ylabel('Before moving average')``plt.legend(frameon=False) # 添加图例，并去掉图例外框``plt.savefig('1.滑动平均前.png') # 保存图``plt.show() # 显示图`
```

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__2d055a3e9ac545f9a.png)

03

—

**对原始滑动平均**

对数据求10min的滑动平均，保存为新的excel：

```
`import pandas as pd``import numpy as np``df = pd.read_excel('滑动平均案例数据.xlsx')``print(df) # 读入数据并打印输出数据结构``df_ma = pd.DataFrame() # 定义一个用于存放求滑动平均后数据的DataFrame``a = np.repeat(1/10,10) # 构造一个等权重的卷积核，规定滑动平均的长度为5，每个点的权重相等，为1/10``for i in df.columns:` `data_temp = np.convolve(df[i],a,mode='valid') # 卷积运算(滑动平均)。如果要生成和原始数据等长的数据，则mode = 'same',但会存在边际效应(首端数据不正常)。` `data_temp = pd.Series(data_temp)` `df_ma = pd.concat([df_ma,data_temp],axis=1)``df_ma.columns = ['a_move_mean','b_move_mean','c_move_mean']``print(df_ma)``df_ma.to_excel('滑动平均案例数据(10min滑动平均后).xlsx') # 将5min滑动平均结果保存为excel`
```

求滑动平均后，数据结果如下：

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__eb5dd8f46df8493e9.png)

04

—

**数据滑动平均后可视化**

对原始数据的10min的滑动平均值进行可视化：

```
`import pandas as pd``import matplotlib.pyplot as plt``df = pd.read_excel('滑动平均案例数据(5min滑动平均后).xlsx')``print(df) # 读入数据并打印输出数据结构``# 提取用于绘图的x，y数据``x = range(len(df['a_move_mean']))``y1 = df['a_move_mean']``y2 = df['b_move_mean']``y3 = df['c_move_mean']``# 绘制折线图``plt.plot(x,y1,label='a_move_mean')``plt.plot(x,y2,label='b_move_mean')``plt.plot(x,y3,label='c_move_mean')``# 设置x，y轴的名称``plt.xlabel('x')``plt.ylabel('After moving average')``plt.legend(frameon=False) # 添加图例，并去掉图例外框``plt.savefig('2.滑动平均后.png') # 保存图``plt.show() # 显示图`
```

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__e8b3a4e69dc94478b.png)

对比原始数据滑动平均前后的折线图，可以看出原始数据的起伏波动，经过滑动平均处理之后变得平滑，能更好的显现出数据的变化趋势。

以上便是本文全部内容，本案例的示例数据及代码点击文末的阅读原文即可获取。希望本文能对读者今后处理类似数据提供思路，最后请大家多多点赞支持，扫描下方二维码关注我，我后续将会分享更多python的实用案例！有问题可在公众号聊天窗口留言，我会及时回复。

<img width="558" height="314" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_0b07cc486c954e719.jpg"/>

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

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___621548f65c2048249.bmp"/>

Scan to Follow