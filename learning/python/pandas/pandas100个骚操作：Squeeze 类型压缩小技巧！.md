# pandas100个骚操作：Squeeze 类型压缩小技巧！

<a id="copyright_logo"></a>Original 东哥起飞 <a id="profileBt"></a><a id="js_name"></a>Python数据科学 *2021-02-20 13:47*

<a id="js_article-tag-card__left"></a>收录于合集 #pandas骚操作 <a id="js_article-tag-card__right"></a>25个

关注上方“Python数据科学”，选择星标，

关键时间，第一时间送达！

<img width="657" height="334" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__9833b2d00e3f44a49.png"/>

来源：Python数据科学

作者：东哥起飞

大家好，我是东哥。

本篇是pandas100个骚操作系列的第 **12**篇：**Squeeze 类型压缩小技巧！**

系列内容，请看👉「[pandas100个骚操作](https://mp.weixin.qq.com/mp/appmsgalbum?__biz=MzUzODYwMDAzNA==&action=getalbum&album_id=1699019347278561282#wechat_redirect)」话题，订阅后文章更新可第一时间推送至订阅号。

* * *

本次分享的`pandas`骚操作非常简单，但很实用。尤其在面临数据处理的过程中，是我们一定会面临的问题，下面一起来看一下。

在我看来，`pandas`的使用就是在和`DataFrame`、`Series`这两种结构打交道，就像使用`Excel`的`sheet`一样。但有的时候，我们希望能够摆脱`pandas`的表结构，而转换为标量（即单纯的数值）为我们所用。

比如下面这个情况，以这个数据为例。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

现在我们要提取`DataFrame`的中`volume`大于`100000000`的值。

volume = df\['Volume'\]
volume_highest = volume\[volume > 100000000\]

然后，我们在`Jupyter Notebook`的代码框里执行`volume_highest`，我们会看到结果是这样的。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

这个值前面还是跟着一个序号`19`，因为此时此刻它是个`Seires`结构，用`type`测试下就可以知道了。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

但我真正的需求是想把这个值赋给一个变量，如果是Seires类型一定会报错的。最开始东哥经常遇到这个情况，初学期间也干过比较蠢的办法，就是照着手动敲进去。

当时我就在想`pandas`里面一定有转换的方法的，只是我不知道而已。过了一段时间，我才知道，使用`squeeze`可以非常简单的处理这种情况。像下面这样一下就可以搞定了，可以直接赋给新的变量。

volume\_highest\_scalar = volume_highest.squeeze()

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

下面是`pandas`官方文档对`squeeze`的介绍。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

意思就是：

> - 具有单个元素的Series或DataFrame被压缩为标量。
>     
> - 具有单列或单行的DataFrame被压缩为Series。
>     
> - 否则，对象不变。
>     

因此，最开始举的例子只是第一种情况。当我们不知道对象是`Series`还是`DataFrame`，但是知道它只有一列时，`squeeze`方法最有用。在这种情况下，我们可以安全地调用`squeeze`以确保它变成一个`Series`。

以上就是本次关于`squeeze`的数据转换操作分享。

如果喜欢东哥的骚操作，请给我点个赞和在看![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

* * *

```


我是东哥，最后给大家分享《100本Python电子书》，包括Python编程技巧、数据分析、爬虫、Web开发、机器学习、深度学习。

```


现在免费分享出来，有需要的读者可以下载学习，在下面的公众号「GitHuboy」里回复关键字：Python，就行。


```


```

![](../../../_resources/0_wx_fmt_png_e375291b9f5e4992bf6a589ec00e599f.png)

**机器学习专栏**

专注于分享机器学习、深度学习、NLP、CV、PyTorch、TensorFlow、Python实战等干货文章。

<a id="js_profile_article"></a>24篇原创内容

Official Account

**爱点赞的人，运气都不会太差![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)**

收录于合集 #<a id="js_album_keep_read_title"></a>pandas骚操作

 <a id="js_album_keep_read_size"></a>25个

<a id="js_album_prev"></a>上一篇 <a id="js_album_keep_read_pre_title"></a>pandas100个骚操作：使用 Datetime 提速 50 倍运行速度！ <a id="js_album_next"></a>下一篇 <a id="js_album_keep_read_next_title"></a>pandas100个骚操作：逆天！一行代码让 apply 速度飙到极致

<a id="js_view_source"></a>Read more

People who liked this content also liked

超强图解pandas，建议收藏(文末赠书)

...

Python数据科学

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___e8b9ca9a232d45109.bmp"/>

Scan to Follow