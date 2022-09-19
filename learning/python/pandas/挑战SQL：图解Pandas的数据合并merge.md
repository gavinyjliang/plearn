# 挑战SQL：图解Pandas的数据合并merge

<a id="copyright_logo"></a>Original Peter <a id="profileBt"></a><a id="js_name"></a>尤而小屋 *2021-07-24 12:07*

收录于话题

<a id="js_article_tag_name__1999223975666581509"></a>#数据分析 <a id="js_article_tag_num__1999223975666581509"></a>104 <a id="js_article_tag_tips__1999223975666581509"></a>个

<a id="js_article_tag_name__2000724815548055554"></a>#MySQL <a id="js_article_tag_num__2000724815548055554"></a>21 <a id="js_article_tag_tips__2000724815548055554"></a>个

<a id="js_article_tag_name__1999223977226862596"></a>#pandas <a id="js_article_tag_num__1999223977226862596"></a>50 <a id="js_article_tag_tips__1999223977226862596"></a>个

> 公众号：尤而小屋
> 作者：Peter
> 编辑：Peter

大家好，我是Peter~

在实际的业务需求中，我们的数据可能存在于不同的库表中。很多情况下，我们需要进行多表的连接查询来实现数据的提取，通过SQL的join，比如left join、left join、inner join等来实现。

在pandas中也有实现合并功能的函数，比如：concat、append、join、merge。本文中重点介绍的是**merge函数**，也是pandas中最为重要的一个实现数据合并的函数。

看完了你会放弃SQL吗？

<img width="657" height="252" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_78632d7f15fb42ffb.jpg"/>

## Pandas连载文章

目前Pandas系列文章已经更新了13篇，文章都是以丰富案例+图解的风格，欢迎大家访问阅读。有很多个人推荐的文章：

