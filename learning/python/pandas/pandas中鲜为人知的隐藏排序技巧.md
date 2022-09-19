# pandas中鲜为人知的隐藏排序技巧

<a id="profileBt"></a><a id="js_name"></a>AI科技大本营 *2022-04-18 18:00*

The following article is from Python大数据分析 Author 费弗里

<a id="copyright_info"></a>[![](../../../_resources/0_c5e5112db43a46d99505e6a99c2975e2.jpg)<br>**Python大数据分析** .<br>分享python编程、可视化设计、大数据分析、机器学习等技术以及数据分析案例，包括但不限于pandas、numpy、spark、matplotlib、sklearn、tensorflow、keras、tableau等](#)

<img width="661" height="117" src="../../../_resources/640_wx_fmt_gif_wxfrom_5_wx_lazy__39774cc43e4c44d99.gif"/>

作者 | 费弗里

出品 | Python大数据分析

我们即将学习的是：`在pandas中实现自然排序顺序`。

自然排序顺序（Natural sort order），不同于默认排序针对字符串逐个比较对应位置字符的`ASCII`码的方式，它更关注字符串实际相对大小意义的排序，举个常见的例子，假如我们有下面这样的一张表，其中`value`字段是百分比格式的字符串：

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__3010f2665cf24e779.png)

这时如果直接照常基于`value`字段进行排序，得到的结果明显不符合数据实际意义：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

而我们今天要介绍的技巧，就需要用到第三方库`natsort`，使用`pip install natsort`完成安装后，利用其`index_natsorted()`对目标字段进行自然顺序排序，再配合`np.argsort()`以及`pandas`的`sort_values()`中的`key`参数，就可以通过自定义`lambda`函数，实现利用目标字段自然排序顺序进行正确排序的目的：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

可以看到，此时得到的排序结果完美符合我们的需求~

更多`natsort`知识欢迎前往`https://github.com/SethMMorton/natsort`学习更多。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![](../../../_resources/0_wx_fmt_png_cc2726363f7640e88c2cf14c4e35cbf0.png)

**AI科技大本营**

为AI领域从业者提供人工智能领域热点报道和海量重磅访谈；面向技术人员，提供AI技术领域前沿研究进展和技术成长路线；面向垂直企业，实现行业应用与技术创新的对接。全方位触及人工智能时代，连接AI技术的创造者和使用者。

<a id="js_profile_article"></a>1427篇原创内容

Official Account

往

期

回

顾

技术

[YYDS！Python实现自动驾驶](http://mp.weixin.qq.com/s?__biz=Mzg4NDQwNTI0OQ==&mid=2247558656&idx=2&sn=fe88af86dfb2f3954d5f55df5da061fc&chksm=cfbb036ef8cc8a782e96d954f729ae0a730185f476a3f6d3ee450442f1afad04e1944ebd3ad4&scene=21#wechat_redirect)

资讯

[我，机器学习工程师，决定跑路了](http://mp.weixin.qq.com/s?__biz=Mzg4NDQwNTI0OQ==&mid=2247558846&idx=1&sn=b5bc45c0738d715c1854ae36982e5c24&chksm=cfbb03d0f8cc8ac6912f57010150e4f972d7f47f3db3521f6fc7346b2c89a2f030483ebe19b3&scene=21#wechat_redirect)

技术

[一起用Python做个AI出牌神器！](http://mp.weixin.qq.com/s?__biz=Mzg4NDQwNTI0OQ==&mid=2247558772&idx=2&sn=6c88e394df011202f1e35657c5cfdd74&chksm=cfbb031af8cc8a0cb75be22579b260c4e74aa8b9bb9e048b043794f13d3105ad55aa7cb9b636&scene=21#wechat_redirect)

技术

[用Python打造一个语音合成系统](http://mp.weixin.qq.com/s?__biz=Mzg4NDQwNTI0OQ==&mid=2247558399&idx=1&sn=f7591eaea3f790e6fe7e06e5675f847f&chksm=cfbb0191f8cc88873e2ecdec59b33deb52555e4538fdea7be61ed3cac7c612ac5122428c3c74&scene=21#wechat_redirect)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

**分享**

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

**点收藏**

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

**点点赞**

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

**点在看**

People who liked this content also liked

pandas 分类数据处理大全

...

AI科技大本营

不看的原因

- 内容质量低
- 不看此公众号

整理20个Pandas统计函数

...

算法进阶

不看的原因

- 内容质量低
- 不看此公众号

机器学习分类建模—哪种机器学习算法适合目前的数据？

...

组学杂谈

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___676479c12d5e488f9.bmp"/>

Scan to Follow