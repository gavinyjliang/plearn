# 这份Pandas练习题，必须成功拿下~

<a id="copyright_logo"></a>Original <a id="js_author_name"></a>尤而小屋 <a id="profileBt"></a><a id="js_name"></a>尤而小屋 *2021-10-12 01:25*

<a id="js_article-tag-card__left"></a>收录于话题 #pandas <a id="js_article-tag-card__right"></a>50个

> 公众号：尤而小屋
> 作者：Peter
> 编辑：Peter

大家好，我是Peter~

写过很多Pandas的文章，主要讲解了常用的操作和函数的用法。今天自制了一份水果订单和销售的数据（模拟数据，仅供学习），主要是用来加深理解下如何灵活且快速使用Pandas来完成我们的需求。

<img width="657" height="252" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_ddd2004510eb459ba.jpg"/>

## Pandas文章

推荐几篇文章：

[30个Pandas高频使用技巧](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247497436&idx=1&sn=98ad06ef2e46c317fa461cd60972c97b&chksm=cf12e606f8656f10f45780051eef0113b7188f256b927a55d94bb725f83a298f19c235bf0973&scene=21#wechat_redirect)

[图解Pandas的轴旋转函数：stack和unstack](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247493111&idx=1&sn=e3c785fc786c061d36675d1bd596ad38&chksm=cf12f52df8657c3b0ac8927717234607008da17c63128f47031eadc9b2fe98d85070b292fa2e&scene=21#wechat_redirect)

[图解Pandas的groupby机制](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247493083&idx=1&sn=44fe0db5aa9a004315709da1b8e123dc&chksm=cf12f501f8657c177770b428fc5580871ae090b388156cb560a743fd2a666f7b33f8f14604c4&scene=21#wechat_redirect)

[创建DataFrame：10种方式任你选](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247493062&idx=1&sn=915f5cd239c9499bffd56b2c398ec202&chksm=cf12f51cf8657c0ade5433ca848aa07a8b4895d29f86d962c69a31854fce7095e020fe1aeabd&scene=21#wechat_redirect)
[](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247493083&idx=1&sn=44fe0db5aa9a004315709da1b8e123dc&chksm=cf12f501f8657c177770b428fc5580871ae090b388156cb560a743fd2a666f7b33f8f14604c4&scene=21#wechat_redirect)

<img width="637" height="531" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_9d347380f39845469.jpg"/>

## 数据讲解

1、模拟的第一份数据有5个字段：**订单号、下单人、商品、价格、数量**

<img width="657" height="920" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_f5485c144a54414a8.jpg"/>

- 订单号：每个订单的订单号，一个订单号中存在一个或者多个商品
    
- 下单人：一个人可能下1个或者多个订单，比如张三只下了一个订单，李四下了多个订单
    
- 商品：同一个商品可能在多个订单中出现
    
- 价格：每个订单中每个商品的价格，不同的订单中，同一个商品的价格都可能是不同的，比如SOD订单中苹果是10，但是在DFH订单中却是9.8
    
- 数量：每个订单中每个商品的销售数量
    

2、模拟的第二份数据中就两个字段：**商品和产地**

![Image](../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_f572c7eb8abf48529.jpg)

同时我们可以看到：**这两份数据是存在不同的sheet中的**，存储成为xlslx文件，并且没有任何的缺失值数据。

## 需求1：不同的方式读取数据

存在同一个Excel中的不同sheet中，我们采取不同的方式来读取：

**方式1：同时指定文件和sheet的名称**

```
import pandas as pd  # 先导入包

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

**方式2：指定文件名和sheet的索引号，索引从0开始**

<img width="657" height="268" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_4b5ac737a5604e859.jpg"/>

## 需求2：两份数据的合并

可以看到两个sheet中的数据是通过“商品”这个字段进行关联的，我们使用pandas中的merge函数，并且保留第一份（左边left）表格中的全部信息。

merge函数是一个非常重要的函数，可以灵活地处理Pandas中的数据合并问题。

<img width="657" height="735" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_f910ed2f14be43958.jpg"/>

**接下来的各种需求都是针对上面合并的数据进行处理的**

## 需求3：订单量、客户量、商品量

订单量：这份数据总共下了多少个订单

unique：中文是独特的意思，就是订单号这个字段有多个独特、唯一的信息。总共是7个订单

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

同样的道理：可以得到多少个下单用户、销售了多少种商品？

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 需求4：每个用户的下单量

就是求每个用户下了多少个订单：使用groupby进行分组统计每个下单人的订单量。

- 先使用groupby函数进行分组
    
- 再使用聚合函数nunique，统计每个“订单号”的个数（去重统计）
    
- 最后再索引重置下
    

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

看到李四下了3张订单，是最多的

## 需求5：每个用户的总消费金额

1、先增加一列：总额

<img width="657" height="617" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_1ec8649836474d38b.jpg"/>

2、两种不同方式的分组再聚合

<img width="657" height="411" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_02cca57c9bb84f2fb.jpg"/>

## 需求6：不同产地的订单量、销量、销售总额

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 需求7：每个订单中价格最高的商品

找出每个订单中价格最高的商品，比如：**SOD订单中价格最高的就是葡萄**

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

方式1：第一种实现的方式如下：

- 先整体通过降序排列
    
- 再根据订单号来分组，取出第一条first数据即可
    

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

方式2：实现方式如下

1、先实现每个订单号根据价格降序排列

<img width="657" height="499" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_39d3971f7f694c36b.jpg"/>

方式2：多个函数的混合使用，可分开运行查看每步骤的结果

```
df.groupby("订单号").apply(lambda x: x.sort_values("价格",ascending=False)).reset_index(drop=True).groupby("订单号").first().reset_index()

```

<img width="657" height="207" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_63468515809b447db.jpg"/>

方式3：分组的时候使用groupby_keys参数

<img width="657" height="208" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_3e989d891ca34e55a.jpg"/>

## 需求8：每个订单中价格最高的前2位

取出每个订单中价格最高的前2位，若只有一位取出一位即可。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

上面是取出分组后最高的数据，即第一条first。在这个需求中我们使用head函数，可以取出任意的n条数据：Top-N

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 需求9：每个商品的笔单价（保留2位小数）

我们来拆解题意：

- 每个商品：确定了分组的元素是groupby="商品"
    
- 笔单价：先求每个商品的总销售额，在求每个商品的订单数，最后相除
    

<img width="657" height="429" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_0100673e4ed64b48b.jpg"/>

如何对上面的商品笔单价保留两位小数呢？两种方法来实现：

<img width="657" height="468" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_f28dc0f6572e4ba4a.jpg"/>

<img width="17" height="36" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__bc9494581654471b9.png"/>

推荐阅读

<img width="17" height="36" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__377759a55d2140238.png"/>

[Python中的for循环，没你想的那么简单~](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247497839&idx=1&sn=0b842c638ba906e01a24bd7bf7dfefb2&chksm=cf12e8b5f86561a379238e443b0e60950dc192237fb8635c4e1b3228f0a1478a9b46672c6af8&scene=21#wechat_redirect)

[大揭秘：必须学会的Python数据分析利器](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247497719&idx=1&sn=e49a68ef1ba58b3c4105a38267f13ac6&chksm=cf12e72df8656e3b55b442effdb1f0a3c3b34c1a52b1fcfeb538bb380f0d1cec497925ac7886&scene=21#wechat_redirect)

[赞！可视化神器Plotly的图例Legend精讲](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247497344&idx=1&sn=d314c3a3a67a07d16a4c59f69a147906&chksm=cf12e65af8656f4c75f2a035fd8fdd42cb787de378807d9f8924dea9e62de4ebbc11c3b94137&scene=21#wechat_redirect)

[Python入门：3000字深入剖析变量和赋值](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247497128&idx=1&sn=3a44b82d7178e4253104dae7d0ca15be&chksm=cf12e572f8656c641555d5f9c4baa75b7c44b4f932c0cb0275e9ea8a8e19a3b5dda6d3b81b49&scene=21#wechat_redirect)

[字节数据分析面试：你会Tableau吗？](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247496904&idx=1&sn=f78a77bcb9569d6975e7fed6148b637c&chksm=cf12e412f8656d04282f8bd68c2b9734a093bbfdfcfe3625e69a618976f44703680c2c187f26&scene=21#wechat_redirect)

> 尤而小屋，一个温馨的小屋。小屋主人，一手代码谋求生存，一手掌勺享受生活，欢迎你的光临

![](../../../_resources/0_wx_fmt_png_c9a35d45fef14f2eb3eb6a6d5cf3761c.png)

**尤而小屋**

尤而小屋，一个温馨且有爱的小屋🏡 小屋主人，一手代码谋求生存，一手掌勺享受生活，欢迎你的光临~

<a id="js_profile_article"></a>216篇原创内容

Official Account

收录于话题 #<a id="js_album_keep_read_title"></a>pandas

 <a id="js_album_keep_read_size"></a>50个

<a id="js_album_prev"></a>上一篇 <a id="js_album_keep_read_pre_title"></a>对比SQL，学习Pandas操作：group_concat如何实现？ <a id="js_album_next"></a>下一篇 <a id="js_album_keep_read_next_title"></a>30个Pandas高频使用技巧

<a id="js_view_source"></a>Read more

People who liked this content also liked

PostgreSQL数据库备份恢复迁移——Barman Before you start\[翻译\]

肥叔菌

不看的原因

- 内容质量低
- 不看此公众号

Hive-数据分析-数据打分

趣说大数据

不看的原因

- 内容质量低
- 不看此公众号

Golang连接Oracle数据库两种方式

阿亮说技术

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___29eea886c7b54837b.bmp"/>

Scan to Follow