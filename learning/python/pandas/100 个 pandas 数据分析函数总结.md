# 100 个 pandas 数据分析函数总结

<a id="profileBt"></a><a id="js_name"></a>数据分析与开发 *2021-03-30 12:10*

The following article is from 数据分析1480 Author 刘顺祥

<a id="copyright_info"></a>[![](../../../_resources/0_a3edd9f0448747d3ab567f382373cee4.jpg)<br>**数据分析1480** .<br>出版《从零开始学Python数据分析与挖掘》和《数据分析从入门到进阶》，定期与大家分享数据分析和挖掘干货，包括R语言与Python的案例实战、大数据平台架构与应用以及各种福利。欢迎大家的关注与交流，真正做到收获一点点，进步一点点！](#)

经过一段时间的整理，本期将分享我认为比较常规的100个实用函数，这些函数大致可以分为六类，分别是统计汇总函数、数据清洗函数、数据筛选、绘图与元素级运算函数、时间序列函数和其他函数。

**一、统计汇总函数**

数据分析过程中，必然要做一些数据的统计汇总工作，那么对于这一块的数据运算有哪些可用的函数可以帮助到我们呢？具体看如下几张表。

<img width="661" height="419" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__0739adbc270a4150b.png"/>

<img width="661" height="390" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__51b0952ae3a14d2eb.png"/>

```


import pandas as pd
import numpy as np
x = pd.Series(np.random.normal(2,3,1000))
y = 3*x + 10 + pd.Series(np.random.normal(1,2,1000))
# 计算x与y的相关系数
print(x.corr(y))
# 计算y的偏度
print(y.skew())
# 计算y的统计描述值
print(x.describe())
z = pd.Series(['A','B','C']).sample(n = 1000, replace = True)
# 重新修改z的行索引
z.index = range(1000)
# 按照z分组，统计y的组内平均值
y.groupby(by = z).aggregate(np.mean)


```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

```


# 统计z中个元素的频次
print(z.value_counts())
a = pd.Series([1,5,10,15,25,30])
# 计算a中各元素的累计百分比
print(a.cumsum() / a.cumsum()[a.size - 1])


```

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__56f73d79a98b44bd8.png)

**二、数据清洗函数**

同样，数据清洗工作也是必不可少的工作，在如下表格中罗列了常有的数据清洗的函数。

<img width="661" height="307" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_72dd1855b41945408.jpg"/>

```


x = pd.Series([10,13,np.nan,17,28,19,33,np.nan,27])
#检验序列中是否存在缺失值
print(x.hasnans)
# 将缺失值填充为平均值
print(x.fillna(value = x.mean()))
# 前向填充缺失值
print(x.ffill())


```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

```


income = pd.Series(['12500元','8000元','8500元','15000元','9000元'])
# 将收入转换为整型
print(income.str[:-1].astype(int))
gender = pd.Series(['男','女','女','女','男','女'])
# 性别因子化处理
print(gender.factorize())
house = pd.Series(['大宁金茂府 | 3室2厅 | 158.32平米 | 南 | 精装',
                   '昌里花园 | 2室2厅 | 104.73平米 | 南 | 精装',
                   '纺大小区 | 3室1厅 | 68.38平米 | 南 | 简装'])
# 取出二手房的面积，并转换为浮点型
house.str.split('|').str[2].str.strip().str[:-2].astype(float)


```

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__d7daccdc13ee406aa.png)

**三、数据筛选**

数据分析中如需对变量中的数值做子集筛选时，可以巧妙的使用下表中的几个函数，其中部分函数既可以使用在序列身上，也基本可以使用在数据框对象中。

<img width="661" height="334" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__4507067c880f4fc2a.png"/>

