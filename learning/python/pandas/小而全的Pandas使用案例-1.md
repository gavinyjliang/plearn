# 小而全的Pandas使用案例

<a id="copyright_logo"></a>Original <a id="js_author_name"></a>尤而小屋 <a id="profileBt"></a><a id="js_name"></a>尤而小屋 *2022-01-09 23:50*

收录于话题

<a id="js_article_tag_name__1999223975666581509"></a>#数据分析 <a id="js_article_tag_num__1999223975666581509"></a>104 <a id="js_article_tag_tips__1999223975666581509"></a>个

<a id="js_article_tag_name__1999693480972845056"></a>#plotly <a id="js_article_tag_num__1999693480972845056"></a>27 <a id="js_article_tag_tips__1999693480972845056"></a>个

<a id="js_article_tag_name__1999693480435974145"></a>#数据可视化 <a id="js_article_tag_num__1999693480435974145"></a>51 <a id="js_article_tag_tips__1999693480435974145"></a>个

<a id="js_article_tag_name__1999223977226862596"></a>#pandas <a id="js_article_tag_num__1999223977226862596"></a>50 <a id="js_article_tag_tips__1999223977226862596"></a>个

> 公众号：尤而小屋
> 作者：Peter
> 编辑：Peter

大家好，我是Peter~

写过很多关于Pandas的文章，本文开展了一个简单的综合使用，主要分为：

- 如何自行模拟数据
    
- 多种数据处理方式
    
- 数据统计与可视化
    
- 用户RFM模型
    
- 用户复购周期
    

<img width="657" height="252" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_610b96e41e624c938.jpg"/>

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

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

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

<img width="657" height="625" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_3b8e6868fcef4be89.jpg"/>

4、生成水果的信息数据

```
infortmation = pd.DataFrame({
    "fruit":fruits,
    "price":[3.8, 8.9, 12.8, 6.8, 15.8, 4.9, 5.8, 7],
    "region":["华南","华北","西北","华中","西北","华南","华北","华中"]
})
infortmation

```

<img width="657" height="537" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_4d2e040eca4b42508.jpg"/>

5、数据合并

将订单信息和水果信息直接合并成一个完整的DataFrame，这个df就是接下来处理的数据

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

6、生成新的字段：订单金额

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

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

<img width="657" height="336" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_83f173aee3f14d87a.jpg"/>

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

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

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

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 商品月度销量变化

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

```
fig = px.bar(df5,x="year_month",y="amount",color="fruit")
fig.update_layout(xaxis_tickangle=45)   # 倾斜角度
fig.show()

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

折线图展示的变化：

<img width="657" height="341" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_6371bb151a7a4121a.jpg"/>

## 分析维度3：地区

### 不同地区的销量

<img width="657" height="369" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_4f51f278957241358.jpg"/>

<img width="657" height="333" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_d4b0d4708ce549f59.jpg"/>

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

<img width="657" height="586" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_f779fc41f0744d689.jpg"/>

```
px.bar(df10,
       x="fruit",
       y="amount",
#            color="number",
       facet_col="name"
      )

