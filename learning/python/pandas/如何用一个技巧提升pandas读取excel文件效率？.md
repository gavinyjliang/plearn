# 如何用一个技巧提升pandas读取excel文件效率？

<a id="copyright_logo"></a>Original pandas集锦 <a id="profileBt"></a><a id="js_name"></a>Python教程初学详解 *2022-05-17 21:00* *Posted on <a id="js_ip_wording"></a>北京*

收录于合集

<a id="js_article_tag_name__2393442551778410499"></a>#dataframe <a id="js_article_tag_num__2393442551778410499"></a>6 <a id="js_article_tag_tips__2393442551778410499"></a>个

<a id="js_article_tag_name__2393442551711301635"></a>#pandas <a id="js_article_tag_num__2393442551711301635"></a>18 <a id="js_article_tag_tips__2393442551711301635"></a>个

<a id="js_article_tag_name__2393442551644192769"></a>#excel <a id="js_article_tag_num__2393442551644192769"></a>13 <a id="js_article_tag_tips__2393442551644192769"></a>个

<a id="js_article_tag_name__2393442551610638336"></a>#series <a id="js_article_tag_num__2393442551610638336"></a>10 <a id="js_article_tag_tips__2393442551610638336"></a>个

<a id="js_article_tag_name__2015128229799428097"></a>#python <a id="js_article_tag_num__2015128229799428097"></a>65 <a id="js_article_tag_tips__2015128229799428097"></a>个

<img width="43" height="45" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__7ecfa5b583444e559.png"/>

*点击*

***蓝字***

*关注我们~*

各位小伙伴们好呀！不知道上篇的pandas读取excel文件大家有没有试过了？所谓不以实践为目的的学习都是耍流氓，那么不能只会个读取Excel表格就完事了吧？咱们今天就俩说说读取表格其他细节的一些知识内容：

<img width="65" height="63" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__8e49b11c760449bd8.png"/>

<img width="85" height="53" src="../../../_resources/640_wx_fmt_gif_wxfrom_5_wx_lazy__8871e49584e44b25a.gif"/>

<img width="23" height="19" src="../../../_resources/640_wx_fmt_gif_wxfrom_5_wx_lazy__16fb3a48414b45a0a.gif"/>

1.读取sheeet表

<img width="23" height="27" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__4f75ac8a4dbb4a9b8.png"/>

我们知道一张表格中可能有多个sheet表格，如果我们想要读取指定的sheeet表怎么办？

<img width="187" height="4" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__a482b70fb74c437b8.png"/>

![Image](../../../_resources/640_wx_fmt_gif_wxfrom_5_wx_lazy__8e80c55f8cb04d71b.gif)

基本语法：
sheet_name=\[索引值或sheet名称\]

<img width="34" height="34" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__4cbffcac0e5c4a388.png"/>

主要有两种方式：索引值和sheet名称：
为了方便大家理解，我通过一个案例来跟大家解释一下：
①我准备了一张表格，名为“四大名著.xlsx”，里面有两个sheeet表：sheet1和sheet2我将它们命名为西游记和三国：

<img width="677" height="48" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_ef42e8392ef64d5e9.jpg"/>

<img width="34" height="34" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__4cbffcac0e5c4a388.png"/>

②我们如果想要读取第一张sheeet表，可以通过索引值来获取，第一个索引值为0：
import pandas as pd
p=pd.read\_excel('四大名著.xlsx',sheet\_name=0)
print(p)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

③我们也可以通过sheet名称来获取，第一个sheet的名称我们是西游记：
import pandas as pd
p=pd.read\_excel('四大名著.xlsx',sheet\_name=\['西游记'\])
print(p)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

好了小伙伴们，关于pandas读取excel文件不同的sheet表今天就到这里啦，欢迎大家评论区讨论呦!

推荐阅读

● [超干货！pandas读取excel文件，赶紧收藏吧！](https://mp.weixin.qq.com/s?__biz=MzIxNDM3MzgzOQ==&mid=2247487492&idx=1&sn=5824b02c2654ef9899734353d735d258&chksm=97a9d4a4a0de5db2a095abd4b4ddebe1a88d2b3e3f3d73e080a670c97f7908afa47399acbed6&token=164482571&lang=zh_CN&scene=21#wechat_redirect)

● [dataframe方法还不会，今天就把你教会！](https://mp.weixin.qq.com/s?__biz=MzIxNDM3MzgzOQ==&mid=2247487471&idx=1&sn=13f589ca0014f48a29b51fc7e0695e81&chksm=97a9cb4fa0de42590535c56a9ac18760b61ca6c3ffe1538334a47823ed71cd4faf36a196594c&token=164482571&lang=zh_CN&scene=21#wechat_redirect)

● [玩转pandas表格--数据创建少不了！](https://mp.weixin.qq.com/s?__biz=MzIxNDM3MzgzOQ==&mid=2247487446&idx=1&sn=b5fbb448af73ffbe650cf33c6b1c845c&chksm=97a9cb76a0de4260d398d2405b345763617985ca34f2e00546801092d4b8af0574507b692af3&token=164482571&lang=zh_CN&scene=21#wechat_redirect)

● [pandas库怎么安装?简单一步搞定！](https://mp.weixin.qq.com/s?__biz=MzIxNDM3MzgzOQ==&mid=2247487401&idx=1&sn=337e8045a9c6929b92b261ff45f6cf7a&chksm=97a9cb09a0de421f4f96d8d0588f3a40ac01a57bcfd98e2e41289e5aba1f17f6f26a532296c2&token=164482571&lang=zh_CN&scene=21#wechat_redirect)

● [Python爬虫知识梳理大全（一）！](https://mp.weixin.qq.com/s?__biz=MzIxNDM3MzgzOQ==&mid=2247487255&idx=1&sn=c3b48c829e79a3839676455e44f78eee&chksm=97a9cbb7a0de42a1a8ea01f6defcbaa39c9c5c69850e6c36b34a505a66f5e1f87311d4205c0d&token=164482571&lang=zh_CN&scene=21#wechat_redirect)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

收录于合集 #<a id="js_album_keep_read_title"></a>pandas

 <a id="js_album_keep_read_size"></a>18个

<a id="js_album_prev"></a>上一篇 <a id="js_album_keep_read_pre_title"></a>超全！pandas读取excel集锦来啦！ <a id="js_album_next"></a>下一篇 <a id="js_album_keep_read_next_title"></a>超干货！pandas读取excel文件，赶紧收藏吧！

People who liked this content also liked

Excel与Word交互（3）

...

VBA爱好者

不看的原因

- 内容质量低
- 不看此公众号

Excel应用（初学VBA1-听说没对象不给学VBA？）

...

阿毛学python

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___25147248f46e48fa9.bmp"/>

Scan to Follow