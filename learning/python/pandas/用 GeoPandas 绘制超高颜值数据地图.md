# 用 GeoPandas 绘制超高颜值数据地图

<a id="copyright_logo"></a>Original <a id="js_author_name"></a>云朵君 <a id="profileBt"></a><a id="js_name"></a>数据STUDIO *2021-12-15 11:30*

收录于话题

<a id="js_article_tag_name__1974978820940054530"></a>#数据分析 <a id="js_article_tag_num__1974978820940054530"></a>53 <a id="js_article_tag_tips__1974978820940054530"></a>个

<a id="js_article_tag_name__1974978822768771072"></a>#python <a id="js_article_tag_num__1974978822768771072"></a>45 <a id="js_article_tag_tips__1974978822768771072"></a>个

<a id="js_article_tag_name__1974991176839544834"></a>#可视化 <a id="js_article_tag_num__1974991176839544834"></a>28 <a id="js_article_tag_tips__1974991176839544834"></a>个

大家好，我是云朵君！
关注和星标『数据STUDIO』，和云朵君一起学习数据分析与挖掘！

![](../../../_resources/0_wx_fmt_png_06b138212ccc4366bfb1dfe80e03462e.png)

**数据STUDIO**

点击领取《Python学习手册》，后台回复「福利」获取。『数据STUDIO』专注于数据科学原创文章分享，内容以 Python 为核心语言，涵盖机器学习、数据分析、可视化、MySQL等领域干货知识总结及实战项目。

<a id="js_profile_article"></a>173篇原创内容

Official Account

👆点击关注｜选择星标｜干货速递👆

## 写在前面

通常情况下，在执行 EDA 时，我们会面临显示有关地理位置的信息的情况。例如，对于 COVID 19 数据集，人们可能希望显示各个区域的病例数。这是 Python 库 GeoPandas 的用武之地。

本文和大家一起学习如何使用 GeoPandas有效地可视化地理空间数据。

### 与 GeoPandas 相关的地理空间分析相关术语

地理空间数据<sup>\[1\]</sup>描述相对于地球位置（坐标）的物体、事件或其他特征。

**空间数据** 由几何对象的基本类型表示。

<img width="677" height="141" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__761e24ed56f14667a.png"/>

| 几何  | 代表  |
| --- | --- |
| 点 points | 地块位置的中心点等。 |
| 线 lines | 道路、溪流 |
| 多边形 polygons | 建筑物、湖泊、州、省等的边界。 |

**CRS/坐标参考系统**告诉我们如何（使用**投影** 或数学方程）将**圆形**地球上的位置（坐标）转换为**扁平**的二维坐标系（例如计算机屏幕或纸张）上的相同位置地图）。最常用的 CRS 是“EPSG:4326”。

<img width="677" height="299" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__7c76cfe14ee34dc0b.png"/>

## 什么是GeoPandas？

GeoPandas 基于Pandas。它扩展了 Pandas 数据类型以包含**几何**列并执行**空间操作**。因此，任何熟悉Pandas的人都可以轻松采用 GeoPandas。

<img width="677" height="355" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__83c6fed27eea4927a.png"/>

▲ GeoPandas – GeoDataFrame 和 GeoSeries

在*GeoPandas*的主要数据结构是*GeoDataFrame*延伸的*PandasDataFrame*。所以所有基本的*DataFrame*操作都可以在*GeoDataFrame*上执行。*GeoDataFrame*包含一个或多个*GeoSeries*（延伸*PandasSeries*）每个都包含在一个不同的几何形状的投影（*GeoSeries.crs*）。虽然*GeoDataFrame*可以有多个*GeoSeries*列，但其中只有一个是活动几何图形，即所有几何操作都在该列上。

在下一节中，我们将一起学习如何使用一些常见的函数，如**边界、质心**和最重要的**绘图**方法。为了演示地理空间可视化的工作，让我们使用来自2021年奥运会数据集的Teams数据。

## 数据准备

在导入 GeoPandas 之前阅读Teams数据集，数据集和代码可以在公众号『数据STUDIO』回复【GeoPandas】获取。

