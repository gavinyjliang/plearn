# Pandas | 设置数据索引

<a id="copyright_logo"></a>Original <a id="js_author_name"></a>付学威 <a id="profileBt"></a><a id="js_name"></a>大气化学python笔记 *2021-12-05 18:11*

收录于话题

<a id="js_article_tag_name__2082367141265080322"></a>#数据分析 <a id="js_article_tag_num__2082367141265080322"></a>14 <a id="js_article_tag_tips__2082367141265080322"></a>个

<a id="js_article_tag_name__2073532319717457922"></a>#pandas <a id="js_article_tag_num__2073532319717457922"></a>17 <a id="js_article_tag_tips__2073532319717457922"></a>个

<a id="js_article_tag_name__2101124600976703488"></a>#pandas学习笔记 <a id="js_article_tag_num__2101124600976703488"></a>6 <a id="js_article_tag_tips__2101124600976703488"></a>个

<img width="613" height="204" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__db24f10f211d4cd4b.png"/>

**Python数据分析系列之**

**Panda学习笔记(六)**

——设置数据索引

本文将介绍如何在设置pandas数据中的索引。索引的作用相当于图书的目录，可以根据目录中的页码快速找到所需的内容。pandas索引的作用可以总结为以下几点：

1.  更方便的查询数据；
    
2.  使用索引可以提升查询性能；
    
3.  有利于后续数据分析，如多维索引可用groupby多维聚合分析数据，时间类型的索引，有利于后续重采样等操作。
    

**目录：**

1.  对Series对象重新设置索引
    
2.  对DataFrame对象重新设置索引
    
3.  设置指定列为索引
    
4.  数据清洗后重新设置连续的行索引
    

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

**01**

**对Series对象重新设置索引**

首先构造一个由三个数据组成的pd.Series类型的示例数据s1，设置其初始index为1,2,3：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

重新设置索引为\[0,1,3,2,4,5\]。从输出结果可以看出，reindex方法根据新索引进行了排序，并且对缺失值进行了NaN填充。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

也可以fill_value传入参数，以指定值对缺失值进行填充。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

通过method指定向前填充(fill，和前面数据一样)、向后填充(bfill，和后面的数据一样)：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

**02**

**对DataFrame****对象重新设置索引**

对于DataFrame对象，reindex方法可以用于修改行索引和列索引。首先构造一个3行×3列的DataFrame对象：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

重新设置行索引，缺失值自动填充为NaN：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

reindex中指定columns的值，重新设置列索引：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

reindex中同时指定index和columns的值，同时重新设置行索引与列索引：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

**03**

**设置指定列为索引**

指定df中A列为DataFrame的索引：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

传入drop=False参数，则将在数据中保留A列，并且将A列设置为索引，drop默认参数为True：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

另一个常用的参数为inplace，当传入inplace = True之后，DataFrame会在他本身上做修改，即更改前后df已经发生了变化；而默认的inplace=False代表传回df的一个复制值，需要用一个新的DataFrame来接收，而df本身并未发生变化。可通过下面示例，体会其中的差别。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

**04**

**数据清洗后重新设置连续索引**

当使用pandas对数据进行清洗，删除缺失值所在的行之后，数据中连续的数值型行索引会断开，若想重新设置为连续的行索引，则可以使用reset_index()的方法，不传入参数，则默认将索引重新设置为连续索引：

构造用于演示的，含有空值的5行×3列数据：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

对缺失数据进行清洗，去除其中空值行(pandas数据清洗常用方法可参考)，清洗后数据的行索引不连续：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

使用reset_index()方法，对清洗后的数据重新设置连续索引，参数drop=True或False，代表是否保留原索引列：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

总结

本文主要介绍了如何使用pandas设置Series和DataFrame的索引，包括了处理数据中最常用的对Series对象重新设置索引、对DataFrame对象重新设置索引、设置指定列为索引、数据清洗后重新设置连续的行索引。后续《pandas学习笔记》系列将继续更新对数据进行排序和排名等操作。请大家多多分享、点赞、点击看一看支持，谢谢！

1

**END**

1

![](../../../_resources/0_wx_fmt_png_2a58c466ddcd4d8f9412778eff1d4f5b.png)

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

MySQL——索引

爪哇缪斯

不看的原因

- 内容质量低
- 不看此公众号

数据库索引

高压锅码农777

不看的原因

- 内容质量低
- 不看此公众号

MySQL管理之道，性能调优，高可用与监控（第二版）

Java全栈布道师

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___8766e012f7f04ac78.bmp"/>

Scan to Follow