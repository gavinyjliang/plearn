# 图解Pandas的轴旋转函数：stack和unstack

<a id="copyright_logo"></a>Original Peter <a id="profileBt"></a><a id="js_name"></a>尤而小屋 *2021-08-05 09:01*

收录于话题

<a id="js_article_tag_name__1999223975448477698"></a>#python <a id="js_article_tag_num__1999223975448477698"></a>57 <a id="js_article_tag_tips__1999223975448477698"></a>个

<a id="js_article_tag_name__1999223975666581509"></a>#数据分析 <a id="js_article_tag_num__1999223975666581509"></a>104 <a id="js_article_tag_tips__1999223975666581509"></a>个

<a id="js_article_tag_name__1999223977696624640"></a>#机器学习 <a id="js_article_tag_num__1999223977696624640"></a>72 <a id="js_article_tag_tips__1999223977696624640"></a>个

<a id="js_article_tag_name__1999223977226862596"></a>#pandas <a id="js_article_tag_num__1999223977226862596"></a>50 <a id="js_article_tag_tips__1999223977226862596"></a>个

> 公众号：尤而小屋
> 作者：Peter
> 编辑：Peter

大家好，我是Peter~ 今天带来的文章是图解Pandas中的两个重要的函数：stack和unstack。

stack和unstack是针对pandas的轴进行重新排列的两个方法，二者互为逆操作：

- stack: 将数据的列columns转旋转成行index
    
- unstack：将数据的行index旋转成列columns
    
- 二者默认操作的都是最内层
    

<img width="657" height="252" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_b6f4f3f1258e42189.jpg"/>

## 一、Pandas连载文章

本文是Pandas更新的第16篇文章，欢迎访问阅读：

