# Python 一行代码算出每个省面积的神器—Geopandas

<a id="copyright_logo"></a>Original <a id="js_author_name"></a>Ckend <a id="profileBt"></a><a id="js_name"></a>Python实用宝典 *2022-05-25 20:20* *Posted on <a id="js_ip_wording"></a>广东*

<a id="js_article-tag-card__left"></a>收录于合集 #绘图专辑 <a id="js_article-tag-card__right"></a>23个

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__4e9b18db1b2f4a628.png)

GeoPandas是一个基于pandas，针对地理数据做了特别支持的第三方模块。

它继承pandas.Series和pandas.Dataframe，实现了GeoSeries和GeoDataFrame类，使得其操纵和分析平面几何对象非常方便。

***1.准备***

开始之前，你要确保Python和pip已经成功安装在电脑上，如果没有，可以访问这篇文章：[超详细Python安装指南](http://mp.weixin.qq.com/s?__biz=MzI3MzM0ODU4Mg==&mid=2247485004&idx=1&sn=6f89120cf926e71c7eb4788744ff625f&chksm=eb25e4c5dc526dd31f216f56b963179a0bc301a5654644ef98f436aa4740caa6f2774046296f&scene=21#wechat_redirect) 进行安装。

(可选1) 如果你用Python的目的是数据分析，可以直接安装Anaconda：[Python数据分析与挖掘好帮手—Anaconda](http://mp.weixin.qq.com/s?__biz=MzI3MzM0ODU4Mg==&mid=2247486014&idx=1&sn=4519422fbd83b5feffcbb21552226bc3&chksm=eb25e8b7dc5261a1aef2fa400ca7bcaa8c06394ea1f9a5860ab02bcf95d4664f41903b12bbd8&scene=21#wechat_redirect)，它内置了Python和pip.

(可选2) 此外，推荐大家用VSCode编辑器，它有许多的优点：[Python 编程的最好搭档—VSCode 详细指南](http://mp.weixin.qq.com/s?__biz=MzI3MzM0ODU4Mg==&mid=2247485849&idx=1&sn=ec098cf67a55bd1d61d4513397434c94&chksm=eb25eb10dc52620682db716d206c18b00bd53c01743729a9dea381e1791566a04a06f1fabca5&scene=21#wechat_redirect)。

请选择以下任一种方式输入命令安装依赖：
1\. Windows 环境 打开 Cmd (开始-运行-CMD)。
2\. MacOS 环境 打开 Terminal (command+空格输入Terminal)。
3\. 如果你用的是 VSCode编辑器 或 Pycharm，可以直接使用界面下方的Terminal.

由于geopandas涉及到许多第三方依赖，pip安装起来非常麻烦。因此在本教程中，我只推荐使用conda安装geopandas：

```
conda install geopandas
```

一行语句即可完成安装。

***2.基本使用***

设定坐标绘制简单的图形：

```
import geopandas
from shapely.geometry import Polygon
p1 = Polygon([(0, 0), (1, 0), (1, 1)])
p2 = Polygon([(0, 0), (1, 0), (1, 1), (0, 1)])
p3 = Polygon([(2, 0), (3, 0), (3, 1), (2, 1)])
g = geopandas.GeoSeries([p1, p2, p3])
# g:
# result:
# 0 POLYGON ((0 0, 1 0, 1 1, 0 0))
# 1 POLYGON ((0 0, 1 0, 1 1, 0 1, 0 0))
# 2 POLYGON ((2 0, 3 0, 3 1, 2 1, 2 0))
# dtype: geometry
```

这些变量所形成的图形如下：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

这里有一个重要且强大的用法，通过area属性，geopandas能直接返回这些图形的面积：

```
>>> print(g.area)
0    0.5
1    1.0
2    1.0
dtype: float64
```

不仅如此，通过plot属性函数，你还可以直接生成matplotlib图。

```
>>> g.plot()
```

通过matplot的pyplot，可以将图片保存下来：

```
import matplotlib.pyplot as plt
g.plot()
plt.savefig("test.png")
```

学会上面的基本用法， 我们就可以进行简单的地图绘制及面积的计算了。

***3.绘制并算出每个省的面积***

此外，它最大的亮点是可以通过 Fiona(底层实现，用户不需要管)，读取比如ESRI shapefile(一种用于存储地理要素的几何位置和属性信息的非拓扑简单格式)。

下面是读取一份省级行政区数据的示例：

```
import geopandas
import matplotlib.pyplot as plt
from shapely.geometry import Polygon
maps = geopandas.read_file('1.shx')
# 读取的数据格式类似于
# geometry
# 0 POLYGON ((1329152.341 5619034.278, 1323327.591...
# 1 POLYGON ((-2189253.375 4611401.367, -2202922.3...
# 2 POLYGON ((761692.092 4443124.843, 760999.873 4...
# 3 POLYGON ((-34477.046 4516813.963, -41105.128 4...
# ... ...
maps.plot()
plt.savefig("test.png")
```

如代码所示，通过read_file你可以读取shx、gpkg、geojson等数据。读取出来的图形如下：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

同样，因为这个shapefile是省级行政区的数据，每一个省级行政区都被划分为一个区块，因此可以一行语句算出每个省级行政区所占面积：

```
print(maps.area)
# 0 4.156054e+11
# 1 1.528346e+12
# 2 1.487538e+11
# 3 4.781135e+10
# 4 1.189317e+12
# 5 1.468277e+11
# 6 1.597052e+11
# 7 9.770609e+10
# 8 1.385692e+11
# 9 1.846538e+11
# 10 1.015979e+11
# ... ...
```

怎么样，是不是很酷？它还有许多更酷的特性，欢迎阅读官方文档：
https://geopandas.readthedocs.io/

本文用到的shx格式省级行政区数据，可以在【Python实用宝典】公众号后台回复 **省级行政区**下载。

我们的文章到此就结束啦，如果你喜欢今天的Python 实战教程，请持续关注Python实用宝典。

有任何问题，可以在公众号后台回复：**加群**，**回答二维码上相应的验证信息**，进入互助群询问。

原创不易，希望你能在下面点个赞和在看支持我继续创作，谢谢！

**点击下方阅读原文可获得更好的阅读体验**

Python实用宝典 (pythondict.com)

不只是一个宝典

欢迎关注公众号：Python实用宝典

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

Modified on 2022-05-26

<a id="js_view_source"></a>Read more

People who liked this content also liked

四行代码秒解微积分！Python这个模块神了！

...

Python实用宝典

不看的原因

- 内容质量低
- 不看此公众号

人人都能懂的 Python 自动发送邮件实战教程

...

Python实用宝典

不看的原因

- 内容质量低
- 不看此公众号

分享10个超级实用事半功倍的Python自动化脚本

...

简说Python

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___44085a5ce64c49f08.bmp"/>

Scan to Follow