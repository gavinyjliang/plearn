# 开启Pandas进阶：图解Pandas透视表、交叉表

<a id="copyright_logo"></a>Original Peter <a id="profileBt"></a><a id="js_name"></a>尤而小屋 *2021-08-12 19:30*

收录于话题

<a id="js_article_tag_name__1999223975666581509"></a>#数据分析 <a id="js_article_tag_num__1999223975666581509"></a>104 <a id="js_article_tag_tips__1999223975666581509"></a>个

<a id="js_article_tag_name__1999223975448477698"></a>#python <a id="js_article_tag_num__1999223975448477698"></a>57 <a id="js_article_tag_tips__1999223975448477698"></a>个

<a id="js_article_tag_name__1999223977696624640"></a>#机器学习 <a id="js_article_tag_num__1999223977696624640"></a>72 <a id="js_article_tag_tips__1999223977696624640"></a>个

<a id="js_article_tag_name__1999223977226862596"></a>#pandas <a id="js_article_tag_num__1999223977226862596"></a>50 <a id="js_article_tag_tips__1999223977226862596"></a>个

![](../../../_resources/0_wx_fmt_png_37b4035930214fb4a1a47ba11d0c6c1a.png)

**尤而小屋**

尤而小屋，一个温馨且有爱的小屋🏡 小屋主人，一手代码谋求生存，一手掌勺享受生活，欢迎你的光临~

<a id="js_profile_article"></a>216篇原创内容

Official Account

## 一、图解Pandas透视表、交叉表

大家好，我是Peter呀~

终于开始**Pandas进阶内容**的写作了。相信很多人都应该知道透视表，在Excel会经常去制作它，来实现数据的分组汇总统计。在Pandas中，我们把它称之为pivot_table。

透视表的制作灵活性高，可以随意定制我们想要的的计算统计要求，一般在制作报表神器的时候常用。

下面通过具体的例子来对比Excel和Pandas中透视表的实现方法。

<img width="657" height="252" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_3eefdfb5f17f4885b.jpg"/>

## 二、Excel透视表

下面是在Excel表格中使用消费数据制作的透视表（部分数据截图），我们统计的是不同性别不同日期下的消费金额和小费，同时还显示了总计的数据。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

<img width="657" height="1039" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_eb0ddf810e4d4b19b.jpg"/>

<img width="657" height="314" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_02befc8430dd4efdb.jpg"/>

那如果是使用pandas该如何来实现呢？？？

## 三、透视表参数

pandas中实现透视表使用的是：`pandas.pivot_table`

```
`pd.pivot_table(data,  # 制作透视表的数据
               values=None,  # 值
               index=None,  # 行索引
               columns=None,  # 列属性
               aggfunc='mean',   # 使用的函数，默认是均值
               fill_value=None,  # 缺失值填充
               margins=False, # 是否显示总计
               dropna=True,   # 缺失值处理
               margins_name='All', # 总计显示为All
               observed=False,  
               sort=True  # 排序功能  版本1.3.0才有
              )
`
```

最重要的参数还是：values、index、columns、aggfunce，甚至包含margins、margins_name

附上官网学习地址：https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.pivot_table.html

## 四、透视表参数详解

### 4.1参数index

index表示的是我们生成透视表指定的行索引

1、单层索引

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

2、多层行索引

<img width="657" height="633" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_974537954b0b4b5da.jpg"/>

### 4.2参数values

在上面index参数的使用中，我们没有指定values参数，pandas会默认将全部的数值型数据进行透视表的计算，现在指定参数计算的数据：

- 带上values，只会显示我们指定的数据
    
- 不带上values，数值型的数据汇总结果全部显示
    

<img width="657" height="534" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_432fb38d0f5d490fa.jpg"/>

### 4.3参数columns

columns是一个显示列属性信息的参数

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

如果我们将day放在index参数中，会是什么样子呢？

相当于是：**将上面的宽表格式转成了下面的长表格式**

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

再对比下两种不同的形式：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 4.4参数aggfunc

aggfunc是一个很灵活的参数，它是用来指定我们汇总想用哪种函数，默认是均值mean，我们也可以使用求和sum、最值max等。多个函数需要放在一个列表中。

我们将默认求平均mean的情况与求和的情况进行对比：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

均值和sum求和之间的关系：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

我们可以在aggfunc函数中指定多个函数，将这些函数放在同一个列表中：

- 求和：np.sum
    
- 求均值：mean
    
- 求个数：size
    

<img width="657" height="217" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_048bae5387ba4b3a8.jpg"/>

再看一个例子：

<img width="657" height="290" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_223eec3652f449a99.jpg"/>

### 4.5参数margins、margins_name

这两个参数的作用是对透视表中的分组数据进行汇总显示。需要注意的是：只有margins=True，参数margins_name的设置才会生效。

<img width="657" height="391" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_0e779168871241ac8.jpg"/>

