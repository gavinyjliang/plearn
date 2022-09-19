# 图解Pandas的排名rank机制

<a id="copyright_logo"></a>Original Peter <a id="profileBt"></a><a id="js_name"></a>尤而小屋 *2021-07-01 09:00*

收录于话题

<a id="js_article_tag_name__1999223975448477698"></a>#python <a id="js_article_tag_num__1999223975448477698"></a>57 <a id="js_article_tag_tips__1999223975448477698"></a>个

<a id="js_article_tag_name__1999223975666581509"></a>#数据分析 <a id="js_article_tag_num__1999223975666581509"></a>104 <a id="js_article_tag_tips__1999223975666581509"></a>个

<a id="js_article_tag_name__1999223977696624640"></a>#机器学习 <a id="js_article_tag_num__1999223977696624640"></a>72 <a id="js_article_tag_tips__1999223977696624640"></a>个

<a id="js_article_tag_name__2000726468170940417"></a>#数据分析师 <a id="js_article_tag_num__2000726468170940417"></a>42 <a id="js_article_tag_tips__2000726468170940417"></a>个

<a id="js_article_tag_name__1999223977226862596"></a>#pandas <a id="js_article_tag_num__1999223977226862596"></a>50 <a id="js_article_tag_tips__1999223977226862596"></a>个

> 公众号：尤而小屋
> 作者：Peter
> 编辑：Peter

大家好，我是Peter呀~ 

在我们的生活经常会遇到各种排名问题：学生成绩排名、销售员业绩排名、各种比赛排名等。在之前一篇关于SQL的文章-《面试必备：SQL排名和窗口函数》中有提到过如何使用SQL来实现3种主要的排名方式：顺序排名、跳跃排名和密集排名。

Pandas这个强大的数据分析库也可以快速实现多种排名方式，主要是通过rank函数来解决的，本文将通过多个例子来讲解。

<img width="657" height="252" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_a0b33490eb5143fc9.jpg"/>

## 一、rank参数

下面是rank函数的主要参数为：

```
`DataFrame.rank(axis=0, 
               method='average', 
               numeric_only=None, 
               na_option='keep', 
               ascending=True, 
               pct=False)
`
```

参数的具体解释为：

- axis：表示排名是根据哪个轴，axis=0表示横轴，axis=1表示纵轴
    
- method：取值可以为'average'，'first'，'min'， 'max'，'dense'；后面重点介绍，默认是average
    
- numeric_only：是否仅仅计算数字型的columns
    
- na_optiaon：NaN值是否参与排名以及如何排名，取值为keep、top、bottom
    
- ascending：升序还是降序；默认是升序
    
- pct：是否以排名的百分比显示排名；所有排名和最大排名的百分比
    

本文将会讲解rank函数在Series和DataFrame两种数据类型的使用。

## 二、Series排名

```
`import pandas as pd
import numpy as np
`
```

首先我们模拟一份简单的数据：

<img width="657" height="288" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_49836f3c2dff42d78.jpg"/>

### 2.1 参数method

1、默认情况的排名method="average"：

<img width="657" height="299" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_82d1d7594fbd4cd28.jpg"/>

2、method="first"

根据值在原始数据中出现的顺序进行排名，相同数值的排名依次加1：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

解释上面两个结果：

- first：直接根据数值的大小顺序进行排名
    
- average：表示的是，如果两个数值相同，排名是它们的均值
    

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

我们看到first的使用就是数值的自然顺序出现的排名；在使用average的情况解释如下：

-5的排名是1.0，0的排名是2.0，3的排名是3.0，5（3号索引位置）的排名是4.0，5(6号索引位置)的排名是5.0，8(0号索引位置)的排名是6.0，8(2号索引)的排名是7.0

通过average的使用，相同数值的排名rank会取出**均值**，5的排名统一成4.5，8的排名统一成6.5

3、max和min的使用

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

