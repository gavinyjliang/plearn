# 对比Excel，用Pandas轻松搞定IF函数操作

<a id="copyright_logo"></a>Original <a id="js_author_name"></a>道才 <a id="profileBt"></a><a id="js_name"></a>可以叫我才哥 *2021-08-30 14:52*

<a id="js_article-tag-card__left"></a>收录于话题 #pandas <a id="js_article-tag-card__right"></a>27个

![](../../../_resources/0_wx_fmt_png_55b25d6baaf747c39e2ac57104d5bbb1.png)

**可以叫我才哥**

学python，玩转数据分析、网络爬虫以及可视化等等

<a id="js_profile_article"></a>148篇原创内容

Official Account

大家好，我是才哥。

在 *Excel* 中*IF 函数*是最常用的函数之一，它可以对值和期待值进行逻辑比较。因此`IF` 语句可能有两个结果：第一个结果是比较结果为 True，第二个结果是比较结果为 False。

例如，`=IF(C2=”Yes”,1,2)` 表示 IF(C2 = Yes, 则返回 1, 否则返回 2)。

那么，在Pandas里我们可以怎么来轻松搞定这一操作呢？

今天，我们就来了解一下！

**目录：**

- 1\. 案例需求
    
- 2\. Excel轻松搞定
    
- 3\. Pandas处理
    
- 4\. 延伸
    

<img width="20" height="18" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__880631c6ebe74f498.png"/>

1\. 案例需求

原始数据如下，是一份虚构的学生成绩单

| 姓名  | 语文  | 数学  | 英语  | 性别  |
| --- | --- | --- | --- | --- |
| 才哥  | 91  | 95  | 92  | 1   |
| 小明  | 82  | 93  | 91  | 1   |
| 小华  | 82  | 87  | 94  | 1   |
| 小草  | 46  | 55  | 58  | 0   |
| 小红  | 51  | 41  | 70  | 0   |
| 小花  | 58  | 59  | 40  | 0   |
| 小龙  | 70  | 55  | 59  | 1   |
| 杰克  | 53  | 44  | 42  | 1   |
| 韩梅梅 | 45  | 51  | 67  | 0   |

现在我们有两个需求：

- 语数英三科评级，低于60分标记为不及格、60-89为及格、90分以上为高分；
    
- 性别中1为男性、0为女性。
    

<img width="20" height="18" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__880631c6ebe74f498.png"/>

2\. Excel轻松搞定

如果用Excel来处理，首先可以想到用IF函数的方法

对于语数英科目评级中，可以用到以下公式实现：

```
`=IF(B2<60,"不及格",IF(B2<90,"及格","高分"))
`
```

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__485fc8ec7a784ee68.png)

语数英科目评级

对于性别标识来说，可以用以下公式实现：

```
`=IF(E2=1,"男","女")
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

性别标识

当然了，以上是IF函数的方法，我们还可以用`lookup`进行实现：

```
`# 语数外三科评级
=LOOKUP(B2,{0,"不及格";60,"及格";90,"高分"})
# 性别标识
=LOOKUP(E2,{0,"女";1,"男"})
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

LOOKUP技巧

需要注意的是，`LOOKUP`中的条件是**向后兼容**哈

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

3\. Pandas处理

这里通过`df.where`和`np.where`两个函数来实现需求，先看代码，然后我们再讲解下

```
`import pandas as pd
# 读取数据
df = pd.read_excel(r'F:\Python\pandas数据处理\案例数据.xlsx')
# 筛选 语数外 评分
score = df.loc[:,'语文':'英语']
# 评级
data = score.where(score>100, np.where(score<60,"不及格", np.where(score<90,"及格","高分")))
# 性别
data['性别'] = df['性别'].where(df['性别']>100, np.where(df['性别']==0, '女性', '男性'))
data.insert(0,'姓名', df['姓名'])
data
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

输出结果

以上实现方案中，用到的两个`where`函数，其实就和`excel`里的if很类似。

**df.where**

该函数可以将满足条件的函数筛选出来，将不满足条件的值赋值为另外一个值，默认情况下为`NaN`。

```
`Signature:
df.where(
    cond,
    other=nan,
    inplace=False,
    axis=None,
    level=None,
    errors='raise',
    try_cast=<no_default>,
)
Docstring:
Replace values where the condition is False.
`
```

从函数介绍来看，它能做到的只有一种条件判断，然后只能对不满足要求的值进行赋值操作，比如：

```
`# 显示≥60的值，低于60分显示为 不及格
df[['语文','数学','英语']].where(df[['语文','数学','英语']]>60, '不及格')
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

df.where

**np.where**

既然`df.where`无法对多种情况进行分别赋值， 那么`np.where`就来了

```
``Docstring:
where(condition, [x, y])
Return elements chosen from `x` or `y` depending on `condition`.
Parameters
----------
condition : array_like, bool
    Where True, yield `x`, otherwise yield `y`.
x, y : array_like
    Values from which to choose. `x`, `y` and `condition` need to be
    broadcastable to some shape.
Returns
-------
out : ndarray
    An array with elements from `x` where `condition` is True, and elements
    from `y` elsewhere.
``
```

和`Excel`中`IF`函数**更接近**的其实就是`np.where`这个函数，**如果条件满足则赋值x，否则赋值y**。

比如：

```
`>>> import numpy as np
>>> a = np.arange(10)
>>> a
array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])
>>> np.where(a < 5, a, 10*a)
array([ 0,  1,  2,  3,  4, 50, 60, 70, 80, 90])
`
```

上述例子中，如果值小于5，则显示值本身；反之则 乘以10。

我们就可以构建对科目评分进行评级的双层条件，具体如下：

