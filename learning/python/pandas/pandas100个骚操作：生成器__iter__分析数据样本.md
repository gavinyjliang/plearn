# pandas100个骚操作：生成器\_\_iter\_\_分析数据样本

<a id="copyright_logo"></a>Original 东哥起飞 <a id="profileBt"></a><a id="js_name"></a>Python数据科学 *2021-01-22 13:35*

<a id="js_article-tag-card__left"></a>收录于合集 #pandas骚操作 <a id="js_article-tag-card__right"></a>25个

关注上方“Python数据科学”，选择星标，

关键时间，第一时间送达！

<img width="657" height="334" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__a470889ebec34346a.png"/>

来源：Python数据科学

作者：东哥起飞

大家好，我是你们的东哥。

本篇是pandas100个骚操作系列的第 **5 **篇：**生成器\_\_iter\_\_分析数据样本**

系列全部内容请看文章标题下方的「**pandas100个骚操作**」话题，订阅后可更新可第一时间推送文章。

* * *

在Jupyter Notebook中通常很难像使用Excel一样难逐行或逐个组地浏览数据集。一个非常有用的技巧是使用 generator 生成器和Ctrl + Enter组合，而不是我们常规的Shift + Enter运行整个单元格。这样做就可以很方便地迭代查看同一单元格中的不同样本了。

一、首先在单元格中使用.groupby()（或.iterrows()）和.\_\_iter \_\_()创建一个**生成器**：

```


generator = df.groupby(['identifier']).__iter__()



```

二、然后，根据自己需要运行的次数，使用键盘快捷键 **Ctrl + Enter **逐个查看数据。

```


group_id, grouped_data = generator.__next__()
print(group_id) 
grouped_data



```

下面是taitanic数据集的示例。正常分析的时候，所有乘客都混在一起，我们是不能单独地隔离每组乘客的，使用这种方法就可以非常简单地分析一组乘客。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

* * *

```


我是东哥，最后给大家分享《100本Python电子书》，包括Python编程技巧、数据分析、爬虫、Web开发、机器学习、深度学习。

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

收录于合集 #<a id="js_album_keep_read_title"></a>pandas骚操作

 <a id="js_album_keep_read_size"></a>25个

<a id="js_album_prev"></a>上一篇 <a id="js_album_keep_read_pre_title"></a>pandas100个骚操作：再见，可视化！你好，pandas！ <a id="js_album_next"></a>下一篇 <a id="js_album_keep_read_next_title"></a>pandas100个骚操作：强大的 accessor 方法

<a id="js_view_source"></a>Read more

People who liked this content also liked

超强图解pandas，建议收藏(文末赠书)

...

Python数据科学

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___63121a7fe11a4f4a9.bmp"/>

Scan to Follow