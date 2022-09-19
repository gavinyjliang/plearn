# pandas100个骚操作：逆天！一行代码让 apply 速度飙到极致

<a id="copyright_logo"></a>Original 东哥起飞 <a id="profileBt"></a><a id="js_name"></a>Python数据科学 *2021-03-04 13:46*

<a id="js_article-tag-card__left"></a>收录于合集 #pandas骚操作 <a id="js_article-tag-card__right"></a>25个

关注上方“Python数据科学”，选择星标，

关键时间，第一时间送达！

<img width="657" height="334" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__7ef84cc6c45d407f8.png"/>

来源：Python数据科学

作者：东哥起飞

大家好，我是东哥。

本篇是pandas100个骚操作系列的第 **13**篇：**一行代码让 pandas 的 apply 速度飙到极致！**

系列内容，请看👉「[pandas100个骚操作](https://mp.weixin.qq.com/mp/appmsgalbum?__biz=MzUzODYwMDAzNA==&action=getalbum&album_id=1699019347278561282#wechat_redirect)」话题，订阅后文章更新可第一时间推送至订阅号。

* * *

**一. pandas提速的方法回顾**

如果想要让`pandas`提速，东哥总结有两个方法：

**1. 向量化**

向量化是最优的方法，具体用法参考文章：[pandas100个骚操作：再见 for 循环！速度提升315倍！](http://mp.weixin.qq.com/s?__biz=MzUzODYwMDAzNA==&mid=2247517636&idx=2&sn=4225c9dcbd9604453620be5bbe6b76f8&chksm=fad7f2c9cda07bdf4b759ec1ac0e0c168538fc570c1c687679ffd1b54bfb850f2b8e0d4cd371&scene=21#wechat_redirect)

举个例子，我们将向量化定义为使用`Numpy`表示整个数组而不是元素的计算。下面有两个数组：

```


array_1 = np.array([1,2,3,4,5])
array_2 = np.array([6,7,8,9,10])



```

我们希望创建一个新数组，该数组是两个数组的总和，结果应该是：

```


result = [7,9,11,13,15]



```

当然，我们也可以在`Python`中使用`for`循环将这些数组求和，但这非常慢。替代的是，`Numpy`允许我们直接在阵列上进行操作，这要快得多，尤其是大型阵列。

```


result = array_1 + array_2


```

**2\. 并行化**

其次是并行化，比如使用`dask`。

相信大家平时对`pandas`使用比较多的一个功能就是apply功能，使用自带函数或者自己写个函数，可以直接对`dataframe`进行变换，非常香！

但由于pandas的apply只是将函数应用于dataframe的每一行，只调用当个处理器，如果行数非常多，那么将非常慢。

但如果我们利用多处理器并行化，将dataframe数据框分成多个部分，然后将每个部分都送到处理器，最后再将各个部分组合拼成回单个dataframe，这就快多了！

**如何将这两个方法结合起来，对apply功能提速？**

本次给大家分享一个神器 Swifter，可以自动让`apply`的运行速度达到最快，并且只需要一行代码！

**二. Swifter介绍**

`Swifter`是这样做的。

**1\.** 首先，检查apply的函数是否可以向量化，如果可以，就自动使用向量化的计算（最有效果）。

**2\.** 如果无法进行向量化，那就检查使用`Dask`进行并行处理或仅使用普通`Pandas`的`apply`（仅使用单个内核）哪个更合理。

**并行化并不是一定要用的，因为并行处理的开销会使小型数据集的处理速度变慢，所以这个也需要根据数据集的大小情况具体分析。**

来看一张图。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

通过上图结果可以总结出：**无论数据大小如何，使用向量化的结果几乎总是更好的，但如果无法做到向量化，那我们就退而求其次，通过并行让pandas速度最优（当数据集大小超过某个阈值的时候，红线和蓝线的交点）。**

太牛掰了，使用`swifter`可以直接为我们自动选择最佳的方式。

**三. 如何使用Swifter？**

`Swifter`的使用非常简单。

```


import pandas as pd
import swifter
df.swifter.apply(lambda x: x.sum() - x.min())


```

我们只需要引入`swifter`，然后简单的一行代码调用即可，赶快试一下这个神器！

以上就是本次分享，如果喜欢东哥的骚操作，**请给我点个赞和在看**![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

完整系列请看👉「[pandas100个骚操作](https://mp.weixin.qq.com/mp/appmsgalbum?__biz=MzUzODYwMDAzNA==&action=getalbum&album_id=1699019347278561282#wechat_redirect)」

**参考链接：**

\[1\]. https://github.com/jmcarpenter2/swifter

\[2\]. https://towardsdatascience.com/one-word-of-code-to-stop-using-pandas-so-slowly-793e0a81343c

👇 点击「**阅读原文**」有惊喜

收录于合集 #<a id="js_album_keep_read_title"></a>pandas骚操作

 <a id="js_album_keep_read_size"></a>25个

<a id="js_album_prev"></a>上一篇 <a id="js_album_keep_read_pre_title"></a>pandas100个骚操作：Squeeze 类型压缩小技巧！ <a id="js_album_next"></a>下一篇 <a id="js_album_keep_read_next_title"></a>pandas100个骚操作：groupby 8 个常用技巧！

<a id="js_view_source"></a>Read more

People who liked this content also liked

超强图解pandas，建议收藏(文末赠书)

...

Python数据科学

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___ab67f1aa953945de9.bmp"/>

Scan to Follow