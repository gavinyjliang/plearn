# 精华！12大Pandas常用配置技巧

<a id="copyright_logo"></a>Original <a id="js_author_name"></a>尤而小屋 <a id="profileBt"></a><a id="js_name"></a>尤而小屋 *2021-12-03 00:08*

收录于话题

<a id="js_article_tag_name__1999223975666581509"></a>#数据分析 <a id="js_article_tag_num__1999223975666581509"></a>104 <a id="js_article_tag_tips__1999223975666581509"></a>个

<a id="js_article_tag_name__1999223977226862596"></a>#pandas <a id="js_article_tag_num__1999223977226862596"></a>50 <a id="js_article_tag_tips__1999223977226862596"></a>个

<a id="js_article_tag_name__2019120411547860993"></a>#数据挖掘 <a id="js_article_tag_num__2019120411547860993"></a>37 <a id="js_article_tag_tips__2019120411547860993"></a>个

<a id="js_article_tag_name__1999223977696624640"></a>#机器学习 <a id="js_article_tag_num__1999223977696624640"></a>72 <a id="js_article_tag_tips__1999223977696624640"></a>个

<a id="js_article_tag_name__2000955722150248449"></a>#爆款文章 <a id="js_article_tag_num__2000955722150248449"></a>20 <a id="js_article_tag_tips__2000955722150248449"></a>个

> 公众号：尤而小屋
> 作者：Peter
> 编辑：Peter

大家好，我是Peter~

在Pandas的使用过程中，除了数据，我们更多的就是和表格打交道。为了更好地展示一份表格数据，必须前期有良好的设置。**文末有配置技巧总结，拿走即用！**

本文介绍的是Pandas的常用配置技巧，主要根据options和setings来展开的。强推官网学习地址：

https://pandas.pydata.org/pandas-docs/stable/user_guide/options.html

<img width="657" height="252" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_ff65593a2d8d46799.jpg"/>

## 导入

这是一种国际惯例的导入方式！

```
import pandas as pd

```

## 忽略警告

因为版本的更新，可能Pandas的某些用法在不久将会被移除，经常会出现一些警告（不是报错），配上如下的代码即可忽略相关的警告：

```
# 忽略警告
import warnings
warnings.filterwarnings('ignore')

```

## float型数据精度

### 查看默认精度

默认是保留6位小数。通过下面的方式来打印当前的精度：

```
pd.get_option( 'display.precision')

```

```
6

```

### 修改精度

将精度设置成2位

```
pd.set_option( 'display.precision',2)
# 写法2：pd.options.display.precision = 2

```

然后我们再次打印当前的精度则变成了2位：

```
pd.get_option( 'display.precision')

```

```
2

```

## 显示行数

### 查看显示行数

默认显示的行数是60

```
pd.get_option("display.max_rows")  # 默认是60

```

```
60

```

默认最少的行数是10位：

```
pd.get_option("display.min_rows")  # 最少显示行

```

```
10

```

### 修改显示行数

修改最大的显示行数成999，然后再查看：

```
pd.set_option("display.max_rows",999)  # 最多显示行数

```

```
pd.get_option("display.max_rows")

```

```
999

```

修改最少显示行数：

```
pd.set_option("display.min_rows",20)  

```

```
pd.get_option("display.min_rows")

```

```
20

```

### 重置功能

使用重置reset_option方法后，设置就会变成默认的形式（数值）：

```
pd.reset_option("display.max_rows")

```

```
pd.get_option("display.max_rows")  # 又恢复到60

```

```
60

```

```
pd.reset_option("display.min_rows")

```

```
pd.get_option("display.min_rows")  # 又恢复到10

```

```
10

```

### 正则功能

如果我们对多个options进行了修改设置，想同时恢复的话，使用正则表达式可以重置多条option。

在这里表示以displacy开头的设置全部重置：

```
# ^表示以某个字符开始，在这里表示以display开始全部重置
pd.reset_option("^display")

```

### 全部重置

如果使用all，则表示对全部的设置进行重置：

```
pd.reset_option('all')

```

## 显示列

既然能够控制显示的行数，当然也是可以控制显示的列数

### 查看显示列数

查看默认显示的列数是20：

```
pd.get_option('display.max_columns')
# 另一种写法：通过属性的方式
pd.options.display.max_columns  

```

```
20

```

