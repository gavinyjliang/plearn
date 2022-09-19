# Plotly可视化地位岌岌可危？Pandas内置绘图大全来袭！

<a id="copyright_logo"></a>Original <a id="js_author_name"></a>尤而小屋 <a id="profileBt"></a><a id="js_name"></a>尤而小屋 *2021-11-24 00:25*

收录于话题

<a id="js_article_tag_name__1999223977226862596"></a>#pandas <a id="js_article_tag_num__1999223977226862596"></a>50 <a id="js_article_tag_tips__1999223977226862596"></a>个

<a id="js_article_tag_name__1999693480435974145"></a>#数据可视化 <a id="js_article_tag_num__1999693480435974145"></a>51 <a id="js_article_tag_tips__1999693480435974145"></a>个

> 公众号：尤而小屋
> 作者：Peter
> 编辑：Peter

大家好，我是Peter~

之前写过很多关于Pandas的文章都是介绍如何使用Pandas来处理数据，这的确是它的强项。

[30个Pandas高频使用技巧](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247497436&idx=1&sn=98ad06ef2e46c317fa461cd60972c97b&chksm=cf12e606f8656f10f45780051eef0113b7188f256b927a55d94bb725f83a298f19c235bf0973&scene=21#wechat_redirect)

[对比SQL，学习Pandas操作：groupby机制](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247497021&idx=1&sn=2c2ea6708103258fdde7724d1dfae7d8&chksm=cf12e5e7f8656cf191aacbb0cc2fa7fd67dc04bf5722ff48fbf7e51cc63f00cace5380d2e6dd&scene=21#wechat_redirect)

[电商用户复购数据实战：图解Pandas的移动函数shift](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247494404&idx=1&sn=a46faab7f0c850c07a538b02330adb12&chksm=cf12fbdef86572c84f48cab7d48283ed9efaf976931ba4dc05ae6e35e414040a9d76c426f500&scene=21#wechat_redirect)

其实，Pandas还有一个内置的功能：绘图。你没有看错：**Pandas自身就是可以绘图的**。本文详细介绍基于Pandas的快速绘图方法。

<img width="657" height="252" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_b60103bc521f402e9.jpg"/>

## Pandas内置绘图

<img width="657" height="486" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_b3f4eef3665b4eb68.jpg"/>

## 参数

下面是常见的参数及解释，详细的请参考官网：https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.plot.html

```
DataFrame.plot(x=None, y=None,   # 指数据框列的标签或位置参数
               kind='line',  # 图的类型：line bar barh kde area scatter hist box等
               ax=None,  # 坐标轴
               subplots=False,   # 是否绘制子图 
               sharex=None,  # 是否共享xy轴
               sharey=False, 
               layout=None,  # 布局
               figsize=None,  # 大小
               use_index=True,  # 索引
               title=None,  # 图形标题
               grid=None,  # 网格线
               legend=True,   # 图例
               style=None,  # 风格
               logx=False,   # 对数化
               logy=False, 
               loglog=False,  # 开启对数化
               xticks=None,   # 设置x、y轴刻度值，序列形式（比如列表）
               yticks=None, 
               xlim=None,   # 设置坐标轴的范围，列表或元组形式
               ylim=None, 
               rot=None,  # 设置轴标签（轴刻度）的显示旋转度数
               xerr=None,  # 误差
               secondary_y=False,    # 开启第二y轴（右y轴）
               sort_columns=False,   # 以字母表顺序绘制各列，默认使用前列顺序
               **kwds)

```

## 模拟数据

本文中主要使用的一份模拟数据：根据numpy库模拟生成的

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 折线图

绘制最基础的图形：折线图

### 基础折线图

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 禁用图例

就是上图右上角的col1、col2、col3

```
# 禁用图例Legend
df.plot(legend=False,kind="line")
plt.show

```

<img width="657" height="364" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_47367e86504a49558.jpg"/>

### 调整两个轴的名称

```
# 设置两个轴的名称
df.plot(kind="line",xlabel="x_new",ylabel="y_new")
plt.show

```

<img width="657" height="383" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_fa76266a98664ff3b.jpg"/>

## 柱状图

### 基础柱状图

```
# 写法1
df.col1.plot(kind="bar",title="use pandas to make bar")
# 写法2
df["col1"].plot(kind="bar",title="use pandas to make bar")
# 写法3
df.col1.plot.bar(title="use pandas to make bar")
plt.show()

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 多元素柱状图

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 堆叠柱状图

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 水平柱状图

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

当然，也可以是堆叠的形式：

<img width="657" height="419" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_09d1a0b545b646dc8.jpg"/>

## 散点图

### 基础散点图

<img width="657" height="378" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_e2f4cc4456c049ce9.jpg"/>

### 改变大小和颜色

```
df.plot(kind="scatter", # 指定类型
        x="col1", y="col3",  # 指定两个轴
        s=df["col2"] *500,  # 点的大小
        c="r"  # 点的颜色
       )  
plt.show()

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 带颜色棒的散点图

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 饼图

### 针对Series

为了绘制饼图，模拟了一份新数据：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

```
series.plot(kind="pie",figsize=(6,6))
plt.show()

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 针对DataFrame

同样的，再绘制一份数据：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

<img width="657" height="316" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_525d8072eb634f0b8.jpg"/>

## 箱型图

### 基础箱型图

<img width="657" height="405" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_1c8311fd59ff40288.jpg"/>

### 自定义箱型图

```
# 自定义颜色
color = {"boxes": "DarkGreen",
         "whiskers": "DarkOrange",
         "medians": "DarkBlue",
         "caps": "Gray"}
df.plot.box(color=color, sym="r+")
plt.show()

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 水平箱型图

```
# 自定义颜色
color = {"boxes": "DarkGreen",
         "whiskers": "DarkOrange",
         "medians": "DarkBlue",
         "caps": "Gray"}
df.plot.box(color=color, 
            vert=False,  # 关键参数
            sym="r+")
plt.show()

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 使用boxplot绘箱型图

<img width="657" height="364" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_cf7cbc43304f4cc48.jpg"/>

参数是同样适用的：

<img width="657" height="412" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_aa0400d39e874554a.jpg"/>

## 蜂窝图

为了绘制不同的蜂窝图，模拟了一份新数据：

<img width="657" height="745" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_ec2006076c62409eb.jpg"/>

### 基础蜂窝图

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 改进版蜂窝图

```
df1.plot(
    kind="hexbin",
    x="A",
    y="B",
    C="C",  # 颜色深度的表示
    reduce_C_function=np.mean, # 指定不同聚合参数：mean/max/min/sum/std
    gridsize=30)
plt.show()

```

<img width="657" height="414" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_9c9f15dab4484ad4a.jpg"/>

## 直方图

```
# 写法1
df.plot(kind="hist",alpha=0.5)
# 写法2
df.plot.hist(alpha=0.5)
plt.show()

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 密度图

使用 Series.plot.kde() 和 DataFrame.plot.kde() 可以画出密度图：

1、针对DataFrame的密度图

<img width="657" height="410" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_f5c548ff550b42789.jpg"/>

2、针对Series的密度图

<img width="657" height="401" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_377a4ef833a34b5ea.jpg"/>

## 面积图

<img width="657" height="403" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_59f09403a43e4657b.jpg"/>

## 多子图

绘制子图主要的参数：

- subplots: 默认False, 如果希望每列绘制子图， 则赋值为True
    
- layout: 子图的布局, 即画布被横竖分为几块, 如:(2,3)表示2行3列
    
- figsize: 整个画布大小
    

```
df.plot(subplots=True,
        layout=(1,3),  # 1行3列
        figsize=(15,6),
        kind="bar"
       )
plt.show()

```

<img width="657" height="294" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_cd29b024ea0d445ab.jpg"/>

开启共享y轴的参数：

```
df.plot(subplots=True,
        layout=(1,3),  # 1行3列
        figsize=(15,6),
        kind="bar",
        sharey=True  # 开启共享y轴
       )
plt.show()

```

<img width="657" height="258" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_6ca5e5c7f7bb4e528.jpg"/>

## 散点矩阵图

```
# 单图导入
from pandas.plotting import scatter_matrix
scatter_matrix(df1,alpha=0.5,figsize=(14,6),diagonal="kde")
plt.show()

```

<img width="657" height="288" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_103ca241b04446cda.jpg"/>

## 平行分类图

为了绘制平行分类图，我们导入著名的iris数据集：

<img width="657" height="280" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_19060f8cff3c4f71b.jpg"/>

其中：**属性Name就是我们进行分类的数据字段**

```
# 导图模块
from pandas.plotting import parallel_coordinates
parallel_coordinates(
    iris, # 数据
    class_column="Name",  # 分类名称所用字段
    color=('#556270', '#4ECDC4', '#C7F464') # 颜色设置
)
plt.show()

```

<img width="657" height="369" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_84685f76e3a14f299.jpg"/>

## 总结

我们总结下Pandas内置绘图的特点：

- 代码量少，最大的优点
    
- 快速简洁，基本绘图可以满足
    
- 静态化，非动态可视化
    
- 图片质量一般
    

<img width="17" height="36" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__4ac28228de024cd1b.png"/>

推荐阅读

<img width="17" height="36" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__744115f280b240d09.png"/>

[这份Pandas练习题，必须成功拿下~](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247497927&idx=1&sn=68dbe9e8c8355b0b98377b5557c8a110&chksm=cf12e81df865610bb420419930a2d24dbbfd8e6b14b141ea60fe696224ee4a2ec120b210033c&scene=21#wechat_redirect)

[18张图+2大案例！精讲Plotly的热力图可视化制作](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247499720&idx=1&sn=18446e95860771e1f660a4571b843529&chksm=cf12ef12f86566043a9c3c756ba421961f91ecc0b1fb5efa3865352eecc6c39b92c0f2f98b1d&scene=21#wechat_redirect)

[Plotly真秀：一份数据，6种玩法](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247499322&idx=1&sn=74cbc35974b0719ae24012092e9baef3&chksm=cf12eee0f86567f6a5f5af0befb694ed5a1298c18bcdfb21f614475615ad7a7aa67af7bd04ff&scene=21#wechat_redirect)

[桑基图或许可以告诉你打工人的故事！](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247493084&idx=1&sn=ff56e5d6bd63ad993b0602ae3e1d2a83&chksm=cf12f506f8657c108416176561ee84ab0f3dda547fe53ed27fc590aff7af2f9f51a09c93fe84&scene=21#wechat_redirect)

[赞！可视化神器Plotly的图例Legend精讲](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247497344&idx=1&sn=d314c3a3a67a07d16a4c59f69a147906&chksm=cf12e65af8656f4c75f2a035fd8fdd42cb787de378807d9f8924dea9e62de4ebbc11c3b94137&scene=21#wechat_redirect)

> 尤而小屋，一个温馨的小屋。小屋主人，一手代码谋求生存，一手掌勺享受生活，欢迎你的光临

![](../../../_resources/0_wx_fmt_png_3f21f7c89ad94596b99abff7cb095a9f.png)

**尤而小屋**

尤而小屋，一个温馨且有爱的小屋🏡 小屋主人，一手代码谋求生存，一手掌勺享受生活，欢迎你的光临~

<a id="js_profile_article"></a>216篇原创内容

Official Account

收录于话题 #<a id="js_album_keep_read_title"></a>pandas

 <a id="js_album_keep_read_size"></a>50个

<a id="js_album_prev"></a>上一篇 <a id="js_album_keep_read_pre_title"></a>Plotly+Pandas+Sklearn：实现用户聚类分群！ <a id="js_album_next"></a>下一篇 <a id="js_album_keep_read_next_title"></a>我宣布啦。。。

<a id="js_view_source"></a>Read more

People who liked this content also liked

Python的可视化-legend（）函数

科技小白的唠唠叨叨

不看的原因

- 内容质量低
- 不看此公众号

pandas module

远小数

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___4c8bdd6e35774f809.bmp"/>

Scan to Follow