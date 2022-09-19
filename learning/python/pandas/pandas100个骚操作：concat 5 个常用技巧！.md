# pandas100个骚操作：concat 5 个常用技巧！

<a id="copyright_logo"></a>Original 东哥起飞 <a id="profileBt"></a><a id="js_name"></a>Python数据科学 *2021-05-11 13:45*

<a id="js_article-tag-card__left"></a>收录于合集 #pandas骚操作 <a id="js_article-tag-card__right"></a>25个

大家好，我是东哥。

本篇是pandas100个骚操作系列的第 **16** 篇：concat 5 个常用技巧！

系列内容，请看👉「[pandas100个骚操作](https://mp.weixin.qq.com/mp/appmsgalbum?__biz=MzUzODYwMDAzNA==&action=getalbum&album_id=1699019347278561282&scene=21#wechat_redirect)」话题，订阅后文章更新可第一时间推送至订阅号。

* * *

`pandas`提供了很多功能可组合合并`DataFrame`，`concat()`函数就是其中之一。

本篇将介绍`concat`常用的5个操作技巧：

- 处理索引和轴
    
- 避免重复索引
    
- 使用keys和names选项添加层次结构索引
    
- 列匹配和排序
    
- 连接CSV文件数据集
    

## 1.处理索引和轴

假设我们有2个关于考试成绩的数据集。

```
df1 = pd.DataFrame（{ 
    'name'：['A'，'B'，'C'，'D']，
    'math'：[60,89,82,70]，
    'physics'：[66， 95,83,66]，
    'chemistry'：[61,91,77,70] 
}）
df2 = pd.DataFrame（{ 
    'name'：['E'，'F'，'G'，'H']，
    'math'：[66,95,83,66]，
    'physics'：[60， 89,82,70]，
    'chemistry'：[90,81,78,90] 
}）

```

最简单的用法就是传递一个含有DataFrames的列表，例如`[df1, df2]`。默认情况下，它是沿axis=0垂直连接的，并且默认情况下会保留df1和df2原来的索引。

```
pd.concat（[df1，df2]）

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

如果想要合并后忽略原来的索引，可以通过设置参数`ignore_index=True`，这样索引就可以从0到n-1自动排序了。

```
pd.concat（[df1，df2]，ignore_index = True）

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

如果想要沿水平轴连接两个`DataFrame`，可以设置参数`axis=1`。

```
pd.concat（[df1，df2]，axis = 1）

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

以上是一些基本操作，我们继续往下看。

## 2.避免重复索引

我们知道了`concat()`函数会默认保留原`dataframe`的索引。那有些情况，我想保留原来的索引，并且我还想验证合并后的结果是否有重复的索引，该怎么办呢？

可以通过设置参数`verify_integrity=True`，将此设置`True`为时，如果存在重复的索引，将会报错。比如下面这样。

```
try:
    pd.concat([df1,df2], verify_integrity=True)
except ValueError as e:
    print('ValueError', e)
ValueError: Indexes have overlapping values: Int64Index([0, 1, 2, 3], dtype='int64')

```

## 3.使用keys和names选项添加层次结构索引

添加层次结构索引非常的有用，可以进行更多层的数据分析。

举个例子，某些情况下我们并不想合并两个`dataframe`的索引，而是想为两个数据集贴上标签。比如我们分别为`df1`和`df2`添加标签`Year 1`和`Year 2`。

这种情况，我们只需指定`keys`参数即可。

```
res = pd.concat（[df1，df2]，keys = ['Year 1'，'Year 2']）
res

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

如果我们想要获取`Year 1`的数据集，可以直接使用`loc`像下面这样操作：

```
res.loc['Year 1']

```

另外，参数`names`可用于为所得的层次索引添加名称。例如，将名称`Class`添加到刚创建的的标签上。

```
pd.concat(
    [df1，df2]，
    keys = ['Year 1'，'Year 2']，
    names = ['Class'，None]，
)

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

如果要重置索引并将其转换为数据列，可以使用 `reset_index()`，这一步操作也是非常的实用。

```
pd.concat(
    [df1, df2], 
    keys=['Year 1', 'Year 2'],
    names=['Class', None],
).reset_index(level=0)   
# reset_index(level='Class')

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 4.列匹配和排序

`concat()`函数还可以将合并后的列按不同顺序排序。虽然，它会自动将两个df的列对齐合并。但默认情况下，生成的`DataFrame`与第一个`DataFrame`具有相同的列排序。例如，在以下示例中，其顺序与`df1`相同。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

如果想要按字母顺序对结果`DataFrame`进行排序，则可以设置参数`sort=True`。

```
pd.concat([df1, df2], sort=True)

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

或者也可以自定义排序，像下面这样：

```
custom_sort = ['math', 'chemistry', 'physics', 'name']
res = pd.concat([df1, df2])
res[custom_sort]

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 5.连接CSV文件数据集

假设我们需要从一堆CSV文件中加载并连接数据集。常规做法，我们可能会使用`for`循环解决，比如下面这样。

```
import pathlib2 as pl2
ps = pl2.Path('data/sp3')
res = None
for p in ps.glob('*.csv'):
    if res is None:
        res = pd.read_csv(p)
    else:
        res = pd.concat([res, pd.read_csv(p)])

```

但上面`pd.concat()`在每次for循环迭代中都会被调用一次，效率不高，推荐使用列表推导式的写法。

```
import pathlib2 as pl2
ps = pl2.Path('data/sp3')
dfs = (
    pd.read_csv(p, encoding='utf8') for p in ps.glob('*.csv')
)
res = pd.concat(dfs)
res

```

这样就可以用一行代码读取所有CSV文件并生成`DataFrames`的列表`dfs`。然后，我们只需要调用`pd.concat(dfs)`一次即可获得相同的结果，简洁高效。

使用`%%timeit`测试下上面两种写法的时间，第二种列表推导式大概省了一半时间。

```
# for-loop solution
298 ms ± 11.8 ms per loop (mean ± std. dev. of 7 runs, 1 loop each)
# list comprehension solution
153 ms ± 6 ms per loop (mean ± std. dev. of 7 runs, 1 loop each)

```

**以上就是5个concat日常操作。**

**原创不易，欢迎点赞、留言、分享，支持我继续写下去![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)**

参考：

\[1\] https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.concat.html

\[2\] https://towardsdatascience.com/pandas-concat-tricks-you-should-know-to-speed-up-your-data-analysis-cd3d4fdfe6dd

* * *

我是东哥，最近正在原创👉「[pandas100个骚操作](https://mp.weixin.qq.com/mp/appmsgalbum?__biz=MzUzODYwMDAzNA==&action=getalbum&album_id=1699019347278561282#wechat_redirect)」系列话题，欢迎订阅。订阅后，文章更新可第一时间推送至订阅号，每篇都不错过。

最后给大家**分享《100本Python电子书》**，包括Python编程技巧、数据分析、爬虫、Web开发、机器学习、深度学习。

现在免费分享出来，有需要的读者可以下载学习，在下面的公众号「**GitHuboy**」里回复关键字：**Python**，就行。

![](../../../_resources/0_wx_fmt_png_2e669e9623ff43648474029a6d56f8e0.png)

**机器学习专栏**

专注于分享机器学习、深度学习、NLP、CV、PyTorch、TensorFlow、Python实战等干货文章。

<a id="js_profile_article"></a>24篇原创内容

Official Account

收录于合集 #<a id="js_album_keep_read_title"></a>pandas骚操作

 <a id="js_album_keep_read_size"></a>25个

<a id="js_album_prev"></a>上一篇 <a id="js_album_keep_read_pre_title"></a>用了一年pandas，才知道category的这些坑！ <a id="js_album_next"></a>下一篇 <a id="js_album_keep_read_next_title"></a>好习惯！pandas 8 个常用的 option 设置

People who liked this content also liked

超强图解pandas，建议收藏(文末赠书)

...

Python数据科学

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___1997814294154feaa.bmp"/>

Scan to Follow