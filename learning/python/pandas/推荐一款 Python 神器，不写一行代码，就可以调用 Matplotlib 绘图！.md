# 推荐一款 Python 神器，不写一行代码，就可以调用 Matplotlib 绘图！

<a id="profileBt"></a><a id="js_name"></a>数据分析与开发 *2022-03-27 12:10*

The following article is from 数据分析与统计学之美 Author 黄伟呢

<a id="copyright_info"></a>[![](../../../_resources/0_fd733d2312f247a69bce81b606a39f1d.jpg)<br>**数据分析与统计学之美** .<br>免费领10w字"Python知识手册"，共400页，后台回复“十万”领取！](#)

## sviewgui介绍

sviewgui是一个基于 PyQt 的 GUI，用于 csv 文件或 Pandas 的 DataFrame 的数据可视化。此 GUI 基于 matplotlib，您可以通过多种方式可视化您的 csv 文件。主要特点：

- Ⅰ 散点图、线图、密度图、直方图和箱线图类型；
    
- Ⅱ 标记大小、线宽、直方图的 bin 数量、颜色图的设置（来自 cmocean）；
    
- Ⅲ 将图另存为可编辑的 PDF；
    
- Ⅳ 绘制图形的代码可用，以便它可以在 sviewgui 之外重用和修改；
    

> 项目地址：https://github.com/SojiroFukuda/sview-gui

这个包用法超级简单，它只有一种方法：buildGUI()。此方法可以传入零个或一个参数。您可以使用 csv 文件的文件路径作为参数，或者使用 pandas 的DataFrame对象作为参数。类似代码写法如下：

```


# 第一种形式
import sviewgui.sview as sv
sv.buildGUI()
# 第二种形式
import sviewgui.sview as sv
FILE_PATH = "User/Documents/yourdata.csv"
sv.buildGUI(FILE_PATH)
# 第三种形式
import sviewgui.sview as sv
import pandas as pd
FILE_PATH = "User/Documents/yourdata.csv"
df = pd.read_csv(FILE_PATH)
sv.buildGUI(df)


```

上面代码，只是帮助驱动打开这个GuI可视化界面。

最后强调一点，由于这个库是基于matplotlib可视化的，因此seaborn风格同样适用于这里，因为seaborn也是基于matplotlib可视化的。

## sviewgui安装

这个库的依赖库相当多，因此大家直接采用下面这行代码安装sviewgui库。

```


pip install sviewgui -i https://pypi.tuna.tsinghua.edu.cn/simple/ --ignore-installed


```

后面这个`--ignore-installed`，我最开始是没加的，但是报错了，大致错误如下：

```


ERROR: Cannot uninstall 'certifi'. It is a distutils installed project and thus we cannot 
accurately determine which files belong to it which would lead to only a partial uninstall.


```

直到加这个就行，不用管为什么，因为我也不知道！

## sviewgui使用

上面我为大家介绍了3种打开GUI图形界面窗口的代码，这里仅介绍下面这种方法：

```


import sviewgui.sview as sv
sv.buildGUI()


```

截图如下：

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__b7e393012117475eb.png)

当你在命令行输入上述代码后，会驱动后台打开这个图形化界面窗口，初始化状态大致是这样的：

<img width="677" height="592" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__47ecef5be5d449f68.png"/>

点击上述select，可以选择数据源：

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__c8bbd2fd7b2f4eef9.png)

然后我们可以点击左侧`菜单栏`，生成对应的图形。但是有一点，貌似不支持中文！！！

<img width="677" height="595" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__5b0b40eed7f641978.png"/>

如果你觉得这里不足以完善你想要的图形，可以复制图形所对应的Python代码，简单修改即可。

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__ff917551ef094e80b.png)

然后，你拿着下面的代码，简单修改，就可以生成漂亮的Matplotlib图形了。

```


import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
import cmocean
#2021/07/13 08:03:18 
#- Import CSV as DataFrame ---------- 
FILE_PATH = 'C:/Users/Administrator/Desktop/plot.csv'
DATA = pd.read_csv(FILE_PATH)
#- Axes Setting ---------- 
fig, ax = plt.subplots()
ax.set_title( "x-y")
ax.set_xlabel( "x")
ax.set_ylabel( "x" )
ax.set_xlim(min(DATA['x'].replace([np.inf, -np.inf], np.nan ).dropna() ) - abs( min(DATA['x'].replace([np.inf, -np.inf], np.nan ).dropna() )/10), max(DATA['x'].replace([np.inf, -np.inf], np.nan).dropna()) + abs(max(DATA['x'].replace([np.inf, -np.inf], np.nan).dropna())/10)  )
ax.set_ylim( min(DATA['x'].replace([np.inf, -np.inf], np.nan ).dropna() ) - abs( min(DATA['x'].replace([np.inf, -np.inf], np.nan ).dropna() )/10), max(DATA['x'].replace([np.inf, -np.inf], np.nan).dropna()) + abs(max(DATA['x'].replace([np.inf, -np.inf], np.nan).dropna())/10)  )
#- PLOT ------------------ 
ax.plot( DATA["x"].replace([np.inf, -np.inf], np.nan), DATA["x"].replace([np.inf, -np.inf], np.nan), linewidth = 3.0, alpha =1.0, color = "#005AFF" )
plt.show() 


```

\- EOF -

推荐阅读  点击标题可跳转

<ins>1、[Matplotlib 可视化之图表层次结构](http://mp.weixin.qq.com/s?__biz=MzA5ODM5MDU3MA==&mid=2650883625&idx=2&sn=1c4fa493ee80cc7a4d1cd71ca9f94cfc&chksm=8b67a16cbc10287a6791f81c962baacb8c795ad07b97320475bfdb3aedc43d6d3b0633bdebdc&scene=21#wechat_redirect)</ins>

<ins>2、[数据分析最有用的 25 个 Matplotlib 图](http://mp.weixin.qq.com/s?__biz=MzA5ODM5MDU3MA==&mid=2650879116&idx=2&sn=361e90e0ee608f29ef38d6f51a8d68f9&chksm=8b67d3c9bc105adfb14201e8c10c250d19481afbffc50e738515e562460d6688ba2656a814f2&scene=21#wechat_redirect)</ins>

<ins>3、[一行代码让 matplotlib 图表变高大上](http://mp.weixin.qq.com/s?__biz=MzA5ODM5MDU3MA==&mid=2650873864&idx=2&sn=0de5759d2fef5586d5335b328f35a4b2&chksm=8b67c74dbc104e5b7a0232cd48b3c19e815d585acb161fd2747fb2cb14c8499a1a41c8c594f9&scene=21#wechat_redirect)</ins>

看完本文有收获？请转发分享给更多人

**推荐关注「数据分析与开发」，提升数据技能**

![](../../../_resources/0_wx_fmt_png_168aaf1508eb4106b54dd88b825bde1e.png)

**数据分析与开发**

「数据分析与开发」分享数据分析与开发相关技术文章、教程、工具

<a id="js_profile_article"></a>55篇原创内容

Official Account

点赞和在看就是最大的支持❤️

People who liked this content also liked

有了这篇 Docker 网络原理，彻底爱了~

高效运维

不看的原因

- 内容质量低
- 不看此公众号

C函数指针别再停留在语法，得上升到软件设计~

嵌入式资讯精选

不看的原因

- 内容质量低
- 不看此公众号

前端竟然用Golang 动态生成图片？

前端技术江湖

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___b6a7e42feabb44719.bmp"/>

Scan to Follow