### 改变列数

修改显示的列数成100：

```
# 修改成100
pd.set_option('display.max_columns',100)

```

查看修改后的列数：

```
# 查看修改后的值
pd.get_option('display.max_columns')

```

```
100

```

### 显示所有列

如果设置成None，则表示显示全部的列：

```
pd.set_option('display.max_columns',None)

```

### 重置

```
pd.reset_option('display.max_columns')

```

## 修改列宽

上面是查看列的数量，下面是针对每个列的宽度进行设置。单列数据宽度，以字符个数计算，超过时用省略号来表示。

### 默认列宽

默认的列宽是50个字符的宽度：

```
pd.get_option ('display.max_colwidth')

```

```
50

```

### 修改列宽

修改显示的列宽成100：

```
# 修改成100
pd.set_option ('display.max_colwidth', 100)

```

查看显示的列宽长度：

```
pd.get_option ('display.max_colwidth')

```

```
100

```

### 显示所有列

显示全部的列：

```
pd.set_option ('display.max_colwidth', None)

```

## 折叠功能

当我们输出数据宽度，超过了设置的宽度时，是否要折叠。通常使用False不折叠，相反True要折叠。

```
pd.set_option("expand_frame_repr", True)  # 折叠

```

```
pd.set_option("expand_frame_repr", False)  # 不折叠

```

## 代码段修改设置

上面介绍的各种设置，如果有修改的话都是整个环境的；我们还可以只给某个代码块进行临时的设置。

跑出当前的代码块，则会失效，恢复到原来的设置。

假设这里是第一个代码块：

```
print(pd.get_option("display.max_rows"))
print(pd.get_option("display.max_columns"))
60
20

```

这里是第二个代码块：

```
# 当前代码块进行设置
with pd.option_context("display.max_rows", 20, "display.max_columns", 10):
    print(pd.get_option("display.max_rows"))
    print(pd.get_option("display.max_columns"))
20
10

```

这里第三个代码块：

```
print(pd.get_option("display.max_rows"))
print(pd.get_option("display.max_columns"))
60
20

```

上面的例子我们可以发现：**到了指定的代码块之外，设置无效**

## 数字格式化

Pandas中有个display.float_format的方法，能够对浮点型的数字进行格式化输出，比如用千分位，百分比，固定小数位表示等。

如果其他数据类型可以转换为浮点数，也可以使用该方法。

> The callable should accept a floating point number and return a string with the desired format of the number

### 千分位表示

当数据比较大的时候，希望通过**千分位**的形式来表示数据，一目了然：

```
df = pd.DataFrame({
    "percent":[12.98, 6.13, 7.4],
    "number":[1000000.3183,2000000.4578,3000000.2991]})
df

```

<img width="657" height="262" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_4a4d5fe486f14f819.jpg"/>

<img width="657" height="271" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_ee76d5627c7d4306a.jpg"/>

### 百分比

<img width="657" height="303" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_1e13a31a0ca54f508.jpg"/>

### 特殊符号

除了%号，我们还可以使用其他的特殊符号来表示：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 零门槛转换

门槛转换是指什么意思呢？首先这个功能的实现使用的是display.chop_threshold方法。

表示将Series或者DF中数据展示为某个数的门槛。大于这个数，直接显示；小于的话，用0显示。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 更改绘图方法

默认情况下，**pandas使用matplotlib作为绘图后端**，我们可以进行设置修改：

```
import matplotlib.pyplot as plt
%matplotlib inline
# 默认情况
df1 = pd.DataFrame(dict(a=[5,3,2], b=[3,4,1]))
df1.plot(kind="bar")
plt.show()

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

更改下绘图后端，变成强大的plotly：

```
# 写法1
pd.options.plotting.backend = "plotly"
df = pd.DataFrame(dict(a=[5,3,2], b=[3,4,1]))
fig = df.plot()
fig.show()
# 写法2
df = pd.DataFrame(dict(a=[5,3,2], b=[3,4,1]))
fig = df.plot(backend='plotly') # 在这里指定
fig.show()

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 修改列头对齐方向

默认情况，属性字段（列头）是靠右对齐的，我们可以进行设置。下面看一个来自官网的例子：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 打印出当前设置并重置所有选项

`pd.describe_option()`是打印当前的全部设置，并重置所有选项。下面是部分设置选项：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 配置技巧

