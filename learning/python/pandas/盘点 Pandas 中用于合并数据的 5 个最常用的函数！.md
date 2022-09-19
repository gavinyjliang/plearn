# 盘点 Pandas 中用于合并数据的 5 个最常用的函数！

<a id="profileBt"></a><a id="js_name"></a>数据分析与开发 *2021-12-08 12:10*

The following article is from 凹凸数据 Author 阿南

<a id="copyright_info"></a>[![](../../../_resources/0_df8d515d6020468eb03dfcd31c0a52a9.jpg)<br>**凹凸数据** .<br>一个不务正业的数据🐶！爬虫、数据分析、可视化、方法论，一条龙服务！业务范围：Python、SQL、Excel、Tableau······](#)

如何在Pandas合并数据，大家肯定都不陌生。

作为一个初学者，我发现自己学了很多，却没有好好总结一下。正好看到一位大佬 *Yong Cui* 总结的文章，我就按照他的方法，给大家分享用于Pandas中合并数据的 5 个最常用的函数。这样大家以后就可以了解它们的差异，并正确使用它们了。

<img width="677" height="407" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__effa0ef4353143308.png"/>

在文章开始之前，我们需要创建两个简单的 DataFrame 对象。

```
import pandas as pd
df0 = pd.DataFrame({"a": [1, 2, 3], "b": [4, 5, 6]})
df1 = pd.DataFrame({"c": [2, 3, 4], "d": [5, 6, 7]})

```

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__578ea3d3a8aa43d5a.png)

## 1、concat

concat 函数字面就是就是连接的意思，它可以帮我们横向或者纵向合并数据。

当你纵向合并数据时，需要将轴axis指定为0，这实际上也是默认值。

```
pd.concat([df0,
           df1.rename(columns={"c": "a", "d": "b"})],
          axis=0)

```

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__85be81cd97a0484b9.png)

当你横向合并数据时，具体操作如下所示。

```
pd.concat([df0, df1], axis=1)

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

默认情况下，当我们横向合并数据（沿列）时，Pandas其实是按照索引来连接的。当两者的索引不相同时，就会用 NaN 填充不重叠的，举个例子如下所示。

```
df2 = df1.copy() 
df2.index = [1, 2, 3]
pd.concat([df0, df2], axis=1) 

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

这只是个小例子，如果希望它们不受索引的影响，可以先重置索引再执行concat连接。

```
pd.concat([df0.reset_index(drop=True),
           df2.reset_index(drop=True)],
          axis=1)

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

重置索引后，df0 和 df2 的索引就变得一致了。

## 2、join

与 concat 对比，join 专门用于使用索引连接 DataFrame 对象之间的列。

```
df0.join(df1)

```

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__951835d1c9cd42079.png)

当索引不同时，join连接默认保留来自左侧 DataFrame 的行。右侧 DF 中没有左侧 DF 中匹配索引的行，会被删除，如下所示：

```
df0.join(df2)

```

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__e845c8252ee9445b8.png)

此外，还可以设置 how 参数，这点与SQL的语法一致。

```
# 右连接，使用 df2 的索引
df0.join(df2, how="right")

```

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__e64b70a247f440709.png)

```
# "outer" 外连接
df0.join(df2, how="outer")

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

```
# "inner" 内连接（交集）
df0.join(df2, how="inner")

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 3、merge

与join相比，merge更通用，它可以对列和索引执行合并操作。

基于列的合并，可以这样操作。

```
df0.merge(df1.rename(columns={"c": "a"}),
          on="a", how="inner") 

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

on 参数定义两个 DataFrame 对象将合并到哪些列。当然，也可以分别指定左侧 DataFrame 和右侧 DataFrame 的合并列，如下所示。

```
df0.merge(df1, left_on="a", right_on="c")

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

除了 a 和 c 的单独列之外，它的结果与之前的合并几乎相同。这里，额外提两个特殊参数：笛卡尔积、使用后缀。

### 笛卡尔积

how 参数设置为`cross`，构成笛卡尔积。是指两个数据框中的数据交叉匹配，出现`n1*n2`的数据量，具体如下所示。

```
df0.merge(df1, how="cross") 

```

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__4f346b8afc8243678.png)

### 使用后缀

当两个 DataFrame 对象有同名的列，且想保持同时存在，就需要添加后缀来重命名这两列。默认情况下，左右数据框的后缀是“\_x”和“\_y”，我们还可以通过suffixes参数自定义设置。

```
df0.merge(df1.rename(columns={"c": "a", "d": "b"}),
          on="a", how="outer", suffixes=("_l", "_r"))