[图解Pandas数据合并：concat、join、append](http://mp.weixin.qq.com/s?__biz=MzI4NjI3MzI2Ng==&mid=2247493318&idx=1&sn=b8274a3b0d4c223214c0fa2e8f971eb0&chksm=ebdddd5bdcaa544de22149b2ca1c886150be497a87e7a62fe44c520666929e168d2ff45913b1&scene=21#wechat_redirect)

[挑战SQL：图解Pandas的数据合并merge](http://mp.weixin.qq.com/s?__biz=MzI4NjI3MzI2Ng==&mid=2247493135&idx=1&sn=ff3c46e206fe19986506ca14805a0bb1&chksm=ebdddd92dcaa548442cc2599f731a42115d28b00c814a65f492b110b4e8d7e9cef9ea318a8ad&scene=21#wechat_redirect)

[图解Pandas重复值处理](http://mp.weixin.qq.com/s?__biz=MzI4NjI3MzI2Ng==&mid=2247492519&idx=1&sn=c4974450fc9ea8b84106f65de6501028&chksm=ebddd83adcaa512c788ae612b0ebc6b4846ad43981aa00fa38a2c56710496651ae69ce180f1a&scene=21#wechat_redirect)

<img width="657" height="391" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_cf7524349f0145cc9.jpg"/>

下面通过详细的例子来进行讲解二者的用法，同时奉上官网学习地址：https://pandas.pydata.org/pandas-docs/stable/user_guide/reshaping.html#reshaping-stacking

## 二、stack

stack函数的主要作用是将原来的列转成最内层的行索引，转换之后都是多层次索引。官方原文：

> Stack the prescribed level(s) from columns to index.

使用方法为：

```
`pd.stack(level=-1, dropna=True)
`
```

- level表示的是转换的是最内层（倒数第一级）
    
- dropna表示的是对缺失值的处理
    

通过官网的一幅图来解释下：**列属性AB变成了行索引AB**

<img width="657" height="399" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_daa2e438bdb64f378.jpg"/>

### 2.1对单层DataFrame进行stack操作

```
`import pandas as pd 
import numpy as np
`
```

<img width="657" height="331" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_19247e16ae1e4e25a.jpg"/>

看下默认的情况：

<img width="657" height="343" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_baef64149ece4318b.jpg"/>

我们发现df2的索引index也变成了多层次索引：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

还有一个特点：当我们对单层的DataFrame进行stack操作之后，会变成Series型数据：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 2.2对多层DataFrame进行stack操作

首先我们生成一个多层次的列数型

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

模拟出一份多层次列属性的数据：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

看下模拟数据df3的相关信息：

```
`type(df3)
pandas.core.frame.DataFrame
df3.index
Index(['小明', '小红'], dtype='object')
df3.columns
MultiIndex([('information',    'sex'),
            ('information', 'weight')],)
`
```

看下stack之后的数据：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 2.3二者比较

对比下原数据和生成的新数据：

1、索引index比较

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

2、列属性比较

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

3、数据类型比较

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 2.4参数level

level控制的是一个或者多个属性进行堆叠；可以使用数字索引或者名称索引。

模拟一份多层次的列属性数据：

```
`multicol2 = pd.MultiIndex.from_tuples([('weight', 'kg'),  # 多层次列属性
                                       ('height', 'm')],
                                     name=["col","unit"])
data1 = pd.DataFrame([[1.0, 2.0], [3.0, 4.0]],
                     index=['cat', 'dog'],
                     columns=multicol2
                    )
data1
`
```

<img width="657" height="294" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_300eba47ea154b319.jpg"/>

我们可以可以看到data1的列属性是多层次的：

```
`data1.columns
# 结果
MultiIndex([('weight', 'kg'),
            ('height',  'm')],
           names=['col', 'unit'])
`
```

<img width="657" height="606" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_f92c4a8398d5457b8.jpg"/>

我们还可以使用列数的名称进行stack操作：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

对于另一个"col"也是相同的操作：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

还可以同时对多个进行操作，指定名称或者索引号：

<img width="657" height="423" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_575e101e5ead4608a.jpg"/>

### 2.5参数dropna

如果原数据中存在缺失值，我们该如何处理？模拟一份存在缺失值的数据：

```
`data2 = pd.DataFrame([[None, 2.0],  # 引入一个缺失值
                      [4.0, 6.0]],
                     index=['cat', 'dog'],
                     columns=multicol2)
data2
`
```

<img width="657" height="293" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_aede10c7ba864fae8.jpg"/>

默认是是True，会删除同时为缺失值的情况：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

如果我们改成False，会保留同时为NaN的数据：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 三、unstack

将最内层的行索引变成列：就是行索引AB变成了列属性AB

<img width="657" height="388" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_02af60218cae4f538.jpg"/>

### 3.1使用方法

unstack是stack的逆操作，将最内层的行索引变成列

```
`unstack(level=- 1, fill_value=None)
`
```

- level：进行操作的索引层级，可以是名称
    
- fill_value：当我们进行操作的时候，如果产生了缺失值，可以用指定的值填充
    

### 3.2参数level

来自官网的两张图，在不同的索引号进行unstack操作的结果对比，默认是unstack(1)：

<img width="657" height="382" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_94ffffa5f7fa452c8.jpg"/>

<img width="657" height="375" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_2f79b1eafc6d4f79b.jpg"/>

我们使用之前的一份生成的数据，接下来我们对df5进行操作。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

image-20210805001628811

1、使用unstack的默认操作：默认是对最里层操作

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

2、我们改成索引为0的行，同时我们也可以使用行的名称作为参数的值

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 3.3参数fill_value

该参数的作用是当我们使用unstack进行操作的时候，产生的缺失值用指定数据填充。

我们使用之前的df6数据框来进行操作：

<img width="657" height="341" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_f360121bae79489aa.jpg"/>

使用unstack的默认情况：会产生两个空值

<img width="657" height="331" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_9925963afa5142389.jpg"/>

对产生的缺失值进行填充：

- 默认情况的使用
    
- 使用名称
    
- 使用索引号
    

<img width="657" height="646" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_25bf2df164e44aefb.jpg"/>

## 四、总结

stack和unstack主要是对Series或者DataFrame型数据进行堆叠和取消堆叠的操作，它们有个共同的特点就是：默认都是对最里层的索引号进行操作，二者互为逆操作，区别在于：stack是列转行，unstack是行转列。

<img width="17" height="36" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__c368e3f374fd4cdca.png"/>

推荐阅读

<img width="17" height="36" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__13550f3dd6404809b.png"/>

[熬夜制作的瓜：8月份无疫烦！哈哈哈哈](http://mp.weixin.qq.com/s?__biz=MzI4NjI3MzI2Ng==&mid=2247493338&idx=1&sn=ebae00e8c5aad29351ea265a347de09d&chksm=ebdddd47dcaa5451085ed41f5469818b5ab510e325c55843977d1b4d228360a09b1877dc1dba&scene=21#wechat_redirect)

[图解Pandas数据合并：concat、join、append](http://mp.weixin.qq.com/s?__biz=MzI4NjI3MzI2Ng==&mid=2247493318&idx=1&sn=b8274a3b0d4c223214c0fa2e8f971eb0&chksm=ebdddd5bdcaa544de22149b2ca1c886150be497a87e7a62fe44c520666929e168d2ff45913b1&scene=21#wechat_redirect)

[挑战SQL：图解Pandas的数据合并merge](http://mp.weixin.qq.com/s?__biz=MzI4NjI3MzI2Ng==&mid=2247493135&idx=1&sn=ff3c46e206fe19986506ca14805a0bb1&chksm=ebdddd92dcaa548442cc2599f731a42115d28b00c814a65f492b110b4e8d7e9cef9ea318a8ad&scene=21#wechat_redirect)

[数据分析师=7大主题，24份资料](http://mp.weixin.qq.com/s?__biz=MzI4NjI3MzI2Ng==&mid=2247492664&idx=1&sn=4136051d1be47a616d52fcb22ff49a49&chksm=ebdddfa5dcaa56b3eb370532daaf6deb74886599a8cc08e010bfddf0562ffedc0ce89f43f18f&scene=21#wechat_redirect)

[图解Pandas重复值处理](http://mp.weixin.qq.com/s?__biz=MzI4NjI3MzI2Ng==&mid=2247492519&idx=1&sn=c4974450fc9ea8b84106f65de6501028&chksm=ebddd83adcaa512c788ae612b0ebc6b4846ad43981aa00fa38a2c56710496651ae69ce180f1a&scene=21#wechat_redirect)

[生日快乐：尤而小屋两周岁啦](http://mp.weixin.qq.com/s?__biz=MzI4NjI3MzI2Ng==&mid=2247492295&idx=1&sn=489d07d8208ffa10fefa67d562f95092&chksm=ebddd95adcaa504c2c3983a899d48785d5fd08f06b52ba2beba404b122f86bae87dbcd557772&scene=21#wechat_redirect)

> 尤而小屋，一个温馨的小屋。小屋主人，一手代码谋求生存，一手掌勺享受生活，欢迎你的光临

收录于话题 #<a id="js_album_keep_read_title"></a>python

 <a id="js_album_keep_read_size"></a>57个

<a id="js_album_prev"></a>上一篇 <a id="js_album_keep_read_pre_title"></a>熬夜制作的瓜：8月份无疫烦！哈哈哈哈 <a id="js_album_next"></a>下一篇 <a id="js_album_keep_read_next_title"></a>27000字，103天，16篇文章！

People who liked this content also liked

Python关联规则挖掘情侣、基友、渣男和狗

尤而小屋

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___1e56c575b9414b8a8.bmp"/>

Scan to Follow