下面总结了常用的配置，复制即可使用：

```
import pandas as pd  # 国际惯例
import warnings
warnings.filterwarnings('ignore')  # 忽略文中的警告
pd.set_option( 'display.precision',2)
pd.set_option("display.max_rows",999)  # 最多显示行数
pd.set_option("display.min_rows",20)   # 最少显示行数
pd.set_option('display.max_columns',None)  # 全部列
pd.set_option ('display.max_colwidth', 100)   # 修改列宽
pd.set_option("expand_frame_repr", True)  # 折叠
pd.set_option('display.float_format',  '{:,.2f}'.format)  # 千分位
pd.set_option('display.float_format', '{:.2f}%'.format)  # 百分比形式
pd.set_option('display.float_format', '{:.2f}￥'.format)  # 特殊符号
pd.options.plotting.backend = "plotly"  # 修改绘图
pd.set_option("colheader_justify","left")  # 列字段对齐方式
pd.reset_option('all')  # 重置

```

<img width="17" height="36" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__634cd93b79b445cf8.png"/>

推荐阅读

<img width="17" height="36" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__d3eabfe2d97e40d6b.png"/>

[Pandas表格美颜技巧！](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247500484&idx=1&sn=5e2610fe92b124e65733a17ace1e4b8a&chksm=cf12d21ef8655b08c372622a3cadb13bfbd46a63c41f58a8d270da5cf5d6151f199445781285&scene=21#wechat_redirect)

[Plotly+Pandas+Sklearn：实现用户聚类分群！](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247500041&idx=1&sn=e29f380b176df342a03850f655d09991&chksm=cf12d1d3f86558c571b088bd846207f22b150e290dc660fe93125d9cf813594515153a6f0c68&scene=21#wechat_redirect)

[18张图+2大案例！精讲Plotly的热力图可视化制作](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247499720&idx=1&sn=18446e95860771e1f660a4571b843529&chksm=cf12ef12f86566043a9c3c756ba421961f91ecc0b1fb5efa3865352eecc6c39b92c0f2f98b1d&scene=21#wechat_redirect)

[纯国产可视化库Pyecharts首秀！](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247499203&idx=1&sn=b19044259e59df024e3e9882a1a1aaa0&chksm=cf12ed19f865640fa1851aee4890397b4407814541c74361ee7072781f82c944f3eef4bd25aa&scene=21#wechat_redirect)

[我宣布啦。。。](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247499580&idx=1&sn=d89595a344b6c29a8b3a1076494f9b6a&chksm=cf12efe6f86566f0ca859be13f57b0dbf59254f3f0b42985a6492886485750628e0f13ccb1d0&scene=21#wechat_redirect)

[可视化神器Plotly玩转子图](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247493100&idx=1&sn=cfd659c197279698089f08ff9e7ef6ba&chksm=cf12f536f8657c201ad151b4f8a93591d7c6e8b1b461edb8817aa75c2c021920e800af04f78d&scene=21#wechat_redirect)

> 尤而小屋，一个温馨的小屋。小屋主人，一手代码谋求生存，一手掌勺享受生活，欢迎你的光临

![](../../../_resources/0_wx_fmt_png_2104e2b6e85147c79c31b105e7566892.png)

**尤而小屋**

尤而小屋，一个温馨且有爱的小屋🏡 小屋主人，一手代码谋求生存，一手掌勺享受生活，欢迎你的光临~

<a id="js_profile_article"></a>216篇原创内容

Official Account

收录于话题 #<a id="js_album_keep_read_title"></a>数据分析

 <a id="js_album_keep_read_size"></a>104个

<a id="js_album_prev"></a>上一篇 <a id="js_album_keep_read_pre_title"></a>机器学习绘图神器Matplotlib首秀！ <a id="js_album_next"></a>下一篇 <a id="js_album_keep_read_next_title"></a>Plotly+Pandas+Sklearn：实现用户聚类分群！

<a id="js_view_source"></a>Read more

People who liked this content also liked

Spring boot 调用 mongodb

全栈精英

不看的原因

- 内容质量低
- 不看此公众号

从零开始学Python爬虫（四）：正则表达式

Coder陈

不看的原因

- 内容质量低
- 不看此公众号

pandas module

远小数

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___30f5e3a36f004a6ba.bmp"/>

Scan to Follow