```

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__df633649639749b68.png)

## 4、combine

combine 函数在两个 DataFrame 对象之间执行按列合并，它与之前的方法还是有很大不同的。combine 的特殊之处，在于它接受一个函数参数。此函数采用两个系列，每个系列对应于每个 DataFrame 中的合并列，并返回一个系列作为相同列的元素操作的最终值。听起来很混乱？

让我们举个例子，看一下：

```
def taking_larger_square(s1, s2):
    return s1 * s1 if s1.sum() > s2.sum() else s2 * s2
df0.combine(df1.rename(columns={"c": "a", "d": "b"}), taking_larger_square)

```

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__efee9c4a95194dd08.png)

自定义的 `take_larger_square` 函数对 df0 和 df1 中的 a 列以及 df0 和 df1 中的 b 列进行操作。在两列 a 和两列 b 之间，`taking_larger_square` 取较大列中值的平方。

在这种情况下，df1 的 a 列和 b 列将作为平方，产生最终值，如上面的代码片段所示

## 5、append

回顾前文，我们讨论的大多数操作都是针对按列来合并数据。

如果按行合并（纵向）该如何操作呢？append 函数专门用于将行附加到现有 DataFrame 对象，创建一个新对象。我们先来看一个例子。

```
df0.append(df1.rename(columns={"c": "a", "d": "b"}))

```

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__798601b8625d4abea.png)

上面的操作是不是很眼熟？就跟第一个方法concat的实现效果一致。

不过除了逐行拼接DataFrame，append还可以附加 dict 字典对象，这种方法更加灵活，具体如下所示：

```
df0.append({"a": 1, "b": 2}, ignore_index=True)

```

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__31464ea817d74c64b.png)

上面显示了一个简单的例子。请注意，您必须将 ignore_index 设置为 True，因为字典对象没有 DataFrame 可以使用的索引信息。

## 小结

总结一下，我们今天重新学习了 Pandas 中用于合并数据的 5 个最常用的函数。他们分别是：

- concat<sup>\[1\]</sup>：按行和按列 合并数据；
    
- join<sup>\[2\]</sup>：使用索引按行合 并数据；
    
- merge<sup>\[3\]</sup>：按列合并数据，如数据库连接操作；
    
- combine<sup>\[4\]</sup>：按列合并数据，具有列间（相同列）元素操作；
    
- append<sup>\[5\]</sup>：以DataFrame或dict对象的形式逐行追加数据。
    

### 参考资料

\[1\]

concat: https://pandas.pydata.org/docs/reference/api/pandas.concat.html

\[2\]

join: https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.join.html#pandas.DataFrame.join

\[3\]

merge: https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.merge.html#pandas.DataFrame.merge

\[4\]

combine: https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.combine.html

\[5\]

append: https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.append.html#pandas.DataFrame.append

\- EOF -

推荐阅读  点击标题可跳转

<ins>1、[安利 3 个 pandas 数据探索分析神器！](http://mp.weixin.qq.com/s?__biz=MzA5ODM5MDU3MA==&mid=2650875999&idx=2&sn=3a975505aa0f912d063a0a2cddc30d43&chksm=8b67cf1abc10460cdc9e812db9bf840fc8666813ea427404a8666d2b7662b7c97cfb490a3e86&scene=21#wechat_redirect)</ins>

<ins>2、[吹爆这个 pandas GUI 神器，自动转代码！](http://mp.weixin.qq.com/s?__biz=MzA5ODM5MDU3MA==&mid=2650875939&idx=2&sn=7fb6e503f956dea7cf77dd69cd81780d&chksm=8b67cf66bc104670d72d6732e99bfed63121376405cdb1a415045fd6816a6ad7ac43824cae20&scene=21#wechat_redirect)</ins>

<ins>3、[pandas 与 GUI 界面的超强结合，爆赞！](http://mp.weixin.qq.com/s?__biz=MzA5ODM5MDU3MA==&mid=2650875264&idx=2&sn=a7a19c59d6ad73cb744d2956c8deb0c7&chksm=8b67c2c5bc104bd3e398c631bada296a31459de372fa21762ba13282515208ac42d85827ba82&scene=21#wechat_redirect)</ins>

看完本文有收获？请转发分享给更多人

**推荐关注「数据分析与开发」，提升数据技能**

![](../../../_resources/0_wx_fmt_png_36e5cb29d137438cb5f5567aadca148b.png)

**数据分析与开发**

「数据分析与开发」分享数据分析与开发相关技术文章、教程、工具

<a id="js_profile_article"></a>55篇原创内容

Official Account

点赞和在看就是最大的支持❤️

People who liked this content also liked

pandas 文本处理大全（附代码）

Python数据科学

不看的原因

- 内容质量低
- 不看此公众号

如何使用 Pandas 进行类似 R 的数据操作？

Linux迷

不看的原因

- 内容质量低
- 不看此公众号

使用 Kafka 和动态数据网格进行流式数据交换

小哈学Java

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___c016139d3a1242c48.bmp"/>

Scan to Follow