# 图解Pandas的groupby机制

<a id="copyright_logo"></a>Original Peter <a id="profileBt"></a><a id="js_name"></a>尤而小屋 *2021-06-26 17:34*

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

在自己的数据处理分析日常中，经常会遇到对数据的某个字段进行分组再求和或均值等其他操作的需求，比如电商中根据不同的支付用户、不同的月份、不同的性别、不同的用户来源进行用户的画像细分，来研究不同组用户的偏好和消费情况等。

在pandas中自己都是使用groupby来解决这类问题，本文结合一份模拟的数据来讲解groupby的内部机制。

<img width="657" height="252" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_0333e0e7801a4396b.jpg"/>

## 一、模拟数据

为了方便解释，自己模拟了一份虚拟数据，仅包含3个字段：员工姓名employees、薪资salary、得分score

```
`import pandas as pd
import numpy as np
employees = ["小明","小周","小孙"]   # 3位员工
df=pd.DataFrame({
    "employees":[employees[x] for x in np.random.randint(0,len(employees),9)],  # 在员工中重复选择9个人
    "salary":np.random.randint(800,1000,9),  # 800-1000之间的薪资选择9个数值
    "score":np.random.randint(6,11,9)  # 6-11的分数选择9个
})
df
`
```

![Image](../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_7e93dec848404877a.jpg)

## 二、DataFrameGroupBy对象

### 2.1 内部情况

我们现在根据员工进行groupby分组，得到的一个DataFrameGroupBy对象

```
`# groupbying = df.groupby("employees")  by可以省略
groupbying = df.groupby(by="employees")
groupbying
`
```

<img width="657" height="184" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_293d317c13f34449b.jpg"/>

那这个DataFrameGroupBy对象到底长的什么样子？我们用list展开看看：

```
`# 查看对象内部的情况
list(groupbying)
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

我们终于看到了这个对象的神秘面目：

- 对象是一个大列表，里面包含3个元素，每个元素有个元组对象：\[tuple1,tuple2,tuple3\]
    
- 元素就是按照我们指定的员工进行分组：分别是小周、小孙、小明的全部数据信息
    

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

我们看看小明的具体信息：发现xiaoming是一个元组，转成列表之后看下具体信息：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

我们发现：元组转成列表后的第一个信息就是分组的员工名，第二个就是这个员工的全部信息构成的一个小DataFrame数据帧。

下面的图形能够很好的展示DataFrameGroupBy对象的内部情况：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

总结：当我们根据某个字段进行group机制分组的时候，最后能够生成多少个子DataFrame，取决于我们的字段中有多少个不同的元素（案例有3个）；当我们分组之后，便可以进行后续的各种聚合操作，比如sum、mean、min等。

### 2.2 遍历DataFrameGroupBy对象

```
`for name,group in groupbying:  # 遍历.DataFrameGroupBy对象
    print(name)
    print(group)
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 2.3 选择分组get_group()

对DataFrameGroupBy对象使用get_group()方法，能够让我们得到分组元素中的指定组的数据：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 2.4 同一个列名使用不同聚合函数

分组之后对同一个列名使用不同的函数，函数使用列表形式：下面👇表示的是对score分别求和、最大值、最小值、均值、个数（size）

```
`df9 = df.groupby("employees")["score"].agg(["sum","max","min","mean","size"]).reset_index()
df9
`
```

<img width="657" height="141" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_63694caf511448319.jpg"/>

## 三、聚合函数

相信很多朋友都知道聚合操作或者聚合函数，在SQL中我们可以这样写：

```
`select
 name  -- 姓名
 ,sum(score)  -- 分数最大值
 ,avg(score)  --  平均值
from score
group by name  -- 根据学生姓名分组统计
`
```

在上面的SQL语句中，sum和avg就是常见的聚合操作，归类整理下pandas常用的聚合操作：

| 函数  | 含义  |
| --- | --- |
| min/max | 最小值、最大值 |
| sum | 求和  |
| mean | 均值  |
| median | 中位数 |
| std | 标准差 |
| var | 方差  |
| count | 计数统计 |