比如当：method= "max"：如果数值相同，取该数值最大的那个排名。比如5最大的排名是5，所以原始数据中两个5的排名都是5；两个8的排名都是7（8的两个排名是6和7，取大值7）

4、method="dense"

相同的数值排名相同，下个数值的排名**不出现跳跃**

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

这个时候排名的时候是不会出现跳跃的情况

### 2.2 参数ascending

默认情况下是升序的情况，可以使用降序：值越大，排名越靠前：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

数值中8的排名，如果是method=“first”，排名是1和2，如是使用average，排名则会变成1.5；其他的数值排名类似。再看看max的情况：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 2.3 参数pct

是否以排名的百分比显示排名；所有排名和最大排名的百分比

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

上面的排名是如何计算出来的呢？我们最大的排名是7：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

<img width="657" height="599" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_86c7edefac744ff49.jpg"/>

再比如dense情况下的pct参数使用类似：

<img width="657" height="293" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_c9352eb8c0184a11a.jpg"/>

### 2.4 参数na_option

这个参数表示的是空值是否参与排名，取值为keep、top、bottom。我们再模拟一份带有空值的数据：

<img width="657" height="255" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_53eb7374889140f49.jpg"/>

看看3种不同的情况：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 三、DataFrame排名

### 模拟数据

还是先模拟一份数据：

```
`df0 = pd.DataFrame({"科目":["语文","语文","语文","语文","语文","数学","数学","数学","数学","数学"],
                  "姓名":["小明","小苏","小周","小孙","小王","小明","小苏","小周","小孙","小王"],
                  "分数":[137,125,125,115,115,80,111,130,130,140]})
df = df0.copy()   # 生成一个副本df
df
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 3.1 单个科目排名

比如我们想看语文这门科目的排名情况，取出同学们的语文成绩：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

分别使用顺序排名、跳跃排名和密集排名来展示排名情况：

```
`# 默认排名方式
df1["均值排名_默认"] = df1["分数"].rank(ascending=False)
df1["跳跃排名_min"] = df1["分数"].rank(method="min",ascending=False)
df1["跳跃_max"] = df1["分数"].rank(method="max",ascending=False)
df1["密集排名_dense"] = df1["分数"].rank(method="dense",ascending=False)
df1
`
```

<img width="657" height="209" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_993d56efa27147d59.jpg"/>

### 3.2 同学总分排名

先通过transform生成每个同学的总分：

```
`df["总分"] = df.groupby("姓名")["分数"].transform("sum")
df
`
```

<img width="657" height="411" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_aa3a7c4f86ef40cb8.jpg"/>

我们使用密集排名的方式对总分进行排名：

<img width="657" height="364" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_f2e1e7170e43465ca.jpg"/>

## 四、分组取出指定排名

我们现在看到每个科目下的第二名的学生，如果成绩相同，排名相同（不跳跃），我们使用密集排名：

```
`# 定义一个排名第二的函数
def rank_second(x):
    return x[x["分数"].rank(method="dense",ascending=False) == 2]
`
```

<img width="657" height="296" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_1ff8aab6b2e847489.jpg"/>

我们看看真实数据中每个科目的第二名同学：

<img width="657" height="522" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_e523d3030d864a6e8.jpg"/>

上面自定义的排名第二的函数分为两步；

1、先实现密集排名

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

2、指定排名等于2

<img width="657" height="367" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_41380ff0caed43aa9.jpg"/>

当我们使用这个自定义函数的时候，我们需要先根据科目进行分组，然后再每个组中单独使用这个自定义函数，就能获得每个科目下的第二名。

## 五、总结

讲解完rank函数的使用，可以和SQL中的窗口函数进行类比：

