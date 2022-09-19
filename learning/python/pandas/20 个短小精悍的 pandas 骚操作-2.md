# 20 个短小精悍的 pandas 骚操作

<a id="copyright_logo"></a>Original 东哥起飞 <a id="profileBt"></a><a id="js_name"></a>Python数据科学 *2021-08-06 13:59*

收录于合集

<a id="js_article_tag_name__1516378551799906305"></a>#pandas <a id="js_article_tag_num__1516378551799906305"></a>12 <a id="js_article_tag_tips__1516378551799906305"></a>个

<a id="js_article_tag_name__1699019347278561282"></a>#pandas骚操作 <a id="js_article_tag_num__1699019347278561282"></a>25 <a id="js_article_tag_tips__1699019347278561282"></a>个

大家好，我是东哥啊。

本次为大家整理了一个`pandas`骚操作操作的大集合，共20个功能，个个短小精悍，一次让你爱个够。系列内容，请看👉「[pandas100个骚操作](https://mp.weixin.qq.com/mp/appmsgalbum?__biz=MzUzODYwMDAzNA==&action=getalbum&album_id=1699019347278561282&scene=173&from_msgid=2247515707&from_itemidx=4&count=3&nolastread=1#wechat_redirect)」话题。

另外，最近收到出版社送的一本新书 **《深入浅出pandas》**，内容非常赞，目前已上架各商城。当然，东哥**给大家争取了5本，免费包邮送出去**，参与方式见文末~

## 1\. ExcelWriter

很多时候`dataframe`里面有中文，如果直接输出到csv里，中文将显示乱码。而Excel就不一样了，`ExcelWriter`是`pandas`的一个类，可以使`dataframe`数据框直接输出到excel文件，并可以指定`sheets`名称。

```
df1 = pd.DataFrame([["AAA", "BBB"]], columns=["Spam", "Egg"])
df2 = pd.DataFrame([["ABC", "XYZ"]], columns=["Foo", "Bar"])
with ExcelWriter("path_to_file.xlsx") as writer:
    df1.to_excel(writer, sheet_name="Sheet1")
    df2.to_excel(writer, sheet_name="Sheet2")

```

如果有时间变量，输出时还可以`date_format`指定时间的格式。另外，它还可以通过`mode`设置输出到已有的excel文件中，非常灵活。

```
with ExcelWriter("path_to_file.xlsx", mode="a", engine="openpyxl") as writer:
    df.to_excel(writer, sheet_name="Sheet3")

```

## 2\. pipe

`pipe`管道函数可以将多个自定义函数装进同一个操作里，让整个代码更简洁，更紧凑。

比如，我们在做数据清洗的时候，往往代码会很乱，有去重、去异常值、编码转换等等。如果使用`pipe`，将是这样子的。

```
diamonds = sns.load_dataset("diamonds")
df_preped = (diamonds.pipe(drop_duplicates).
                      pipe(remove_outliers, ['price', 'carat', 'depth']).
                      pipe(encode_categoricals, ['cut', 'color', 'clarity'])
            )

```

两个字，**干净！**

## 3\. factorize

`factorize`这个函数类似`sklearn`中`LabelEncoder`，可以实现同样的功能。

```
# Mind the [0] at the end
diamonds["cut_enc"] = pd.factorize(diamonds["cut"])[0]
>>> diamonds["cut_enc"].sample(5)
52103    2
39813    0
31843    0
10675    0
6634     0
Name: cut_enc, dtype: int64

```

区别是，`factorize`返回一个二值元组：编码的列和唯一分类值的列表。

```
codes, unique = pd.factorize(diamonds["cut"], sort=True)
>>> codes[:10]
array([0, 1, 3, 1, 3, 2, 2, 2, 4, 2], dtype=int64)
>>> unique
['Ideal', 'Premium', 'Very Good', 'Good', 'Fair']

```

## 4\. explode

`explode`爆炸功能，可以将array-like的值比如列表，炸开转换成多行。

```
data = pd.Series([1, 6, 7, [46, 56, 49], 45, [15, 10, 12]]).to_frame("dirty")
data.explode("dirty", ignore_index=True)

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

这部分可以参考我之前写的骚操作系列：[explode 列转行的 2 个常用技巧！](https://mp.weixin.qq.com/s?__biz=MzUzODYwMDAzNA==&mid=2247517310&idx=2&sn=ba2780550178313589ff84d9bbf23816&chksm=fad7f373cda07a65ffca588b5127178d9d3d99aca0e50e1e8969cca49d292a0f718e3877bf33&token=887841574&lang=zh_CN&scene=21#wechat_redirect)

## 5\. squeeze

很多时候，我们用`.loc`筛选想返回一个值，但返回的却是个`series`。其实，只要使用`.squeeze()`即可完美解决。比如：

```
# 没使用squeeze
subset = diamonds.loc[diamonds.index < 1, ["price"]]
# 使用squeeze
subset.squeeze("columns")

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

可以看到，压缩完结果已经是`int64`的格式了，而不再是`series`。这部分可以参考我之前写的骚操作系列：[Squeeze 类型压缩小技巧！](https://mp.weixin.qq.com/s?__biz=MzUzODYwMDAzNA==&mid=2247520408&idx=2&sn=0b49ff18c0f6012a174c6ee3a38135dc&chksm=fad7e795cda06e83ac4632335831f060c13b48afc71436a1eb96a345661b58ec80d2234e4daa&token=887841574&lang=zh_CN&scene=21#wechat_redirect)

## 6\. between

`dataframe`的筛选方法有很多，常见的`loc`、`isin`等等，但其实还有个及其简洁的方法，专门筛选数值范围的，就是`between`，用法很简单。

```
diamonds[diamonds["price"]\
      .between(3500, 3700, inclusive="neither")].sample(5)

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 7\. T

这是所有的`dataframe`都有的一个简单属性，实现**转置**功能。它在显示`describe`时可以很好的搭配。

```
boston.describe().T.head(10)

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 8\. pandas styler

`pandas`也可以像excel一样，设置表格的可视化条件格式，而且只需要一行代码即可（可能需要一丢丢的前端HTML和CSS基础知识）。

```
>>> diabetes.describe().T.drop("count", axis=1)\
                 .style.highlight_max(color="darkred")

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

当然了，条件格式有非常多种。详细的可以参考我之前写的骚操作系列：[一行 pandas 代码搞定 Excel “条件格式”！](https://mp.weixin.qq.com/s?__biz=MzUzODYwMDAzNA==&mid=2247516200&idx=2&sn=f1debad6a2068dd789efe855e678cce3&chksm=fad7f725cda07e3374a6e6903630e87db4e15bb22682122fd7295d2de714dba3cb62789cba83&scene=21&cur_album_id=1699019347278561282#wechat_redirect)

## 9\. Pandas options

`pandas`里提供了很多宏设置选项，被分为下面5大类。

```
dir(pd.options)
['compute', 'display', 'io', 'mode', 'plotting']

```

一般情况下使用`display`会多一点，比如最大、最小显示行数，画图方法，显示精度等等。

```
pd.options.display.max_columns = None
pd.options.display.precision = 5

```

这部分东哥之前进行过总结，可以参考我之前写的骚操作系列：[pandas 8 个常用的 option 设置](https://mp.weixin.qq.com/s?__biz=MzUzODYwMDAzNA==&mid=2247536448&idx=1&sn=a3ebeca4b6323976ba031b8a044b7887&chksm=fad7384dcda0b15b2b74dbe256a25a24704f691e2f5430cc4981aeaf618ba830caf920cdbc89&scene=21&cur_album_id=1699019347278561282#wechat_redirect)

## 10\. convert_dtypes

经常使用`pandas`的都知道，`pandas`对于经常会将变量类型直接变成`object`，导致后续无法正常操作。这种情况可以用`convert_dtypes`进行批量的转换，它会自动推断数据原来的类型，并实现转换。

```
sample = pd.read_csv(
    "data/station_day.csv",
    usecols=["StationId", "CO", "O3", "AQI_Bucket"],
)
>>> sample.dtypes
StationId      object
CO            float64
O3            float64
AQI_Bucket     object
dtype: object
>>> sample.convert_dtypes().dtypes
StationId      string
CO            float64
O3            float64
AQI_Bucket     string
dtype: object

```

可以参考我之前写的骚操作系列：[pandas变量类型自动转换](https://mp.weixin.qq.com/s?__biz=MzUzODYwMDAzNA==&mid=2247515190&idx=5&sn=e243f5dd1b3d5414153f08929659ebdd&chksm=fad7cb3bcda0422d15fd04f9b8fc597e57680dc748ecb467cb3dcff4ed15ef61beca4060b63e&scene=21&cur_album_id=1699019347278561282#wechat_redirect)

## 11\. select_dtypes

在需要筛选变量类型的时候，可以直接用`selec _dtypes`，通过`include`和`exclude`筛选和排除变量的类型。

```
# 选择数值型的变量
diamonds.select_dtypes(include=np.number).head()
# 排除数值型的变量
diamonds.select_dtypes(exclude=np.number).head()

```

## 12\. mask

`mask`可以在自定义条件下快速替换单元值，在很多三方库的源码中经常见到。比如下面我们想让age为50-60以外的单元为空，只需要在`con`和`ohter`写好自定义的条件即可。

```
ages = pd.Series([55, 52, 50, 66, 57, 59, 49, 60]).to_frame("ages")
ages.mask(cond=~ages["ages"].between(50, 60), other=np.nan)

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 13\. 列轴的min、max

虽然大家都知道`min`和`max`的功能，但应用在列上的应该不多见。这对函数其实还可以这么用：

```
index = ["Diamonds", "Titanic", "Iris", "Heart Disease", "Loan Default"]
libraries = ["XGBoost", "CatBoost", "LightGBM", "Sklearn GB"]
df = pd.DataFrame(
    {lib: np.random.uniform(90, 100, 5) for lib in libraries}, index=index
)
>>> df

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

```
>>> df.max(axis=1)
Diamonds         99.52684
Titanic          99.63650
Iris             99.10989
Heart Disease    99.31627
Loan Default     97.96728
dtype: float64

```

## 14\. nlargest、nsmallest

有时我们不仅想要列的最小值/最大值，还想看变量的前 N 个或 `~(top N)` 个值。这时`nlargest`和`nsmallest`就派上用场了。

```
diamonds.nlargest(5, "price")

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 15\. idmax、idxmin

我们用列轴使用`max`或`min`时，`pandas` 会返回最大/最小的值。但我现在不需要具体的值了，我需要这个最大值的位置。因为很多时候要锁定位置之后对整个行进行操作，比如单提出来或者删除等，所以这种需求还是很常见的。

使用`idxmax`和`idxmin`即可解决。

```
>>> diamonds.price.idxmax()
27749
>>> diamonds.carat.idxmin()
14

```

## 16\. value_counts

在数据探索的时候，`value_counts`是使用很频繁的函数，它默认是不统计空值的，但空值往往也是我们很关心的。如果想统计空值，可以将参数`dropna`设置为`False`。

```
ames_housing = pd.read_csv("data/train.csv")
>>> ames_housing["FireplaceQu"].value_counts(dropna=False, normalize=True)
NaN    0.47260
Gd     0.26027
TA     0.21438
Fa     0.02260
Ex     0.01644
Po     0.01370
Name: FireplaceQu, dtype: float64

```

## 17\. clip

异常值检测是数据分析中常见的操作。使用`clip`函数可以很容易地找到变量范围之外的异常值，并替换它们。

```
>>> age.clip(50, 60)

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 18\. at\_time、between\_time

在有时间粒度比较细的时候，这两个函数超级有用。因为它们可以进行更细化的操作，比如筛选某个时点，或者某个范围时间等，可以细化到小时分钟。

```
>>> data.at_time("15:00")

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

```
from datetime import datetime
>>> data.between_time("09:45", "12:00")

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 19\. hasnans

`pandas`提供了一种快速方法`hasnans`来检查给定series是否包含空值。

```
series = pd.Series([2, 4, 6, "sadf", np.nan])
>>> series.hasnans
True

```

该方法只适用于`series`的结构。

## 20\. GroupBy.nth

此功能仅适用于`GroupBy`对象。具体来说，分组后，`nth`返回每组的第n行：

```
>>> diamonds.groupby("cut").nth(5)

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

**原创不易，欢迎点赞、留言、分享，支持我继续写下去。**

参考：

\[1\] https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.ExcelWriter.html

\[2\] https://towardsdatascience.com/25-pandas-functions-you-didnt-know-existed-p-guarantee-0-8-1a05dcaad5d0

**赠书福利**

**书籍：**赠送 **5** 本《**深入浅出pandas：利用Python进行数据处理与分析**》，由「**机械工业出版社华****章公司**」赞助提供，pandas学习的经典之作 ，超赞！推荐入手一本。更多介绍和目录可以点击下面链接了解。

**赠送规则：**「**转发本文至朋友圈**」+「**留言**」，文章内容相关的优质留言或感想才可上墙。留言点赞数量最多**前5位**读者将获得一本。

**开奖****时间：**「**8月8日20:00」**

**领书须知：**提供转发截图+ 集赞截图

**注意事项：**禁止刷赞，发现后将进入黑名单，取消上墙资格。最终获赠者请在24小时以内添加我的微信，备注：赠书👇

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

```


爱点赞的人，运气都不会太差![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)


```

收录于合集 #<a id="js_album_keep_read_title"></a>pandas骚操作

 <a id="js_album_keep_read_size"></a>25个

<a id="js_album_prev"></a>上一篇 <a id="js_album_keep_read_pre_title"></a>好习惯！pandas 8 个常用的 index 设置 <a id="js_album_next"></a>下一篇 <a id="js_album_keep_read_next_title"></a>pandas 筛选数据的 8 个骚操作

People who liked this content also liked

超强图解pandas，建议收藏(文末赠书)

...

Python数据科学

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___a3ae4a604c234eae8.bmp"/>

Scan to Follow