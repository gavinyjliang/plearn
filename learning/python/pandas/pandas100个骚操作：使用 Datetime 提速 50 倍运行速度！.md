# pandas100个骚操作：使用 Datetime 提速 50 倍运行速度！

<a id="copyright_logo"></a>Original 东哥起飞 <a id="profileBt"></a><a id="js_name"></a>Python数据科学 *2021-02-04 13:45*

<a id="js_article-tag-card__left"></a>收录于合集 #pandas骚操作 <a id="js_article-tag-card__right"></a>25个

关注上方“Python数据科学”，选择星标，

关键时间，第一时间送达！

<img width="657" height="334" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__718f51266b9c48c58.png"/>

来源：Python数据科学

作者：东哥起飞

大家好，我是东哥。

本篇是pandas100个骚操作系列的第 **10**篇：**使用 Datetime 提速 50 倍运行速度！**

系列内容，请看👉「[pandas100个骚操作](https://mp.weixin.qq.com/mp/appmsgalbum?__biz=MzUzODYwMDAzNA==&action=getalbum&album_id=1699019347278561282#wechat_redirect)」话题，订阅后文章更新可第一时间推送至订阅号。

* * *

平时我们运行`pandas`少不了和时间打交道，而大多情况下许多朋友都是暴力解决问题，直接让`pandas`自己转换和处理。

对于平时的学习和小测试是没什么问题的，但当跑一些大数据的时候往往会非常的慢，而这个时间性能其实是完全可以优化的。

本次东哥介绍一个非常简单的操作，使用`Datetime`变换时间类型，让你的代码运行速度飞速提升。

下面，我们来看一个例子。

```


# 导入数据集
>>> df = pd.read_csv('demand_profile.csv')
>>> df.head()
     date_time  energy_kwh
0  1/1/13 0:00       0.586
1  1/1/13 1:00       0.580
2  1/1/13 2:00       0.572
3  1/1/13 3:00       0.596
4  1/1/13 4:00       0.592



```

从运行上面代码得到的结果来看，好像没有什么问题。但实际上`pandas`和`numpy`都有一个 `dtypes` 的概念。如果没有特殊声明，那么`date_time`将会使用一个 `object` 的 `dtype` 类型，如下面代码所示：

```


>>> df.dtypes
date_time      object
energy_kwh    float64
dtype: object
>>> type(df.iat[0, 0])
str



```

`object`类型像一个大的容器，不仅仅可以承载 `str`，也可以包含那些不能很好地融进一个数据类型的任何特征列。而如果我们将日期作为 `str`类型就会极大的影响效率。

因此，对于时间序列的数据而言，我们需要让上面的`date_time`列格式化为`datetime`对象数组（`pandas`称之为时间戳）。`pandas`在这里操作非常简单，操作如下：

```


>>> df['date_time'] = pd.to_datetime(df['date_time'])
>>> df['date_time'].dtype
datetime64[ns]



```

我们来运行一下这个`df`看看转化后的效果是什么样的。

```


>>> df.head()
               date_time    energy_kwh
0    2013-01-01 00:00:00         0.586
1    2013-01-01 01:00:00         0.580
2    2013-01-01 02:00:00         0.572
3    2013-01-01 03:00:00         0.596
4    2013-01-01 04:00:00         0.592



```

`date_time`的格式已经自动转化了，但这还没完，在这个基础上，我们还是可以继续提高运行速度的。如何提速呢？为了更好的对比，我们首先通过`timeit`装饰器来测试一下上面代码的转化时间。

```


>>> @timeit(repeat=3, number=10)
... def convert(df, column_name):
...     return pd.to_datetime(df[column_name])
>>> df['date_time'] = convert(df, 'date_time')
Best of 3 trials with 10 function calls per trial:
Function `convert` ran in average of 1.610 seconds.



```

**1.61s，**看上去挺快，但其实可以更快，我们来看一下下面的方法。

```


>>> @timeit(repeat=3, number=100)
>>> def convert_with_format(df, column_name):
...     return pd.to_datetime(df[column_name],
...                           format='%d/%m/%y %H:%M')
Best of 3 trials with 100 function calls per trial:
Function `convert_with_format` ran in average of 0.032 seconds.



```

**结果只有0.032s，快了将近50倍。**

**原因是：**我们设置了转化的格式`format`。由于在`CSV`中的`datetimes`并不是`ISO 8601`格式的，如果不进行设置的话，那么`pandas`将使用`dateutil`包把每个字符串`str`转化成`date`日期。

相反，如果原始数据`datetime`已经是`ISO 8601`格式了，那么`pandas`就可以立即使用最快速的方法来解析日期。这也就是为什么提前设置好格式`format`可以提升这么多。

当然，这个只是在时间处理上的一个提速小操作，**下一篇将会介绍一个更常用的牛逼提速方法。**

以上就是本次分享。

如果喜欢东哥的骚操作，请给我点个赞和在看![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

* * *

```


我是东哥，最后给大家分享《100本Python电子书》，包括Python编程技巧、数据分析、爬虫、Web开发、机器学习、深度学习。

```


现在免费分享出来，有需要的读者可以下载学习，在下面的公众号「GitHuboy」里回复关键字：Python，就行。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

```


```


# 

```


爱点赞的人，运气都不会太差![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)


```




```


```




```


```

收录于合集 #<a id="js_album_keep_read_title"></a>pandas骚操作

 <a id="js_album_keep_read_size"></a>25个

<a id="js_album_prev"></a>上一篇 <a id="js_album_keep_read_pre_title"></a>pandas100个骚操作：explode 列转行的 2 个常用技巧！ <a id="js_album_next"></a>下一篇 <a id="js_album_keep_read_next_title"></a>pandas100个骚操作：Squeeze 类型压缩小技巧！

People who liked this content also liked

超强图解pandas，建议收藏(文末赠书)

...

Python数据科学

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___0cef4e1298e34e298.bmp"/>

Scan to Follow