团队的数据集包含团队名称、项目、NOC（国家/地区）和事件列。在本练习中，我们将仅使用 **NOC** 和 **项目** 列。

```
import pandas as pd
df_teams = pd.read_excel("data/Teams.xlsx")

```

<img width="677" height="367" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__28a099ef4c3c42b09.png"/>

#### 总结每个国家的项目并绘制它。

```
df_teams_countries_disciplines = df_teams   \
    .groupby(by="NOC").agg({'Discipline':'count'} )  \
    .reset_index().sort_values(by='Discipline', ascending=False)
    
ax = df_teams_countries_disciplines.plot.bar(x='NOC', xlabel = '', figsize=(20,8))

```

<img width="677" height="355" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__1e3145925b86409fb.png"/>

▲ df\_teams\_countries_disciplines–条形图

### 导入 GeoPandas 并读取数据

```
import geopandas as gpd
df_world = gpd.read_file(gpd.datasets.get_path('naturalearth_lowres'))
print(f"{type(df_world)}, {df_world.geometry.name}")
print(df_world.head())
print(df_world.geometry.geom_type.value_counts())

```

“ *naturalearth_lowres* ”是我们加载的*geopandas*提供的*底图*。

<img width="677" height="239" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__5025d1a9b973471b9.png"/>

▲ df_world

*df_world* 的类型是 *GeoDataFrame* 与大陆（国家）的名称和几何列（国家地区）。*geometry* 属于*GeoSeries* 类型，是具有以 *Polygon* 和 *MultiPolygon* 类型表示的国家区域的活动几何体。

#### 现在绘制世界地图

```
df_world.plot(figsize=(10,6))

```

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__f94f69ba5fca411b9.png)

▲ df_world-plot

### 合并 teams 和 world 数据集

```
df_world_teams = df_world.merge(df_teams_countries_disciplines, 
                                how="left", 
                                left_on=['name'], 
                                right_on=['NOC'])
print("Type of DataFrame : ", 
      type(df_world_teams), df_world_teams.shape[0])
df_world_teams.head()

```

<img width="677" height="130" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__4a87661281d2466da.png"/>

▲ 合并数据框

#### 注意：

- df\_world\_teams 将有一些 NOC 和 Discipline 为 NaN 的记录。在里用的到是**'left'**而不是**'right'**合并，这里是有意这样做的，因为我们数据中也有一些没有参与的国家。
    
- 很少有国家名称在奥运会和世界数据集之间不一致。所以尽可能调整了国家名称。详细信息在**源代码**中。
    

## 开始绘图

### 显示一个简单的世界地图 \- 只有边界的地图

作为第一步，我们绘制基本地图——只有边界的世界。在接下来的步骤中，将为我们感兴趣的国家/地区着色。

```
ax = df_world["geometry"].boundary.plot(figsize=(20,16))

```

<img width="677" height="333" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__6f65e19ae9f445b38.png"/>

▲ 世界地图

### 显示 Choropleth 地图 - 绘制区域

接下来，我们根据国家参加的学科数量为参加奥运会的国家涂上颜色的深浅。国家参加的学科越多，颜色越深，反之亦然。**等值线图为**与数据变量相关的区域/多边形着色。

```
df_world_teams.plot( column="Discipline", ax=ax, cmap='OrRd', 
                     legend=True, 
                    legend_kwds={"label": "Participation", 
                                 "orientation":"horizontal"})
ax.set_title("参加2021年奥运会的国家Vs项目数量")

```

在这里需要注意的是：

- **ax**是绘制地图的轴
    
- **cmap**是颜色图的名称
    
- **legend & legend_kwds**控制图例的显示
    

#### 参加奥运会的国家

<img width="677" height="461" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__31899213583e4b348.png"/>

▲ 参加奥运会的国家

根据阴影，我们可以很快看出，中国、日本、美国、意大利、德国和澳大利亚是参与较多项目的国家。

请注意，底部的图例看起来不太好。我们修改 `df_world_teams.plot` 以使可视化更易于展示。

