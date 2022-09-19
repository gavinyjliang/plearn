# Pandas行列转换的4大技巧！

<a id="copyright_logo"></a>Original <a id="js_author_name"></a>尤而小屋 <a id="profileBt"></a><a id="js_name"></a>尤而小屋 *2021-12-16 00:06*

收录于话题

<a id="js_article_tag_name__1999223977226862596"></a>#pandas <a id="js_article_tag_num__1999223977226862596"></a>50 <a id="js_article_tag_tips__1999223977226862596"></a>个

<a id="js_article_tag_name__1999223975448477698"></a>#python <a id="js_article_tag_num__1999223975448477698"></a>57 <a id="js_article_tag_tips__1999223975448477698"></a>个

<a id="js_article_tag_name__1999223975666581509"></a>#数据分析 <a id="js_article_tag_num__1999223975666581509"></a>104 <a id="js_article_tag_tips__1999223975666581509"></a>个

> 公众号：尤而小屋
> 作者：Peter
> 编辑：Peter

大家好，我是Peter~

本文介绍的是Pandas中4个行列转换的方法，包含：

- melt
    
- 转置T或者transpose
    
- wide\_to\_long
    
- explode（爆炸函数）
    

最后回答一个读者朋友问到的数据处理问题。

<img width="657" height="252" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_a598bdf050024d42a.jpg"/>

## Pandas行列转换

pandas中有多种方法能够实现行列转换：

