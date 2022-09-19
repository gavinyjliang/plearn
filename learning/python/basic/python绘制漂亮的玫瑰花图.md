# python绘制漂亮的玫瑰花图

<a id="profileBt"></a><a id="js_name"></a>Python数据之道 *2022-05-22 11:01* *Posted on <a id="js_ip_wording"></a>湖南*

The following article is from python数据分析之禅 Author 小dull鸟

<a id="copyright_info"></a>[![](../../../_resources/0_60eb388590604dcb90f440d4fde26779.jpg)<br>**python数据分析之禅** .<br>点击领取pandas高清速查表，后台回复“速查表”获取](#)

![](../../../_resources/0_wx_fmt_png_2bdec07d20874c079bf2b01b4efe2c10.png)

**Python数据之道**

点击领取《Python知识手册》高清电子版，回复数字 “600” 获取。「Python数据之道」秉承“让数据更有价值”的理念​，聚焦于 Python 数据分析、数据可视化、AI、机器学习、深度学习等领域。

<a id="js_profile_article"></a>240篇原创内容

Official Account

> 来源：python数据分析之禅

* * *

今天主要给大家介绍如何用pyecharts画各种漂亮的数学图形

<img width="677" height="376" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__a04fd3e89a7d48998.png"/>

# 一、基本极坐标图

说简单点，基本极坐标图就是圆形的散点图（柱状图或折线图），代码如下：

```
`import random
from pyecharts import options as opts
from pyecharts.charts import Polar
data = [(i, random.randint(1, 100)) for i in range(101)]
c = (
    Polar()
    .add("", data, type_="scatter", label_opts=opts.LabelOpts(is_show=False))
    .set_global_opts(title_opts=opts.TitleOpts(title="基本极坐标图"))
)
c.render_notebook()
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

数据data是1个列表，列表内的元素为元组，单个元组有2个数据，第一个数据为半径，第二个数据相当于角度，这样就好理解了

也可把type改为bar

```
`c = (
    Polar()
    .add("", data, type_="bar", label_opts=opts.LabelOpts(is_show=False))
    .set_global_opts(title_opts=opts.TitleOpts(title="基本极坐标图"))
)
c.render_notebook()
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

同理也可以改成折线（line）等，大家可以自己尝试一下。

# 二、极半径图

在极坐标中引入柱状图

```
`from pyecharts import options as opts
from pyecharts.charts import Polar
from pyecharts.faker import Faker
c = (
    Polar()
    .add_schema(
        radiusaxis_opts=opts.RadiusAxisOpts(data=Faker.week,  #数据项
                                            type_="category"  #坐标轴类型，类目轴，适用于离散的类目数据，为该类型时必须通过 data 设置类目数据。
                                           ),
        angleaxis_opts=opts.AngleAxisOpts(is_clockwise=True, #是否顺时针排布
                                          max_=10            #坐标轴刻度最大值
                                         ),
    )
    .add("A", [1, 2, 3, 4, 3, 5, 1], type_="bar")
    .set_global_opts(title_opts=opts.TitleOpts(title="Polar-RadiusAxis"))
)
c.render_notebook()
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 参数介绍

```
`RadiusAxisOpts：极坐标系径向轴配置项``AngleAxisOpts：极坐标系角度轴配置项`
```

# 三、画玫瑰花图

首先我们要引入数学中的sin函数，假设角度为theta，则长度为n(m+sin(theta)),n和m都为常量，那么元组（长度，角度）就可以在极坐标中确定一个点，把一系列的点放入列表中，并用折线图连接起来，就可以画出漂亮的数学图形。

```
`import math
import pyecharts.options as opts
from pyecharts.charts import Polar
data = []
for i in range(0, 101):
    theta = i / 100 * 360
    r = 5 * (1 + math.sin(theta / 180 * math.pi))
    data.append([r, theta])
c=(
    Polar()
    .add(series_name="line", data=data, label_opts=opts.LabelOpts(is_show=False))
    .add_schema(
        angleaxis_opts=opts.AngleAxisOpts(
            start_angle=0, type_="value", is_clockwise=True
        )
    )
    .set_global_opts(
        tooltip_opts=opts.TooltipOpts(trigger="axis", axis_pointer_type="cross"),
        title_opts=opts.TitleOpts(title="极坐标双数值轴"),
    )
)
c.render_notebook()
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### AngleAxisOpts参数介绍：

```
`start_angle：极坐标开始的角度``type_：坐标轴类型，'value'表示数值轴，适用于连续数据``is_clockwise：是否为顺时针`
```

### TooltipOpts参数介绍：

```
`trigger：触发类型，'axis'表示坐标轴触发，主要在柱状图，折线图等会使用类目轴的图表中使用``axis_pointer_type：指示器类型，'cross'表示十字准星指示器`
```

## 开始画玫瑰花图

```
`import math
from pyecharts import options as opts
from pyecharts.charts import Polar
data = []
for i in range(401):
    t = i / 180 * math.pi
    r = math.sin(9*t)
    data.append([r, i])
c = (
    Polar()
    .add_schema(angleaxis_opts=opts.AngleAxisOpts(start_angle=0, min_=0))
    .add("flower", data, label_opts=opts.LabelOpts(is_show=False))
    .set_global_opts(title_opts=opts.TitleOpts(title="Polar-Flower"))
)
c.render_notebook()
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

是不是很漂亮，利用这种方法还可以画出更多漂亮的图形，赶紧动手试试吧！

Python数据之道

，赞 19

People who liked this content also liked

分享10个超级实用事半功倍的Python自动化脚本

...

简说Python

不看的原因

- 内容质量低
- 不看此公众号

又一款python开发神器

...

基因学苑

不看的原因

- 内容质量低
- 不看此公众号

使用图片隐写的Python远控恶意样本分析

...

ChaMd5安全团队

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___0229fa30f5a64e7a9.bmp"/>

Scan to Follow