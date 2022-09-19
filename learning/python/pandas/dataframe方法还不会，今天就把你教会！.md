# dataframe方法还不会，今天就把你教会！

<a id="copyright_logo"></a>原创 pandas <a id="profileBt"></a><a id="js_name"></a>Python教程初学详解 *2022-05-14 20:00* *发表于<a id="js_ip_wording"></a>北京*

收录于合集

<a id="js_article_tag_name__2393442551711301635"></a>#pandas <a id="js_article_tag_num__2393442551711301635"></a>18 <a id="js_article_tag_tips__2393442551711301635"></a>个

<a id="js_article_tag_name__2393442551778410499"></a>#dataframe <a id="js_article_tag_num__2393442551778410499"></a>6 <a id="js_article_tag_tips__2393442551778410499"></a>个

<img width="43" height="45" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__45cb82cf60544684a.png"/>

*点击*

***蓝字***

*关注我们~*

上篇文章已经介绍了series的基本用法了，这篇文章我们继续数据结构中DataFrame用法：

<img width="50" height="36" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__5c2d69d6dcfb4536a.png"/>

<img width="23" height="19" src="../../../_resources/640_wx_fmt_gif_wxfrom_5_wx_lazy__2e8fd9be26ef4cae8.gif"/>

1.DataFrame相当于excel表中的一张表。

<img width="34" height="34" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__8587eadbc71f48048.png"/>

它的基本用法和series格式是差不多的，长这个样子：
pd.DataFrame(data,index=index,columns)
DataFrame和series基本格式差不多，不同的是DataFrame代表着一张表，需要设置行索引和列索引才能完整的构成一张表，即index（行索引）的设置和columns（列索引）的设置。

<img width="34" height="34" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__20d62f49a63e4b22a.png"/>

<img width="30" height="11" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__a583b53e83d24bda8.png"/>

情况一：data的值可以是字典，比如这样子的：

![图片](../../../_resources/640_wx_fmt_gif_wxfrom_5_wx_lazy__144a7e517b9e4124b.gif)

import pandas as pd
d = {"one": \[1.0, 2.0, 3.0, 4.0\], "two": \[4.0, 3.0, 2.0, 1.0\]}
p=pd.DataFrame(d)
print(p)
我们首先定义了一个字典，就是他：
d = {"one": \[1.0, 2.0, 3.0, 4.0\], "two": \[4.0, 3.0, 2.0, 1.0\]}
这个字典呢，必然是有健值对了，那么跟series差不多，它的键就会被当成了列索引来使用，行索引就会被系统自定义，因此我们可以不用设置index（行索引）和columns（列索引）。运行下看看效果实在这样的（如下图，图片标注有误，大家理解下哈），是不是？

<img width="677" height="151" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_5adc2cdffa3e42878.jpg"/>

![图片](../../../_resources/640_wx_fmt_gif_wxfrom_5_wx_lazy__144a7e517b9e4124b.gif)

但是现实的情况是，我们会根据实际情况，想到更索引，怎么办呢？，比如我不想要自定义的行索引，我想设置为abcd，直接将index设置为对应的值即可：
p=pd.DataFrame(d, index=\["a", "b", "c", "d"\])
效果就出来了，可以自行做个对比：

![图片](../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_910397c381ea4047b.jpg)

<img width="30" height="11" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__a583b53e83d24bda8.png"/>

情况二：data值可以任意的值，比如是一个字符串，那么就需要设置对应的行索引和列索引了，比如我们设置成这样。虽然只有一个值，但是根据我们设置的索引对应的值进行复制我们设置的值：

![图片](../../../_resources/640_wx_fmt_gif_wxfrom_5_wx_lazy__144a7e517b9e4124b.gif)

a=pd.DataFrame('hello', index=\["m", "n", "o", "p"\],columns=\[1,2,3,4\])
最终呈现效果是这样子的，大家可以动手试一下：

<img width="677" height="197" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_70e30cc59a3d42529.jpg"/>

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

好了，今天关于DataFrame的用法就先说到这了，最后给大家留个小问题，当data值可以任意的值的时候，我们设置的列索引和行索引的数量不一致，最后呈现的效果是怎么样的？欢迎评论区讨论哦

**推 荐 阅 读**

[玩转pandas表格--数据创建少不了！](https://mp.weixin.qq.com/s?__biz=MzIxNDM3MzgzOQ==&mid=2247487446&idx=1&sn=b5fbb448af73ffbe650cf33c6b1c845c&chksm=97a9cb76a0de4260d398d2405b345763617985ca34f2e00546801092d4b8af0574507b692af3&token=1621149828&lang=zh_CN&scene=21#wechat_redirect)
[一看就会！pandas安装教程来啦！](https://mp.weixin.qq.com/s?__biz=MzIxNDM3MzgzOQ==&mid=2247487424&idx=1&sn=42473eaad3ae7ae08db6ed9a48f023bb&chksm=97a9cb60a0de42762aebdc88feca77093cd033e34cbdf6b2f3cee0bf073523908416b73d7649&token=1621149828&lang=zh_CN&scene=21#wechat_redirect)
[Python爬虫知识梳理大全（一）！](https://mp.weixin.qq.com/s?__biz=MzIxNDM3MzgzOQ==&mid=2247487255&idx=1&sn=c3b48c829e79a3839676455e44f78eee&chksm=97a9cbb7a0de42a1a8ea01f6defcbaa39c9c5c69850e6c36b34a505a66f5e1f87311d4205c0d&token=1621149828&lang=zh_CN&scene=21#wechat_redirect)

既然在看了，就点一下吧！！

收录于合集 #<a id="js_album_keep_read_title"></a>pandas

 <a id="js_album_keep_read_size"></a>18个

<a id="js_album_prev"></a>上一篇 <a id="js_album_keep_read_pre_title"></a>超干货！pandas读取excel文件，赶紧收藏吧！ <a id="js_album_next"></a>下一篇 <a id="js_album_keep_read_next_title"></a>玩转pandas表格--数据创建少不了！

喜欢此内容的人还喜欢

2万字带你精通MySQL索引

...

哪吒编程

不看的原因

- 内容质量低
- 不看此公众号

午休专列&问题思考：关于多维数组统计各元素的数量

...

A11Dot派

不看的原因

- 内容质量低
- 不看此公众号

pandas 变量类型转换的 6 种方法

...

Python和数据分析

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___e5c8b804afb04483a.bmp"/>

微信扫一扫
关注该公众号