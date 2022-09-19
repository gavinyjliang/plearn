# pandas100个骚操作：用 pandas 快速爬数据

<a id="copyright_logo"></a>Original 东哥起飞 <a id="profileBt"></a><a id="js_name"></a>Python数据科学 *2021-01-19 14:04*

<a id="js_article-tag-card__left"></a>收录于合集 #pandas骚操作 <a id="js_article-tag-card__right"></a>25个

关注上方“Python数据科学”，选择星标，

关键时间，第一时间送达！

<img width="657" height="334" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__64f5944cf3424584a.png"/>

来源：Python数据科学

作者：东哥起飞

大家好，我是你们的东哥。

本篇是pandas100个骚操作系列的第3篇：**利用pandas快速爬取数据**

系列全部内容请看文章标题下方的「**pandas100个骚操作**」话题，订阅后可更新可第一时间通知。

* * *

提起爬虫，大家可能都知道`requests`、`beautifulsoup`、`scrapy`、`selenium`等等一些工具库。但其实对于一些日常的**网页Table表格数据抓取**来讲，没有必要去F12研究HTML页面结构甚至写正则表达式解析字段。

本次东哥介绍一个超级简单的方法，用`pandas`也可以玩爬虫。

`pandas`自带一个方法是`read_html`，利用这个方法可以直接爬虫网页的**Table表格型数据**，无需敲更多的爬虫代码，简单！粗暴！

查看HTML结构，如果发现是下面这个table格式的，那直接可以上手开干。

```
<table class="..." id="...">
     <thead>
     <tr>
     <th>...</th>
     </tr>
     </thead>
     <tbody>
        <tr>
            <td>...</td>
        </tr>
        <tr>...</tr>
        <tr>...</tr>
        ...
        <tr>...</tr>
        <tr>...</tr>
    </tbody>
</table>

```

下面我们来看下如何操作。

## 一、使用方法

举一个例子，拿wiki百科上的各国家收入的页面抓取演示一下。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

这个页面中有非常多的表格，符合我们的要求，直接使用`read_html`，它可以自动将网页的所有表格数据全部抓取下来。代码如下：

```
import pandas as pd
url = 'https://en.wikipedia.org/wiki/Gross_national_income'
tables = pd.read_html(url)

```

这里返回的`tables`是一个`DataFrames`的列表，每个`DataFrame`就是网页中从上到下顺序的数据表格。因此，可以用列表的切片`tables[x]`来提取网页指定的表格数据。

比如，我们对第4个表格感兴趣，那么直接：

```
talbes[3]

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

当然，上面表格看起来有点别扭，我们可以简单几个操作调整一下表结构。

```
df = tables[3].droplevel(0, axis=1)\
.rename(columns={'No.':'No', 'GDP[10]':'GDP'})\
.set_index('No')

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

这样看起来就好多了。

最后，`read_html`中也配有很多参数可供调整，比如匹配方式、标题所在行、网页属性识别表格等等，具体说明可以参看`pandas`的官方文档学习。

> 官网链接：https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.read_html.html

以上所有代码已上传至我的GitHub：

> 项目链接：https://github.com/xiaoyusmd/PythonDataScience

**原创不易，GitHub给个Star求续命![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)**

```


![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg== "音符")

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

<a id="js_album_prev"></a>上一篇 <a id="js_album_keep_read_pre_title"></a>pandas100个骚操作：JSON自动解析为DataFrame <a id="js_album_next"></a>下一篇 <a id="js_album_keep_read_next_title"></a>pandas100个骚操作：再见，可视化！你好，pandas！

People who liked this content also liked

超强图解pandas，建议收藏(文末赠书)

...

Python数据科学

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___e31898ef26524dd68.bmp"/>

Scan to Follow