# pandas100个骚操作：explode 列转行的 2 个常用技巧！

<a id="copyright_logo"></a>Original 东哥起飞 <a id="profileBt"></a><a id="js_name"></a>Python数据科学 *2021-02-02 13:45*

<a id="js_article-tag-card__left"></a>收录于合集 #pandas骚操作 <a id="js_article-tag-card__right"></a>25个

关注上方“Python数据科学”，选择星标，

关键时间，第一时间送达！

<img width="657" height="334" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__c157f3faef3047e3a.png"/>

来源：Python数据科学

作者：东哥起飞

大家好，我是东哥。

本篇是pandas100个骚操作系列的第 **9**篇：**explode 列转行的 2 个常用技巧！**

系列内容，请看👉「[pandas100个骚操作](https://mp.weixin.qq.com/mp/appmsgalbum?__biz=MzUzODYwMDAzNA==&action=getalbum&album_id=1699019347278561282#wechat_redirect)」话题，订阅后文章更新可第一时间推送至订阅号。

* * *

在我们处理数据的过程中，经常会遇到这样的情况。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

工作中，比如用户画像的数据中也会遇到，客户使用的app类型就会以这种长列表的形式或者以逗号隔开的字符串形式展现出来。

那么面对这样的数据格式，我们希望把它转换为结构化的表，脑海中想象的是下面这种格式。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

使用`pandas`如何实现呢？

一、直接explode

其实非常简单，使用`explode`这个 **“**爆炸” ****方法即可，东哥平时喜欢叫它爆炸。其实，这个和`hive`中的`lateral view explode`有异曲同工的效果，也就是 **“**列转行”****的功能。

仍用上面这个例子，要达到想要的效果，只需要这么做。

```


df.explode('爱好')


```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

看到**爱好**这个字段被爆炸开了，列表里所有特征都被转换为对应程序员的行数据。

但列表有重复的值，就可能导致爆炸出来的行存在重复行，如上面**小码哥**出现了两次敲代码。所以一般我们会在后面跟一个去重的方法。

```


df.explode('爱好').drop_duplicates()


```

## **![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)**

## **二、explode不能直接处理的**

但是，`explode`这个爆炸方法只能处理`列表`、`元组`、`Series`和`numpy`的`ndarray`的类型。

如果面对下面这种格式，该如何爆炸？

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

其实也不难，只要运用一个小技巧即可，就是`Series.str.split()`分割字符串的方法来创建列表。

```
df["爱好"] = df["爱好"].str.split()

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

然后，我们再用`explode`爆炸就完事了。

以上就是本次关于 列转行 的2个骚操作分享。

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

<a id="js_album_prev"></a>上一篇 <a id="js_album_keep_read_pre_title"></a>pandas100个骚操作：transform 数据转换的 4 个常用技巧！ <a id="js_album_next"></a>下一篇 <a id="js_album_keep_read_next_title"></a>pandas100个骚操作：使用 Datetime 提速 50 倍运行速度！

People who liked this content also liked

超强图解pandas，建议收藏(文末赠书)

...

Python数据科学

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___91e4c9f8e32442cca.bmp"/>

Scan to Follow