# Pandas vs SQL

<a id="profileBt"></a><a id="js_name"></a>数据分析与开发 *2022-03-05 12:10*

The following article is from 渡码 Author 渡码

<a id="copyright_info"></a>[![](../../../_resources/0_8c2f1f9fb7944951ae4c9a2a9e6d1737.jpg)<br>**渡码** .<br>持续输出 Python 全栈优质文章](#)

今天来分享下 Pandas 与 SQL 的对比。

Pandas 和 SQL 有很多相似之处，都是对二维表的数据进行查询、处理，都是数据分析中常用的工具。

对于只会 Pandas 或只会 SQL 的朋友，可以通过今天例子快速学会另一个。

#### 1\. 数据查询

首先，读取数据

```
import pandas as pd
import numpy as np
tips = pd.read_csv('tips.csv')

```

<img width="254" height="241" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__7923e714eca24ea39.png"/>

tips

##### 1.1 查询列

查询 `total_bill`和`tip` 两列

```
tips[["total_bill", "tip"]]

```

用 SQL 实现：

```
select total_bill, tip
from tips;

```

##### 1.2 增加列

查询结果中，新增一列`tip_rate`

```
tips['tip_rate'] = tips["tip"] / tips["total_bill"]

```

用 SQL 实现：

```
select *, tip/total_bill as tip_rate
from tips;

```

##### 1.3 筛选条件

查询 `time`列等于`Dinner`并且`tip`列大于5的数据

```
tips[(tips["time"] == "Dinner") & (tips["tip"] > 5.00)]

```

用 SQL 实现：

```
select *
from tips
where time = 'Dinner' and tip > 5.00;

```

#### 2\. 分组聚合

按照某列分组计数

```
tips.groupby("sex").size()
'''
sex
Female     87
Male      157
dtype: int64
'''

```

用 SQL 实现：

```
select sex, count(*)
from tips
group by sex;

```

按照多列聚合多个值

```
tips.groupby(["smoker", "day"]).agg({"tip": [np.size, np.mean]})

```

用 SQL 实现：

```
select smoker, day, count(*), avg(tip)
from tips
group by smoker, day;

```

#### 3\. join

构造两个临时`DataFrame`

```
df1 = pd.DataFrame({"key": ["A", "B", "C", "D"], "value": np.random.randn(4)})
df2 = pd.DataFrame({"key": ["B", "D", "D", "E"], "value": np.random.randn(4)})

```

先用 Pandas 分别实现`inner join`、`left join`、`right join`和`full join`。

```
# inner join
pd.merge(df1, df2, on="key")
# left join
pd.merge(df1, df2, on="key", how="left")
# inner join
pd.merge(df1, df2, on="key", how="right")
# inner join
pd.merge(df1, df2, on="key", how="outer")

```

用 SQL 分别实现：

```
# inner join
select *
from df1 inner join df2
on df1.key = df2.key;
# left join
select *
from df1 left join df2
on df1.key = df2.key;
# right join
select *
from df1 right join df2
on df1.key = df2.key;
# full join
select *
from df1 full join df2
on df1.key = df2.key;

```

#### 4\. union

将两个表纵向堆叠

```
pd.concat([df1, df2])

```

用 SQL 实现：

```
select *
from df1
union all
SELECT *
from df2;

```

将两个表纵向堆叠并去重

```
pd.concat([df1, df2]).drop_duplicates()

```

用 SQL 实现：

```
select *
from df1
union
SELECT *
from df2;

```

#### 5\. 开窗

对`tips`中`day`列取值相同的记录按照`total_bill`排序。

```
(tips.assign(
        rn=tips.sort_values(["total_bill"], ascending=False)
        .groupby(["day"])
        .cumcount()
        + 1
    )
    .sort_values(["day", "rn"])
)

```

用 SQL 实现：

```
select
    *,
    row_number() over(partition by day order by total_bill desc) as rn
from tips t

```

`day`列取值相同的记录会被划分到同一个窗口内，并按照`total_bill`排序，窗口之间的数据互不影响，这类操作便被称为**开窗**。

今天的内容就到这里啦。通过几个简单的实践案例大家可以直观感受下 Pandas 和 SQL 在数据处理上的相似之处。

\- EOF -

推荐阅读  点击标题可跳转

<ins>1、[这 25 个 Pandas 实用技巧你都会吗](http://mp.weixin.qq.com/s?__biz=MzA5ODM5MDU3MA==&mid=2650882347&idx=2&sn=e45d7459833ce10186141c3ec26ea500&chksm=8b67a66ebc102f785e46ccd3b7242d543e3ff2670b2129e0ebba0a10275e4de85cb8e26b84df&scene=21#wechat_redirect)</ins>

<ins>2、[18000 字的 SQL 优化大全，收藏直接起飞！](http://mp.weixin.qq.com/s?__biz=MzA5ODM5MDU3MA==&mid=2650881675&idx=2&sn=967dae1a639fddd2b5f855e185f4ecdb&chksm=8b67d9cebc1050d8d04dbb626b48ace5e06a9b6012028b773a2235da4ee819125df660b09882&scene=21#wechat_redirect)</ins>

<ins>3、[最强最全面的大数据 SQL 面试题和答案](http://mp.weixin.qq.com/s?__biz=MzA5ODM5MDU3MA==&mid=2650879785&idx=1&sn=13f2f5ede115df806eeddf5290c82953&chksm=8b67d06cbc10597ab020e5e32ac1c24156f45cdc8663492401d1c8654d82335792a967519d63&scene=21#wechat_redirect)</ins>

看完本文有收获？请转发分享给更多人

**推荐关注「数据分析与开发」，提升数据技能**

![](../../../_resources/0_wx_fmt_png_cf8e998aa1d345a5803a888567402d72.png)

**数据分析与开发**

「数据分析与开发」分享数据分析与开发相关技术文章、教程、工具

<a id="js_profile_article"></a>55篇原创内容

Official Account

点赞和在看就是最大的支持❤️

People who liked this content also liked

运维老司机才看得懂的 Linux 趣图，你能看懂几个？

高效运维

不看的原因

- 内容质量低
- 不看此公众号

Python 一个快速视频剪辑编辑神器 — Moviepy

Python实用宝典

不看的原因

- 内容质量低
- 不看此公众号

Excel图表配色原理，超全面！

爱数据LoveData

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___41c1035724ed40c9a.bmp"/>

Scan to Follow