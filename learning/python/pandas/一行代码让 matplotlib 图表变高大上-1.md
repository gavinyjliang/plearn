# 一行代码让 matplotlib 图表变高大上

<a id="profileBt"></a><a id="js_name"></a>数据分析与开发 *2021-08-16 12:10*

The following article is from Python大数据分析 Author 费弗里

<a id="copyright_info"></a>[![](../../../_resources/0_a7213bbf02524c34a78f4b90f3b93dd3.jpg)<br>**Python大数据分析** .<br>分享python编程、可视化设计、大数据分析、机器学习等技术以及数据分析案例，包括但不限于pandas、numpy、spark、matplotlib、sklearn、tensorflow、keras、tableau等](#)

1 简介

matplotlib作为Python生态中最流行的数据可视化框架，虽然功能非常强大，但默认样式比较简陋，想要制作具有简洁商务风格的图表往往需要编写众多的代码来调整各种参数。

而今天要为大家介绍的`dufte`，就是用来通过简短的代码，对默认的`matplotlib`图表样式进行自动改造的`Python`库：

<img width="406" height="245" src="../../../_resources/640_wx_fmt_svg_wxfrom_5_wx_lazy__b4b3bf70dcf441328.svg"/>

# 2 利用dufte自动改造matplotlib图表

通过`pip install dufte`安装完成后，我们就可以将`dufte`的几个关键`API`穿插在常规`matplotlib`图表的绘制过程中，目前主要有以下几种功能：

## 2.1 主题设置

`dufte`最重要的功能是其自带的主题风格，而在`matplotlib`中有两种设置主题的方式，一种是利用`plt.style.use(主题)`来全局设置，一般不建议这种方式。

另一种方式则是以下列方式来在`with`的作用范围内局部使用主题：

```
`# 局部主题设置
with plt.style.context(主题):
    # 绘图代码
    ...
`
```

我们今天就都使用第二种方式，首先导入演示所需的依赖库，并从本地注册`思源宋体`：

```
`import dufte
import numpy as np
import matplotlib.pyplot as plt
from matplotlib import font_manager
# 注册本地思源宋体
fontproperties = font_manager.FontProperties(fname='NotoSerifSC-Regular.otf')
`
```

接下来我们以折线图和柱状图为例：

- 折线图
    

```
`# 折线图示例
with plt.style.context(dufte.style):
    x = range(100)
    y = np.random.standard_normal(100).cumsum()
    
    fig, ax = plt.subplots(figsize=(10, 5), facecolor='white', edgecolor='white')
    
    ax.plot(x, y, linestyle='-.', color='#607d8b')
    
    ax.set_xlabel('x轴示例', fontproperties=fontproperties, fontsize=16)
    ax.set_ylabel('y轴示例', fontproperties=fontproperties, fontsize=16)
    
    ax.set_title('折线图示例', fontproperties=fontproperties, fontsize=20)
    fig.savefig('图2.png', dpi=300, bbox_inches='tight')
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

- 柱状图
    

```
`# 柱状图示例
with plt.style.context(dufte.style):
    x = range(25)
    y = np.random.standard_normal(25)
    fig, ax = plt.subplots(figsize=(10, 5), facecolor='white', edgecolor='white')
    
    ax.bar(x, y)
    
    ax.set_xlabel('x轴示例', fontproperties=fontproperties, fontsize=16)
    ax.set_ylabel('y轴示例', fontproperties=fontproperties, fontsize=16)
    
    ax.set_title('柱状图示例', fontproperties=fontproperties, fontsize=20)
    fig.savefig('图3.png', dpi=300, bbox_inches='tight')
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

可以看到，`dufte`自带了一套简洁的绘图风格，主张去除多余的轴线，只保留必要的参考线，适用于我们日常工作中的通用出图需求。

## 2.2 自动图例美化

除了前面介绍的整体主题风格之外，`dufte`还自带了一套图例风格化策略，只需要在绘图过程中利用`dufte.legend()`来代替`matplotlib`原有的`legend()`即可，以下面的折线图为例：

```
`# 折线图示例
with plt.style.context(dufte.style):
    x = range(100)
    y1 = np.random.randint(-5, 6, 100).cumsum()
    y2 = np.random.randint(-5, 10, 100).cumsum()
    y3 = np.random.randint(-5, 6, 100).cumsum()
    
    fig, ax = plt.subplots(figsize=(10, 5), facecolor='white', edgecolor='white')
    
    ax.plot(x, y1, linestyle='dotted', label='Series 1')
    ax.plot(x, y2, linestyle='dashed', label='Series 2')
    ax.plot(x, y3, linestyle='dashdot', label='Series 3')
    
    ax.set_xlabel('x轴示例', fontproperties=fontproperties, fontsize=16)
    ax.set_ylabel('y轴示例', fontproperties=fontproperties, fontsize=16)
    dufte.legend()
    ax.set_title('dufte.legend()示例', fontproperties=fontproperties, fontsize=20)
    fig.savefig('图4.png', dpi=300, bbox_inches='tight')
`
```

可以看到，对于多系列图表，只需要一行`dufte.legend()`就可以自动添加出下列别致的图例说明：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 2.3 柱状图自动标注

很多时候我们在绘制柱状图时，希望把每个柱体对应的y值标注在柱体上，而通过`dufte.show_bar_values()`，只要其之前的绘图流程中设置了`xticks`，它就会帮我们自动往柱体上标注信息：

```
`# 柱状图示例
with plt.style.context(dufte.style):
    x = range(15)
    y = np.random.randint(5, 15, 15)
    fig, ax = plt.subplots(figsize=(10, 5), facecolor='white', edgecolor='white')
    
    ax.bar(x, y)
    
    ax.set_xticks(x)
    ax.set_xticklabels([f'项目{i}' for i in x], fontproperties=fontproperties, fontsize=10)
    dufte.show_bar_values()
    
    ax.set_xlabel('x轴示例', fontproperties=fontproperties, fontsize=16)
    ax.set_ylabel('y轴示例', fontproperties=fontproperties, fontsize=16)
    
    ax.set_title('柱状图示例', fontproperties=fontproperties, fontsize=20)
    fig.savefig('图5.png', dpi=300, bbox_inches='tight')
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

作为一个处于开发初期的库，`dufte`未来势必会加入更多的实用功能，感兴趣的朋友可以对其持续关注。

\- EOF -

推荐阅读  点击标题可跳转

<ins>1、[数据分析最有用的 25 个 Matplotlib 图表](http://mp.weixin.qq.com/s?__biz=MzA5ODM5MDU3MA==&mid=2650873392&idx=1&sn=427358f10a2087d1583f41b12f3baec6&chksm=8b67f975bc107063cf18b9b581cc15503ab8e68ed288ff8c0186b9ca94aa7134300a09840210&scene=21#wechat_redirect)</ins>

<ins>2、[太强了，用 Matplotlib+Imageio 制作动画！](http://mp.weixin.qq.com/s?__biz=MzA5ODM5MDU3MA==&mid=2650872921&idx=2&sn=c46fc0a33f756995193ffad846aeed87&chksm=8b67fb1cbc10720a6aaffa609da3009d8dbf345b2607c585e1ea95712fe0b8d68837408b1916&scene=21#wechat_redirect)</ins>

<ins>3、[Python 绘图库 Matplotlib 入门教程](http://mp.weixin.qq.com/s?__biz=MzA5ODM5MDU3MA==&mid=2650863770&idx=1&sn=7e2ff6c5555a86c867cb952379c94e51&chksm=8b661fdfbc1196c941ebd713dfe30aa7702803457207196cd3d81540c9f7d5d7291ae169192b&scene=21#wechat_redirect)</ins>

看完本文有收获？请转发分享给更多人

**推荐关注「数据分析与开发」，提升数据技能**

![](../../../_resources/0_wx_fmt_png_9bfe840688224a69828f15fcb9f1362d.png)

**数据分析与开发**

「数据分析与开发」分享数据分析与开发相关技术文章、教程、工具

<a id="js_profile_article"></a>55篇原创内容

Official Account

点赞和在看就是最大的支持❤️

People who liked this content also liked

你是外包，麻烦不要偷吃零食？？？

...

数据分析与开发

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___a3559d27799f4967b.bmp"/>

Scan to Follow