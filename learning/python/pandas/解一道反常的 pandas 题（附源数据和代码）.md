# 解一道反常的 pandas 题（附源数据和代码）

<a id="profileBt"></a><a id="js_name"></a>数据分析与开发 *2021-04-03 11:50*

The following article is from 数据不吹牛 Author 小z

<a id="copyright_info"></a>[![](../../../_resources/0_6a2ae499e3b1457593d6d7b3e12d5e57.jpg)<br>**数据不吹牛** .<br>有趣+干货的数据分析宝藏](#)

潘大师（Pandas）基础教程和实战案例我写了不少，增、删、改、查这样的常规操作，感兴趣的同学多看、多练基本上都能掌握的差不多。

但是，实际业务场景，由于各种原因，总会有一些反常的需求。今天这个反常又有代表性的需求，来源于粉丝的提问，相关数据已经做了完全脱敏处理，供大家实战练手。

**需求背景**

有两张表，A表记录了很多款产品的三个基础字段，分别是产品ID，地区代码和重量：

<img width="346" height="218" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__1360956658084dfc9.png"/>

B表是运费明细表，这个表结构很“业务”。每行对应着单个地区，不同档位重量，所对应的运费：

<img width="661" height="160" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__4906a33d3cef480d9.png"/>

比如121地区，0-0.5kg的产品，运费是5.38元；2.01（实际应该是大于1）-3kg，运费则是5.44元。

现在，我们想要结合A表和B表，统计出A表每个产品付多少运费，应该怎么实现？

可以先自己思考一分钟<img width="20" height="20" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__1331ad18d8724426b.png"/>

**解题思路**

 **人海战术**

任何数据需求，在人海战术面前都是弟弟。

A表一共215行，我们只需要找215个人，每个人只需要记好自己要统计那款产品的地区代码和重量字段，然后在B表中根据地区代码，找到所在地区运费标准，然后一眼扫过去，就能得到最终运费了。

两个“只需要”，问题就这样easy的解决了。

问题变成了，我还差214个人。

 **解构战术**

通过人海战术，我们其实已经明确了解题的朴素思路：根据地区代码和重量，和B表匹配，返回运费结果。

难点在于，B表是偏透视表结构的，运费是横向分布，用Pandas就算用地区代码匹配，还是不能找到合适的运费区间。

怎么办呢？

如果我们把B表解构，变成“源数据”格式，问题就全部解决了：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

转换完成后，和A表根据地区代码做一个匹配筛选，答案就自己跑出来了。

下面是动手时刻。

**具体实现**

先导入数据，A表（product）：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

B表（cost）：

<img width="661" height="205" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__bd50c24b94e240e19.png"/>

要想把B表变成“源数据”的格式，关键在于理解stack()堆叠操作，结合示例图比较容易搞懂：

<img width="661" height="293" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__7f518680612d40ce9.png"/>

通过stack操作，把多列变为单列多行，原本的2列数据堆成了1列，从而方便了一些场景下的匹配。要变回来也很简单，unstack即可：

<img width="661" height="298" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__e54290e7272148229.png"/>

在我们的具体场景中，先指定好不变的索引列，然后直接上stack：

<img width="661" height="365" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__038363a6b6f946588.png"/>

这样，就得到了我们目标的源数据。接着，A表和B表做匹配：

<img width="661" height="240" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__1f7134f1a17645878.png"/>

值得注意的是，因为我们根据每个地方的重量区间做了堆叠，这里的匹配结果，每个产品保留了对应地区，所有重量区间的价格，离最终结果还有一步之遥。

需要把重量区间做拆分，从而和产品重量对比，找到对应的重量区间：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

接着，根据重量的最低、最高区间，判断每一行的重量是否符合区间：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

最后，筛选出符合区间的产品，及对应的价格等字段：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

大功告成~

这道题也不止一种解法，为方便感兴趣的同学练手，老规矩，文中的案例数据源和代码，已经打包整理好，公众号后台回复：**fc** 即可。

\- EOF -

推荐阅读  点击标题可跳转

<ins>1、[100 个 pandas 数据分析函数总结](http://mp.weixin.qq.com/s?__biz=MzA5ODM5MDU3MA==&mid=2650871526&idx=1&sn=3d538c9bab078f9f72f772ad5b65fa88&chksm=8b67f1a3bc1078b51d3a1ad605de5bdfbdf45167ac79cff8831a175a89c01a3610932c9539c1&scene=21#wechat_redirect)</ins>

<ins>2、[Pandas 必知必会的使用技巧，值得收藏！](http://mp.weixin.qq.com/s?__biz=MzA5ODM5MDU3MA==&mid=2650869810&idx=2&sn=cea7c191f87235d091c30191eda28abc&chksm=8b67f777bc107e615e747b266937c032624708209d0339086d0edff9fe69fc843ee1aa696b4d&scene=21#wechat_redirect)</ins>

<ins>3、[用 pandas-profiling 做出更好的探索性数据分析](http://mp.weixin.qq.com/s?__biz=MzA5ODM5MDU3MA==&mid=2650866936&idx=2&sn=af894fc31c49cf35006c091638f9816f&chksm=8b67e3bdbc106aab0be79c89d3cb319bbb7038a48aa8b07dacd94a5e03ef8f2e61785b305dc6&scene=21#wechat_redirect)</ins>

看完本文有收获？请转发分享给更多人

**推荐关注「数据分析与开发」，提升数据技能**

![](../../../_resources/0_wx_fmt_png_4d5f07281f1d4d0499155947cfb4e75c.png)

**数据分析与开发**

「数据分析与开发」分享数据分析与开发相关技术文章、教程、工具

<a id="js_profile_article"></a>55篇原创内容

Official Account

点赞和在看就是最大的支持❤️

People who liked this content also liked

我学习 Python 的三个神级网站

数据分析与开发

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___49327d7eb456478fb.bmp"/>

Scan to Follow