```
`# 如果小于60就不及格，否则再进行后面的判断
np.where(score<60,"不及格", np.where(score<90,"及格","高分"))
`
```

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__7b13d7a2069245a99.png)

基于以上的介绍，我们要完成本次的需求就有了以下的实现方案：

```
`# 筛选 语数外 评分
score = df.loc[:,'语文':'英语']
# 评级
data = score.where(score>100, np.where(score<60,"不及格", np.where(score<90,"及格","高分")))
# 性别
data['性别'] = np.where(df['性别']==0, '女性', '男性')
`
```

需要注意的是，这里咱们对性别标识的处理稍微区别于开头的完整代码中的，大家知道为什么可以这么写吗？（*DataFrame和Series的小区别*）

以上，就是本次用Pandas实现Excel里IF函数方法的操作了，感兴趣的你可以试试哦！

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

4\. 延伸

**tips one**

既然有 `df.where` 筛选满足条件的值显示，不满足的进行赋值。那么，是不是有筛选满足条件的值进行赋值，不满足的值显示呢？

答案是肯定的！

这便是`df.mask`函数方法，其效果和`df.where`基本相反。

```
`>>> import pandas as pd
>>> s = pd.Series(range(5))
>>> s.where(s > 0)
0    NaN
1    1.0
2    2.0
3    3.0
4    4.0
dtype: float64
>>> s.mask(s > 0)
0    0.0
1    NaN
2    NaN
3    NaN
4    NaN
dtype: float64
>>> s.where(s > 1, 10)
0    10
1    10
2     2
3     3
4     4
dtype: int64
>>> s.mask(s > 1, 10)
0     0
1     1
2    10
3    10
4    10
dtype: int64
`
```

**tips two**

其实吧，实现我们今天案例的需求还有很多方案，这里再简单介绍几种思路（答案可见后续推文）

- 通过 自定义函数 if else来处理，然后apply或map调用
    
- 通过 cut分箱 来处理
    
- 通过 replace 来处理
    
- 又或者 where与mask组合来处理
    
- 其他方案
    

感兴趣的朋友，可以试试哦，然后我们一起交流交流

**添加作者微信 gdc2918，加入交流群搞起~~**

推荐阅读

[只需8招，搞定Pandas数据筛选与查询<br>2021-08-29<br><img width="51" height="22" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_118ba9a3c23146779.jpg"/>](http://mp.weixin.qq.com/s?__biz=MzUzODA5MjM5NQ==&mid=2247490587&idx=1&sn=f85d9e4eddaa42a93e56be0e9017886d&chksm=faddaa1fcdaa230915c901aecf3f51fa704f0c810914bdfdac28f23251afa3634c94866143e3&scene=21#wechat_redirect)

[怎么用Python绘制这样的图？<br>2021-08-26<br><img width="51" height="22" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_6d5d7dd06b364e2d8.jpg"/>](http://mp.weixin.qq.com/s?__biz=MzUzODA5MjM5NQ==&mid=2247490310&idx=1&sn=6537038dfd5b0e96cbdb11a6c3729d40&chksm=faddad02cdaa2414e4d0b21f1846cc6ac5c1fcbc56996cc531d4e7b7b47566546360183bdd3f&scene=21#wechat_redirect)

[用python开发一个炸金花小游戏，注意别玩上瘾了~~<br>2021-08-24<br><img width="51" height="22" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_2d030d1efb80418db.jpg"/>](http://mp.weixin.qq.com/s?__biz=MzUzODA5MjM5NQ==&mid=2247490279&idx=1&sn=ba6bd5f9a79935023777c3cad6f22ed3&chksm=faddace3cdaa25f57b828c4b73a5c89b15c96f20a79734f8f2356ae30058f0fc5ce9b41e7361&scene=21#wechat_redirect)

[5招学会Pandas数据类型转化<br>2021-08-22<br><img width="51" height="22" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_5a822eaac8864b798.jpg"/>](http://mp.weixin.qq.com/s?__biz=MzUzODA5MjM5NQ==&mid=2247490212&idx=1&sn=6794bebe54f7ff20d4e2553d7a3ed932&chksm=faddaca0cdaa25b6706e0b5b9655b0dac6a0bf1188ef19b42fa67d8d6e2d4fec1a042ee66843&scene=21#wechat_redirect)

[30个函数玩转Pandas统计计算！<br>2021-08-21<br><img width="51" height="22" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_771b628c1ad948a29.jpg"/>](http://mp.weixin.qq.com/s?__biz=MzUzODA5MjM5NQ==&mid=2247490154&idx=1&sn=62cb32da477b6e485ea38ab7c6a1dbb8&chksm=faddac6ecdaa2578fe7e81422d79dc6d53885bb66c7383dafa97b5f7dce9fbc0e180be4e461b&scene=21#wechat_redirect)

![](../../../_resources/0_wx_fmt_png_55b25d6baaf747c39e2ac57104d5bbb1.png)

**可以叫我才哥**

学python，玩转数据分析、网络爬虫以及可视化等等

<a id="js_profile_article"></a>148篇原创内容

Official Account

收录于话题 #<a id="js_album_keep_read_title"></a>pandas

 <a id="js_album_keep_read_size"></a>27个

<a id="js_album_prev"></a>上一篇 <a id="js_album_keep_read_pre_title"></a>你知道怎么用Pandas绘制带交互的可视化图表吗？ <a id="js_album_next"></a>下一篇 <a id="js_album_keep_read_next_title"></a>只需8招，搞定Pandas数据筛选与查询

People who liked this content also liked

Python自动化办公之Excel对比工具

可以叫我才哥

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___4b69d7e5e8464dac9.bmp"/>

Scan to Follow