```

<img width="657" height="332" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_4592346264fd48cfb.jpg"/>

## 用户分层—RFM模型

RFM模型是衡量客户价值和创利能力的重要工具和手段。

通过这个模型能够反映一个用户的交期交易行为、交易的总体频率和总交易金额3项指��，通过3个指标来描述该客户的价值状况；同时依据这三项指标将客户划分为8类客户价值：

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

<img width="657" height="502" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_c1136c12654043919.jpg"/>

<img width="657" height="335" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_1c945125110e48b29.jpg"/>

当数据量足够大，用户足够多的时候，就可以只用RFM模型来将用户分成8个类型

## 用户复购周期分析

复购周期是用户每两次购买之间的时间间隔：以xiaoming用户为例，前2次的复购周期分别是4天和22天

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

下面是求解每个用户复购周期的过程：

1、每个用户的购买时间升序

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

2、将时间移动一个单位：

<img width="657" height="408" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_82ac6696b3724dbaa.jpg"/>

3、合并后的差值：

出现空值是每个用户的第一条记录之前是没有数据，后面直接删除了空值部分

<img width="657" height="574" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_d42dbc3c42714faaa.jpg"/>

<img width="657" height="461" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_d7f31dade501408ca.jpg"/>

直接取出天数的数值部分：

<img width="657" height="455" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_61d471afb6f6496b9.jpg"/>

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

<img width="657" height="333" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_cfa32030aaea44579.jpg"/>

上图中矩形越窄表示间隔越小；每个用户整个复购周期由整个矩形长度决定。查看每个用户的整体复购周期之和与平均复购周期：

<img width="657" height="353" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_96476b27331d4a4a9.jpg"/>

得到一个结论：Michk和Mike两个用户整体的复购周期是比较长的，长期来看是忠诚的用户；而且从平均复购周期来看，相对较低，说明在短时间内复购活跃。

从下面的小提琴中同样可以观察到，Michk和Mike的复购周期分布最为集中。

<img width="657" height="338" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_5c39baaec7f04859b.jpg"/>

<img width="17" height="36" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__0a5de965eec34c9a8.png"/>

推荐阅读

<img width="17" height="36" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__f6e160e4aabb4e4c8.png"/>

[Plotly+Seaborn+Folium可视化探索爱彼迎租房数据](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247502834&idx=1&sn=a077cf1875b9601a9f0b8a964b0b0278&chksm=cf12db28f865523e2fc71e9a2da31736a0eda3b07de906378e852c253b4777a5b7bc3b5b74df&scene=21#wechat_redirect)

[精华！Pandas数据排序实现](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247502756&idx=1&sn=0055ada3fd2eb5c4caa7bc9968c068e8&chksm=cf12db7ef86552681cb5a4590007a95ac0f36231cf05d973ebc2e9708678e1e91ad845340231&scene=21#wechat_redirect)

[可视化神器Plotly玩转子图](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247493100&idx=1&sn=cfd659c197279698089f08ff9e7ef6ba&chksm=cf12f536f8657c201ad151b4f8a93591d7c6e8b1b461edb8817aa75c2c021920e800af04f78d&scene=21#wechat_redirect)

[精选23个Pandas常用函数](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247502204&idx=1&sn=030b34c84657cfa6a04e2afee7836178&chksm=cf12d9a6f86550b085e6f5a0385d4346fa34038fd7aeecef6bf45d1b02dd6561bd2991c4956e&scene=21#wechat_redirect)

[Python爬虫：渣男 or 渣女？上十字架](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247501904&idx=1&sn=71b17c7ff5d8d28c151ee8a80809f7e2&chksm=cf12d88af865519ca7f8d24f9cd67f3c617d1304bfe2b34abf3f9d9f4b7391b0876e0aeb6f40&scene=21#wechat_redirect)

[可视化神器Plotly玩转小提琴图](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247493076&idx=1&sn=8bf16aed83366e0e62dcfe5ea9e71f0a&chksm=cf12f50ef8657c18391bde39d592e929a53a7f76ff12c288ebae6092c8a7f47c055b16c21c14&scene=21#wechat_redirect)

> 尤而小屋，一个温馨的小屋。小屋主人，一手代码谋求生存，一手掌勺享受生活，欢迎你的光临

![](../../../_resources/0_wx_fmt_png_7cde0da65e824cefa61cdd0d840f035f.png)

**尤而小屋**

尤而小屋，一个温馨且有爱的小屋🏡 小屋主人，一手代码谋求生存，一手掌勺享受生活，欢迎你的光临~

<a id="js_profile_article"></a>216篇原创内容

Official Account

收录于话题 #<a id="js_album_keep_read_title"></a>数据分析

 <a id="js_album_keep_read_size"></a>104个

<a id="js_album_prev"></a>上一篇 <a id="js_album_keep_read_pre_title"></a>30个数据可视化小技巧（文末赠书） <a id="js_album_next"></a>下一篇 <a id="js_album_keep_read_next_title"></a>Plotly+Seaborn+Folium可视化探索爱彼迎租房数据

<a id="js_view_source"></a>Read more

People who liked this content also liked

SQL数据查询 | 超全思维导图

良松数据分析

不看的原因

- 内容质量低
- 不看此公众号

在R语言中为回归结果添加森林图——bstfun包的应用

BNUlgr

不看的原因

- 内容质量低
- 不看此公众号

使用 nmon 对 Linux 系统性能进行故障排除和监控

毛毛虫AI进化之旅

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___69b9d5ee2a284e2c8.bmp"/>

Scan to Follow