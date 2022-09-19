# Pandas学习笔记(三)

<a id="copyright_logo"></a>Original <a id="js_author_name"></a>付学威 <a id="profileBt"></a><a id="js_name"></a>大气化学python笔记 *2021-10-31 12:00*

收录于话题

<a id="js_article_tag_name__2073532319717457922"></a>#pandas <a id="js_article_tag_num__2073532319717457922"></a>17 <a id="js_article_tag_tips__2073532319717457922"></a>个

<a id="js_article_tag_name__2101124600976703488"></a>#pandas学习笔记 <a id="js_article_tag_num__2101124600976703488"></a>6 <a id="js_article_tag_tips__2101124600976703488"></a>个

<a id="js_article_tag_name__2070277671715930115"></a>#python <a id="js_article_tag_num__2070277671715930115"></a>29 <a id="js_article_tag_tips__2070277671715930115"></a>个

<a id="js_article_tag_name__2082367141265080322"></a>#数据分析 <a id="js_article_tag_num__2082367141265080322"></a>14 <a id="js_article_tag_tips__2082367141265080322"></a>个

<img width="613" height="204" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__7f9d63d5b40b4b189.png"/>

**Python数据分析系列之**

**Panda学习笔记(三)**

——抽取pandas.DataFrame中指定的数据。

在数据分析过程中，并不是所有的数据都是我们想要的，pandas提供了便捷的方法，能让我们快速抽取到我们需要的目标数据。主要使用的是pd.DataFrame对象中的loc属性和iloc属性。

DataFrame对象中loc属性和iloc属性都可以抽取数据，区别如下：

- loc属性：以列名(columns)和行名(index)作为参数，当只有一个参数时，默认是行名，即抽取整行的数据；
    
- iloc属性：以行和列位置索引(即0,1,2...)作为参数，例如0代表第一行，1代表第二行。当只有一个参数是，默认是抽取整行数据，例如:抽取第二行数据df.iloc\[1\]，抽取第二行第三列的数据df.iloc\[1,2\]。
    

**目录：**

1.  抽取一行数据
    
2.  抽取多行数据
    
3.  抽取指定列数据
    
4.  抽取指定行列数据
    
5.  按指定条件抽取数
    

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

**01**

**抽取一行数**

首先构建一个4行3列的DataFrame对象：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

分别按照行名和位置索引来抽取第三行数据：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

**02**

**抽取多行数据**

**通过loc和iloc指定多个行名和行索引即可实现抽取多行数据**

如下分别通过名称和位置索引抽取第1行和第3行数据：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

在loc和iloc中使用冒号“:”，用于抽取连续多行数据：

**注意:**

用行名取值可以取到冒号后面的行，而用位置索引取名则取不到冒号后面的行(与字符串中根据位置索引切片相同)。如下面例子中我们要抽取 明日(第1行) 到 高袁圆(第3行) 所有的行，使用名称索引df.loc\['明日':'高袁圆'\]即可，而使用位置索引则需要使用df.iloc\[0:3\]，等同于df.ioc\[\[0,1,2\]\]。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

同样可以省略冒号前后的值，代表前面所有行和后面所有行：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

**03**

**抽取指定列数据**

抽取指定列的数据，可以直接使用列名，也可以使用loc和iloc的方法。

使用列名的方法：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

使用loc和iloc的方法：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

**04**

**抽取指定行列数据**

抽取指定行列数据主要使用loc和iloc方法，这两个方法中行列的参数都指定之后，便可以进行行列数据的提取。

使用loc按行名列名进行提取：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

使用iloc按行、列索引位置进行提取：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

**05**

**按指定条件抽取数据**

使用df\[筛选条件\]或df.loc\[筛选条件\]的方法来按照指定条件抽取数据：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

总结

本文主要介绍了抽取pd.DataFrame中所需数据的方法。介绍了loc和iloc如何抽取一行、多行(连续与不连续)，单列，多列(连续与不连续)，指定行列(连续与不连续)的数据，还介绍了如何按指定条件抽取数据。后续《pandas学习笔记》系列将更新如何对pd.DataFrame中的数据进行增加、修改和删除。请大家多多转发分享、点赞支持，谢谢！

1

**END**

1

![](../../../_resources/0_wx_fmt_png_c25174974a954d99ba7c041eb300b5ef.png)

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

为什么每个微服务都要有自己的数据源？

零壹陋室

不看的原因

- 内容质量低
- 不看此公众号

R语言调整列的顺序

R语言arh

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___bd3a8cce875e40068.bmp"/>

Scan to Follow