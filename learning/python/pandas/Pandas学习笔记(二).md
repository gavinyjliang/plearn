# Pandas学习笔记(二)

<a id="copyright_logo"></a>Original <a id="js_author_name"></a>付学威 <a id="profileBt"></a><a id="js_name"></a>大气化学python笔记 *2021-10-25 12:42*

收录于话题

<a id="js_article_tag_name__2101124600976703488"></a>#pandas学习笔记 <a id="js_article_tag_num__2101124600976703488"></a>6 <a id="js_article_tag_tips__2101124600976703488"></a>个

<a id="js_article_tag_name__2082367141265080322"></a>#数据分析 <a id="js_article_tag_num__2082367141265080322"></a>14 <a id="js_article_tag_tips__2082367141265080322"></a>个

<a id="js_article_tag_name__2073532319717457922"></a>#pandas <a id="js_article_tag_num__2073532319717457922"></a>17 <a id="js_article_tag_tips__2073532319717457922"></a>个

<a id="js_article_tag_name__2070277671715930115"></a>#python <a id="js_article_tag_num__2070277671715930115"></a>29 <a id="js_article_tag_tips__2070277671715930115"></a>个

<img width="613" height="204" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__733f95982d5b44fd8.png"/>

**Python数据分析系列之**

**Panda学习笔记(二)**

——Pandas数据结构: Series与DataFrame对象。

Series是Pandas库中的一种数据结构，它类似于一维数组，由一组数据及数据的标签(索引)所组成，或者仅有一组数据不指定索引也可以创建一个简单的带数值索引的Series对象。Series可用于储存整数、浮点数、字符串、、Python对象等多种类型的数据。

DataFarme是Pandas库中的另一种数据结构，它是由多种类型的列组成的二维数据结构，类似于Excel、SQL及多个Series构成的字典。DataFrame与Series一样支持储存多种类型的数据。

|     |     |     |
| --- | --- | --- |
| 维数  | 名称  | 描述  |
| **1** | Series | 带标签的一维数组 |
| **2** | DataFrame | 带标签的二维数组(表格) |

<img width="36" height="36" src="../../../_resources/640_wx_fmt_gif_wxfrom_5_wx_lazy__ec244f47600c44c4a.gif"/>

**01**

**创建Series对象**

创建Series对象，主要使用的Pandas中Series方法(注意Series首字母必须大写)，语法如下：

```
`import pandas as pd``s = pd.Series(data,index=index)`
```

**1.1**

**创建不指定索引的Series对象**

若无指定索引，则会自动创建数值型索

<img width="677" height="229" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_3e120ce2ce3d47b1a.jpg"/>

**1.2**

**创建指定索引的Series对象**

<img width="677" height="221" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_494a6d85dda24db18.jpg"/>

<img width="677" height="176" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_6070666153a14c628.jpg"/>

**1.3**

**Series对象的索引取值**

<img width="677" height="187" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_7777ee5ad023428e9.jpg"/>

<img width="677" height="184" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_a683318702f540f7a.jpg"/>

**1.4**

**Series对象的切片取值**

名称索引切片取值

<img width="677" height="149" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_0d4c5415c7ab48bf9.jpg"/>

位置索引切片取值

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

**02**

**创建DataFrame对象**

创建DataFrame主要使用Panda库中的DataFrame方法。语法如下：

```
`import pandas as pd``s = pd.DataFrame(data,index,columns,dtype,copy)`
```

- data：表示数据，可以是ndarray数组、Series对象、列表、字典等；
    
- index：表示行标签(索引)；
    
- columns：表示列标签(列名)；
    
- dtype：每一列数据的数据类型；
    
- copy：用于复制数据
    

**2.1**

**通过二维数组创建DataFrame**

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

**2.2**

**通过字典创建DataFrame**

<img width="677" height="273" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_373263eb59684a0a9.jpg"/>

**2.3**

**通过读取外部数据创建DataFrame**

上一篇推文[Pandas学习笔记(一)](http://mp.weixin.qq.com/s?__biz=MzkwMDI5MDAzOQ==&mid=2247483829&idx=1&sn=b95141e691e55199536a77bf2c41004e&chksm=c047052df7308c3befc13cdd3a1e1f05fcd91b47b992a34ef1a96246d32be50c01b6374c271b&scene=21#wechat_redirect)有总结Pandas读取数据文件的方法，读取进来的数据即为DataFrame数据类型。

```
`import pandas as pd``# 从csv、xlsx、table、sql、json、html读取数据``pd.read_csv(filename)``pd.read_table(filename)``pd.read_excel(filename)``pd.read_sql(query, connection_object)``pd.read_json(json)``pd.read_html(url)`
```

<img width="36" height="36" src="../../../_resources/640_wx_fmt_gif_wxfrom_5_wx_lazy__ec244f47600c44c4a.gif"/>

总结

本文主要介绍了pandas库中Series和DataFrame两种数据类型。介绍了pandas库创建Series和DataFrame的方法，以及从Series中取值的方法。从DataFrame中取值的方法比Series中取值要复杂，将在后续推文中详细介绍。

```
`import pandas as pd``# 创建Series``s = pd.Series(data,index)``# 索引、切片取值``s[2] # 取出s中第3个数据``s[0:3] # 取出s中第1-3个数据``s[['a','b']] # 取出s中a和b索引对应的数据``s['a':'c'] # 取出s中a到c索引对应的数据``# 创建DataFrame``df = pd.DataFrame(data,index,columns,dtype,copy)`
```

1

**END**

1

![](../../../_resources/0_wx_fmt_png_ec0b983275da48ab8a2d2461a0acd603.png)

**大气化学python笔记**

记录与分享python学习心得。请多多支持！

<a id="js_profile_article"></a>32篇原创内容

Official Account

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

**球分享**

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

**球点赞**

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

**球在看**

People who liked this content also liked

Pandas | 设置数据索引

大气化学python笔记

不看的原因

- 内容质量低
- 不看此公众号

使用pandas实现Excel中vlookup功能

大气化学python笔记

不看的原因

- 内容质量低
- 不看此公众号

初识 MongoDB - MongoDB 介绍及安装 | 最流行的文档数据库

数人之道

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___4b8fbc5c07a745f9b.bmp"/>

Scan to Follow