```


np.random.seed(1234)
x = pd.Series(np.random.randint(10,20,10))
# 筛选出16以上的元素
print(x.loc[x > 16])
print(x.compress(x > 16))
# 筛选出13~16之间的元素
print(x[x.between(13,16)])
# 取出最大的三个元素
print(x.nlargest(3))
y = pd.Series(['ID:1 name:张三 age:24 income:13500',
               'ID:2 name:李四 age:27 income:25000',
               'ID:3 name:王二 age:21 income:8000'])
# 取出年龄，并转换为整数
print(y.str.findall('age:(\d+)').str[0].astype(int))


```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

**四、绘图与元素级函数**

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

```


np.random.seed(123)
import matplotlib.pyplot as plt
x = pd.Series(np.random.normal(10,3,1000))
# 绘制x直方图
x.hist()
# 显示图形
plt.show()
# 绘制x的箱线图
x.plot(kind='box')
plt.show()
installs = pd.Series(['1280万','6.7亿','2488万','1892万','9877','9877万','1.2亿'])
# 将安装量统一更改为“万”的单位
def transform(x):
    if x.find('亿') != -1:
        res = float(x[:-1])*10000
    elif x.find('万') != -1:
        res = float(x[:-1])
    else:
        res = float(x)/10000
    return res
installs.apply(transform)


```

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__eeb88c5ee40b4a289.png)

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__3aa0cb5ba9494c09b.png)

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__e109a414016645e4a.png)

**五、时间序列函数**

<img width="661" height="290" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__f41a3cb985d94d68a.png"/>

<img width="661" height="250" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__8cb8cf81f4c54ad1a.png"/>

<img width="661" height="224" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__dddd944c59264b7a8.png"/>

**六、其他函数**

<img width="661" height="328" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__9d089ce1f0c24e86b.png"/>

```


import numpy as np
import pandas as pd
np.random.seed(112)
x = pd.Series(np.random.randint(8,18,6))
print(x)
# 对x中的元素做一阶差分
print(x.diff())
# 对x中的元素做降序处理
print(x.sort_values(ascending = False))
y = pd.Series(np.random.randint(8,16,100))
# 将y中的元素做排重处理，并转换为列表对象
y.unique().tolist()


```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__d132fc732a3343758.png)

\- EOF -

推荐阅读  点击标题可跳转

<ins>1、[Pandas 必知必会的使用技巧，值得收藏！](http://mp.weixin.qq.com/s?__biz=MzA5ODM5MDU3MA==&mid=2650869810&idx=2&sn=cea7c191f87235d091c30191eda28abc&chksm=8b67f777bc107e615e747b266937c032624708209d0339086d0edff9fe69fc843ee1aa696b4d&scene=21#wechat_redirect)</ins>

<ins>2、[Python 数据处理库 pandas 入门教程](http://mp.weixin.qq.com/s?__biz=MzA5ODM5MDU3MA==&mid=2650863791&idx=2&sn=2e82fff1556fdf69181fbcfb4cb2ab5c&chksm=8b661feabc1196fc67a9d5cd45a13fb514ff6def918243e482a30b129aa3296b705a18adba69&scene=21#wechat_redirect)</ins>

<ins>3、[Pandas 可视化综合指南：手把手从零教你绘制数据图表](http://mp.weixin.qq.com/s?__biz=MzA5ODM5MDU3MA==&mid=2650865469&idx=1&sn=e6e6ea23a0c0e77586c0eb318e9ecee3&chksm=8b661878bc11916e5eae2ca895ec54d711437d799023c5b9382589b8ddd0e0f8783f09975380&scene=21#wechat_redirect)</ins>

看完本文有收获？请转发分享给更多人

**推荐关注「数据分析与开发」，提升数据技能**

![](../../../_resources/0_wx_fmt_png_13e6008edb3d461ca1c0a6b42c318464.png)

**数据分析与开发**

「数据分析与开发」分享数据分析与开发相关技术文章、教程、工具

<a id="js_profile_article"></a>55篇原创内容

Official Account

点赞和在看就是最大的支持❤️

People who liked this content also liked

我学习 Python 的三个神级网站

数据分析与开发

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___bf2d2b1a422841f1a.bmp"/>

Scan to Follow