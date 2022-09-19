# Pandas 数据排序，人人都能学会的几种方法

<a id="profileBt"></a><a id="js_name"></a>AI科技大本营 *2022-05-26 18:40* *Posted on <a id="js_ip_wording"></a>北京*

The following article is from Python数据之道 Author 阳哥

<a id="copyright_info"></a>[![](../../../_resources/0_0b77e4ee35d14741bce4ba0fd706f466.jpg)<br>**Python数据之道** .<br>点击领取《Python知识手册》高清电子版，回复数字 “600” 获取。「Python数据之道」秉承“让数据更有价值”的理念​，聚焦于 Python 数据分析、数据可视化、AI、机器学习、深度学习等领域。](#)

<img width="661" height="117" src="../../../_resources/640_wx_fmt_gif_wxfrom_5_wx_lazy__5a956a491b1641648.gif"/>

作者 | 阳哥

来源 | Python数据之道

Pandas 可以说是 在Python数据科学领域应用最为广泛的工具之一。

Pandas是一种高效的数据处理库，它以 `dataframe` 和 `series` 为基本数据类型，呈现出类似excel的二维数据。

在数据处理过程中，咱们经常需要将数据按照一定的要求进行排序，以方便展示。

这里，来给大家分享下 在 Pandas 中排序的几种常用方法，主要包括 `sort_index`  和 `sort_values` 。

## 01 按索引排序

### 数据准备

文中主要使用了 `pandas` 和 `numpy` ，首先导入 Python 库，如下：

```


import pandas as pd
import numpy as np
print(f'pandas version: {pd.__version__}') 
# pandas version 1.3.2



```

本次使用的数据如下：

```


data = {
    'brand':['Python数据之道','价值前瞻','菜鸟数据之道','Python','Java'],
    'B':[4,6,8,12,10],
    'A':[10,2,5,20,16],
    'D':[6,18,14,6,12],
    'years':[4,1,1,30,30],
    'C':[8,12,18,8,2],
}
index = [9,3,4,5,2]
df = pd.DataFrame(data=data,index=index)
df



```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 按行索引排序

`sort_index()` 是 pandas 中按索引排序的函数，默认情况下， `sort_index` 是按行索引来排序。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

通过设置参数 `ascending` 可以设置升序或降序排列，默认情况下是 `ascending=True` ，为升序排列。

设置 `ascending=False` 时，为降序排列，如下：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 按列的名称排序

通过设置参数 `axis=1` 可实现按列的名称排序，如下：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

同样的，可以设置 参数 `ascending` 的值，如下：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

关于按列的名称排序，更多的方法，可以参考下面的内容：

- [Pandas实用技能，将列（column）排序的几种方法](https://mp.weixin.qq.com/s?__biz=MzI2NjY5NzI0NA==&mid=2247501852&idx=2&sn=65af7b9b53aa49d76bb8664492d67579&scene=21#wechat_redirect)
    

## 02 按数值排序

`sort_values()` 是 pandas 中按数值排序的函数。

### 按单个列的值排序

`sort_values()` 中设置单个列的列名称，可以对单个列进行排序，通过设置参数 `ascending` 可以设置升序或降序排列，如下：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 按多个列的值排序

同时，`sort_values()` 可以对多个列进行不同的排序，通过设置列明和排序方式组合来实现，如下：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

设置参数 `ascending` ，`years` 列为升序，`B` 列为降序，如下：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 选择排序算法

选择排序算法，参数 kind 默认是 'quicksort'，其他算法有 mergesort, heapsort, stable。

该参数只针对单个列时才有效。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

在 numpy 的 sort文档中，对几种排序的特点进行了描述，主要是程序运行时占用的资源和运行速度有差异。

numpy 文档地址：

https://numpy.org/doc/stable/reference/generated/numpy.sort.html#numpy.sort

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

示例如下：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 忽略索引

在排序过程中，还可以引入 `ignore_index` 参数，来对行索引重新设置，如下：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### inplace

`inplace` 是 pandas 中常见的一个参数。

`inplace = True`：不创建新的对象，直接对原始对象进行修改；默认是 `False`，即创建新的对象进行修改，原对象不变，和深复制和浅复制有些类似。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 缺失值

先构造一个含缺失值的 dataframe，如下：

```


data = {
    'brand':['Python数据之道','价值前瞻','菜鸟数据之道','Python','Java'],
    'B':[4,6,8,np.nan,12],
    'A':['Lemon','emma','ZW','app','John'],
    'D':[6,18,14,6,12],
    'years':[4,1,1,30,30],
    'C':[8,12,18,8,2],
}
index = [9,3,4,5,2]
df1 = pd.DataFrame(data=data,index=index)
df1



```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

缺失值排在最前面：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

缺失值排在最后面：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### key 参数

通过设置 key 参数，可以将列按照特定条件进行排序，对比下下面的排序：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

**往期回顾**

[介绍Pandas实战中的一些高端玩法](http://mp.weixin.qq.com/s?__biz=Mzg4NDQwNTI0OQ==&mid=2247560261&idx=2&sn=840f1c8524764beba14b032beb2c28b3&chksm=cfbb092bf8cc803d3967d1fcb3cb33236a0d118413af06e3ec438e6f5d5dc66b8f2060e6c649&scene=21#wechat_redirect)

[真香！详解Python好用的内置函数](http://mp.weixin.qq.com/s?__biz=Mzg4NDQwNTI0OQ==&mid=2247560344&idx=1&sn=b9ef8b2aeed82e15693bc631cf92d25a&chksm=cfbb09f6f8cc80e0f666a6f4b7fc8fd09abd09f16a20498ca95bdc02c477718f0bcacf6fe2a8&scene=21#wechat_redirect)

[Python 实现 GIF 动图以及视频卡通化](http://mp.weixin.qq.com/s?__biz=Mzg4NDQwNTI0OQ==&mid=2247560380&idx=1&sn=c85a86800762b8da9a35923dad46f4c8&chksm=cfbb09d2f8cc80c4f950d636463ccb4ec6323c1a9526a005312a973fc9a9c79e9da9077eefba&scene=21#wechat_redirect)

[如何用一行Python代码制作一个GUI？](http://mp.weixin.qq.com/s?__biz=Mzg4NDQwNTI0OQ==&mid=2247560214&idx=3&sn=9ccf13326a163c9cc3f2e02fe0d7e184&chksm=cfbb0978f8cc806ef7eb68e55b203e62b0372fc92f10f7c587bbff6b1aff740b9fac445e27fd&scene=21#wechat_redirect)

```


![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

分享

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

点收藏

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

点点赞

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

点在看












```

<a id="js_view_source"></a>Read more

People who liked this content also liked

20 多个好用的 Vue 组件库，请查收！

...

编程导航

不看的原因

- 内容质量低
- 不看此公众号

pandas 分类数据处理大全

...

AI科技大本营

不看的原因

- 内容质量低
- 不看此公众号

Hydra - 通过 PostgreSQL 集成 Snowflake 的 HTAP 数据库

...

PostgreSQL考试认证中心

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___779e59115aff464cb.bmp"/>

Scan to Follow