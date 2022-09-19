# 吊打Pyecharts，又一超级棒的开源可视化库！

<a id="profileBt"></a><a id="js_name"></a>数据不吹牛 *2022-05-10 22:03* *Posted on <a id="js_ip_wording"></a>浙江*

The following article is from 机器学习专栏 Author boy

<a id="copyright_info"></a>[![](../../../_resources/0_25e0ea4b5b924390b16dfb82de4e8f2e.jpg)<br>**机器学习专栏** .<br>专注于分享机器学习、深度学习、NLP、CV、PyTorch、TensorFlow、Python实战等干货文章。](#)

![](../../../_resources/0_wx_fmt_png_9e9016293dde4eb7bdbf81471e135b3d.png)

**数据不吹牛**

有趣+干货的数据分析宝藏

<a id="js_profile_article"></a>69篇原创内容

Official Account

今天给大家推荐的这个开源项目是一个非常棒的可视化库。

这个开源项目就是：**PyG2Plot** 。

首先介绍下 G2Plot，G2Plot是蚂蚁集团开源的一个基于图表分类学的可视分析图表库，内置 25+ 常见图表类型，简单、易用、并具备一定扩展能力和组合能力的统计图表库，基于图形语法理论搭建而成。

再说 PyG2Plot，它是 @AntV/G2Plot 在 Python3 上的封装，并且在数据结构上，完全不做任何二次封装，所以配置文档上完全可以参考 G2Plot 官方文档，从而降低自己维护成本，以及开发者的学习上手成本。

安装和使用都非常简单，如下：

****1、安装****

```


pip install pyg2plot


```

****2、使用方法****

#### **渲染成 HTML**

```


from pyg2plot import Plot
line = Plot("Line")
line.set_options({
  "data": [
    { "year": "1991", "value": 3 },
    { "year": "1992", "value": 4 },
    { "year": "1993", "value": 3.5 },
    { "year": "1994", "value": 5 },
    { "year": "1995", "value": 4.9 },
    { "year": "1996", "value": 6 },
    { "year": "1997", "value": 7 },
    { "year": "1998", "value": 9 },
    { "year": "1999", "value": 13 },
  ],
  "xField": "year",
  "yField": "value",
})
# 1. 渲染成 html 文件
line.render("plot.html")
# 2. 渲染成 html 字符串
line.render_html()



```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

#### **在 Jupyter 中使用**

```


from pyg2plot import Plot
line = Plot("Line")
line.set_options({
  "height": 400, # set a default height in jupyter preview
  "data": [
    { "year": "1991", "value": 3 },
    { "year": "1992", "value": 4 },
    { "year": "1993", "value": 3.5 },
    { "year": "1994", "value": 5 },
    { "year": "1995", "value": 4.9 },
    { "year": "1996", "value": 6 },
    { "year": "1997", "value": 7 },
    { "year": "1998", "value": 9 },
    { "year": "1999", "value": 13 },
  ],
  "xField": "year",
  "yField": "value",
})
# 1. 渲染到 notebook
line.render_notebook()
# 2. 渲染到 jupyter lab
line.render_jupyter_lab()


```

目前 pyg2plot 只提供简单的一个 API：**Plot**，使用方法如下：

1.  Plot(plot_type: str): 获取 Plot 对应的类实例。
    
2.  plot.set_options(options: object): 给图表实例设置一个 G2Plot 图形的配置，文档可以直接参考 G2Plot 官网，未进行任何二次数据结构包装。
    
3.  plot.render(path, env, **kwargs): 渲染出一个 HTML 文件，同时可以传入文件的路径，以及 jinja2 env 和 kwargs 参数。
    
4.  plot.render_notebook(env, **kwargs): 将图形渲染到 jupyter 的预览。
    
5.  plot.render\_jupyter\_lab(env, **kwargs): 将图形渲染到 jupyter lab 的预览。
    
6.  plot.render_html(env, **kwargs): 渲染出 HTML 字符串，同时可以传入 jinja2 env 和 kwargs 参数。
    
7.  plot.dump\_js\_options(env, **kwargs): 输出 Javascript 的 option 配置结构，同时可以传入 jinja2 env 和 kwargs 参数，可以用于 Server 中的 HTTP 结构返回数据结构。
    

****3、支持图表****

pyg2plot 支持很多类型的图表，非常好用，效果图如下：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

举几个例子，下面是分别是面积图、柱形图、双轴图，可以看到可视化效果是非常棒的。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

更多图表样式的绘制可参考：**https://github.com/hustcc/PyG2Plot/blob/main/docs/plot.md**

# ****4、技术原理****

PyG2Plot 原理其实非常简单，其中借鉴了 **pyecharts** 的实现，但是因为蚂蚁金服的 G2Plot 完全基于可视分析理论的配置式结构，所以封装上比 pyecharts 简洁非常非常多。

**基本的原理，就是通过 Python 语法提供 API，然后在调用 render 的时候，生成最终的 G2Plot HTML 文本，而针对不同的环境，生成的 HTML 稍有区别。**

- 针对 HTML 生成，则直接使用正常的 html 模板，然后 script 引入 G2Plot 资源，生成 G2Plot 的 JavaScript 代码，渲染即可
    
- 针对 Jupyter 环境，生成的的内容中比较特殊的时候，使用 requireJS 去加载 G2Plot 资源，后续的逻辑一致
    

这个原理可以理解是所有的语种封装 JavaScript 模块的统一做法。

所以对于 PyG2Plot，**核心文件**是：

- plot.py：提供了 PyG2Plot 的几乎全部 API
    
- engine.py：提供了渲染 HTML 的能力，其实是基于 jinja2 这个模板引擎实现的
    
- templates：提供了所有的 jinja2 模板文件，对于模板怎么用，jinja2 的文档是非常非常详细的
    

开源项目地址：**https://github.com/hustcc/PyG2Plot**

开源项目作者：**hustcc**

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

```


![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

<ins>●[分析师如何正确的提建议？](http://mp.weixin.qq.com/s?__biz=MzU5Mjg2OTQ1MA==&mid=2247509523&idx=1&sn=5612dab06f2dd041fb6b192bd35fe881&chksm=fe1bc736c96c4e20cc6123fd651544c8d9995e5e0c4c962b1094a5aa760c4eaea2951ff0136b&scene=21#wechat_redirect)</ins>

<ins>●[品牌知名度](http://mp.weixin.qq.com/s?__biz=MzU5Mjg2OTQ1MA==&mid=2247501207&idx=1&sn=b17fc4d67f0794da55c15ca5765bf884&chksm=fe1ba4b2c96c2da49e694c8a403807526e87b1e54b1f7d266efe3e31f657435248c2073efb30&scene=21#wechat_redirect)分析</ins>




























```

People who liked this content also liked

Go 开源项目推荐：一个简单的 Go 练手项目

...

polarisxu

不看的原因

- 内容质量低
- 不看此公众号

将 Go 语言当成脚本来使用 - gomacro

...

k8s技术圈

不看的原因

- 内容质量低
- 不看此公众号

初创数据库公司的疯狂行为：删掉花7个月开发的27万行C++代码，用Rust全部重写一遍

...

InfoQ

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___14d080f836c64f328.bmp"/>

Scan to Follow