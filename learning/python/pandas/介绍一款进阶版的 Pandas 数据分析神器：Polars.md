# 介绍一款进阶版的 Pandas 数据分析神器：Polars

<a id="profileBt"></a><a id="js_name"></a>AI科技大本营 *2022-04-12 18:33*

The following article is from 关于数据分析与可视化 Author 俊欣

<a id="copyright_info"></a>[![](../../../_resources/0_be987964c4534ead85fc8a6a4c26adc4.jpg)<br>**关于数据分析与可视化** .<br>本公众号定期分享数据分析与可视化干货文章，并有时结合热点话题进行深入讨论，希望您会喜欢，要是哪里写的不好，也渴望倾听您的想法和意见，感谢！❤️](#)

<img width="677" height="119" src="../../../_resources/640_wx_fmt_gif_wxfrom_5_wx_lazy__cf9c053fcc9948a0a.gif"/>

作者 | 俊欣

出品 | 关于数据分析与可视化

相信对于不少的数据分析从业者来说呢，用的比较多的是`Pandas`以及`SQL`这两种工具，`Pandas`不但能够对数据集进行清理与分析，并且还能够绘制各种各样的炫酷的图表，但是遇到数据集很大的时候要是还使用`Pandas`来处理显然有点力不从心。

<img width="637" height="357" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__636c1dcc05fe4ad48.png"/>

今天小编就来介绍另外一个数据处理与分析工具，叫做`Polars`，它在数据处理的速度上更快，当然里面还包括两种API，一种是`Eager API`，另一种则是`Lazy API`，其中`Eager API`和`Pandas`的使用类似，语法类似差不太多，立即执行就能产生结果。

<img width="637" height="207" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__523fd3f1a7634fc98.png"/>

而`Lazy API`和`Spark`很相似，会有并行以及对查询逻辑优化的操作。

### 模块的安装与导入

我们先来进行模块的安装，使用`pip`命令

```


pip install polars



```

在安装成功之后，我们分别用`Pandas`和`Polars`来读取数据，看一下各自性能上的差异，我们导入会要用到的模块

```


import pandas as pd
import polars as pl
import matplotlib.pyplot as plt
%matplotlib inline



```

### 用`Pandas`读取文件

本次使用的数据集是某网站注册用户的用户名数据，总共有360MB大小，我们先用`Pandas`模块来读取该`csv`文件

```


%%time 
df = pd.read_csv("users.csv")
df.head()



```

output

<img width="637" height="363" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__8a4100e8c39e44e4a.png"/>

可以看到用`Pandas`读取`CSV`文件总共花费了12秒的时间，数据集总共有两列，一列是用户名称，以及用户名称重复的次数“n”，我们来对数据集进行排序，调用的是`sort_values()`方法，代码如下

```


%%time 
df.sort_values("n", ascending=False).head()



```

output

<img width="637" height="275" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__0b13a6bbc4bb4f31b.png"/>

### 用`Polars`来读取操作文件

下面我们用`Polars`模块来读取并操作文件，看看所需要的多久的时间，代码如下

```


%%time 
data = pl.read_csv("users.csv")
data.head()



```

output

<img width="292" height="203" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__cfa66b1ff2b14cf7b.png"/>

可以看到用`polars`模块来读取数据仅仅只花费了730毫秒的时间，可以说是快了不少的，我们根据“n”这一列来对数据集进行排序，代码如下

```


%%time
data.sort(by="n", reverse=True).head()



```

output

<img width="252" height="231" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__d25fa23e5de64241b.png"/>

对数据集进行排序所消耗的时间为1.39秒，接下来我们用polars模块来对数据集进行一个初步的探索性分析，数据集总共有哪些列、列名都有哪些，我们还是以熟知“泰坦尼克号”数据集为例

```


df_titanic = pd.read_csv("titanic.csv")
df_titanic.columns



```

output

```


['PassengerId',
 'Survived',
 'Pclass',
 'Name',
 'Sex',
 'Age',
 ......]



```

和`Pandas`一样输出列名调用的是`columns`方法，然后我们来看一下数据集总共是有几行几列的，

```


df_titanic.shape



```

output

```


(891, 12)



```

看一下数据集中每一列的数据类型

```


df_titanic.dtypes



```

output

```


[polars.datatypes.Int64,
 polars.datatypes.Int64,
 polars.datatypes.Int64,
 polars.datatypes.Utf8,
 polars.datatypes.Utf8,
 polars.datatypes.Float64,
......]



```

### 填充空值与数据的统计分析

我们来看一下数据集当中空值的分布情况，调用`null_count()`方法

```


df_titanic.null_count()



```

output

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

我们可以看到“Age”以及“Cabin”两列存在着空值，我们可以尝试用平均值来进行填充，代码如下

```


df_titanic["Age"] = df_titanic["Age"].fill_nan(df_titanic["Age"].mean())



```

计算某一列的平均值只需要调用`mean()`方法即可，那么中位数、最大/最小值的计算也是同样的道理，代码如下

```


print(f'Median Age: {df_titanic["Age"].median()}')
print(f'Average Age: {df_titanic["Age"].mean()}')
print(f'Maximum Age: {df_titanic["Age"].max()}')
print(f'Minimum Age: {df_titanic["Age"].min()}')



```

output

```


Median Age: 29.69911764705882
Average Age: 29.699117647058817
Maximum Age: 80.0
Minimum Age: 0.42



```

### 数据的筛选与可视化

我们筛选出年龄大于40岁的乘客有哪些，代码如下

```


df_titanic[df_titanic["Age"] > 40]



```

output

<img width="637" height="148" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__0eb9e508d14c4ee09.png"/>

最后我们简单地来绘制一张图表，代码如下

```


fig, ax = plt.subplots(figsize=(10, 5))
ax.boxplot(df_titanic["Age"])
plt.xticks(rotation=90)
plt.xlabel('Age Column')
plt.ylabel('Age')
plt.show()



```

output

<img width="637" height="321" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__1599e3e5c19a4296a.png"/>

总体来说呢，`polars`在数据分析与处理上面和`Pandas`模块有很多相似的地方，其中会有一部分的API存在着差异，感兴趣的童鞋可以参考其官网：https://www.pola.rs/

<img width="578" height="116" src="../../../_resources/640_wx_fmt_gif_wxfrom_5_wx_lazy__981e5ff2c1664b5c9.gif"/>

往

期

回

顾

技术

[YYDS！Python实现自动驾驶](http://mp.weixin.qq.com/s?__biz=Mzg4NDQwNTI0OQ==&mid=2247558656&idx=2&sn=fe88af86dfb2f3954d5f55df5da061fc&chksm=cfbb036ef8cc8a782e96d954f729ae0a730185f476a3f6d3ee450442f1afad04e1944ebd3ad4&scene=21#wechat_redirect)

资讯

[OpenAI发布DALL·E进化版，真香](http://mp.weixin.qq.com/s?__biz=Mzg4NDQwNTI0OQ==&mid=2247558625&idx=1&sn=4d37fc7ed4b08bb3df40f53cfb4fc33f&chksm=cfbb008ff8cc8999d5e6a426d9e3cf9783821ff932d5d701a3387b32994685f7f28c2e77e4b7&scene=21#wechat_redirect)

技术

[如何 “干掉” ERP?](http://mp.weixin.qq.com/s?__biz=Mzg4NDQwNTI0OQ==&mid=2247558687&idx=1&sn=7e3f688ddc0514de45e4418ec2e0fd86&chksm=cfbb0371f8cc8a6766396e3fefb32c50f2bdcc262a5c71cc0b7e006689b493c68feb02db96de&scene=21#wechat_redirect)

技术

[用Python打造一个语音合成系统](http://mp.weixin.qq.com/s?__biz=Mzg4NDQwNTI0OQ==&mid=2247558399&idx=1&sn=f7591eaea3f790e6fe7e06e5675f847f&chksm=cfbb0191f8cc88873e2ecdec59b33deb52555e4538fdea7be61ed3cac7c612ac5122428c3c74&scene=21#wechat_redirect)

<img width="30" height="28" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__06b851f9667641368.png"/>

**分享**

<img width="30" height="27" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__9af7269b11b040eab.png"/>

**点收藏**

<img width="30" height="28" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__0b1635d82ba54242b.png"/>

**点点赞**

<img width="30" height="28" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__c859f4f30c04414cb.png"/>

**点在看**

People who liked this content also liked

详解 Pandas 读取 csv 文件时2个有趣的参数设置

...

AI科技大本营

不看的原因

- 内容质量低
- 不看此公众号

用 Python 制作可视化 GUI 界面，一键实现自动分类管理文件！

...

AI科技大本营

不看的原因

- 内容质量低
- 不看此公众号

一文弄懂Python中的 if \_\_name\_\_==\_\_main\_\_

...

AI算法之道

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___70ea94e750294836a.bmp"/>

Scan to Follow