```
fig, ax = plt.subplots(1, 1, figsize=(20, 16))
divider = make_axes_locatable(ax)
cax = divider.append_axes("right", size="2%", pad="0.5%")
df_world_teams.plot(column="Discipline", ax=ax, cax=cax, cmap='OrRd',
legend=True, legend_kwds={"label": "Participation"})

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

▲ 带有整洁的颜色图

**这个可视化不是更整洁吗？**

### 对未参加的国家进行着色

#### 绘制missing_kwds

现在，哪些没有参加的国家呢？所有没有阴影（即白色）的国家都是没有参加的国家。但是我们通过将这些国家/地区涂成灰色来使这一点更加明显。我们可以使用带有纯色或带有颜色和图案的 *missing_kwds*。

```
df_world_teams.plot(column="Discipline", 
                    ax=ax, cax=cax, 
                    cmap='OrRd',
                    legend=True, 
                    legend_kwds={"label": "Participation"},
missing_kwds={'color': 'lightgrey'})

```

<img width="677" height="327" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__7f06c304c6ac4b0ea.png"/>

▲ 未参加奥运会的国家-灰色阴影

```
df_world_teams.plot(column= 'Discipline', ax=ax,  
                    cax=cax, cmap='OrRd', 
                    legend=True, 
                    legend_kwds={"label": "Participation"}, 
                    missing_kwds={"color": "lightgrey", 
                                  "edgecolor": "white", "hatch": "|"})

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

▲ 未参加奥运会的国家-灰色阴影和阴影线

### 标记参与最少的项目的国家-绘制点

#### 哪个项目的参与最少？

```
df_discipline_countries = \
df_teams.groupby(by='Discipline'
                ).agg({'NOC':'count'}
                     ).sort_values(by='NOC', 
                                   ascending=False)
ax = df_discipline_countries.plot.bar(figsize=(8, 6))

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

▲ 项目与国家数量

因此，棒球/垒球是参与国家数量最少的项目（12 个）。现在我们来了解一下有哪些国家参加了这个项目？

为此，首先创建一个仅包含参与最少的国家的数据集，然后将此数据集 *df\_teams\_least\_participated\_disciplines* 和 *df_world* 合并，然后计算质心。

```
# 创建一个只有参与最少的国家的数据集
councountries_in_least_participated_disciplines = df_discipline_countries[df_discipline_countries['NOC']<13].index.tolist()
print(least_participated_disciplines)
df_teams_least_participated_disciplines = \
df_teams[df_teams['Discipline'].
         isin(countries_in_least_participated_disciplines)]\
.groupby(by=['NOC','Discipline']).agg({'Discipline':'count'})
df_teams_least_participated_disciplines.groupby(by=['NOC']
                                               ).agg({'Discipline':'count'}
                                                    ).sort_values(by='Discipline',
                                                                  ascending=False)
# 合并 
df_teams_least_participated_disciplines 与 df_world
df_world_teams_least_participated_disciplines = df_world.merge(
  df_teams_least_participated_disciplines,
  how="right", 
  left_on=['name'], 
  right_on=['NOC'])
df_world_teams_least_participated_disciplines['centroid'] = \
df_world_teams_least_participated_disciplines.centroid
print("Type of DataFrame : ",
type(df_world_teams_least_disciplines),
      df_world_teams_least_participated_disciplines.shape[0])
print(df_world_teams_least_participated_disciplines[:5])

```

<img width="677" height="119" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__e8ea58df8ac541b88.png"/>

所以澳大利亚、加拿大、多米尼加共和国和其他国家参加了参与最少的学科。

将以下行添加到我们之前编写的绘图代码中，用深蓝色填充圆圈标记这些国家。

```
df_world_teams_least_participated_disciplines["centroid"] \
   .plot(ax=ax, color="DarkBlue")
df_world_teams_least_participated_disciplines.apply(lambda x: ax.annotate(text=x['name'], 
xy=(x['centroid'].coords[0][0],
x['centroid'].coords[0][ 1]-5), 
ha='center'),axis=1)