[开启Pandas进阶：图解Pandas透视表、交叉表](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247493182&idx=1&sn=9b37f4aee2cfe3ae52f90ceff9296b0a&chksm=cf12f6e4f8657ff27e45a212d2e29e6733cc37f347828bdf12c7573ae3f6d56d484a300c8833&scene=21#wechat_redirect)

[图解Pandas的轴旋转函数：stack和unstack](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247493111&idx=1&sn=e3c785fc786c061d36675d1bd596ad38&chksm=cf12f52df8657c3b0ac8927717234607008da17c63128f47031eadc9b2fe98d85070b292fa2e&scene=21#wechat_redirect)

[一切从爆炸函数开始！](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247493052&idx=1&sn=45d82fc7759e43f7477cc0d983b32406&chksm=cf12f566f8657c70dba9b115127f0b709c81c836b3a1d4ef77f05f3053c2e5794ca08e523b2f&scene=21#wechat_redirect)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 导入库

```
import pandas as pd
import numpy as np

```

## 函数melt

melt的主要参数：

```
pandas.melt(frame, 
            id_vars=None, 
            value_vars=None, 
            var_name=None, 
            value_name='value',
            ignore_index=True,  
            col_level=None)

```

下面解释参数的含义：

- frame：要处理的数据框DataFrame。
    
- id_vars：表示**不需要被转换**的列名
    
- value_vars：表示**需要转换**的列名，如果剩下的列全部都需要进行转换，则不必写
    
- var\_name和value\_name：自定义设置对应的列名，相当于是取新的列名
    
- igonore_index：是否忽略原列名，默认是True，就是忽略了原索引名，重新生成0,1,2,3,4....的自然索引
    
- col_level：如果列是多层索引列MultiIndex，则使用此参数；这个参数少用
    

![](../../../_resources/0_wx_fmt_png_909b333b10364abc8e5e9952d9b864a5.png)

**机器学习杂货店**

记录和分享机器学习、深度学习、kaggle实战的知识

<a id="js_profile_article"></a>2篇原创内容

Official Account

### 模拟数据

```
# 待转换的数据：frame
df = pd.DataFrame({"col1":[1,1,1,1,1],
                   "col2":[3,3,3,3,3],
                   "col3":["a","a","a","b","b"]
                  })
df

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### id_vars

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### value_vars

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

上面两个参数的同时使用：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

同时转换多个列属性：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### var\_name和value\_name

```
pd.melt(
    df,
    id_vars=["col1"],  # 不变
    value_vars=["col3"],  # 转变
    var_name="col4",  # 新的列名
    value_name="col5" # 对应值的新列名
)

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### ignore_index

默认情况下是生成自然索引：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

可以改成False，使用原来的索引：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 转置函数

pandas中的T属性或者transpose函数就是实现行转列的功能，准确地说就是转置

### 简单转置

模拟了一份数据，查看转置的结果：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

使用transpose函数进行转置：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

还有另一个方法：先对值values进行转置，再把索引和列名进行交换：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

最后看一个简单的案例：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## wide\_to\_long函数

字面意思就是：将数据集从宽格式转换为长格式

```
wide_to_long(
    df,
    stubnames,
    i,
    j,
    sep: str = "",
    suffix: str = "\\d+"

```

参数的具体解释：

- df：待转换的数据框
    
- stubnames：宽表中列名相同的存根部分
    
- i：要用作 id 变量的列
    
- j：给长格式的“后缀”列设置 columns
    
- sep：设置要删除的分隔符。例如 columns 为 A-2020，则指定 sep='-' 来删除分隔符。默认为空。
    
- suffix：通过设置正则表达式取得“后缀”。默认'\\d+'表示取得数字后缀。没有数字的“后缀”可以用'\\D+'来取得
    

### 模拟数据

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 转换过程

使用函数实施转换：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 设置多层索引

先模拟一份数据：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

如果不习惯多层索引，可以转成下面的格式：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### sep和suffix

```
df5 = pd.DataFrame({
    'a': [1, 1, 2, 2, 3, 3, 3],
    'b': [1, 2, 2, 3, 1, 2, 3],
    'stu_one': [2.8, 2.9, 1.8, 1.9, 2.2, 2.3, 2.1],
    'stu_two': [3.4, 3.8, 2.8, 2.4, 3.3, 3.4, 2.9]
})
df5

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

```
pd.wide_to_long(
    df5, 
    stubnames='stu', 
    i=['a', 'b'], 
    j='number',
    sep='_', # 列名中存在连接符时使用；默认为空
    suffix=r'\w+')  # 基于正则表达式的后缀；默认是数字\d+；这里改成\w+，表示字母

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 爆炸函数-explode

```
explode(column, ignore_index=False)

```

这个函数的参数就只有两个：

- column：待爆炸的元素
    
- ignore_index：是否忽略索引；默认是False，保持原来的索引
    

### 模拟数据

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 单个字段爆炸

对单个字段实施爆炸过程，将宽表转成长表：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 参数ignore_index

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 多个字段爆炸

连续对多个字段实施爆炸的过程：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 读者解疑

在这里回答一个读者的问题，数据采用模拟的形式。有下面的这样一份数据，需求：

**每个shop下每个fruit在各自shop的占比**

```
fruit = pd.DataFrame({
    "shop":["shop1","shop3","shop2","shop3",
            "shop2","shop1","shop3","shop2",
            "shop3","shop2","shop3","shop2","shop1"],
    "fruit":["苹果","葡萄","香蕉","苹果",
             "葡萄","橘子","梨","哈密瓜",
             "葡萄","香蕉","苹果","葡萄","橘子"],
    "number":[100,200,340,150,
              200,300,90,80,340,
              150,200,300,90]})
fruit

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

首先我们是需要统计每个shop每个fruit的销量

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 方法1：多步骤

方法1采用的是多步骤解决：

1、每个shop的总销量

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

2、增加总和shop_sum列

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

3、生成占比

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 方法2：使用transform函数

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

推荐阅读

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

[国产写作神器Typora收费？](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247500797&idx=1&sn=207c94eaa4e9b44c47dc6ecf217dbf38&chksm=cf12d327f8655a31b61559431219f50fb2fd3df3198ec1a17e69e7a0d2fa864d41745c06dd20&scene=21#wechat_redirect)

[6大监督学习模型：毒蘑菇分类](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247501032&idx=1&sn=5a66d73e7d68b7f73bfd7cdde644efa8&chksm=cf12d432f8655d2467ff740702d3250de0ebe8e8bfb25ccc25843a4b49bb2bd5b93da5835b3f&scene=21#wechat_redirect)

[精华！12大Pandas常用配置技巧](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247500584&idx=1&sn=37ab7b40cecedbbd9433b9dd8b4fec36&chksm=cf12d3f2f8655ae4207b2d8c7c7e5523e20ea77be7a77cfa5506e3377cf719fa2b1fff099e9a&scene=21#wechat_redirect)

[Plotly+Pandas+Sklearn：实现用户聚类分群！](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247500041&idx=1&sn=e29f380b176df342a03850f655d09991&chksm=cf12d1d3f86558c571b088bd846207f22b150e290dc660fe93125d9cf813594515153a6f0c68&scene=21#wechat_redirect)

[大揭秘：必须学会的Python数据分析利器](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247497719&idx=1&sn=e49a68ef1ba58b3c4105a38267f13ac6&chksm=cf12e72df8656e3b55b442effdb1f0a3c3b34c1a52b1fcfeb538bb380f0d1cec497925ac7886&scene=21#wechat_redirect)

> 尤而小屋，一个温馨的小屋。小屋主人，一手代码谋求生存，一手掌勺享受生活，欢迎你的光临

![](../../../_resources/0_wx_fmt_png_c748f73f17bf4c1188fb0c428ec5999e.png)

**尤而小屋**

尤而小屋，一个温馨且有爱的小屋🏡 小屋主人，一手代码谋求生存，一手掌勺享受生活，欢迎你的光临~

<a id="js_profile_article"></a>216篇原创内容

Official Account

收录于话题 #<a id="js_album_keep_read_title"></a>pandas

 <a id="js_album_keep_read_size"></a>50个

<a id="js_album_prev"></a>上一篇 <a id="js_album_keep_read_pre_title"></a>精选23个Pandas常用函数 <a id="js_album_next"></a>下一篇 <a id="js_album_keep_read_next_title"></a>机器学习绘图神器Matplotlib首秀！

<a id="js_view_source"></a>Read more

People who liked this content also liked

python读取hdfs文件

杂乱无章丶

不看的原因

- 内容质量低
- 不看此公众号

python-OpenCV轻松入门-利用图像运算打码图片

和孔哥一起学

不看的原因

- 内容质量低
- 不看此公众号

pandas module

远小数

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___67870d93a4b6408ca.bmp"/>

Scan to Follow