除了上面的聚合函数，我们还可以使用numpy库的方法，比如unique（不同的元素）、nunique（不同元素的个数，count是统计全部）等，下面会结合实际的例子来说明。

## 四、agg聚合操作

聚合操作是通过agg来完成的，可以指定一个列或者多个列分别使用不同的聚合函数来聚合。

1、对单个列进行聚合操作，比如：我们想对salary列求总和sum：

```
`# df.groupby("employees")["salary"].sum  
# 如果只是单个元素，上下两种写法等价
df.groupby("employees").agg({"salary":"sum"})
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

一般情况下，结果是一个以分组字段为行索引的数据帧，那如果我们也想把这个行索引变成数据帧中的一个列名属性，使用reset_index完成：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

一行代码写作为：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

2、对多个列使用不同的聚合函数，比如：我们想对salry求和、对score求均值，使用字段对的方式来实现

```
`salary_score = df.groupby("employees").agg({"salary":"sum",
                                            "score":"mean"
                                           })
salary_score
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

同样地，我们可以进行索引重置工作：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

但是重置之后，我们无法看到具体的聚合函数，考虑给生成的数据帧改下列名，新的名字可以根据自己的喜欢任意指定，一般是要能够代表列名的含义：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

⚠️前方高能：使用一行代码实现上面的全部过程：

```
`df.groupby("employees").agg({"salary":"sum","score":"mean"}).reset_index().rename(columns={"salary":"salary_sum","score":"score_mean"})
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

详细地解释下上面的一行代码的各个函数功能：

1.  groupby：指定分组的列名字段
    
2.  agg：指定列名和想实施的聚合函数
    
3.  reset_index：对生成的数据帧进行索引重置
    
4.  rename：对生成的列名进行修改；上面是手动指定，需要全部列出来；rename可以对我们想要修改的列名进行重命名
    

下面的图形的很好地展示从原始数据到DataFrameGroupBy对象，最后再到多个列名聚合的过程：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 五、统计列名的非重复个数

在这个需求中我们使用的numpy中的nunqiue方法，给定一个需求：我们想统计每位员工的不同分数以及对应个数。实际的结果应该是：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

1、统计每个员工的不同分数：使用的unique方法

```
`df.groupby("employees").agg({"score":"unique"}).reset_index()
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

2、统计每位员工的不同分数个数：使用nunique方法

<img width="657" height="182" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_22f63ef902a642b1a.jpg"/>

**为甚么在这里要特别讲解这两个方法？**

因为在自己的实际需求中经常遇到类似的需求。模拟一下自己的需求，比如给定一组数据，包含手机订单中的：订单号、型号、厂家

![Image](../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_0a4b3a2b7dbc44668.jpg)

现在需要：**统计每个型号对应的订单数、厂家名（去重）、厂家数（去重）**。这个时候就可以使用上面的方法来实现这个需求，最终的结果应该是这样子：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

1、先生成订单数和厂家名相关的数据：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

2、再生成和厂家个数相关的数据：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

3、通过pandas中的merge函数进行数据的合并：

可以看到下面的数据和实际的结果是一致的

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

**一行代码也可以实现上面的需求**：对厂家列同时使用两个不同的函数，一个是统计名称，一个是统计个数，结果是一个多重索引的数据帧。

```
`df4.groupby("型号").agg({"订单号":"count","厂家":["unique","nunique"]}).reset_index()
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 六、transform函数

transform方法解决的是什么问题呢？它和agg又有什么不同呢？我们从一个实际的新需求出发，比如：我们想在数据的后面加上一列，代表的是每位员工的平均得分，数据会变成下面的样���，绿色部分就是我们的需要：

<img width="657" height="536" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_505f0e126b6d4a479.jpg"/>

好方法：通过transform能够轻松实现上面的需求

```
`df["score_mean"] = df.groupby("employees")["score"].transform("mean")  # transform后面指定需要聚合的函数
df
`
```

<img width="657" height="354" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_86e4ef5ea7594c6eb.jpg"/>

