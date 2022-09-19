# 天秀！一张图就能彻底搞定Pandas！

<a id="profileBt"></a><a id="js_name"></a>SQL数据库开发 *2022-04-28 08:30* *Posted on <a id="js_ip_wording"></a>广东*

The following article is from 早起Python Author 刘早起

<a id="copyright_info"></a>[![](../../../_resources/0_a1fd3af561bb42fba219569d39ee6171.jpg)<br>**早起Python** .<br>点击领取pandas数据分析300题](#)

![](../../../_resources/0_wx_fmt_png_3aa2acb851b849efb3d86314e0b36596.png)

**SQL数据库开发**

专注数据相关领域，主要分享MySQL，数据分析，Python，Linux ，大数据等相关技术内容，关注回复「1024」获取资源大礼包。

<a id="js_profile_article"></a>426篇原创内容

Official Account

大家好，在三月初，我曾给大家分享过一份Matplotlib绘图小抄

[](https://mp.weixin.qq.com/s?__biz=MzI1MTUyMjc1Mg==&mid=2247484355&idx=1&sn=cebaca6ad0d7e6495d68dd38ff5a2fdf&scene=21#wechat_redirect)<img width="667" height="472" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_0174f5d2326f4b71b.jpg"/>

昨天在面向GitHub编程时，无意发现了Pandas官方竟提供了同款小抄，项目地址如下

```
`https://github.com/pandas-dev/pandas/blob/master/doc/cheatsheet/Pandas_Cheat_Sheet.pdf
`
```

<img width="667" height="181" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_10776cd668214927a.jpg"/>可以看到这份小抄提供了PPT和PDF两个版本，虽然最新一条更新记录为两年前，但是并不影响我们拿来学习，下面我们来看看这份**小抄(速查表)** 的强大！

这份速查表一共有两页，我已经将它转换为图片👇发在公众号可能会被压缩，你可以**在文末下载高清大图**<img width="667" height="516" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_24d4393b6c384f329.jpg"/><img width="667" height="516" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_9344811dd48a40db8.jpg"/>

经过一番研究，这两张图片一共覆盖了12个常用的Pandas操作👇

# 1、数据创建

介绍了几种常用的DataFrame创建语法<img width="667" height="1262" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_98fb1057ee7149e1a.jpg"/>

# 2、数据重塑

这部分主要是一些在数据清洗中常用的方法，比如**数据连接、数据排序、数据删除**等，并且还对四个常用的操作给出了图示，理解起来简直不要太方便！<img width="667" height="244" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_9c087696d2bd4a60a.jpg"/>

# 3、数据筛选

这一块区域主要是分别用行/列来讲解一些常用的数据查看、抽样、切片等操作，包含了`tail`、`head`、`loc`、`iloc`等非常重要的方法，并且同样给出了部分动画便于理解<img width="667" height="316" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_5eeb13f69a754bf99.jpg"/>

# 4、数据探索

这一块主要给出了一些在进行探索性分析时常用的方法，比如`max`、`min`、`count`等，不过官方将apply放在这里，并没有展开讲解<img width="667" height="852" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_37ec364d66f642d98.jpg"/>

# 5、数据修改

这两个区域为缺失值处理和创建新的列，重点用动画示例了`assign`和`qcut`方法，缺失值处理部分仅给出了两个方法，应该是偷懒了<img width="667" height="887" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_f40704376a504f70b.jpg"/>

# 6、数据分组

主要就是`groupby`和相关方法![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

# 7、数据连接

这里介绍的还是非常详细！用图片例子来展示`pd.merge`中的各种参数变化的不同，一看就懂<img width="667" height="1567" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_0a0b38cf0b244809b.jpg"/>

以上就是我对这份小抄的基本概括，其实大家应该清楚，**仅仅靠靠两张图片根本没法把整个Pandas学明白**，所以官方也**有选择性的对一些重要的方法给出了详细的讲解，而有些功能则一笔带过**，比如我之前👉[花很大力气介绍的pandas绘图功能](https://mp.weixin.qq.com/s?__biz=MzI1MTUyMjc1Mg==&mid=2247508012&idx=1&sn=38ad0e2f1db88aa6e3b570657a60a286&scene=21#wechat_redirect)仅给出了区区一角<img width="667" height="272" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_5ec285aca83c44538.jpg"/>

**所以你应该这样用这份小抄**，把它当成速查表，**「用于了解哪些操作可以用Pandas完成」**，**在你不确定或者不明白如何处理数据时，通过这份速查表快速查到Pandas中的哪个方法可以完成**，之后再进一步通过搜索学习对应的方法！

好了，以上就是本文全部内容，因为微信会对图片进行压缩，所以你可以在后台回复**Pandas**获取**高清、完整、可复制文字版本**Pandas速查表！

![Image](../../../_resources/640_wx_fmt_gif_wxfrom_5_wx_lazy__bd0bd0da62124806a.gif "音符")

我是岳哥，最后给大家分享我写的SQL两件套：**《SQL基础知识第二版》**和**《SQL高级知识第二版》**的PDF电子版。里面有各个语法的解释、大量的实例讲解和批注等等，非常通俗易懂，方便大家跟着一起来实操。

有需要的读者可以下载学习，在下面的公众号「**数据前线**」(非本号)后台回复关键字：**SQL**，就行

**数据前线**

<img width="244" height="244" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_4357f1c796414fa0b.jpg"/>

**——End——**

#### 

```


后台回复关键字：1024，获取一份精心整理的技术干货

后台回复关键字：进群，带你进入高手如云的交流群。

```


推荐阅读

- [一千行 MySQL 学习笔记](http://mp.weixin.qq.com/s?__biz=MzA3MTg4NjY4Mw==&mid=2457326778&idx=2&sn=955e62a5136fc2b31c31bde258084982&chksm=88a5c68ebfd24f986416e16c41afd82fb88fa5c486812f93e8f9f4489ec6c5f0d29ea5277448&scene=21#wechat_redirect)
    
- [SQL自定义排序](http://mp.weixin.qq.com/s?__biz=MzA3MTg4NjY4Mw==&mid=2457326649&idx=2&sn=f84c02633c15cbac9527ec85818b8266&chksm=88a5c60dbfd24f1b2d4df7f3be8486def1f3a63f978f30bc1691d5fad58dbb05678aa1f43869&scene=21#wechat_redirect)
    
- [SQL 性能优化梳理](http://mp.weixin.qq.com/s?__biz=MzA3MTg4NjY4Mw==&mid=2457326564&idx=2&sn=1564e56ff3d2f8fbf000146d774a870b&chksm=88a5c1d0bfd248c6f19bb59f238f17ef712c9f1dcaa248d90e515c239c82548009f326fd4ada&scene=21#wechat_redirect)
    
- [一行SQL代码能做什么？](http://mp.weixin.qq.com/s?__biz=MzA3MTg4NjY4Mw==&mid=2457326590&idx=1&sn=edff5602c22c607663dd8e09db13b3d6&chksm=88a5c1cabfd248dc268cfd7c268181b8ea594d45bd1891fc365934fc5a9cc4c60313842288a9&scene=21#wechat_redirect)
    
- [安利几款我常用的数据软件](http://mp.weixin.qq.com/s?__biz=MzA3MTg4NjY4Mw==&mid=2457326486&idx=1&sn=d44e8bf6286230996c26ac377196362d&chksm=88a5c1a2bfd248b4820a1e490827519a139047cabae2b0f4329ee6c33ec4cc6071bccd2bae88&scene=21#wechat_redirect)<img width="544" height="151" src="../../../_resources/640_wx_fmt_gif_wxfrom_5_wx_lazy__1268ed22dc124a889.gif"/>
    


```


```

People who liked this content also liked

跟着Nature Plants学作图：R语言ggplot2画变种火山图

...

小明的数据分析笔记本

不看的原因

- 内容质量低
- 不看此公众号

办公中最常用的函数公式，赶紧收藏，要用时直接套！

...

Word联盟

不看的原因

- 内容质量低
- 不看此公众号

故障分析 | 血的教训-由慢查询引发的备份等待导致数据库连接打满

...

爱可生开源社区

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___fbd6236dcf1d4c058.bmp"/>

Scan to Follow