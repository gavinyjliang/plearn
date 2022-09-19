# Python-csvkit：强大的CSV文件命令行工具

<a id="profileBt"></a><a id="js_name"></a>数据不吹牛 *2022-04-08 22:03*

The following article is from Python大数据分析 Author 朱卫军

<a id="copyright_info"></a>[![](../../../_resources/0_48b6b1448c5f4ce9b0f56cd97c59c331.jpg)<br>**Python大数据分析** .<br>分享python编程、可视化设计、大数据分析、机器学习等技术以及数据分析案例，包括但不限于pandas、numpy、spark、matplotlib、sklearn、tensorflow、keras、tableau等](#)

![](../../../_resources/0_wx_fmt_png_5504c625f7b24f0986f62c7e212b2447.png)

**数据不吹牛**

有趣+干货的数据分析宝藏

<a id="js_profile_article"></a>69篇原创内容

Official Account

如果你在学Python数据处理，一定对CSV文件不陌生。日常本地数据存储中，除了Excel文件外，大部分数据都是以CSV文件格式保存的。

CSV（Comma-Separated Values）是一种文本文件，也叫作逗号分隔值文件格式。顾名思义，它就是用来保存纯文本，被分隔符分隔为多个字段。

CSV文件能够被Excel、notepad++、Java、Python等各种软件读取，非常方便。

因为它结构简单、易传输、易读取的特性，使其广受个人和商业领域欢迎。

<img width="383" height="214" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__7a5ccd44d7c04f9e8.png"/>

在Python中，可以使用read函数、pandas库、csv库等读写CSV文件，而且这些也是常用的方法。

这次给大家介绍一个非常强大的第三方库-csvkit，它是专门处理CSV文件的命令行工具，可以实现文件互转、数据处理、数据统计等，十分便捷。

因为csvkit是Python第三方库，我们直接使用pip来安装csvkit。

`pip install csvkit`

csvkit是命令行工具，所以代码都在命令行执行，下面列举一些常见的使用场景。

我们先在本地保存一个Excel表（DoubanMovie），其内容是豆瓣电影数据。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

注意命令行地址要切换到该表所在位置。

比如我放在`E:\csvkit_tutorial\`里面，可以用下面命令来切换。

```
`E：
cd csvkit_tutorial
`
```

## 1、Excel转CSV

csvkit支持将Excel等其他数据文件转化为CSV文件，使用`in2csv`命令实现。

```
`in2csv DoubanMovie.xlsx > DoubanMovie.csv
`
```

除了Excel的xlsx和xls文件外，你还可以对下面多种数据格式进行CSV的转换

包括：`dbf , fixed , geojson , json , ndjson`

## 2、对SQL数据库进行读写和查询操作

从MySQL数据库中读取一张表存到本地CSV文件中，使用`csvsql`命令实现。

```
`csvsql --db "mysql://user:pass@host/database?charset=utf8" --tables "test1" --insert test1.csv
`
```

直接对MySQL数据库进行数据查询，使用`sql2csv`命令实现

```
`sql2csv --db "mysql://user:pass@host/database?charset=utf8" --query "select * from test2"
`
```

注意代码中--db参数后面需要输入数据库的信息，用于连接数据库。

## 3、将CSV文件转换为Json格式

除了将Json文件转化为CSV格式外，csvkit也支持将CSV文件转化为Json格式，使用`csvjson`命令实现。

```
`csvjson test.csv
`
```

如果你是做地理空间分析，还可以将csv文件转化为GeoJson格式。

## 4、数据处理和分析

csvkit中还有用于数据处理分析的命令，如下：

- csvcut：对数据进行索引切片
    
- csvgrep：对数据进行过滤，可按照正则表达式规则
    
- csvjoin：对不同数据表按键进行连接
    
- csvsort：对数据进行排序
    
- csvstack：将多个数据表进行合并
    
- csvlook：以 Markdown 兼容的固定宽度格式将 CSV 呈现到命令行
    
- csvstat：对数据进行简单的统计分析
    

## 小结

csvkit适合那些经常处理CSV文件的小伙伴，可快速的进行转化、清晰、分析等任务。特别当你的文件较大，一般软件难以打开时，csvkit的速度绝对会让你惊艳到。

学习文档：https://csvkit.readthedocs.io/en/latest/index.html

****万水千山总是情，点个 👍 行不行**。**

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

```


![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

<ins>●[中国城市人口分布区域分析实战！](http://mp.weixin.qq.com/s?__biz=MzU5Mjg2OTQ1MA==&mid=2247509485&idx=1&sn=079d4bce62e6764295470c1e70001b12&chksm=fe1bc4c8c96c4dde320e77f9757316a073518293f89c7b2442a6fa03d4034b5960d11097b478&scene=21#wechat_redirect)</ins>

<ins>●[品牌知名度分析](http://mp.weixin.qq.com/s?__biz=MzU5Mjg2OTQ1MA==&mid=2247501207&idx=1&sn=b17fc4d67f0794da55c15ca5765bf884&chksm=fe1ba4b2c96c2da49e694c8a403807526e87b1e54b1f7d266efe3e31f657435248c2073efb30&scene=21#wechat_redirect)</ins>




























```

People who liked this content also liked

又一款python开发神器

...

基因学苑

不看的原因

- 内容质量低
- 不看此公众号

技术分享 | 为什么我的 MySQL 客户端字符集为 latin1

...

爱可生开源社区

不看的原因

- 内容质量低
- 不看此公众号

用Python去除图片背景：​Rembg库

...

Python大数据分析

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___aced4cdf75604c798.bmp"/>

Scan to Follow