如果不用transform该如何实现上面的需求呢？首先我们还是生成每位员工的平均分数，用字典的形式来表示

<img width="657" height="117" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_6ce1c21226484faf9.jpg"/>

接下来，我们使用map来进行员工的匹配：

```
`df["score_avg"] = df["employees"].map(avg_score)  # 每位员工和平均值进行匹配
`
```

<img width="657" height="440" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_512b6d293fc441a08.jpg"/>

transform和agg到底是什么样的区别呢？

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

总结下二者的区别：

1.  transform是在原数据基础上加新的列，agg是根据分组字段和聚合函数生成新的数据帧
    
2.  transform的数据是填充到分组对象的每列上，agg只是生成了一个最终的聚合结果
    

## 七、总结

在实际的数据处理和统计工作中，分组统计求和、均值、个数等是十分常见的需求，本文结合实际的例子详解了pandas内部的groupby机制，以及如何聚合函数，希望对读者有所帮助。

<img width="10" height="10" src="../../../_resources/640_wx_fmt_gif_wxfrom_5_wx_lazy__5ed28272d84944c5b.gif"/>

推荐阅读

<img width="14" height="14" src="../../../_resources/640_wx_fmt_gif_wxfrom_5_wx_lazy__b1ac060d1fb84828a.gif"/>

[数据处理基石：数据探索](http://mp.weixin.qq.com/s?__biz=MzI4NjI3MzI2Ng==&mid=2247491098&idx=1&sn=5bc104d393b8725c857142ac24c2550a&chksm=ebde2587dca9ac919f11c5d7fc153a261f42b9de8af3e66603efaf9ec8c0bd6c33812a7663a8&scene=21#wechat_redirect)

[从理论到实战：基于Python的用户增长Cohort分析](http://mp.weixin.qq.com/s?__biz=MzI4NjI3MzI2Ng==&mid=2247491711&idx=1&sn=05686452b4ddc941dd25ae983e425b0b&chksm=ebdddbe2dcaa52f425a6bf2f8264eb8da21283779b4636b2d9c4bc5264c5889cd786cf81b23f&scene=21#wechat_redirect)

[Pandas数据类型操作](http://mp.weixin.qq.com/s?__biz=MzI4NjI3MzI2Ng==&mid=2247491606&idx=1&sn=71ca0fdd7a24b9dd34125c7995df6e98&chksm=ebdddb8bdcaa529d3ac4a32f4393dbb5a792f7134fda628c5ce778c6416a724f719d48965428&scene=21#wechat_redirect)

[LeetCode-SQL-184-部门工资最高的员工](http://mp.weixin.qq.com/s?__biz=MzI4NjI3MzI2Ng==&mid=2247490880&idx=1&sn=4947f120f9fed93eeda86a072cf7af06&chksm=ebde26dddca9afcbc3e4dd87e3ccada43a935ef3fe508a11fd8c42c5ae3d8bf984d05fb47466&scene=21#wechat_redirect)

[面试必备：SQL排名和窗口函数](http://mp.weixin.qq.com/s?__biz=MzI4NjI3MzI2Ng==&mid=2247490520&idx=1&sn=29792322118c456c186bbdeca3bc89c7&chksm=ebde2045dca9a953d62abdc807c6de7788f8864392de580bf2748008377df8c1af9b88f08739&scene=21#wechat_redirect)

> 尤而小屋，一个温馨的小屋。小屋主人，一手代码谋求生存，一手掌勺享受生活，欢迎你的光临

收录于话题 #<a id="js_album_keep_read_title"></a>python

 <a id="js_album_keep_read_size"></a>57个

<a id="js_album_prev"></a>上一篇 <a id="js_album_keep_read_pre_title"></a>最后一篇：玩转Pandas取数 <a id="js_album_next"></a>下一篇 <a id="js_album_keep_read_next_title"></a>从理论到实战：基于Python的用户增长Cohort分析

People who liked this content also liked

Python关联规则挖掘情侣、基友、渣男和狗

尤而小屋

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___f27a60434c744e2c8.bmp"/>

Scan to Follow