```

<img width="677" height="334" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__4fccd07b35df4c9c9.png"/>

▲ 参与最少的项目的国家

现在我们在世界地图上显示了奥运代表队。我们可以进一步扩展它，使其信息更丰富。

警告：不要以牺牲清晰度为代价向地图添加太多细节。

### 参考资料

\[1\] 

地理空间数据: *https://www.ibm.com/topics/geospatial-data*

\[2\] 

图片来源: *https://www.earthdatascience.org/courses/earth-analytics/spatial-data-r/intro-to-coordinate-reference-systems/*

往期推荐 点击查看

[技术菜鸟如何做出好看的奥运会奖牌榜](http://mp.weixin.qq.com/s?__biz=Mzk0OTI1OTQ2MQ==&mid=2247494983&idx=1&sn=f49de327d67e843697c07c4b95c1fb20&chksm=c359b0edf42e39fb4c3717de374f9221812e2258e8f49142ef2930253a341eca5e74272ef181&scene=21#wechat_redirect)

[用一行Python代码创建高级财务图表](http://mp.weixin.qq.com/s?__biz=Mzk0OTI1OTQ2MQ==&mid=2247507142&idx=1&sn=730166685a5099d994a700a95b6f31d2&chksm=c359816cf42e087a221a7b5b4922d1d17ef3d86540eeae0b610e9627167fe948417559ff569d&scene=21#wechat_redirect)

[这种个性化可视化图也太可爱了吧！](http://mp.weixin.qq.com/s?__biz=Mzk0OTI1OTQ2MQ==&mid=2247506874&idx=1&sn=a88072399a7984b7839602a49ef5e474&chksm=c3598610f42e0f06e22ef10a439460a703e10570e2834525f4918811642db34daf8ea6118543&scene=21#wechat_redirect)

[4W字，最强 Matplotlib 实操指南！](http://mp.weixin.qq.com/s?__biz=Mzk0OTI1OTQ2MQ==&mid=2247506452&idx=1&sn=99d6e100ed908eff1d77ff8c822699b5&chksm=c35987bef42e0ea8f76a2329288b5e6fc221481ee8409734eed503ee50a4fbfee57216f21fe6&scene=21#wechat_redirect)

[Python Bokeh 库进行数据可视化实用指南](http://mp.weixin.qq.com/s?__biz=Mzk0OTI1OTQ2MQ==&mid=2247506011&idx=1&sn=fc2f26fa115a8d9c234af124d4f004ba&chksm=c35985f1f42e0ce741bd2870dd039718e3ca548540d8542125a14ee8fee8107e83b74c38d059&scene=21#wechat_redirect)

[数据分析最有用的25个Matplotlib图](http://mp.weixin.qq.com/s?__biz=Mzk0OTI1OTQ2MQ==&mid=2247505997&idx=2&sn=2a53b914c9e5f080dc57db3856c47a97&chksm=c35985e7f42e0cf10d3114dd41f11918e1b0f7ff5e905046ddaedcc5887a0fa787e1bcc60d3e&scene=21#wechat_redirect)

[Seaborn 绘制 21 种超实用精美图表](http://mp.weixin.qq.com/s?__biz=Mzk0OTI1OTQ2MQ==&mid=2247499555&idx=1&sn=9f4e46d4c9c563fdd559f354dfc5f34e&chksm=c359a289f42e2b9f31df218e43175503a4e2b11aeb05646f4f91267c82b3310cdc34e656a86e&scene=21#wechat_redirect)

[对比学习，用Excel和Python绘制「棒棒糖图」](http://mp.weixin.qq.com/s?__biz=Mzk0OTI1OTQ2MQ==&mid=2247495939&idx=1&sn=7df54c3207806ccf296debb7e6695f17&chksm=c359aca9f42e25bf077f89dcc33d34b40945f7e2f9f43e3040143ebfe29475ed48b4ac291252&scene=21#wechat_redirect)

长按👇关注\- 数据STUDIO - 选择星标，干货速递

<img width="677" height="227" src="../../../_resources/640_wx_fmt_gif_wxfrom_5_wx_lazy__94c3b76c5ece4a7aa.gif"/>

People who liked this content also liked

为什么回归问题用MSE？

数据STUDIO

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___8ff366b9f8e246658.bmp"/>

Scan to Follow