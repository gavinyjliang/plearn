# matplotlib 可视化，看这一篇就够了！

<a id="profileBt"></a><a id="js_name"></a>早起Python *2022-01-02 17:00*

The following article is from Python数据之道 Author 阳哥

<a id="copyright_info"></a>[![](../../../_resources/0_8ee073465a62488bbdd1b7ca781a8edd.jpg)<br>**Python数据之道** .<br>点击领取《Python知识手册》高清电子版，回复数字 “600” 获取。「Python数据之道」秉承“让数据更有价值”的理念​，聚焦于 Python 数据分析、数据可视化、AI、机器学习、深度学习等领域。](#)

大家好，我是早起。

大家知道，在利用Python进行数据可视化过程中，基本上是很难绕开 Matplotlib 的，因为 不少其他的可视化库多多少少是建立在 Matplotlib 的基础上的，今天就给大家分享一波阳哥整理的干货！

<img width="657" height="391" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_8e561dd289f748e9a.jpg"/>

Python绘图库生态

## 01背景

阳哥在学习数据可视化的过程中，也是不断地在学习 Matplotlib 的。

虽然有时候会觉得 Matplotlib 的语言繁琐，做出来的图不怎么高大上（现在看来还是自己水平菜~~）

但时不时的看到有高手用 Matplotlib 绘制出令人惊叹的图表，又会去研习一番。

在 2018年的时候，阳哥跟大家分享了 [Matplotlib 可视化最有价值的 50 个图表](https://mp.weixin.qq.com/s?__biz=MzI2NjY5NzI0NA==&mid=2247485238&idx=1&sn=f2d6b9136ff94697bdce9c9f48397e78&scene=21#wechat_redirect) 。

2020年，继续分享了 Matplotlib 可视化的 100个案例

- [Matplotlib 实操干货，100个案例带你从入门到进阶！-Part 1](https://mp.weixin.qq.com/s?__biz=MzI2NjY5NzI0NA==&mid=2247492863&idx=1&sn=4bb9f33a0f3b38232a31beb23c80c18a&scene=21#wechat_redirect)
    
- [Matplotlib 实操干货，100个案例带你从入门到进阶！-Part 2](https://mp.weixin.qq.com/s?__biz=MzI2NjY5NzI0NA==&mid=2247493274&idx=1&sn=323662b49f3b8e0d619d44315537a76a&scene=21#wechat_redirect)
    

对于初学者而言，如果能够熟练的掌握上面的 150 个案例，其实还是可以很好的学习到 Matplotlib 的不少精髓的。

最近，我又在研究 Matplotlib 在动画视频中的一些应用，在找资料的过程中，发现了一本不错的 Matplotlib 可视化书籍。

书名是《Scientific Visualization: Python + Matplotlib》，这本书是由来自法国计算机科学研究所的研究员 Nicolas P. Rougier 编写的，是一本关于使用 Python 和 Matplotlib 进行科学可视化的书籍。

书籍是可以开放获取的，其 github 地址如下：

https://github.com/rougier/scientific-visualization-book

## 02书籍介绍

之所以想给大家介绍下这本书，是因为我觉得书中确实有不少可以去学习的案例。

下面来分享下部分内容。

### 1\. Matplotlib 的发展历程

可能大家并不一定知道，其实 Matplotlib 库成立至今已经有 18个年头了， 1.0版于2011年发布，至今也已经10年了。

Matplotlib 可谓是一个有着持续生命力的 Python 库，可见其在应用中的广泛度。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

Matplotlib 的发展历程

### 2\. Matplotlib 图的层次结构

Matplotlib 图由层次结构丰富的多种元素组成，最终通过构图逻辑形成下图所示的实际图形。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

Matplotlib图的层次结构

这张图来自 Matplotlib 的官方文档，熟练理解图中的各种元素，有助于咱们深入理解 Matplotlib 的绘图技巧。

### 3\. 书中部分精彩的内容

这本书是针对进行科学计算可视化使用 Matplotlib 的书籍，具有一定的目的性。

在这本书中，基础部分的内容，其实讲的不是很多。毕竟这本书只有 200多页，如果要详细的描述 Matplotlib ，光基础内容，估计都不止 这些页数了。

所以，如果你以前没有接触过程 Matplotlib ，在学习本书内容时，需要有一些基础。不妨先了解下 Matplotlib 的一些基础知识。下面的100个案例，应该够用了~~

- [Matplotlib 100个案例-Part 1](https://mp.weixin.qq.com/s?__biz=MzI2NjY5NzI0NA==&mid=2247492863&idx=1&sn=4bb9f33a0f3b38232a31beb23c80c18a&scene=21#wechat_redirect)
    
- [Matplotlib 100个案例-Part 2](https://mp.weixin.qq.com/s?__biz=MzI2NjY5NzI0NA==&mid=2247493274&idx=1&sn=323662b49f3b8e0d619d44315537a76a&scene=21#wechat_redirect)
    

#### 密度曲线重叠图

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

上面的这张图的类型，有个英文名称，叫 “Joy Plot”，但中文翻译我也不知道怎么称呼了，姑且称之为 “密度曲线重叠图” 。

Joy Plot 允许不同组的密度曲线重叠，这是一种可视化大量分组数据的彼此关系分布的好方法。

#### 散点+直方图

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

上面这张图是散点图和直方图的组合，这种类型的图，我是很少见，觉得还是比较新颖的。大家不妨学习下。

#### 极坐标图

极坐标下绘图，可以有多种变化。

咱们常见的有 雷达图。

在本书中，也介绍了好几种极坐标图的绘制方法。图示如下：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

#### 轮廓图

等高线图属于轮廓图中的一种，用 Matplotlib绘制等高线图，也是挺不错的，如下：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

之前，我也分享过等高线怎么来绘制的：[Matplotlib 中等高线图的绘制](https://mp.weixin.qq.com/s?__biz=MzI2NjY5NzI0NA==&mid=2247484237&idx=1&sn=2a1c2339a9b9234cc54a47421139da05&scene=21#wechat_redirect)

在书中，还有一种轮廓图，我觉得很不错的，值得学习下，图示如下：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

将轮廓作为背景，将文字包裹在轮廓中，大家可以换个内容试试。

#### 堆积面积图

其实，堆积面积图是一种常见的图形，但我觉得书中的效果确实给人一种高大上的感觉。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

配色的魅力，大概就体验在这里了吧 （我自己的图，经常配色好丑~~）

关于配色，再看看下面这两幅图，是不是很惊艳呀

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

#### 注释对齐

下面的这张图，将文字说明的注释内容对齐，并用折线箭头指示，这种效果，还是挺美观的。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

#### 3D图

Matplotlib 的3D 绘制功能，其实是很强大的，只是可能我们平时用的不多，有所忽略。

你知道吗，用 matplotlib 还可以绘制一只兔子哈，如下：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

#### 科学领域

下一这幅图是基底神经节切片示意图，虽然我不明白具体的科学含义是什么，但我相信，在相关专业领域的人士会用到这些功能。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

#### 视觉之美

书中，还有不少精致的绘图，足以展现 Matplotlib 的强大之处，跟大家分享下图片效果（**可以左右滑动查看**）

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

&lt;<< 左右滑动见更多 &gt;>>

## 03小结

难能可贵的是，作者在这本书中提供的案例，都同时提供了相应的源代码，大家可以在其 Github 中获取。

https://github.com/rougier/scientific-visualization-book

考虑到 github 的访问速度有时很慢，我将书籍pdf版以及配套源代码也下载下来了，怎么获取呢？

点击下方卡片关注公众号「**Python数据之道**」，关注后回复 **matplot** 即可获取。

![](../../../_resources/0_wx_fmt_png_6061b226f9e64851a48c04e1925dbdb2.png)

**Python数据之道**

点击领取《Python知识手册》高清电子版，回复数字 “600” 获取。「Python数据之道」秉承“让数据更有价值”的理念​，聚焦于 Python 数据分析、数据可视化、AI、机器学习、深度学习等领域。

<a id="js_profile_article"></a>235篇原创内容

Official Account

<a id="js_view_source"></a>Read more

People who liked this content also liked

BIM价值的真正基础是模型质量

奇葩BIM

不看的原因

- 内容质量低
- 不看此公众号

SQL基础入门（二）

啦啦的小天地

不看的原因

- 内容质量低
- 不看此公众号

ggplot2优雅的绘制曲面条形图

R语言数据分析指南

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___f2fd8f9a2efe47ea8.bmp"/>

Scan to Follow