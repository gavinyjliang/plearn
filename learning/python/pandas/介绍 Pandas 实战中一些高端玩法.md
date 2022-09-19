# 介绍 Pandas 实战中一些高端玩法

<a id="profileBt"></a><a id="js_name"></a>AI科技大本营 *2022-05-23 18:00* *Posted on <a id="js_ip_wording"></a>北京*

The following article is from 关于数据分析与可视化 Author 俊欣

<a id="copyright_info"></a>[![](../../../_resources/0_fa18100e04914aedb19c1d32044ab515.jpg)<br>**关于数据分析与可视化** .<br>本公众号定期分享数据分析与可视化干货文章，并有时结合热点话题进行深入讨论，希望您会喜欢，要是哪里写的不好，也渴望倾听您的想法和意见，感谢！❤️](#)

<img width="661" height="117" src="../../../_resources/640_wx_fmt_gif_wxfrom_5_wx_lazy__f9e9144826b041e89.gif"/>

作者 | 俊欣

来源 | 关于数据分析与可视化

相信大家平常在工作学习当中，需要处理的数据集是十分复杂的，数据集当中的索引也是有多个层级的，那么今天小编就来和大家分享一下`DataFrame`数据集当中的分层索引问题。

## 什么是多重/分层索引

多重/分层索引(MultiIndex)可以理解为堆叠的一种索引结构，它的存在为一些相当复杂的数据分析和操作打开了大门，尤其是在处理高纬度数据的时候就显得十分地便利，我们首先来创建带有多重索引的`DataFrame`数据集

## 多重索引的创建

首先在“列”方向上创建多重索引，即我们在调用`columns`参数时传递两个或者更多的数组，代码如下

```


df1 = pd.DataFrame(np.random.randint(0, 100, size=(2, 4)),
                   index= ['ladies', 'gentlemen'],
                   columns=[['English', 'English', 'French', 'French'],
                            ['like', 'dislike', 'like', 'dislike']])



```

output

<img width="317" height="150" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__728a0822d19b46aaa.png"/>

那么同理我们想要在“行”方向上存在多重索引，则是在调用`index`参数的时候传递两个或者更多数组即可，代码如下

```


df = pd.DataFrame(np.random.randint(0, 100, size=(4, 2)),
                   index= [['English','', 'Chinese',''],
                           ['like','dislike','like','dislike']],
                   columns=['ladies', 'gentlemen'])



```

output

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

除此之外，还有其他几种常见的方式来创建多重索引，分别是

- `pd.MultiIndex.from_arrays`
    
- `pd.MultiIndex.from_frame`
    
- `pd.MultiIndex.from_tuples`
    
- `pd.MultiIndex.from_product`
    

小编这里就挑其中的一种来为大家演示如何来创建多重索引，代码如下

```


df2 = pd.DataFrame(np.random.randint(0, 100, size=(4, 2)), 
                   columns= ['ladies', 'gentlemen'],
                   index=pd.MultiIndex.from_product([['English','French'],
                                                    ['like','dislike']]))



```

output

<img width="337" height="192" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__cd0e6d375a99460b9.png"/>

## 获取多重索引的值

接下来我们来看一下怎么获取带有多重索引的数据集当中的数据，使用到的数据集是英国三大主要城市伦敦、剑桥和牛津在2019年全天的气候数据，如下所示

```


import pandas as pd
from pandas import IndexSlice as idx 
df = pd.read_csv('dataset.csv',
    index_col=[0,1],
    header=[0,1]
)
df = df.sort_index()
df



```

output

<img width="621" height="331" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__d6eb3d089d2b4da4a.png"/>

在“行”索引上，我们可以看到是“城市”以及“日期”这两个维度，而在“列”索引上，我们看到的是则是“不同时间段”以及一些“气温”等指标，首先来看一下“列”方向多重索引的层级，代码如下

```


df.columns.levels



```

output

```


FrozenList([['Day', 'Night'], ['Max Temperature', 'Weather', 'Wind']])



```

我们想要获取第一层级上面的索引值，代码如下

```


df.columns.get_level_values(0)



```

output

```


Index(['Day', 'Day', 'Day', 'Night', 'Night', 'Night'], dtype='object')



```

那么同理，第二层级的索引值，只是把当中的0替换成1即可，代码如下

```


df.columns.get_level_values(1)



```

output

```


Index(['Weather', 'Wind', 'Max Temperature', 'Weather', 'Wind',
       'Max Temperature'],
      dtype='object')



```

那么在“行”方向上多重索引值的获取也是一样的道理，这里就不多加以赘述了

## 数据的获取

那么涉及到数据的获取，方式也有很多种，最常用的就是`loc()`方法以及`iloc()`方法了，例如

```


df.loc['London' , 'Day']
## 或者是
df.loc[('London', ) , ('Day', )]



```

output

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

通过调用`loc()`方法来获取第一层级上的数据，要是我们想要获取所有“行”的数据，代码如下

```


df.loc[:, 'Day']
## 或者是
df.loc[:, ('Day',)]



```

output

<img width="362" height="293" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__ca8b9d1cbf8e4a25a.png"/>

或者是所有“列”的数据，代码如下

```


df.loc['London' , :]
## 或者是
df.loc[('London', ) , :]



```

output

<img width="621" height="176" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__85a4f9235ef545639.png"/>

当然我们也可以这么来做，在行方向上指定第二层级上的索引，代码如下

```


df.loc['London' , '2019-07-02']
## 或者是
df.loc[('London' , '2019-07-02')]



```

output

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 多重索引的数据获取

假设我们想要获取剑桥在2019年7月3日白天的数据，代码如下

```


df.loc['Cambridge', 'Day'].loc['2019-07-03']



```

output

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

在第一次调用`loc['Cambridge', 'Day']`的时候返回的是DataFrame数据集，然后再通过调用`loc()`方法来提取数据，当然这里还有更加快捷的方法，代码如下

```


df.loc[('Cambridge', '2019-07-01'), 'Day']



```

我们需要传入元祖的形式的索引值来进行数据的提取。要是我们不只是想要获取单行或者是单列的数据，可以这么来操作

```


df.loc[ 
    ('Cambridge' , ['2019-07-01','2019-07-02'] ) ,
    'Day'
]



```

output

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

或者是获取多列的数据，代码如下

```


df.loc[ 
    'Cambridge' ,
    ('Day', ['Weather', 'Wind'])
]



```

output

<img width="295" height="214" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__7f292715c8454e3d8.png"/>

我们要是想要获取剑桥在2019年7月1日到3日，连续3天的白天气候数据，代码如下

```


df.loc[
    ('Cambridge', '2019-07-01': '2019-07-03'),
    'Day'
]



```

output

<img width="621" height="109" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__042c8886e6aa4b6eb.png"/>

这么来写是会报语法错误的，正确的方法应该是这么来做，

```


df.loc[
    ('Cambridge','2019-07-01'):('London','2019-07-03'),
    'Day'
]



```

### `xs()`方法的调用

小编另外推荐`xs()`方法来指定多重索引中的层级，例如我们只想要2019年7月1日各大城市的数据，代码如下

```


df.xs('2019-07-01', level='Date')



```

output

<img width="621" height="172" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__39f30d7666fb421ea.png"/>

还能够接受多个维度的索引，例如想要获取伦敦在2019年7月4日的全天数据，代码如下

```


df.xs(('London', '2019-07-04'), level=['City','Date'])



```

output

<img width="621" height="112" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__7223ec570c2241d18.png"/>

另外还有`axis`参数来指定是获取“列”方向还是“行”方向上的数据，例如我们想要获取“Weather”这一列的数据，代码如下

```


df.xs('Weather', level=1, axis=1)



```

output

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

当中的`level`参数代表的是层级，我们将其替换成0，看一下出来的结果

```


df.xs('Day', level=0, axis=1)



```

output

<img width="621" height="503" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__7c8e705ec4db40a9a.png"/>

筛选出来的是三个主要城市2019年白天的气候数据

### IndexSlice()方法的调用

同时`Pandas`内部也提供了`IndexSlice()`方法来方便我们更加快捷地提取出多重索引数据集中的数据，代码如下

```


from pandas import IndexSlice as idx
df.loc[ 
    idx[: , '2019-07-04'], 
    'Day'
]



```

output

<img width="621" height="190" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__233ff94b46ec4a599.png"/>

我们同时可以指定行以及列方向上的索引来进行数据的提取，代码如下

```


rows = idx[: , '2019-07-02']
cols = idx['Day' , ['Max Temperature','Weather']]
df.loc[rows, cols]



```

output

<img width="370" height="184" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__c9fe63575519495d8.png"/>

<img width="677" height="135" src="../../../_resources/640_wx_fmt_gif_wxfrom_5_wx_lazy__8a8a38dcb712460ea.gif"/>

<img width="677" height="388" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_c17744fc0de84c8ea.jpg"/>

**往期回顾**

[一文介绍Pandas中的9种数据访问方式](http://mp.weixin.qq.com/s?__biz=Mzg4NDQwNTI0OQ==&mid=2247560214&idx=1&sn=dfcd6a108f835a85bf1b7bf99da67c54&chksm=cfbb0978f8cc806eeb18b079a9be451775d92da5bbc7c1549772ad4ec3d4c3a8288279c6fe43&scene=21#wechat_redirect)

[“由于一段Python代码，我的号被封了”](http://mp.weixin.qq.com/s?__biz=Mzg4NDQwNTI0OQ==&mid=2247560150&idx=1&sn=e45b944ff2ec6b932eae5c298a56ef86&chksm=cfbb0eb8f8cc87aec4c0a308484d9563e3573cf9b0c9ba50f7e1d9dc24937ba8e1aaf8b37f2f&scene=21#wechat_redirect)

[云上风景虽好，但不要盲目跟风！](http://mp.weixin.qq.com/s?__biz=Mzg4NDQwNTI0OQ==&mid=2247559943&idx=2&sn=054537cae62bd26286c028acb3ae7782&chksm=cfbb0e69f8cc877f89b5c87ed6b79c7c143c34a96a20e3ecaa245344380ed18abc4a18c04123&scene=21#wechat_redirect)

[如何用一行Python代码制作一个GUI？](http://mp.weixin.qq.com/s?__biz=Mzg4NDQwNTI0OQ==&mid=2247560214&idx=3&sn=9ccf13326a163c9cc3f2e02fe0d7e184&chksm=cfbb0978f8cc806ef7eb68e55b203e62b0372fc92f10f7c587bbff6b1aff740b9fac445e27fd&scene=21#wechat_redirect)

```


![Image](https://mmbiz.qpic.cn/mmbiz_png/VeJKXItpwPX2FZVxJbbER98riaKxfbdicAoaWibibhVHicMhT0lJR8XvXs6U1fqAnmhH2q1ooImrAgSIVfNicjNLMzZQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

分享

![Image](https://mmbiz.qpic.cn/mmbiz_png/VeJKXItpwPX2FZVxJbbER98riaKxfbdicAL8bkIiaLIqnn0OY6W10TRgtSMWLMOxicm3q2ibbJ8Y9ybJBsiaqeTqBpqQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

点收藏

![Image](https://mmbiz.qpic.cn/mmbiz_png/VeJKXItpwPX2FZVxJbbER98riaKxfbdicACtBqqOgf8qDicxXJYAynmgBjgqaFYZcH5PwYjibIceYLvXC66FAfxTOQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

点点赞

![Image](https://mmbiz.qpic.cn/mmbiz_png/VeJKXItpwPX2FZVxJbbER98riaKxfbdicA2N4IEaJic9hzA5VlkZ7zyD8Eibe5gRX5iae5ZH6bRf72qDz6v1nnBEpxw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

点在看












```

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

用 Python 自制了一张网页，一键自动生成探索性数据分析报告

...

AI科技大本营

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___0687a8c1985f4d098.bmp"/>

Scan to Follow