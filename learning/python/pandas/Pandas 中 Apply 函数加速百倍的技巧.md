# Pandas 中 Apply 函数加速百倍的技巧

<a id="profileBt"></a><a id="js_name"></a>数据分析与开发 *2022-05-11 12:10* *Posted on <a id="js_ip_wording"></a>浙江*

↓推荐关注↓

![](../../../_resources/0_wx_fmt_png_4c521eb2b7f74b47b16ffbf4e7b42bc0.png)

**Python开发精选**

分享 Python 技术文章、资源、课程、资讯。

<a id="js_profile_article"></a>14篇原创内容

Official Account

**\[ 引言 \]** 虽然目前dask,cudf等包的出现，使得我们的数据处理大大得到了加速，但是并不是每个人都有比较好的gpu，非常多的朋友仍然还在使用pandas工具包，但有时候真的很无奈，pandas的许多问题我们都需要使用apply函数来进行处理，而apply函数是非常慢的，本文我们就介绍如何加速apply函数600倍的技巧。

**实验对比**

**01 Apply(Baseline)**

我们以Apply为例，原始的Apply函数处理下面这个问题，需要**18.4s**的时间。

```
import pandas as pd
import numpy as np
df = pd.DataFrame(np.random.randint(0, 11, size=(1000000, 5)), columns=('a','b','c','d','e'))
def func(a,b,c,d,e):
    if e == 10:
        return c*d
    elif (e < 10) and (e>=5):
        return c+d
    elif e < 5:
        return a+b
%%time
df['new'] = df.apply(lambda x: func(x['a'], x['b'], x['c'], x['d'], x['e']), axis=1)
CPU times: user 17.9 s, sys: 301 ms, total: 18.2 s
Wall time: 18.4 s
```

```



```

**02 ****Swift加速**

因为处理是并行的，所以我们可以使用Swift进行加速，在使用Swift之后，相同的操作在我的机器上可以提升到7.67s。

```
%%time
# !pip install swifter
import swifter
df['new'] = df.swifter.apply(lambda x : func(x['a'],x['b'],x['c'],x['d'],x['e']),axis=1)
HBox(children=(HTML(value='Dask Apply'), FloatProgress(value=0.0, max=16.0), HTML(value='')))
CPU times: user 329 ms, sys: 240 ms, total: 569 ms
Wall time: 7.67 s
```

```



```

**03 ****向量化**

使用Pandas和Numpy的最快方法是将函数向量化。如果我们的操作是可以直接向量化的话，那么我们就尽可能的避免使用：

- for循环；
    
- 列表处理；
    
- apply等操作
    

在将上面的问题转化为下面的处理之后，我们的时间缩短为：421 ms。

```
%%time
df['new'] = df['c'] * df['d'] #default case e = =10
mask = df['e'] < 10
df.loc[mask,'new'] = df['c'] + df['d']
mask = df['e'] < 5
df.loc[mask,'new'] = df['a'] + df['b']
CPU times: user 134 ms, sys: 149 ms, total: 283 ms
Wall time: 421 ms
```

```



```

**04 类别转化+向量化**

我们先将上面的类别转化为int16型，再进行相同的向量化操作，发现时间缩短为：116 ms。

```
for col in ('a','b','c','d'):
    df[col] = df[col].astype(np.int16) 
%%time
df['new'] = df['c'] * df['d'] #default case e = =10
mask = df['e'] < 10
df.loc[mask,'new'] = df['c'] + df['d']
mask = df['e'] < 5
df.loc[mask,'new'] = df['a'] + df['b']
CPU times: user 71.3 ms, sys: 42.5 ms, total: 114 ms
Wall time: 116 ms
```

**05 转化为values处理**

在能转化为.values的地方尽可能转化为.values，再进行操作。

- 此处先转化为.values等价于转化为numpy，这样我们的向量化操作会更加快捷。
    

于是，上面的操作时间又被缩短为：74.9ms。

```
%%time
df['new'] = df['c'].values * df['d'].values #default case e = =10
mask = df['e'].values < 10
df.loc[mask,'new'] = df['c'] + df['d']
mask = df['e'].values < 5
df.loc[mask,'new'] = df['a'] + df['b']
CPU times: user 64.5 ms, sys: 12.5 ms, total: 77 ms
Wall time: 74.9 ms
```

```



```

**实验汇总**

通过上面的一些小的技巧，我们将简单的Apply函数加速了几百倍，具体的：

- Apply: 18.4 s
    
- Apply + Swifter: 7.67 s
    
- Pandas vectorizatoin: 421 ms
    
- Pandas vectorization + data types: 116 ms
    
- Pandas vectorization + values + data types: 74.9ms
    

参考文献：Do You Use Apply in Pandas? There is a 600x Faster Way

> 作者：杰少
> 
> 来源：kaggle竞赛宝典

\- EOF -

推荐阅读  点击标题可跳转

<ins>1、[Pandas vs SQL](http://mp.weixin.qq.com/s?__biz=MzA5ODM5MDU3MA==&mid=2650883256&idx=2&sn=e9831e6f2f02106a94ed84039307aefd&chksm=8b67a3fdbc102aeb281eeac9229a11f0f49981d15ba1db92c7fc049d7893d2d1477e9925e870&scene=21#wechat_redirect)</ins>

<ins>2、[5 个必须知道的 Pandas 数据合并技巧](http://mp.weixin.qq.com/s?__biz=MzA5ODM5MDU3MA==&mid=2650884317&idx=2&sn=61430aa56d3efadf127f65f6a5659db7&chksm=8b67af98bc10268e5b24e5ca34ef62a813daf14bc4c5585c4a8d13e1ec78b9f21a6e04d69da3&scene=21#wechat_redirect)</ins>

<ins>3、[盘点 66 个 Pandas 函数，轻松搞定“数据清洗”！](http://mp.weixin.qq.com/s?__biz=MzA5ODM5MDU3MA==&mid=2650884386&idx=2&sn=4fd2143b6d678a184e4bcda0dcd8465c&chksm=8b67ae67bc102771f66d042f8c3b5a43566010c6845dff92d8e75cfbeaf409c9b5c983f6851b&scene=21#wechat_redirect)</ins>

看完本文有收获？请转发分享给更多人

**推荐关注「数据分析与开发」，提升数据技能**

![](../../../_resources/0_wx_fmt_png_741a0213433c4b7b813e140e9bb205bf.png)

**数据分析与开发**

「数据分析与开发」分享数据分析与开发相关技术文章、教程、工具

<a id="js_profile_article"></a>55篇原创内容

Official Account

点赞和在看就是最大的支持❤️

People who liked this content also liked

MySQL8.0 InnoDB并行查询特性

...

yangyidba

不看的原因

- 内容质量低
- 不看此公众号

还在使用 ELK 收集日志吗？赶快来试试这个基于 ClickHouse 的方案吧~

...

k8s技术圈

不看的原因

- 内容质量低
- 不看此公众号

Git 不要只会 pull 和 push，试试这 5 条提高效率的命令

...

开源前线

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___4161668ce7144c84a.bmp"/>

Scan to Follow