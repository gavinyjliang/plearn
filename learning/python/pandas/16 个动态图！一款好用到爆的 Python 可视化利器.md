# 16 个动态图！一款好用到爆的 Python 可视化利器

<a id="profileBt"></a><a id="js_name"></a>算法爱好者 *2022-04-12 11:57*

The following article is from 关于数据分析与可视化 Author 俊欣

<a id="copyright_info"></a>[![](../../../_resources/0_cbaf7f4b4a5243b88abbd372cd71318b.jpg)<br>**关于数据分析与可视化** .<br>本公众号定期分享数据分析与可视化干货文章，并有时结合热点话题进行深入讨论，希望您会喜欢，要是哪里写的不好，也渴望倾听您的想法和意见，感谢！❤️](#)

<img width="677" height="339" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__f72513afea744bdab.png"/>

一般提及数据可视化，会Python的读者朋友可能第一时间想到的就是`matplotlib`模块或者是`seaborn`模块，而谈及绘制动态图表，大家想到的比较多的是`Plotly`或者是`Pyecharts`。

今天为大家介绍另外一个绘制动态图表的模块，使用起来也是非常的便捷，而且绘制出来的图表也是十分的精湛好看，它叫`pygal`，相比较seaborn等常用的模块相比，该模块的优点有：

- 高度可定制，而且用法简单
    
- 图表**可交互性强**
    
- 图像可导出SVG格式（矢量图形）
    
- 与Django、Flask等Web框架高度集成
    

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

因此，pygal模块还是值得拿出来讲讲的，我们大致会说这些内容：

- `pygal`模块的安装
    
- 柱状图的绘制
    
- 折线图的绘制
    
- 饼图的绘制
    
- 雷达图的绘制
    
- 箱型图
    
- 仪表盘
    
- 树形图
    
- 地图
    

## 模块的安装

模块的安装十分的简单，通过`pip install`就能够实现，

```
`pip install pygal
`
```

当然国内的小伙伴要是觉得下载的速度慢，也可以通过加入第三方的镜像来提速

```
`pip install -i http://pypi.douban.com/simple/ pygal
`
```

## 柱状图

我们来看柱状图的绘制，先看单列柱状图的例子

```
`view = pygal.Bar()
#图表名
view.title = '柱状图'
#添加数据
view.add('数据', [1,3,5,7,9,11])
#在浏览器中查看
#view.render_in_browser()
view.render_to_file('bar.svg')
`
```

output

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

我们既可以通过`render_to_file()`方法来导出成文件，也可以通过`render_in_browser()`方法在浏览器中查看

我们再来看多列柱状图的绘制，代码如下

```
`view.add('奇数', [1,3,5,7,9,11])
view.add('偶数', [2,4,6,8,10,12])
`
```

output

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

要是我们想将柱状图横过来看，将上述代码当中的一小部分替换成

```
`view = pygal.HorizontalBar()
`
```

output

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

而要是我们想要堆叠形式的柱状图，则需要将上述代码当中的一小部分替换成

```
`view = pygal.HorizontalStackedBar()
`
```

output

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 折线图

对于折线图的绘制，其实与上面柱状图的绘制基本一致，我们直接来看代码

```
`view = pygal.Line()
#图表名
view.title = '折线图'
#添加数据
view.add('奇数', [1,3,5,7,9,11])
view.add('偶数', [2,4,6,8,10,12])
#在浏览器中查看
view.render_in_browser()
`
```

output

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

也和上面柱状图的代码逻辑保持一致，折线图中也有堆叠式的折线图，只需要将上面的代码当中的一部分替换成

```
`view = pygal.StackedLine(fill=True)
`
```

## 饼图

同样，饼图的绘制也是相似的代码逻辑

```
`view = pygal.Pie()
#图表名
view.title = '饼状图'
#添加数据
view.add('A', 23)
view.add('B', 40)
view.add('C', 15)
view.render_to_file('pie.svg')
`
```

output

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

同时我们也可以绘制圆环图，在饼图的中心掏空出来一块，代码大致相同，只是需要将上面的一小部分替换成

```
`#设置空心圆半径
view = pygal.Pie(inner_radius=0.4)
`
```

output

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

当我们每个类当中不止只有一个数字的时候，可以绘制多级饼图，代码如下

```
`view = pygal.Pie()
#图表名
view.title = '多级饼图'
#添加数据
view.add('A', [20, 25, 30, 45])
view.add('B', [15, 19, 25, 50])
view.add('C', [18, 22, 28, 35])
view.render_to_file('pie_multi.svg')
`
```

output

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 雷达图

雷达图可以帮我们从多个维度来分析数据，例如我们分析运动员的运动能力的时候，就会从多个维度来综合看待，这个时候雷达图就变得非常有用，代码如下

```
`radar_chart = pygal.Radar()
radar_chart.title = 'NBA 各球员能力比拼'
radar_chart.x_labels = ['得分', '防守', '助攻', '失误', '篮板']
radar_chart.add('库里', [70, 98, 96, 85, 97])
radar_chart.add('詹姆斯', [60, 95, 50, 75, 99])
radar_chart.add('杜兰特', [94, 45, 88, 91, 98])
radar_chart.render_to_file('radar_nba.svg')
`
```

output

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

当然上面的数据都是瞎编的，喜欢NBA的读者朋友或者是喜欢上面几个球形的读者朋友看了可别喷我哦

## 箱型图

箱型图可以快速地帮我们了解数据的分布，查看是否存在极值。在`pygal`模块当中也提供了绘制箱型图的方法，代码如下

```
`box_plot = pygal.Box()
box_plot.title = '各浏览器的使用量'
box_plot.add('Chrome', [6395, 8212, 7520, 7218, 12464, 1660, 2123, 8607])
box_plot.add('Firefox', [7512, 8099, 11700, 2651, 6361, 1044, 8502, 9450])
box_plot.add('360安全卫士', [3472, 2933, 4203, 5510, 5810, 1828, 9013, 4669])
box_plot.add('Edge', [4310, 4109, 5935, 7902, 14404, 13608, 34004, 10210])
box_plot.render_to_file("box.svg")
`
```

output

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 仪表盘

仪表盘可以帮助我们量化指标的好坏，代码如下

```
`gauge_chart = pygal.Gauge(human_readable=True)
gauge_chart.title = '不同浏览器的性能差异'
gauge_chart.range = [0, 10000]
gauge_chart.add('Chrome', 8212)
gauge_chart.add('Firefox', 8099)
gauge_chart.add('360安全卫士', 2933)
gauge_chart.add('Edge', 2530)
gauge_chart.render_to_file('gauge_1.svg')
`
```

output

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 热力图

热力图可以更加直观的观测每个区域当中数据的分布，代码如下

```
`treemap = pygal.Treemap()
treemap.title = 'Binary TreeMap'
treemap.add('A', [12, 15, 12, 40, 2, 10, 10, 13, 12, 13, 40, None, 19])
treemap.add('B', [4, 2, 5, 10, 30, 4, 2, 7, 4, -10, None, 8, 30, 10])
treemap.add('C', [3, 8, 3, 3, 5, 15, 3, 5, 4, 12])
treemap.add('D', [23, 18])
treemap.add('E', [11, 2, 1, 12, 3, 13, 1, 2, 13,
      14, 3, 1, 2, 10, 1, 10, 12, 1])
treemap.add('F', [31])
treemap.add('G', [15, 9.3, 8.1, 12, 4, 34, 2])
treemap.add('H', [12, 13, 3])
treemap.render_in_browser()
`
```

output

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 地图

首先我们来看世界地图的绘制，在这之前，我们还要下载绘制整个世界地图所需要的插件

```
`pip install pygal_maps_world
`
```

代码如下

```
`worldmap_chart = pygal.maps.world.World()
worldmap_chart.title = 'Some countries'
worldmap_chart.add('A countries', ['国家名称的缩写'])
worldmap_chart.add('B countries', ['国家名称的缩写'])
worldmap_chart.add('C countries', ['国家名称的缩写'])
worldmap_chart.render_in_browser()
`
```

output

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

我们也可以针对不同国家的计数来进行地图的绘制，例如不同国家重大疾病的死亡人数，代码如下

```
`worldmap_chart = pygal.maps.world.World()
worldmap_chart.title = 'Minimum deaths by capital punishement (source: Amnesty International)'
worldmap_chart.add('In 2012', {
  '国家名称的缩写': 数量,
  '国家名称的缩写': 数量,
  .....
})
worldmap_chart.render_in_browser()
`
```

output

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

我们也可以绘制以五大洲为主的世界地图，代码如下

```
`worldmap_continent = pygal.maps.world.SupranationalWorld()
worldmap_continent.add('Asia', [('asia', 1)])
worldmap_continent.add('Europe', [('europe', 1)])
worldmap_continent.add('Africa', [('africa', 1)])
worldmap_continent.add('North america', [('north_america', 1)])
worldmap_continent.add('South america', [('south_america', 1)])
worldmap_continent.add('Oceania', [('oceania', 1)])
worldmap_continent.add('Antartica', [('antartica', 1)])
worldmap_continent.render_in_browser()
`
```

output

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

当然我们也可以将某个国家作为绘制，例如我们以法国为例，首先我们需要下载绘制单独某个国家的地图所依赖的插件

```
`pip install pygal_maps_fr
`
```

代码如下

```
`fr_chart = pygal.maps.fr.Regions()
fr_chart.title = '法国区域图'
fr_chart.add('区域名称', ['数量'])
fr_chart.render_in_browser()
`
```

output

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

但是提及绘制某个国家的地图而言，目前支持的国家的数量并不多，在官网上面也只罗列**法国和瑞士**这两个国家，其他国家的插件下载，尝试下载了一下，都下载不了，后面就等官方的更新与优化。

\- EOF -

推荐阅读  点击标题可跳转

<ins>1、[看完微软大神写的求平均值代码，我意识到自己还是 too young了](http://mp.weixin.qq.com/s?__biz=MzI1MTIzMzI2MA==&mid=2650580978&idx=1&sn=494473403ec3ba794461b5b29459e78a&chksm=f1fe1171c689986701050a4e00bb4845f154140a988e8dfa1248977452c15ef718b4dce5d67a&scene=21#wechat_redirect)</ins>

<ins>2、[45 个 Git 经典操作场景，专治不会合代码](http://mp.weixin.qq.com/s?__biz=MzI1MTIzMzI2MA==&mid=2650581123&idx=1&sn=dfde76b8968fb9a18bc418f706735265&chksm=f1fe1e00c6899716118f4feeab55daedcbcc5057fafe5b81aa0c7e603406447d10a9a111a9b3&scene=21#wechat_redirect)</ins>

<ins>3、[这次终于彻底理解了傅里叶变换！](http://mp.weixin.qq.com/s?__biz=MzI1MTIzMzI2MA==&mid=2650581150&idx=1&sn=fa8df1ac590fe7f6ad5552dce8b2642d&chksm=f1fe1e1dc689970b38ce5107784d8b4566aafabd653306a3bac22ffbcfd331f9b2fa477b8d7a&scene=21#wechat_redirect)</ins>

觉得本文有帮助？请分享给更多人

****推荐关注「算法爱好者」**，修炼编程内功**

![](../../../_resources/0_wx_fmt_png_73b032db5beb4743b241ff33d0a2fe73.png)

**算法爱好者**

算法是程序员的内功！「算法爱好者」专注分享算法相关文章、工具资源和算法题，帮程序员修炼内功。

<a id="js_profile_article"></a>26篇原创内容

Official Account

点赞和在看就是最大的支持❤️

People who liked this content also liked

有了MVC，为什么还要DDD？

...

石杉的架构笔记

不看的原因

- 内容质量低
- 不看此公众号

【第2617期】React 组件库 CSS 样式方案分析

...

前端早读课

不看的原因

- 内容质量低
- 不看此公众号

Python处理PDF神器：PyMuPDF的安装与使用

...

杰哥的IT之旅

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___a4b19f37a3594748a.bmp"/>

Scan to Follow