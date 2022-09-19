# Pandas中Apply函数加速百倍的技巧！

<a id="profileBt"></a><a id="js_name"></a>数据不吹牛 *2022-04-24 22:03*

![](../../../_resources/0_wx_fmt_png_b4a0b62699dc42898fc352d767fe9d7c.png)

**数据不吹牛**

有趣+干货的数据分析宝藏

<a id="js_profile_article"></a>69篇原创内容

Official Account

##### 来源：kaggle竞赛宝典

转自：Python大数据分析

大家好，我是小z，也可以叫我阿粥~

虽然目前dask,cudf等包的出现，使得我们的数据处理大大得到了加速，但是并不是每个人都有比较好的gpu，非常多的朋友仍然还在使用pandas工具包，但有时候真的很无奈，pandas的许多问题我们都需要使用apply函数来进行处理，而apply函数是非常慢的，本文我们就介绍如何加速apply函数600倍的技巧。

**实验对比**

**01 Apply(Baseline)**

我们以Apply为例，原始的Apply函数处理下面这个问题，需要**18.4s**的时间。

```


```
`import pandas as pd``import numpy as np``df = pd.DataFrame(np.random.randint(0, 11, size=(1000000, 5)), columns=('a','b','c','d','e'))``def func(a,b,c,d,e):` `if e == 10:` `return c*d` `elif (e < 10) and (e>=5):` `return c+d` `elif e < 5:` `return a+b``%%time``df['new'] = df.apply(lambda x: func(x['a'], x['b'], x['c'], x['d'], x['e']), axis=1)``CPU times: user 17.9 s, sys: 301 ms, total: 18.2 s``Wall time: 18.4 s`
```


```

**02 ****Swift加速**

因为处理是并行的，所以我们可以使用Swift进行加速，在使用Swift之后，相同的操作在我的机器上可以提升到7.67s。

```


```
`%%time``# !pip install swifter``import swifter``df['new'] = df.swifter.apply(lambda x : func(x['a'],x['b'],x['c'],x['d'],x['e']),axis=1)``HBox(children=(HTML(value='Dask Apply'), FloatProgress(value=0.0, max=16.0), HTML(value='')))``CPU times: user 329 ms, sys: 240 ms, total: 569 ms``Wall time: 7.67 s`
```


```

**03 ****向量化**

使用Pandas和Numpy的最快方法是将函数向量化。如果我们的操作是可以直接向量化的话，那么我们就尽可能的避免使用：

- for循环；
    
- 列表处理；
    
- apply等操作
    

在将上面的问题转化为下面的处理之后，我们的时间缩短为：421 ms。

```


```
`%%time``df['new'] = df['c'] * df['d'] #default case e = =10``mask = df['e'] < 10``df.loc[mask,'new'] = df['c'] + df['d']``mask = df['e'] < 5``df.loc[mask,'new'] = df['a'] + df['b']``CPU times: user 134 ms, sys: 149 ms, total: 283 ms``Wall time: 421 ms`
```


```

**04 类别转化+向量化**

我们先将上面的类别转化为int16型，再进行相同的向量化操作，发现时间缩短为：116 ms。

```


```
`for col in ('a','b','c','d'):` `df[col] = df[col].astype(np.int16)` `%%time``df['new'] = df['c'] * df['d'] #default case e = =10``mask = df['e'] < 10``df.loc[mask,'new'] = df['c'] + df['d']``mask = df['e'] < 5``df.loc[mask,'new'] = df['a'] + df['b']``CPU times: user 71.3 ms, sys: 42.5 ms, total: 114 ms``Wall time: 116 ms`
```




```

**05 转化为values处理**

在能转化为.values的地方尽可能转化为.values，再进行操作。

- 此处先转化为.values等价于转化为numpy，这样我们的向量化操作会更加快捷。
    

于是，上面的操作时间又被缩短为：74.9ms。

```


```
`%%time``df['new'] = df['c'].values * df['d'].values #default case e = =10``mask = df['e'].values < 10``df.loc[mask,'new'] = df['c'] + df['d']``mask = df['e'].values < 5``df.loc[mask,'new'] = df['a'] + df['b']``CPU times: user 64.5 ms, sys: 12.5 ms, total: 77 ms``Wall time: 74.9 ms`
```


```

**实验汇总**

通过上面的一些小的技巧，我们将简单的Apply函数加速了几百倍，具体的：

- Apply: 18.4 s
    
- Apply + Swifter: 7.67 s
    
- Pandas vectorizatoin: 421 ms
    
- Pandas vectorization + data types: 116 ms
    
- Pandas vectorization + values + data types: 74.9ms
    

作者：杰少，本文大部分内容参考引文

参考文献：Do You Use Apply in Pandas? There is a 600x Faster Way

**最后，进入到粉丝福利环节：**

送书两本，**北京大学出版社的****《R语言数据分析与可视化从入门到精通》**，理论为辅、实践为主。本书涉及一些必要的理论知识，特别是在数据分析部分，但总体以实践为主，因此几乎每节都有大量的代码，方便读者实践。上手最好的方式就是跟着实现一遍，这本书可以说很硬核了~

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

**参与方式简单粗暴，本文点赞&在看&留言，留言排名第1位、第5位各赠书一本，4月27日22:00开奖~**

People who liked this content also liked

还在使用 ELK 收集日志吗？赶快来试试这个基于 ClickHouse 的方案吧~

...

k8s技术圈

不看的原因

- 内容质量低
- 不看此公众号

MySQL8.0 InnoDB并行查询特性

...

yangyidba

不看的原因

- 内容质量低
- 不看此公众号

Python中最简单易用的并行加速技巧

...

Python大数据分析

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___9e2c753831fb47e5a.bmp"/>

Scan to Follow