[面试必备：SQL排名和窗口函数](http://mp.weixin.qq.com/s?__biz=MzI4NjI3MzI2Ng==&mid=2247490520&idx=1&sn=29792322118c456c186bbdeca3bc89c7&chksm=ebde2045dca9a953d62abdc807c6de7788f8864392de580bf2748008377df8c1af9b88f08739&scene=21#wechat_redirect)

- row_number：顺序排名，rank函数的中的method=first
    
- rank：跳跃排名，rank函数的中的method=min
    
- dense_rank：密集排名，rank函数的中的method=dense
    

<img width="657" height="232" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_037fce4969cd41ecb.jpg"/>

最后附上rank函数的官网学习地址，还得多看官网：

https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.rank.html

<img width="10" height="10" src="../../../_resources/640_wx_fmt_gif_wxfrom_5_wx_lazy__fb56e9b8d3c6473fa.gif"/>

推荐阅读

<img width="14" height="14" src="../../../_resources/640_wx_fmt_gif_wxfrom_5_wx_lazy__5a6f3ca0e8de4c2a9.gif"/>

[桑基图或许可以告诉你打工人的故事！](http://mp.weixin.qq.com/s?__biz=MzI4NjI3MzI2Ng==&mid=2247491889&idx=1&sn=4fa63ba5f13b595bc1a80b1c837fe21b&chksm=ebdddaacdcaa53ba82cb9b0a41ddbf4da773709db613f44535d678203fa064c1de0845b3806b&scene=21#wechat_redirect)

[图解Pandas的groupby机制](http://mp.weixin.qq.com/s?__biz=MzI4NjI3MzI2Ng==&mid=2247491765&idx=1&sn=e63fe83db79cdb8b74f403c458a135cc&chksm=ebdddb28dcaa523e3e9392a792776fc2f2a511a0d2e546014ee2e32d753c120f2aac79aed9da&scene=21#wechat_redirect)

[从理论到实战：基于Python的用户增长Cohort分析](http://mp.weixin.qq.com/s?__biz=MzI4NjI3MzI2Ng==&mid=2247491711&idx=1&sn=05686452b4ddc941dd25ae983e425b0b&chksm=ebdddbe2dcaa52f425a6bf2f8264eb8da21283779b4636b2d9c4bc5264c5889cd786cf81b23f&scene=21#wechat_redirect)

[数据处理基石：数据探索](http://mp.weixin.qq.com/s?__biz=MzI4NjI3MzI2Ng==&mid=2247491098&idx=1&sn=5bc104d393b8725c857142ac24c2550a&chksm=ebde2587dca9ac919f11c5d7fc153a261f42b9de8af3e66603efaf9ec8c0bd6c33812a7663a8&scene=21#wechat_redirect)

[面试必备：SQL排名和窗口函数](http://mp.weixin.qq.com/s?__biz=MzI4NjI3MzI2Ng==&mid=2247490520&idx=1&sn=29792322118c456c186bbdeca3bc89c7&chksm=ebde2045dca9a953d62abdc807c6de7788f8864392de580bf2748008377df8c1af9b88f08739&scene=21#wechat_redirect)
[](http://mp.weixin.qq.com/s?__biz=MzI4NjI3MzI2Ng==&mid=2247491889&idx=1&sn=4fa63ba5f13b595bc1a80b1c837fe21b&chksm=ebdddaacdcaa53ba82cb9b0a41ddbf4da773709db613f44535d678203fa064c1de0845b3806b&scene=21#wechat_redirect)

> 尤而小屋，一个温馨的小屋。小屋主人，一手代码谋求生存，一手掌勺享受生活，欢迎你的光临

收录于话题 #<a id="js_album_keep_read_title"></a>python

 <a id="js_album_keep_read_size"></a>57个

<a id="js_album_prev"></a>上一篇 <a id="js_album_keep_read_pre_title"></a>Pandas数据类型操作 <a id="js_album_next"></a>下一篇 <a id="js_album_keep_read_next_title"></a>图解Pandas的排序机制sort_values

<a id="js_view_source"></a>Read more

People who liked this content also liked

Python关联规则挖掘情侣、基友、渣男和狗

尤而小屋

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___67be9ac7e38c44bf9.bmp"/>

Scan to Follow