[图解Pandas重复值处理](http://mp.weixin.qq.com/s?__biz=MzI4NjI3MzI2Ng==&mid=2247492519&idx=1&sn=c4974450fc9ea8b84106f65de6501028&chksm=ebddd83adcaa512c788ae612b0ebc6b4846ad43981aa00fa38a2c56710496651ae69ce180f1a&scene=21#wechat_redirect)

[图解Pandas的排序机制sort_values](http://mp.weixin.qq.com/s?__biz=MzI4NjI3MzI2Ng==&mid=2247492035&idx=1&sn=f49ffd392a4d499c992d22ab3f496959&chksm=ebddda5edcaa5348dadaf7d340e02b3f5243bb28dccba78a627edb0856868c8c4263f1495f98&scene=21#wechat_redirect)

[图解Pandas的排名rank机制](http://mp.weixin.qq.com/s?__biz=MzI4NjI3MzI2Ng==&mid=2247492004&idx=1&sn=d0602ee2d71f3a23417209cb8da519ec&chksm=ebddda39dcaa532f1fa6e9f10d067ad2de0997d31a31ca104f017caf74168186883e70eac4e8&scene=21#wechat_redirect)

[图解Pandas的groupby机制](http://mp.weixin.qq.com/s?__biz=MzI4NjI3MzI2Ng==&mid=2247491765&idx=1&sn=e63fe83db79cdb8b74f403c458a135cc&chksm=ebdddb28dcaa523e3e9392a792776fc2f2a511a0d2e546014ee2e32d753c120f2aac79aed9da&scene=21#wechat_redirect)

[数据处理基石：数据探索](http://mp.weixin.qq.com/s?__biz=MzI4NjI3MzI2Ng==&mid=2247491098&idx=1&sn=5bc104d393b8725c857142ac24c2550a&chksm=ebde2587dca9ac919f11c5d7fc153a261f42b9de8af3e66603efaf9ec8c0bd6c33812a7663a8&scene=21#wechat_redirect)

[创建DataFrame：10种方式任你选](http://mp.weixin.qq.com/s?__biz=MzI4NjI3MzI2Ng==&mid=2247489661&idx=1&sn=c8f2d2c30038801371c065720fe6460e&chksm=ebde23e0dca9aaf6b6d79d44b472cc933b7ad754bf75e3dcf116f75ffabb5314056155c6f3c0&scene=21#wechat_redirect)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 参数

官网学习地址：https://pandas.pydata.org/pandas-docs/stable/user_guide/merging.html#

```
`pd.merge(left,   # 待合并的2个数据框
         right, 
         how='inner',  # ‘left’, ‘right’, ‘outer’, ‘inner’, ‘cross’
         on=None, # 连接的键，默认是相同的键
         left_on=None,  # 指定不同的连接字段：键不同，但是键的取值有相同的内容
         right_on=None, 
         left_index=False,   # 根据索引来连接
         right_index=False, 
         sort=False, # 是否排序
         suffixes=('_x', '_y'),   # 改变后缀
         copy=True, 
         indicator=False,   # 显示字段来源
         validate=None)
`
```

参数的具体解释为：

- left、right：待合并的数据帧
    
- how：合并的方式，有5种：{‘left’, ‘right’, ‘outer’, ‘inner’, ‘cross’}, 默认是 ‘inner’
    
    1、 left：左连接，保留left的全部数据；right类似；类比于SQL的left join 或者right join
    
    2、outer：全连接功能，类似SQL的full outer join
    
    3、inner：交叉连接，类比于SQL的inner join
    
    4、cross：创建两个数据帧DataFrame的**笛卡尔积**，默认保留左边的顺序
    
- on：连接的列属性；默认是两个DataFrame的相同字段
    
- left\_on/right\_on：指定两个不同的键进行联结
    
- left\_index、right\_index：通过索引进行合并
    
- suffixes：指定我们自己想要的后缀
    
- indictor：显示字段的来源
    

## 模拟数据

我们创建了4个DataFrame数据框；其中df1和df2、df3是具有相同的键userid；df4有类似的键userid1，取值也是ac，和df1或df2的userid取值有相同的部分。

```
`import pandas as pd
import numpy as np
`
```

<img width="657" height="589" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_1379f025977040cea.jpg"/>

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 参数left、right

left、how就是需要连接的两个数据帧，一般有两种写法：

- pd.merge(left,right)，个人习惯
    
- left.merge(right)
    

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

图解过程如下：

- 两个数据框df1（left）、df2（right）有相同的字段userid
    
- 默认是通过相同的字段（键）进行关联，取出键中相同的值（ac），而且每个键的记录要全部显示，比如a有多条记录
    

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 参数how

### inner

inner称之为**内连接**。它会直接根据相同的列属性userid进行关联，取出属性下面相同的数据信息a、c

⚠️上面的图解过程就是默认的使用how="inner"

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### outer

outer称之为外连接，在拼接的过程中会取两个数据框中键的并集进行拼接

- 外连接，取出全部交集键的并集。例子中是user的并集
    
- 如果某个键在某个数据框中不存在数据，则为NaN
    

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

图解过程如下：

- 也是根据相同的字段来进行连结：userid
    
- 保留两边的全部数据，所以abcde全部存在
    
- 如果某边不存在键下面的某个值，则结果中用NaN补充。比如df1的userid中存在b，但是df3中不存在，则结果b对应的score为NaN，cd类似；e在df3中存在e的取值，但是df1中不存在，则age的值为NaN
    

<img width="657" height="356" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_cdc6268668c345f98.jpg"/>

### left

以左边数据框中的键为基准；如果左边存在但是右边不存在，则右边用NaN表示

<img width="657" height="380" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_6b51ef1d06df4398b.jpg"/>

图解过程如下：

- 和上面图解过程的结果差别在于，没有出现e；
    
- 当how="left"，只会保留df1（left）中userid下面的全部取值，不包含e
    

<img width="657" height="311" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_8ce189ee018a448c8.jpg"/>

### right

以右边数据框中的键的取值为基准；如果右边存在但是左边不存在，则左边用NaN表示

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

image-20210724095138183

图解过程如下：

- 当how="right"，只会保留df3（right）中userid的全部取值
    
- 结果只保留了df3的userid下面的全部取值：a、e
    

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### cross

笛卡尔积：两个数据框中的数据交叉匹配，出现`n1*n2`的数据量

<img width="657" height="505" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_c67996c5f3694b4c9.jpg"/>

笛卡尔积的图解过程如下：

- 出现的数据量是4*2，userid下面的数据交叉匹配
    
- 在最终结果中相同的字段userid为了避免混淆，会带上默认的后缀`_x、_y`
    

<img width="657" height="422" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_19cfd9a44f55416ba.jpg"/>

## 参数on

如果待连接的两个数据框有相同的键，则默认使用该相同的键进行联结。

上面的所有图解例子的参数on默认都是使用相同的键进行联结，所以有时候可省略。

<img width="657" height="364" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_90674d59041a43cfb.jpg"/>

再看个例子：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

还可以将left和right的位置进行互换：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

上面的两个例子都是针对数据框只有具有相同的一个键，如果不止通过一个键进行联结，该如何处理？通过一个来自官网的例子来解释，我们先创建两个DataFrame：df5、df6

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

现在进行两个数据框的合并：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

合并的图解过程如下：

- 通过on参数指定两个连接的字段key1、key2
    
- 只有当两个数据框中的key1和key2的取值完全相同的时候（交集），才会保留下来；比如都出现了key1=K0,key2=K0和key1=K1,key2=K0。
    

<img width="657" height="276" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_0d46c82659794fff9.jpg"/>

在看一个通过how="outer"进行连接的案例：

<img width="657" height="308" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_87798ec545104c27a.jpg"/>

看看图解的过程：

- 指定连接的两个键key1、key2
    
- 使用how="outer"，会保留两个数据框中的全部数据。某个数据框中不存在键的值，则取NaN
    

<img width="657" height="339" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_9e68789510dd41699.jpg"/>

## 参数left\_on、right\_on

上面在连接合并的时候，两个数据框之前都是有相同的字段，比如userid或者key1和key2。但是如何两个数据框中没有相同的键，但是这些键中的取值有相同的部分，比如我们的df1、df3：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

在这个时候我们就使用left\_on和right\_on参数，分别指定两边连接的键：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

如果我们不指定，系统就会报错，因为这两个数据框是没有相同的键，本身是无法连接的：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 参数suffixes

如果连接之后结果有相同的字段出现，默认后缀是`_x_、_y`。这个参数就是改变我们默认的后缀。我们回顾下笛卡尔积的形成；

<img width="657" height="413" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_3712d0524bc743918.jpg"/>

现在我们可以指定想要的后缀：

<img width="657" height="311" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_590d9cc53e294eb2a.jpg"/>

## indicator

这个参数的作用是表明生成的一条记录是来自哪个DataFrame：both、left\_only、right\_only

如果带上参数会显示一个新字段`_merge`：

<img width="657" height="233" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_c31e811159a94b2cb.jpg"/>

不带上参数的话，默认是不会显示来源的，看默认的情况：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 总结

merge函数真的是非常强大，在工作中也使用地很频繁，完全可以实现SQL中的join效果。希望本文的图解能够帮助读者理解并掌握这个合并函数的使用。同时pandas还有另外几个与合并相关的函数，比如：join、concat、append，会在下一篇文中统一讲解。

<img width="17" height="36" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__b75992bffe4a4ea3b.png"/>

推荐阅读

<img width="17" height="36" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__7f85193f5e154596a.png"/>

[Python入门-列表初相识](http://mp.weixin.qq.com/s?__biz=MzI4NjI3MzI2Ng==&mid=2247493045&idx=1&sn=edfc48f8153aa8843bb69a1ea9647f08&chksm=ebddde28dcaa573eabdb34a6c6643e7f51f3ecbd4ca863e5b2250b16cd520b75ffe770adb24a&scene=21#wechat_redirect)

[数据分析师=7大主题，24份资料](http://mp.weixin.qq.com/s?__biz=MzI4NjI3MzI2Ng==&mid=2247492664&idx=1&sn=4136051d1be47a616d52fcb22ff49a49&chksm=ebdddfa5dcaa56b3eb370532daaf6deb74886599a8cc08e010bfddf0562ffedc0ce89f43f18f&scene=21#wechat_redirect)

[生日快乐：尤而小屋两周岁啦](http://mp.weixin.qq.com/s?__biz=MzI4NjI3MzI2Ng==&mid=2247492295&idx=1&sn=489d07d8208ffa10fefa67d562f95092&chksm=ebddd95adcaa504c2c3983a899d48785d5fd08f06b52ba2beba404b122f86bae87dbcd557772&scene=21#wechat_redirect)

[55个案例：吃透Python字符串格式化](http://mp.weixin.qq.com/s?__biz=MzI4NjI3MzI2Ng==&mid=2247492370&idx=1&sn=7416d7147771d2b84bb622df1d05df46&chksm=ebddd88fdcaa5199476819a140679c503f7e5c66f7d6e26ac3e48d1fabb06b13a4d31d7a591b&scene=21#wechat_redirect)

[Python入门-字符串初相识](http://mp.weixin.qq.com/s?__biz=MzI4NjI3MzI2Ng==&mid=2247492272&idx=1&sn=1e4aa938c458423ef2f3bcab9f67c595&chksm=ebddd92ddcaa503b5e9b4a7296d50462650c72e308a1e967c81c31a8038f0e7fc57cf5c2f510&scene=21#wechat_redirect)
[](http://mp.weixin.qq.com/s?__biz=MzI4NjI3MzI2Ng==&mid=2247492370&idx=1&sn=7416d7147771d2b84bb622df1d05df46&chksm=ebddd88fdcaa5199476819a140679c503f7e5c66f7d6e26ac3e48d1fabb06b13a4d31d7a591b&scene=21#wechat_redirect)

> 尤而小屋，一个温馨的小屋。小屋主人，一手代码谋求生存，一手掌勺享受生活，欢迎你的光临

收录于话题 #<a id="js_album_keep_read_title"></a>数据分析

 <a id="js_album_keep_read_size"></a>104个

<a id="js_album_prev"></a>上一篇 <a id="js_album_keep_read_pre_title"></a>终于打卡结束：MySQL经典50题 <a id="js_album_next"></a>下一篇 <a id="js_album_keep_read_next_title"></a>中国奥运会成绩，知道多少？13张图告诉你

People who liked this content also liked

Python关联规则挖掘情侣、基友、渣男和狗

尤而小屋

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___bb2c5caa637a4aed8.bmp"/>

Scan to Follow