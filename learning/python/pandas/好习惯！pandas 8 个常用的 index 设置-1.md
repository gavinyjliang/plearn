# 好习惯！pandas 8 个常用的 index 设置

<a id="copyright_logo"></a>Original 东哥起飞 <a id="profileBt"></a><a id="js_name"></a>Python数据科学 *2021-07-30 23:15*

<a id="js_article-tag-card__left"></a>收录于合集 #pandas骚操作 <a id="js_article-tag-card__right"></a>25个

大家好，我是东哥。

本篇是pandas100个骚操作系列的第 18 篇：8 个常用的 index 设置

系列内容，请看👉「[pandas100个骚操作](https://mp.weixin.qq.com/mp/appmsgalbum?__biz=MzUzODYwMDAzNA==&action=getalbum&album_id=1699019347278561282&scene=173&from_msgid=2247515707&from_itemidx=4&count=3&nolastread=1#wechat_redirect)」话题，订阅后文章更新可第一时间推送至订阅号。

* * *

在数据处理时，经常会因为index报错而发愁。不要紧，本次来和大家聊聊`pandas`中处理索引的几种常用方法。

## 1.读取时指定索引列

很多情况下，我们的数据源是 CSV 文件。假设有一个名为的文件`data.csv`，包含以下数据。

```
date,temperature,humidity
07/01/21,95,50
07/02/21,94,55
07/03/21,94,56

```

默认情况下，`pandas`将会创建一个从0开始的索引行，如下：

```
>>> pd.read_csv("data.csv", parse_dates=["date"])
        date  temperature  humidity
0 2021-07-01           95        50
1 2021-07-02           94        55
2 2021-07-03           94        56

```

但是，我们可以在导入过程中通过将`index_col`参数设置为某一列可以直接指定索引列。

```
>>> pd.read_csv("data.csv", parse_dates=["date"], index_col="date")
            temperature  humidity
date                             
2021-07-01           95        50
2021-07-02           94        55
2021-07-03           94        56

```

## 2\. 使用现有的 DataFrame 设置索引

当然，如果已经读取数据或做完一些数据处理步骤后，我们可以通过`set_index`手动设置索引。

```
>>> df = pd.read_csv("data.csv", parse_dates=["date"])
>>> df.set_index("date")
            temperature  humidity
date                             
2021-07-01           95        50
2021-07-02           94        55
2021-07-03           94        56

```

这里有两点需要注意下。

1.  `set_index`方法默认将创建一个新的 DataFrame。如果要就地更改`df`的索引，需要设置`inplace=True`。
    

```
df.set_index(“date”, inplace=True)

```

2.  如果要保留将要被设置为索引的列，可以设置`drop=False`。
    

```
df.set_index(“date”, drop=False)

```

## 3\. 一些操作后重置索引

在处理 DataFrame 时，某些操作（例如删除行、索引选择等）将会生成原始索引的子集，这样默认的数字索引排序就乱了。如要重新生成连续索引，可以使用`reset_index`方法。

```
>>> df0 = pd.DataFrame(np.random.rand(5, 3), columns=list("ABC"))
>>> df0
          A         B         C
0  0.548012  0.288583  0.734276
1  0.342895  0.207917  0.995485
2  0.378794  0.160913  0.971951
3  0.039738  0.008414  0.226510
4  0.581093  0.750331  0.133022
>>> df1 = df0[df0.index % 2 == 0]
>>> df1
          A         B         C
0  0.548012  0.288583  0.734276
2  0.378794  0.160913  0.971951
4  0.581093  0.750331  0.133022
>>> df1.reset_index(drop=True)
          A         B         C
0  0.548012  0.288583  0.734276
1  0.378794  0.160913  0.971951
2  0.581093  0.750331  0.133022

```

通常，我们是不需要保留旧索引的，因此可将`drop`参数设置为`True`。同样，如果要就地重置索引，可设置`inplace`参数为`True`，否则将创建一个新的 DataFrame。

## 4\. 将索引从 groupby 操作转换为列

`groupby`分组方法是经常用的。比如下面通过添加一个分组列team来进行分组。

```
>>> df0["team"] = ["X", "X", "Y", "Y", "Y"]
>>> df0
          A         B         C team
0  0.548012  0.288583  0.734276    X
1  0.342895  0.207917  0.995485    X
2  0.378794  0.160913  0.971951    Y
3  0.039738  0.008414  0.226510    Y
4  0.581093  0.750331  0.133022    Y
>>> df0.groupby("team").mean()
             A         B         C
team                              
X     0.445453  0.248250  0.864881
Y     0.333208  0.306553  0.443828

```

默认情况下，分组会将分组列编程index索引。但是很多情况下，我们不希望分组列变成索引，因为可能有些计算或者判断逻辑还是需要用到该列的。因此，我们需要设置一下让分组列不成为索引，同时也能完成分组的功能。

有两种方法可以完成所需的操作，第一种是用`reset_index`，第二种是在groupby方法里设置`as_index=False`。个人更喜欢第二种方法，它只涉及两个步骤，更简洁。

```
>>> df0.groupby("team").mean().reset_index()
  team         A         B         C
0    X  0.445453  0.248250  0.864881
1    Y  0.333208  0.306553  0.443828
>>> df0.groupby("team", as_index=False).mean()
  team         A         B         C
0    X  0.445453  0.248250  0.864881
1    Y  0.333208  0.306553  0.443828

```

## 5.排序后重置索引

当用`sort_value`排序方法时也会遇到这个问题，因为默认情况下，索引index跟着排序顺序而变动，所以是乱雪。如果我们希望索引不跟着排序变动，同样需要在`sort_values`方法中设置一下参数`ignore_index`即可。

```
>>> df0.sort_values("A")
          A         B         C team
3  0.039738  0.008414  0.226510    Y
1  0.342895  0.207917  0.995485    X
2  0.378794  0.160913  0.971951    Y
0  0.548012  0.288583  0.734276    X
4  0.581093  0.750331  0.133022    Y
>>> df0.sort_values("A", ignore_index=True)
          A         B         C team
0  0.039738  0.008414  0.226510    Y
1  0.342895  0.207917  0.995485    X
2  0.378794  0.160913  0.971951    Y
3  0.548012  0.288583  0.734276    X
4  0.581093  0.750331  0.133022    Y

```

## 6.删除重复后重置索引

删除重复项和排序一样，默认执行后也会打乱排序顺序。同理，可以在`drop_duplicates`方法中设置`ignore_index`参数`True`即可。

```
>>> df0
          A         B         C team
0  0.548012  0.288583  0.734276    X
1  0.342895  0.207917  0.995485    X
2  0.378794  0.160913  0.971951    Y
3  0.039738  0.008414  0.226510    Y
4  0.581093  0.750331  0.133022    Y
>>> df0.drop_duplicates("team", ignore_index=True)
          A         B         C team
0  0.548012  0.288583  0.734276    X
1  0.378794  0.160913  0.971951    Y

```

## 7\. 索引的直接赋值

当我们有了一个 DataFrame 时，想要使用不同的数据源或单独的操作来分配索引。在这种情况下，可以直接将索引分配给现有的 `df.index`。

```
>>> better_index = ["X1", "X2", "Y1", "Y2", "Y3"]
>>> df0.index = better_index
>>> df0
           A         B         C team
X1  0.548012  0.288583  0.734276    X
X2  0.342895  0.207917  0.995485    X
Y1  0.378794  0.160913  0.971951    Y
Y2  0.039738  0.008414  0.226510    Y
Y3  0.581093  0.750331  0.133022    Y

```

## 8.写入CSV文件时忽略索引

数据导出到 CSV 文件时，默认 DataFrame 具有从 0 开始的索引。如果我们不想在导出的 CSV 文件中包含它，可以在`to_csv`方法中设置`index`参数。

```
>>> df0.to_csv("exported_file.csv", index=False)

```

如下所示，导出的 CSV 文件中，索引列未包含在文件中。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

其实，很多方法中都有关于索引的设置，只不过大家一般比较关心数据，而经常忽略了索引，才导致继续运行时可能会报错。以上几个高频的操作都是有索引设置的，建议大家平时用的时候养成设置索引的习惯，这样会节省不少时间。

**原创不易，欢迎点赞、留言、分享，支持我继续写下去。**

参考：https://towardsdatascience.com/8-quick-tips-on-manipulating-index-with-pandas-c10ef9d1b44f

* * *

我是东哥，最近正在原创👉「[pandas100个骚操作](https://mp.weixin.qq.com/mp/appmsgalbum?__biz=MzUzODYwMDAzNA==&action=getalbum&album_id=1699019347278561282#wechat_redirect)」系列话题，欢迎订阅。订阅后，文章更新可第一时间推送至订阅号，每篇都不错过。

![](../../../_resources/0_wx_fmt_png_44d707ccabd34b90b402144b69f5c755.png)

**Python数据科学**

以Python为核心语言，专攻于「数据科学」领域，文章涵盖数据分析，数据挖掘，机器学习等干货内容，分享大量数据挖掘实战项目分析和讲解，以及海量的学习资源。

<a id="js_profile_article"></a>186篇原创内容

Official Account

**推荐阅读**

[Python 电商数据分析实战：市场营销增长](http://mp.weixin.qq.com/s?__biz=MzUzODYwMDAzNA==&mid=2247541314&idx=1&sn=f19abf8937da16858426c60e69f06f17&chksm=fad7554fcda0dc59da000e22347f83bef1f873cdcd17705f3161c3eb95c0668c7fb7379f66ea&scene=21#wechat_redirect)

[Toad：基于 Python 的标准化评分卡模型](http://mp.weixin.qq.com/s?__biz=MzUzODYwMDAzNA==&mid=2247541238&idx=1&sn=b7f132f77d6355d1de9fa294ad2f58b5&chksm=fad756fbcda0dfed75994865761bb3c36695ae55766e123a5cb83b0c7f80945e521ad9ee7394&scene=21#wechat_redirect)

[整理了 47 个 Python 人工智能库](http://mp.weixin.qq.com/s?__biz=MzUzODYwMDAzNA==&mid=2247541314&idx=2&sn=fd839dd5e843cc49220707bf8458d495&chksm=fad7554fcda0dc59d07d00409a82b6f87ce0fca1439e04d8856b6c23f95924bccf47346f64b6&scene=21#wechat_redirect)

收录于合集 #<a id="js_album_keep_read_title"></a>pandas骚操作

 <a id="js_album_keep_read_size"></a>25个

<a id="js_album_prev"></a>上一篇 <a id="js_album_keep_read_pre_title"></a>好习惯！pandas 8 个常用的 option 设置 <a id="js_album_next"></a>下一篇 <a id="js_album_keep_read_next_title"></a>20 个短小精悍的 pandas 骚操作

People who liked this content also liked

超强图解pandas，建议收藏(文末赠书)

...

Python数据科学

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___06c0108329834ed4a.bmp"/>

Scan to Follow