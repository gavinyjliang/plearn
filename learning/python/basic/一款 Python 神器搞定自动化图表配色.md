# 一款 Python 神器搞定自动化图表配色

<a id="profileBt"></a><a id="js_name"></a>开源前哨 *2021-11-10 21:13*

The following article is from 快学Python Author 朱小五

<a id="copyright_info"></a>[![](../../../_resources/0_f3ba9134bbc74184a1616ca4a18d7d8f.png)<br>**快学Python** .<br>Python可视化、自动化办公、数据分析、爬虫、Web开发！人生苦短，快学Python！](#)

我们在利用Python进行数据可视化时，有着大量的高质量库可以用，比如：**Matplotlib**、**seaborn**、**Plotly**、**Bokeh**、**ggplot**等等。但图表好不好看，配色占一半。如果没有良好的审美观，很容易做出来的东西辣眼睛……

所以想做好数据可视化，就要有合适的配色方案。除了可以借鉴参考配色网站的案例，也可以自己自定义一套配色方案。

<img width="677" height="245" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__50e8a1f8775b44cca.png"/>

如何去自定义呢？

我倒是有一个想法，配色的美感需要培养，但在一开始可以在优秀的作品上寻找灵感，比如经典电影、海报、风景图、Logo等等，这些都是绝佳的参考。

自然风景的颜色往往令人惊艳，咱们不妨以风景图为例。下图是一副海上夕阳图，通过一番操作就提取到了一套配色方案（见图右）。

<img width="677" height="383" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__f5cac21b089941c5a.png"/>

那么，我们用Python能不能做到呢？

答案当然是可以，毕竟Python除了不能生孩子，什么都能做！

## 提取图片中的配色

在Python中对图片进行操作，最常用的两个模块就是PIL和opencv了。所以一开始我的方案是，用Python库打开图片，然后遍历像素颜色，最后按照色彩比例进行排序，即可得到该图片的配色方案。

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__1e23fca49fef40d5a.png)

结果做到一半，我发现自己忽略了一件事。大家都知道，Python 是一门优雅的语言，简洁的语法，强大的功能。同时它还有拥有极其丰富的第三方库，这些库几乎都可以在github 或者 pypi上找到源码。

于是我搜了一下，确实有相关的库可以提取图片中的配色，那我们就不用重复造轮子了。

这个模块就是——**Haishoku**，可以用于从图像中获取主色调和主要配色方案。

<img width="414" height="173" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__faec2428f1004530a.png"/>

其GitHub网址为：*https://github.com/LanceGin/haishoku*

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__81a3378f338941cda.png)

具体用法，还是先安装

```
pip install haishoku

```

将前文提到的海上夕阳图，保存到本地并命名为`test.png`。

```
from haishoku.haishoku import Haishoku
image = 'test.png'
haishoku = Haishoku.loadHaishoku(image)

```

导入模块，运行代码会返回一个Haishoku实例，你可以通过实例属性`haishoku.dominant` 和 `haishoku.palette`，从而直接获取到对应的主色调和配色方案。

### 主色调

首先，要怎么获取图片的主色调呢？

```
print(haishoku.dominant)

```

这返回了一个结构为 (R, G, B) 的元组，就是该图片的主色调。

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__55ea6bb618b54ffba.png)

运行下面这行代码

```
Haishoku.showDominant(image)

```

则会打开一个临时文件，用来预览主色调的颜色。

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__022f59f6adfd42d5a.png)

主色调（最多的颜色）

### 配色方案

```
#获取配色方案
pprint.pprint(haishoku.palette)

```

返回一个结构为：\[(R, G, B), (R, G, B), …\] 最大长度为8的数组。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

这里使用了`pprint`模块，对于这种多层嵌套的元组，正好可以美观地打印出来。

运行下面这行代码

```
Haishoku.showPalette(image)

```

则会打开一个临时文件，用来预览图片配色方案。（不会保存在本地）

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

配色方案

就这样，只需几行代码就提取到图片中的配色方案，是不是很简单。

另外，Haishoku库从`v1.1.4`版本后，支持从 url 中直接加载图像。

```
imagepath = 'https://img-blog.csdnimg.cn/20190222215216318.png'
    
haishoku = Haishoku.loadHaishoku(imagepath)

```

## 配色方案与可视化

通过前面的操作，我们就提取到了合适的配色，那么就实战一下吧。

经典电影、海报、风景图、Logo都是绝佳的参考对象。

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__0b0c748c48554dc09.png)

所以这次，我选择了Google的Logo，并提取到它的配色方案。

```
imagepath = 'google.png'
haishoku = Haishoku.loadHaishoku(imagepath)
pprint.pprint(haishoku.palette)
Haishoku.showPalette(imagepath)

```

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__6cda8746d1fc4e4b9.png)

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__fb3b48e1536b46599.png)

那么，这套配色方案应用到了数据可视化中，会是怎么样呢？？

这次用刚才得到的Google配色，Python绘制一个环形图试试看

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__316c465455844d378.png)

感觉还不错，这套配色方案我要收藏起来。如果大家觉得本文还不错，记得给个一键三连！

<img width="677" height="127" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__cac70868d46d47e5a.png"/>

- EOF - 

**更多优秀开源项目**（点击下方图片可跳转）

[![Image](../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_393a7edcbbea47f7a.jpg)](http://mp.weixin.qq.com/s?__biz=MzAxMDM0MzQ4Mg==&mid=2451062056&idx=1&sn=7f1bffb079ab11f50786a9c981d57eab&chksm=8cbd587dbbcad16b1994f83b9e4dc9916c949d62e86af3b72acdb1c7e9d328ac2f5c8905de31&scene=21#wechat_redirect)

[![Image](../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_dcb5887cb59849d29.jpg)](http://mp.weixin.qq.com/s?__biz=MzAxMDM0MzQ4Mg==&mid=2451062102&idx=1&sn=1dd3c874174a5ed3df631c625d4a76fe&chksm=8cbd5803bbcad11569e4f4e7d2948109de52fac0bdbce31073b898a0dbe0911a0e63a257d20f&scene=21#wechat_redirect)

[![Image](../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_1ec3e24571034572b.jpg)](http://mp.weixin.qq.com/s?__biz=MzAxMDM0MzQ4Mg==&mid=2451062155&idx=1&sn=f400a66e6fa503e5f7bc6fdcaf4bd7b2&chksm=8cbd58debbcad1c847c169a599c06582a871022e8d713d2ca98fa4a791901a4969484b5f07f5&scene=21#wechat_redirect)

* * *

**开源前哨**

日常分享热门、有趣和实用的开源项目。参与维护10万+star 的开源技术资源库，包括：Python, Java, C/C++, Go, JS, CSS, Node.js, PHP, .NET 等

<img width="132" height="132" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_e45edf60d42642908.jpg"/>

**关注后获取**

回复 资源 获取 10万+ star 开源资源

分享、点赞和在看

支持我们分享更多优秀开源项目，谢谢！

People who liked this content also liked

Python从入门到放弃day1

我要当咸鱼

不看的原因

- 内容质量低
- 不看此公众号

70个Python练手项目列表，偷偷练习卷死他们，得不到的永远在骚动

编程小姐姐的糖果铺

不看的原因

- 内容质量低
- 不看此公众号

python小记（2）

找Bug

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___d0216163f591406d9.bmp"/>

Scan to Follow