修改汇总显示的名字：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

如果有列字段，也会显示汇总的数据：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 五、交叉表crosstab

交叉表可以理解成一种特殊的透视表，专门用于计算分组的频率。

5.1参数

交叉表中每个参数的解释，很多还是和透视表相同的：

```
`pandas.crosstab(index, # 行索引，必须是数组结构数据，或者Series，或者是二者的列表形式
                columns, # 列字段；数据要求同上
                values=None,  # 待透视的数据
                rownames=None,  # 行列名字
                colnames=None,  
                aggfunc=None,  # 透视的函数
                margins=False,  # 汇总及名称设置
                margins_name='All', 
                dropna=True, # 舍弃缺失值
                normalize=False  # 数据归一化；可以是布尔值、all、index、columns、或者{0,1}
               )
`
```

对最后一个参数的解释：如何选择归一化的标准

- If passed ‘all’ or True, will normalize over all values：使用all，对全部的数值型数据归一化
    
- If passed ‘index’ will normalize over each row：使用index，仅在行上归一化
    
- If passed ‘columns’ will normalize over each column：使用columns，仅在列上归一化
    
- If margins is True, will also normalize margin values：如果margins=True，总计值也会参与归一化
    

### 5.2参数使用

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

当然，有时候透视表和交叉表是可以实现相同的功能：

<img width="657" height="457" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_d29d5ed44f8e4d11a.jpg"/>

## 六、groupby实现

其实透视表或者交叉表的本质还是分组汇总统计结果，我们也可以利用groupby来实现：

1、先分组统计

<img width="657" height="192" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_9d4e389e674d4d43b.jpg"/>

2、轴旋转unstack

上面的结果格式上不是很友好，使用的是多层次索引，我们使用轴旋转函数unstack将行转成列：

<img width="657" height="188" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_6588c1a330c642c39.jpg"/>

## 七、groupby和透视表比较

最后再用一个例子来比较下groupby和透视表：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 八、备忘录

这个网上非常流行的一张图解Pandas透视表函数的图形，它利用一份简单的数据，清晰明了地讲解了pivot_table函数的每个参数的含义，保存备用！

![Image](../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_4776a2b3e6af49e2b.jpg)

网络图

<img width="17" height="36" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__bf566f2b511f4a869.png"/>

推荐阅读

<img width="17" height="36" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__58ae13ab7ee84582a.png"/>

[27000字，103天，16篇文章！](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247493112&idx=1&sn=2c153674d25f43175bd2dccfb71e99dd&chksm=cf12f522f8657c34ec09f6805c3a1f14b04fccd46b002c2390395a19332230ffce73e9dd49dc&scene=21#wechat_redirect)

[图解Pandas的轴旋转函数：stack和unstack](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247493111&idx=1&sn=e3c785fc786c061d36675d1bd596ad38&chksm=cf12f52df8657c3b0ac8927717234607008da17c63128f47031eadc9b2fe98d85070b292fa2e&scene=21#wechat_redirect)

[图解Pandas数据合并：concat、join、append](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247493109&idx=1&sn=6cf7ba6f16e884c7830f08568748d02d&chksm=cf12f52ff8657c39c0dc4248befbfcbb8afb58fa42939f8f6a1c79ad54e723619ac2f1a49c21&scene=21#wechat_redirect)

[数据分析师=7大主题，24份资料](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247493099&idx=1&sn=b5c1885a38e1de34a79fa5ff581bfaa7&chksm=cf12f531f8657c278f814c8363ca3551616ada416d8edf317dae05b618fda4cc7bdbad3585ae&scene=21#wechat_redirect)

[挑战SQL：图解Pandas的数据合并merge](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247493106&idx=1&sn=7aefa2284d6db3bf2cceedf5db177aaf&chksm=cf12f528f8657c3ee9065f6d6d33b70790be38c833bc0b52b4856609384847a007a3ca0d484d&scene=21#wechat_redirect)

> 尤而小屋，一个温馨的小屋。小屋主人，一手代码谋求生存，一手掌勺享受生活，欢迎你的光临

![](../../../_resources/0_wx_fmt_png_37b4035930214fb4a1a47ba11d0c6c1a.png)

**尤而小屋**

尤而小屋，一个温馨且有爱的小屋🏡 小屋主人，一手代码谋求生存，一手掌勺享受生活，欢迎你的光临~

<a id="js_profile_article"></a>216篇原创内容

Official Account

收录于话题 #<a id="js_album_keep_read_title"></a>数据分析

 <a id="js_album_keep_read_size"></a>104个

<a id="js_album_prev"></a>上一篇 <a id="js_album_keep_read_pre_title"></a>27000字，103天，16篇文章！

People who liked this content also liked

Python关联规则挖掘情侣、基友、渣男和狗

尤而小屋

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___31e395cd45114520b.bmp"/>

Scan to Follow