# 精华！Pandas数据排序实现

<a id="copyright_logo"></a>Original <a id="js_author_name"></a>尤而小屋 <a id="profileBt"></a><a id="js_name"></a>尤而小屋 *2022-01-02 00:18*

收录于话题

<a id="js_article_tag_name__1999223977226862596"></a>#pandas <a id="js_article_tag_num__1999223977226862596"></a>50 <a id="js_article_tag_tips__1999223977226862596"></a>个

<a id="js_article_tag_name__1999223977696624640"></a>#机器学习 <a id="js_article_tag_num__1999223977696624640"></a>72 <a id="js_article_tag_tips__1999223977696624640"></a>个

<a id="js_article_tag_name__2019120411547860993"></a>#数据挖掘 <a id="js_article_tag_num__2019120411547860993"></a>37 <a id="js_article_tag_tips__2019120411547860993"></a>个

<a id="js_article_tag_name__1999223975666581509"></a>#数据分析 <a id="js_article_tag_num__1999223975666581509"></a>104 <a id="js_article_tag_tips__1999223975666581509"></a>个

> 公众号：尤而小屋
> 作者：Peter
> 编辑：Peter

大家好，我是Peter~新年快乐

这是新年的第一篇文章，请大家多多支持！

在以前的一篇文章   [图解Pandas的排序机制sort_values](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247493088&idx=1&sn=45be6a272a2da35fdec1311d52b643c7&chksm=cf12f53af8657c2c4ec80e91f741627edbb1c589a1155aebd2d7ee85f3241fd6940d165a42e8&scene=21#wechat_redirect)   详细介绍了如何使用pandas的内置函数sort_values来实现数据的排序。本文讲解的是如何使用自定义方式来实现排序：

- 映射关系实现
    
- CategoricalDtype类型实现
    

<img width="657" height="252" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_99c6e785337545bd8.jpg"/>

## 模拟数据

先模拟一份简单的数据：

```
import pandas as pd
import numpy as np
df = pd.DataFrame({
    "nick":["aaa","bbb","aba","abc","cac","ccc"],  # 昵称
    "math":[100,120,130,111,100,128],  # 数学
    "english":[140,80,120,90,125,116],  # 英语
    "size":["S","M","L","XS","XL","L"]   # 衣服大小
    })
df

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## sort_values

```
DataFrame.sort_values(by, 
               axis=0, 
               ascending=True, 
               inplace=False, 
               kind='quicksort', 
               na_position='last', # last，first；默认是last
               ignore_index=False, 
               key=None)

```

参数的具体解释为：

- by：表示根据什么字段或者索引进行排序，可以是一个或多个
    
- axis：排序是在横轴还是纵轴，默认是纵轴axis=0
    
- ascending：排序结果是升序还是降序，默认是升序
    
- inplace：表示排序的结果是直接在原数据上的就地修改还是生成新的DatFrame
    
- kind：表示使用排序的算法，快排quicksort,，归并mergesort， 堆排序heapsort，稳定排序stable ，默认是 ：快排quicksort
    
- na_position：缺失值的位置处理，默认是最后，另一个选择是首位
    
- ignore_index：新生成的数据帧的索引是否重排，默认False（采用原数据的索引）
    
- key：排序之前使用的函数
    

下面通过几个简单的例子来复习下sort_values的使用：

### 单个字段排序

通过nick字段排序，字符串是根据字母的ASCII码；默认是从小到大的升序。第一个字母相同，则比较第二个，类推：

<img width="657" height="409" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_11c237ea54354c56b.jpg"/>

根据数值的大小来升序排列：

<img width="657" height="368" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_8203aa7a99d24d6f9.jpg"/>

可以将排序方式改为降序：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 多个字段排序

多个字段的同时排序，默认也是升序。当第一个字段的取值相同，再根据第二个字段来升序排列

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

给不同的字段指定不同的排序方式：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

再完整地对比下两种不同的方式：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

上面的就是sort_values方法的常见排序方式。

## 自定义排序

使用sort_values方法排序的时候都是内置的字母或者数值型数据的大小直接来排序，当遇到下面的情况，该如何操作？

当我们根据衣服的大小size来排序，得到的结果是：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

明显这样的排序方式不是我们理想中的样子，在我们的认知中：

- XS：很小
    
- S：小
    
- M：中等
    
- L：大
    
- XL：超大
    

该如何解决这个问题？提供两种方式：

### 方法1：通过映射

1、先找到每个size的顺序对应的数值大小

2、生成新的字段order

3、我们对order进行排序

<img width="657" height="436" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_a7a08d853a0a4d448.jpg"/>

<img width="657" height="372" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_f2d92521244c4b2db.jpg"/>

### 方法2：使用CategoricalDtype

CategoricalDtype是具有类别和顺序的分类数据的类型，能够创建我们自定义的排序数据类型。官网地址：

https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.CategoricalDtype.html

1、指定一个分类的数据类型CategoricalDtype

```
category_size = pd.CategoricalDtype(
    ['XS', 'S', 'M', 'L', 'XL'], 
    ordered=True)
category_size

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

2、将size字段设置成上面的CategoricalDtype类型

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

3、我们直接对size使用sort_values就可以达到我们的目的，和上面的map映射的效果是相同的

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

而且通过查看df的数据类型，我们也看到size的类型是category：

![Image](../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_18ba2e8d7fd14a518.jpg)

<img width="17" height="36" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__767526bf60354f539.png"/>

推荐阅读

<img width="17" height="36" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__71f51e54e0da4adb9.png"/>

[精选23个Pandas常用函数](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247502204&idx=1&sn=030b34c84657cfa6a04e2afee7836178&chksm=cf12d9a6f86550b085e6f5a0385d4346fa34038fd7aeecef6bf45d1b02dd6561bd2991c4956e&scene=21#wechat_redirect)

[空间数据可视化神器keplergl](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247502024&idx=1&sn=3efc14c2d9347e4cc018dbe5fe72d5ef&chksm=cf12d812f8655104daf97633c853ed19ca33abc2ffb4c5dd9daa30e85a12a62a194e61937080&scene=21#wechat_redirect)

[从统计和数据角度出发，如何看待房价？](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247500282&idx=1&sn=2b2179c133a2957917f8cc364a8949ff&chksm=cf12d120f86558362a9e915fe6e1ce147d14ff78a09b44e885046c27f0df65895fb6b060315c&scene=21#wechat_redirect)

[图解Pandas的排序机制sort_values](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247493088&idx=1&sn=45be6a272a2da35fdec1311d52b643c7&chksm=cf12f53af8657c2c4ec80e91f741627edbb1c589a1155aebd2d7ee85f3241fd6940d165a42e8&scene=21#wechat_redirect)

[Pandas行列转换的4大技巧！](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247501523&idx=1&sn=f4a011bec8abc547ad2ffd6fb8c06ef4&chksm=cf12d609f8655f1f0c781431e4d7f39ed202a1a799d372eb5db74da3a06e8e13c74248b4d218&scene=21#wechat_redirect)

[Plotly+Pandas+Sklearn：实现用户聚类分群！](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247500041&idx=1&sn=e29f380b176df342a03850f655d09991&chksm=cf12d1d3f86558c571b088bd846207f22b150e290dc660fe93125d9cf813594515153a6f0c68&scene=21#wechat_redirect)

[Python爬虫：渣男 or 渣女？上十字架](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247501904&idx=1&sn=71b17c7ff5d8d28c151ee8a80809f7e2&chksm=cf12d88af865519ca7f8d24f9cd67f3c617d1304bfe2b34abf3f9d9f4b7391b0876e0aeb6f40&scene=21#wechat_redirect)

> 尤而小屋，一个温馨的小屋。小屋主人，一手代码谋求生存，一手掌勺享受生活，欢迎你的光临

![](../../../_resources/0_wx_fmt_png_67ff696b038e42b59204831e80813324.png)

**尤而小屋**

尤而小屋，一个温馨且有爱的小屋🏡 小屋主人，一手代码谋求生存，一手掌勺享受生活，欢迎你的光临~

<a id="js_profile_article"></a>216篇原创内容

Official Account

收录于话题 #<a id="js_album_keep_read_title"></a>pandas

 <a id="js_album_keep_read_size"></a>50个

<a id="js_album_prev"></a>上一篇 <a id="js_album_keep_read_pre_title"></a>小而全的Pandas使用案例 <a id="js_album_next"></a>下一篇 <a id="js_album_keep_read_next_title"></a>精选23个Pandas常用函数

<a id="js_view_source"></a>Read more

People who liked this content also liked

SQL数据查询 | 超全思维导图

良松数据分析

不看的原因

- 内容质量低
- 不看此公众号

在R语言中为回归结果添加森林图——bstfun包的应用

BNUlgr

不看的原因

- 内容质量低
- 不看此公众号

使用 nmon 对 Linux 系统性能进行故障排除和监控

毛毛虫AI进化之旅

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___615d32e3f51542cba.bmp"/>

Scan to Follow