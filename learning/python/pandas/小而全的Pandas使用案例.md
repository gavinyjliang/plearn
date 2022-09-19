# 小而全的Pandas使用案例

<a id="profileBt"></a><a id="js_name"></a>Python数据之道 *2022-02-17 12:16*

The following article is from 尤而小屋 Author 尤而小屋

<a id="copyright_info"></a>[![](../../../_resources/0_8a6d42bd3477410caba10eae3f6a7d0b.jpg)<br>**尤而小屋** .<br>尤而小屋，一个温馨且有爱的小屋🏡 小屋主人，一手代码谋求生存，一手掌勺享受生活，欢迎你的光临~](#)

![](../../../_resources/0_wx_fmt_png_1aaa2ba8bd344cd3a424f4006ea65a7b.png)

**Python数据之道**

点击领取《Python知识手册》高清电子版，回复数字 “600” 获取。「Python数据之道」秉承“让数据更有价值”的理念​，聚焦于 Python 数据分析、数据可视化、AI、机器学习、深度学习等领域。

<a id="js_profile_article"></a>235篇原创内容

Official Account

> 来源：尤而小屋

写过很多关于Pandas的文章，本文开展了一个简单的综合使用，主要分为：

- 如何自行模拟数据
    
- 多种数据处理方式
    
- 数据统计与可视化
    
- 用户RFM模型
    
- 用户复购周期
    

<img width="657" height="252" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_4e7fc41cfd5141ffa.jpg"/>

## 构建数据

本案例中用的数据是小编自行模拟的，主要包含两个数据：订单数据和水果信息数据，并且会将两份数据合并

```
import pandas as pd
import numpy as np
import random
from datetime import *
import time
import plotly.express as px
import plotly.graph_objects as go
import plotly as py
# 绘制子图
from plotly.subplots import make_subplots

```

1、时间字段

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

2、水果和用户

<img width="657" height="236" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_c1bb760438ac4fa0b.jpg"/>

3、生成订单数据

```
order = pd.DataFrame({
    "time":time_range,  # 下单时间
    "fruit":fruit_list,  # 水果名称
    "name":name_list,  # 顾客名
    # 购买量
    "kilogram":np.random.choice(list(range(50,100)), size=len(time_range),replace=True) 
})
order

```

<img width="657" height="625" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_2e5dbbf3bab54165a.jpg"/>

4、生成水果的信息数据

```
infortmation = pd.DataFrame({
    "fruit":fruits,
    "price":[3.8, 8.9, 12.8, 6.8, 15.8, 4.9, 5.8, 7],
    "region":["华南","华北","西北","华中","西北","华南","华北","华中"]
})
infortmation

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

5、数据合并

将订单信息和水果信息直接合并成一个完整的DataFrame，这个df就是接下来处理的数据

<img width="657" height="346" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_6686889151ce4d898.jpg"/>

6、生成新的字段：订单金额

<img width="657" height="467" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_5ca7cf2767664aeb9.jpg"/>

到这里你可以学到：

- 如何生成时间相关的数据
    
- 如何从列表（可迭代对象）中生成随机数据
    
- Pandas的DataFrame自行创建，包含生成新字段
    
- Pandas数据合并
    

## 分析维度1：时间

### 2019-2021年每月销量走势

1、先把年份和月份提取出来：

```
df["year"] = df["time"].dt.year
df["month"] = df["time"].dt.month
# 同时提取年份和月份
df["year_month"] = df["time"].dt.strftime('%Y%m')
df

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

2、查看字段类型：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

3、分年月统计并展示：

```
# 分年月统计销量
df1 = df.groupby(["year_month"])["kilogram"].sum().reset_index()
fig = px.bar(df1,x="year_month",y="kilogram",color="kilogram")
fig.update_layout(xaxis_tickangle=45)   # 倾斜角度
fig.show()

```

<img width="657" height="336" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_c53ae60feeb84f56a.jpg"/>

### 2019-2021销售额走势

```
df2 = df.groupby(["year_month"])["amount"].sum().reset_index()
df2["amount"] = df2["amount"].apply(lambda x:round(x,2))
fig = go.Figure()
fig.add_trace(go.Scatter(  #
    x=df2["year_month"],
    y=df2["amount"],
    mode='lines+markers', # mode模式选择
    name='lines')) # 名字
fig.update_layout(xaxis_tickangle=45)   # 倾斜角度
fig.show()

```

<img width="657" height="299" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_f8d4b66a41124c4d9.jpg"/>

### 年度销量、销售额和平均销售额

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 分析维度2：商品

### 水果年度销量占比

```
df4 = df.groupby(["year","fruit"]).agg({"kilogram":"sum","amount":"sum"}).reset_index()
df4["year"] = df4["year"].astype(str)
df4["amount"] = df4["amount"].apply(lambda x: round(x,2))
from plotly.subplots import make_subplots
import plotly.graph_objects as go
fig = make_subplots(
    rows=1, 
    cols=3,
    subplot_titles=["2019年","2020年","2021年"],
    specs=[[{"type": "domain"},   # 通过type来指定类型
           {"type": "domain"},
           {"type": "domain"}]]
)  
years = df4["year"].unique().tolist()
for i, year in enumerate(years):
    name = df4[df4["year"] == year].fruit
    value = df4[df4["year"] == year].kilogram
    
    fig.add_traces(go.Pie(labels=name,
                        values=value
                       ),
                 rows=1,cols=i+1
                )
fig.update_traces(
    textposition='inside',   # 'inside','outside','auto','none'
    textinfo='percent+label',
    insidetextorientation='radial',   # horizontal、radial、tangential
    hole=.3,
    hoverinfo="label+percent+name"
)
fig.show()

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 各水果年度销售金额对比

```
years = df4["year"].unique().tolist()
for _, year in enumerate(years):
    
    df5 = df4[df4["year"]==year]
    fig = go.Figure(go.Treemap( 
        labels = df5["fruit"].tolist(),
        parents = df5["year"].tolist(),
        values = df5["amount"].tolist(),
        textinfo = "label+value+percent root"
    ))
    
    fig.show()

```

<img width="657" height="274" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_f43cdc76127747248.jpg"/>

<img width="657" height="282" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_9d509bde676f49a78.jpg"/>

<img width="657" height="274" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_603697df461d4a038.jpg"/>

### 商品月度销量变化

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

```
fig = px.bar(df5,x="year_month",y="amount",color="fruit")
fig.update_layout(xaxis_tickangle=45)   # 倾斜角度
fig.show()

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

折线图展示的变化：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 分析维度3：地区

### 不同地区的销量

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 不同地区年度平均销售额

```
df7 = df.groupby(["year","region"])["amount"].mean().reset_index()

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 分析维度4：用户

### 用户订单量、金额对比

```
df8 = df.groupby(["name"]).agg({"time":"count","amount":"sum"}).reset_index().rename(columns={"time":"order_number"})
df8.style.background_gradient(cmap="Spectral_r")

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 用户水果喜好

根据每个用户对每种水果的订单量和订单金额来分析：

```
df9 = df.groupby(["name","fruit"]).agg({"time":"count","amount":"sum"}).reset_index().rename(columns={"time":"number"})
df10 = df9.sort_values(["name","number","amount"],ascending=[True,False,False])
df10.style.bar(subset=["number","amount"],color="#a97fcf")

```

<img width="657" height="586" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_0fee3dbb87d64693a.jpg"/>

```
px.bar(df10,
       x="fruit",
       y="amount",
#            color="number",
       facet_col="name"
      )

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 用户分层—RFM模型

RFM模型是衡量客户价值和创利能力的重要工具和手段。

通过这个模型能够反映一个用户的交期交易行为、交易的总体频率和总交易金额3项指标，通过3个指标来描述该客户的价值状况；同时依据这三项指标将客户划分为8类客户价值：

- Recency（R）是客户最近一次购买日期距离现在的天数，这个指标与分析的时间点有关，因此是变动的。理论上客户越是在近期发生购买行为，就越有可能复购
    
- Frequency（F）指的是客户发生购买行为的次数--最常购买的消费者，忠诚度也就较高。增加顾客购买的次数意味着能占有更多的时长份额。
    
- Monetary value（M）是客户购买花费的总金额。![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)
    

下面通过Pandas的多个方法来分别求解这个3个指标，首先是F和M：每位客户的订单次数和总金额

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

如何求解R指标呢？

1、先求解每个订单和当前时间的差值

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

2、根据每个用户的这个差值R来进行升序排列，排在第一位的那条数据就是他最近购买记录：以xiaoming用户为例，最近一次是12月15号，和当前时间的差值是25天

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

3、根据用户去重，保留第一条数据，这样便得到每个用户的R指标：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

4、数据合并得到3个指标：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

当数据量足够大，用户足够多的时候，就可以只用RFM模型来将用户分成8个类型

## 用户复购周期分析

复购周期是用户每两次购买之间的时间间隔：以xiaoming用户为例，前2次的复购周期分别是4天和22天

<img width="657" height="388" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_cd17478737694e52a.jpg"/>

下面是求解每个用户复购周期的过程：

1、每个用户的购买时间升序

<img width="657" height="391" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_05974b554ece44bcb.jpg"/>

2、将时间移动一个单位：

<img width="657" height="408" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_2f2b0bba3d5a46bc8.jpg"/>

3、合并后的差值：

出现空值是每个用户的第一条记录之前是没有数据，后面直接删除了空值部分

<img width="657" height="574" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_0d25599cd36d49a1b.jpg"/>

<img width="657" height="461" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_d55d8f970d0b4b219.jpg"/>

直接取出天数的数值部分：

<img width="657" height="455" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_525d69e6e35a4c378.jpg"/>

5、复购周期对比

```
px.bar(df16,
       x="day",
       y="name",
       orientation="h",
       color="day",
       color_continuous_scale="spectral"   # purples
      )

```

<img width="657" height="333" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_1f2d3fc27a324884b.jpg"/>

上图中矩形越窄表示间隔越小；每个用户整个复购周期由整个矩形长度决定。查看每个用户的整体复购周期之和与平均复购周期：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

得到一个结论：Michk和Mike两个用户整体的复购周期是比较长的，长期来看是忠诚的用户；而且从平均复购周期来看，相对较低，说明在短时间内复购活跃。

从下面的小提琴中同样可以观察到，Michk和Mike的复购周期分布最为集中。

<img width="657" height="338" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_94d25596001c48269.jpg"/>

**\-\-\-\-\-\-\-\- End --------**

<img width="657" height="67" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_0fade038bb9643709.jpg"/>

#### 精选资料

回复关键词，获取对应的资料：

| 关键词 | 资料名称 |
| --- | --- |
| **600** | [《Python知识手册》](https://mp.weixin.qq.com/mp/appmsgalbum?__biz=MzI2NjY5NzI0NA==&action=getalbum&album_id=1370549534602133504#wechat_redirect) |
| **md** | [《Markdown速查表》](https://mp.weixin.qq.com/s?__biz=MzI2NjY5NzI0NA==&mid=2247495631&idx=1&sn=720c931b1f5cdb82ceccd9b34b182a95&scene=21#wechat_redirect) |
| **time** | [《Python时间使用指南》](https://mp.weixin.qq.com/s?__biz=MzI2NjY5NzI0NA==&mid=2247492370&idx=1&sn=1dc6b3edef0fcb241d07757fb9e2ae03&scene=21#wechat_redirect) |
| **str** | [《Python字符串速查表》](https://mp.weixin.qq.com/s?__biz=MzI2NjY5NzI0NA==&mid=2247496716&idx=1&sn=8ec7a6b373059fa49f04433990581aa6&scene=21#wechat_redirect) |
| **pip** | [《Python：Pip速查表》](https://mp.weixin.qq.com/s?__biz=MzI2NjY5NzI0NA==&mid=2247500405&idx=1&sn=c5a760279babd1075c6153858af84af8&scene=21#wechat_redirect) |
| **style** | [《Pandas表格样式配置指南》](https://mp.weixin.qq.com/s?__biz=MzI2NjY5NzI0NA==&mid=2247501026&idx=1&sn=378292e5435b7ef5eede36192812da3b&scene=21#wechat_redirect) |
| **mat** | [《Matplotlib入门100个案例》](https://mp.weixin.qq.com/s?__biz=MzI2NjY5NzI0NA==&mid=2247493274&idx=1&sn=323662b49f3b8e0d619d44315537a76a&scene=21#wechat_redirect) |
| **px** | [《Plotly Express可视化指南》](https://mp.weixin.qq.com/s?__biz=MzI2NjY5NzI0NA==&mid=2247501349&idx=1&sn=04502491758816e83d43525e911cce56&scene=21#wechat_redirect) |

#### 精选内容

**数据科学：** [VS Code 中 Python配置使用指南](https://mp.weixin.qq.com/s?__biz=MzI2NjY5NzI0NA==&mid=2247495918&idx=1&sn=6b06eadc1604b693f48a127e7b5986a4&scene=21#wechat_redirect) | [财经工具 Tushare](https://mp.weixin.qq.com/s?__biz=MzI2NjY5NzI0NA==&mid=2247497358&idx=1&sn=e9574b69ca3c05142edd534e438e855e&scene=21#wechat_redirect) | [Matplotlib 最有价值的 50 个图表](https://mp.weixin.qq.com/s?__biz=MzI2NjY5NzI0NA==&mid=2247485238&idx=1&sn=f2d6b9136ff94697bdce9c9f48397e78&scene=21#wechat_redirect)

**书籍阅读：** [如何阅读一本书](https://mp.weixin.qq.com/s?__biz=MzU3OTA1MTUwOA==&mid=2247484765&idx=1&sn=0d3a39a00797b8710af2ea175bb20566&scene=21#wechat_redirect) | [巴菲特之道](https://mp.weixin.qq.com/s?__biz=MzU3OTA1MTUwOA==&mid=2247483880&idx=1&sn=a28b5ef681e81a54b5281242e7e2ad2f&scene=21#wechat_redirect) | [价值](https://mp.weixin.qq.com/s?__biz=MzU3OTA1MTUwOA==&mid=2247484963&idx=1&sn=341e105747bb1642e3bc89b74d69e98e&scene=21#wechat_redirect) | [原则](https://mp.weixin.qq.com/s?__biz=MzU3OTA1MTUwOA==&mid=2247485540&idx=1&sn=ce0d2dc7a87a5695d388e8ff7d4405b9&scene=21#wechat_redirect) | [投资最重要的事](https://mp.weixin.qq.com/s?__biz=MzU3OTA1MTUwOA==&mid=2247485096&idx=1&sn=a73862a6b3fc166916d0c71b8d16bfac&scene=21#wechat_redirect) | [戴维斯王朝](https://mp.weixin.qq.com/s?__biz=MzU3OTA1MTUwOA==&mid=2247485191&idx=1&sn=a8f45215977c2f1e1776fa396b105fbf&scene=21#wechat_redirect) | [客户的游艇在哪里](https://mp.weixin.qq.com/s?__biz=MzU3OTA1MTUwOA==&mid=2247485799&idx=1&sn=6197a65ac44c5d8001f61dd75d8419c8&scene=21#wechat_redirect) | [刻意练习](https://mp.weixin.qq.com/s?__biz=MzU3OTA1MTUwOA==&mid=2247485688&idx=1&sn=ae808f8999cc40dcab5c551be673b56b&scene=21#wechat_redirect) | [林肯传](https://mp.weixin.qq.com/s?__biz=MzU3OTA1MTUwOA==&mid=2247485857&idx=1&sn=d2149626b4b3d2fd5a8fab31acff2002&scene=21#wechat_redirect) | [金字塔原理](https://mp.weixin.qq.com/s?__biz=MzU3OTA1MTUwOA==&mid=2247484887&idx=1&sn=e8404120ca4c7800214d8bcf3971eb3d&scene=21#wechat_redirect)

**投资小结：** [2021Q4](https://mp.weixin.qq.com/s?__biz=MzU3OTA1MTUwOA==&mid=2247487417&idx=1&sn=a45d93aec5a3b91413f8a9d6b6a8decb&scene=21#wechat_redirect) | [2021Q3](https://mp.weixin.qq.com/s?__biz=MzU3OTA1MTUwOA==&mid=2247487065&idx=1&sn=8766ac15616c871c0ba23cada09aa47b&scene=21#wechat_redirect) | [2021Q2](https://mp.weixin.qq.com/s?__biz=MzU3OTA1MTUwOA==&mid=2247486390&idx=1&sn=91c3cad42676f67d96e94f9b2f205d9e&scene=21#wechat_redirect) | [2021Q1](https://mp.weixin.qq.com/s?__biz=MzU3OTA1MTUwOA==&mid=2247485738&idx=1&sn=5a5636423731174cde98902af2e47d7f&scene=21#wechat_redirect) | [2020Q4](https://mp.weixin.qq.com/s?__biz=MzU3OTA1MTUwOA==&mid=2247485416&idx=1&sn=0cf825834c4f86a81ba87abf300416a7&scene=21#wechat_redirect)

#### 精选视频

**可视化：** [Plotly Express](https://mp.weixin.qq.com/s?__biz=MzI2NjY5NzI0NA==&mid=2247501395&idx=2&sn=9306078c98e6a2efc6e478a5921f5276&scene=21#wechat_redirect)

**财经：** [Plotly在投资领域的应用](https://mp.weixin.qq.com/s?__biz=MzI2NjY5NzI0NA==&mid=2247496637&idx=1&sn=91910ea5f9034fc6685d28ef0f00a70c&scene=21#wechat_redirect) | [绘制K线图表](https://mp.weixin.qq.com/s?__biz=MzI2NjY5NzI0NA==&mid=2247497147&idx=3&sn=9e788955d2268e1ba3f9f8c85ec4ece2&scene=21#wechat_redirect)

**排序算法：** [汇总](https://mp.weixin.qq.com/s?__biz=MzI2NjY5NzI0NA==&mid=2247505089&idx=1&sn=71021ec9da57933b41b64174f3d904c2&scene=21#wechat_redirect) | [冒泡排序](https://mp.weixin.qq.com/s?__biz=MzI2NjY5NzI0NA==&mid=2247504041&idx=2&sn=8dc4e755344edda2fdbb1f4922fa013b&scene=21#wechat_redirect) | [选择排序](https://mp.weixin.qq.com/s?__biz=MzI2NjY5NzI0NA==&mid=2247504041&idx=3&sn=b9aeb552253f6c5e8efe635bbcaead1b&scene=21#wechat_redirect) | [快速排序](https://mp.weixin.qq.com/s?__biz=MzI2NjY5NzI0NA==&mid=2247504041&idx=4&sn=22a3d67b3cec0269060502c13fabae35&scene=21#wechat_redirect) | [归并排序](https://mp.weixin.qq.com/s?__biz=MzI2NjY5NzI0NA==&mid=2247504041&idx=5&sn=90feeb44a998a57dbdabb9e1823bb051&scene=21#wechat_redirect) | [堆排序](https://mp.weixin.qq.com/s?__biz=MzI2NjY5NzI0NA==&mid=2247504041&idx=6&sn=5e8421ee83aa301b37f7f25d7ba6388f&scene=21#wechat_redirect) | [插入排序](https://mp.weixin.qq.com/s?__biz=MzI2NjY5NzI0NA==&mid=2247504041&idx=7&sn=1889203106bce18adc50ff151399e6fc&scene=21#wechat_redirect) | [希尔排序](https://mp.weixin.qq.com/s?__biz=MzI2NjY5NzI0NA==&mid=2247504041&idx=8&sn=d1eccecfbb6f37e34b2eaf3d8908248b&scene=21#wechat_redirect) | [计数排序](https://mp.weixin.qq.com/s?__biz=MzI2NjY5NzI0NA==&mid=2247504165&idx=3&sn=0d79c516472fcfa73ebe97fdf1c13004&scene=21#wechat_redirect) | [桶排序](https://mp.weixin.qq.com/s?__biz=MzI2NjY5NzI0NA==&mid=2247504461&idx=3&sn=78afcc98eef1bc38060b701af6ac3822&scene=21#wechat_redirect) | [基数排序](https://mp.weixin.qq.com/s?__biz=MzI2NjY5NzI0NA==&mid=2247505087&idx=5&sn=52b1af66f7a48de7ab7d61e769b81f82&scene=21#wechat_redirect)

<img width="657" height="370" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_a41448e208ee45958.jpg"/>

People who liked this content also liked

当eBPF遇上Linux内核网络

Linux内核之旅

不看的原因

- 内容质量低
- 不看此公众号

SQL Server 中 ROWLOCK 行级锁

跟着阿笨一起玩NET

不看的原因

- 内容质量低
- 不看此公众号

数据分析利器Pandas Profiling，一行搞定探索性分析

我不爱机器学习

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___6a096ffbcb7a4b828.bmp"/>

Scan to Follow