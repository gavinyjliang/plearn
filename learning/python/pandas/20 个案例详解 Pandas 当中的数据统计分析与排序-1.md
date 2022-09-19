# 20 个案例详解 Pandas 当中的数据统计分析与排序

<a id="profileBt"></a><a id="js_name"></a>数据分析与开发 *2022-02-17 12:10*

The following article is from 关于数据分析与可视化 Author 俊欣

<a id="copyright_info"></a>[![](../../../_resources/0_daec127ffe884ed2869a7d0696f5dfdc.jpg)<br>**关于数据分析与可视化** .<br>本公众号定期分享数据分析与可视化干货文章，并有时结合热点话题进行深入讨论，希望您会喜欢，要是哪里写的不好，也渴望倾听您的想法和意见，感谢！❤️](#)

今天给大家讲一下`Pandas`模块当中的数据统计与排序，说到具体的就是`value_counts()`方法以及`sort_values()`方法。

`value_counts()`方法，顾名思义，主要是用于计算各个类别出现的次数的，而`sort_values()`方法则是对数值来进行排序，当然除了这些，还有很多大家不知道的衍生的功能等待被挖掘，下面就带大家一个一个的说过去。

### 导入模块并且读取数据库

我们这次用到的数据集是“非常有名”的泰坦尼克号的数据集，该数据源能够在很多平台上都能够找得到

```
`import pandas as pd
df = pd.read_csv("titanic_train.csv")
df.head()
`
```

output

<img width="618" height="102" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__2a2fd37e02824e04b.png"/>

### 常规的用法

首先我们来看一下常规的用法，代码如下

```
`df['Embarked'].value_counts()
`
```

output

```
`S    644
C    168
Q     77
Name: Embarked, dtype: int64
`
```

下面我们简单来介绍一下`value_counts()`方法当中的参数，

```
`DataFrame.value_counts(subset=None,
                       normalize=False,
                       sort=True,
                       ascending=False,
                       dropna=True)
`
```

常用到参数的具体解释为：

- subset: 表示根据什么字段或者索引来进行统计分析
    
- normalize: 返回的是比例而不是频次
    
- ascending: 降序还是升序来排
    
- dropna: 是否需要包含有空值的行
    

### 对数值进行排序

上面返回的结果是按照从大到小来进行排序的，当然我们也可以反过来，从小到大来进行排序，代码如下

```
`df['Embarked'].value_counts(ascending=True)
`
```

output

```
`Q     77
C    168
S    644
Name: Embarked, dtype: int64
`
```

### 对索引的字母进行排序

同时我们也可以对索引，按照字母表的顺序来进行排序，代码如下

```
`df['Embarked'].value_counts(ascending=True).sort_index(ascending=True)
`
```

output

```
`C    168
Q     77
S    644
Name: Embarked, dtype: int64
`
```

当中的`ascending=True`指的是升序排序

### 包含对空值的统计

默认的是`value_counts()`方法不会对空值进行统计，那要是我们也希望对空值进行统计的话，就可以加上`dropna`参数，代码如下

```
`df['Embarked'].value_counts(dropna=False)
`
```

output

```
`S      644
C      168
Q       77
NaN      2
Name: Embarked, dtype: int64
`
```

### 百分比式的数据统计

我们可以将数值的统计转化成百分比式的统计，可以更加直观地看到每一个类别的占比，代码如下

```
`df['Embarked'].value_counts(normalize=True)
`
```

output

```
`S    0.724409
C    0.188976
Q    0.086614
Name: Embarked, dtype: float64
`
```

要是我们希望对能够在后面加上一个百分比的符号，则需要在`Pandas`中加以设置，对数据的展示加以设置，代码如下

```
`pd.set_option('display.float_format', '{:.2%}'.format)
df['Embarked'].value_counts(normalize = True)
`
```

output

```
`S   72.44%
C   18.90%
Q    8.66%
Name: Embarked, dtype: float64
`
```

当然除此之外，我们还可以这么来做，代码如下

```
`df['Embarked'].value_counts(normalize = True).to_frame().style.format('{:.2%}')
`
```

output

```
`  Embarked
S 72.44%
C 18.90%
Q 8.66%
`
```

### 连续型数据分箱

和`Pandas`模块当中的`cut()`方法相类似的在于，我们这里也可以将连续型数据进行分箱然后再来统计，代码如下

```
`df['Fare'].value_counts(bins=3)
`
```

output

```
`(-0.513, 170.776]     871
(170.776, 341.553]     17
(341.553, 512.329]      3
Name: Fare, dtype: int64
`
```

我们将`Fare`这一列同等份的分成3组然后再来进行统计，当然我们也可以自定义每一个分组的上限与下限，代码如下

```
`df['Fare'].value_counts(bins=[-1, 20, 100, 550])
`
```

output

```
`(-1.001, 20.0]    515
(20.0, 100.0]     323
(100.0, 550.0]     53
Name: Fare, dtype: int64
`
```

### 分组再统计

`pandas`模块当中的`groupby()`方法允许对数据集进行分组，它也可以和`value_counts()`方法联用更好地来进行统计分析，代码如下

```
`df.groupby('Embarked')['Sex'].value_counts()
`
```

output

```
`Embarked  Sex   
C         male       95
          female     73
Q         male       41
          female     36
S         male      441
          female    203
Name: Sex, dtype: int64
`
```

上面的代码是针对“Embarked”这一类别下的“Sex”特征进行分组，然后再进一步进行数据的统计分析，当然出来的结果是`Series`数据结构，要是我们想让`Series`的数据结果编程`DataFrame`数据结构，可以这么来做，

```
`df.groupby('Embarked')['Sex'].value_counts().to_frame()`
```

### 数据集的排序

下面我们来谈一下数据的排序，主要用到的是`sort_values()`方法，例如我们根据“年龄”这一列来进行排序，排序的方式为降序排，代码如下

```
`df.sort_values("Age", ascending = False).head(10)
`
```

output

<img width="617" height="202" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__88654743369d408d9.png"/>

### 对行索引重新排序

我们看到排序过之后的`DataFrame`数据集行索引依然没有变，我们希望行索引依然可以是从0开始依次的递增，就可以这么来做，代码如下

```
`df.sort_values("Age", ascending = False, ignore_index = True).head(10)
`
```

output

<img width="617" height="207" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__627595cf133d48128.png"/>

下面我们简单来介绍一下`sort_values()`方法当中的参数

```
`DataFrame.sort_values(by, 
               axis=0, 
               ascending=True, 
               inplace=False, 
               kind='quicksort', 
               na_position='last', # last，first；默认是last
               ignore_index=False, 
               key=None)
`
```

常用到参数的具体解释为：

- by: 表示根据什么字段或者索引来进行排序，可以是一个或者是多个
    
- axis: 是水平方向排序还是垂直方向排序，默认是垂直方向
    
- ascending: 排序方式，是升序还是降序来排
    
- inplace: 是生成新的`DataFrame`还是在原有的基础上进行修改
    
- kind: 所用到的排序的算法，有快排quicksort或者是归并排序mergesort、堆排序heapsort等等
    
- ignore_index: 是否对行索引进行重新的排序
    

### 对多个字段的排序

我们还可以对多个字段进行排序，代码如下

```
`df.sort_values(["Age", "Fare"], ascending = False).head(10)
`
```

output

<img width="617" height="211" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__1bc05006b513451db.png"/>

同时我们也可以对不同的字段指定不同的排序方式，如下

```
`df.sort_values(["Age", "Fare"], ascending = [False, True]).head(10)
`
```

output

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

我们可以看到在“Age”一样的情况下，“Fare”字段是按照升序的顺序来排的

### 自定义排序

我们可以自定义一个函数方法，然后运用在`sort_values()`方法当中，让其按照自己写的方法来排序，我们看如下的这组数据

```
`df = pd.DataFrame({
    'product': ['keyboard', 'mouse', 'desk', 'monitor', 'chair'],
    'category': ['C', 'C', 'O', 'C', 'O'],
    'year': [2002, 2002, 2005, 2001, 2003],
    'cost': ['$52', '$24', '$250', '$500', '$150'],
    'promotion_time': ['20hr', '30hr', '20hr', '20hr', '2hr'],
})
`
```

output

<img width="617" height="327" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__5ca219467bfc45dd8.png"/>

当中的“cost”这一列带有美元符号“$”，因此就会干扰排序的正常进行，我们使用`lambda`方法自定义一个函数方法运用在`sort_value()`当中

```
`df.sort_values(
    'cost', 
    key=lambda val: val.str.replace('$', '').astype('float64')
)
`
```

output

<img width="617" height="286" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__20ee7ed44ff140a19.png"/>

当然我们还可以自定义一个更加复杂一点的函数，并且运用在`sort_values()`方法当中，代码如下

```
`def sort_by_cost_time(x):
    if x.name == 'cost':
        return x.str.replace('$', '').astype('float64')
    elif x.name == 'promotion_time':
        return x.str.replace('hr', '').astype('int')
    else:
        return x
        
df.sort_values(
   ['year', 'promotion_time', 'cost'], 
   key=sort_by_cost_time
)
`
```

output

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

还有另外一种情况，例如我们遇到衣服的尺码，`XS`码、`S`码、`M`码、`L`码又或者是月份，`Jan`、`Feb`、`Mar`、`Apr`等等，需要我们自己去定义大小，这个时候我们需要用到的是`CategoricalDtype`

```
`cat_size_order = CategoricalDtype(
    ['XS', 'S', 'M', 'L', 'XL'], 
    ordered=True
)
cat_size_order
`
```

output

```
`CategoricalDtype(categories=['XS', 'S', 'M', 'L', 'XL'], ordered=True)
`
```

于是针对下面的数据

```
`df = pd.DataFrame({
    'cloth_id': [1001, 1002, 1003, 1004, 1005, 1006],
    'size': ['S', 'XL', 'M', 'XS', 'L', 'S'],
})
`
```

output

<img width="251" height="234" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__d4c997015640464b9.png"/>

我们将事先定义好的顺序应用到该数据集当中，代码如下

```
`df['size'] = df['size'].astype(cat_size_order)
df.sort_values('size')
`
```

output

<img width="268" height="258" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__dfb04bf2e3a74a889.png"/>

先通过`astype()`来转换数据类型，然后再进行排序

\- EOF -

推荐阅读  点击标题可跳转

<ins>1、[Pandas 缺失数据处理大全（附代码）](http://mp.weixin.qq.com/s?__biz=MzA5ODM5MDU3MA==&mid=2650881301&idx=2&sn=9a15270ef6ee99bdfbc8ce0a68feaacd&chksm=8b67da50bc105346a162d0611c3a9b84cd5073859e76828f14aa9ddc402856173beca96b49e4&scene=21#wechat_redirect)</ins>

<ins>2、[pandas 与 lambda 完美结合指南](http://mp.weixin.qq.com/s?__biz=MzA5ODM5MDU3MA==&mid=2650881097&idx=2&sn=5369b5f82c7b05f971f961a0e453ac3a&chksm=8b67db0cbc10521a6fdf9013241fc7cca377b5414632dc8a3177e3b2394162607639bc47e547&scene=21#wechat_redirect)</ins>

<ins>3、[10000 字的 pandas 核心操作知识大全！](http://mp.weixin.qq.com/s?__biz=MzA5ODM5MDU3MA==&mid=2650880984&idx=2&sn=86226abb54d2c7798691da674cba1938&chksm=8b67dc9dbc10558b0c280cb506707bb3a2aa9e7435541e4dc0a804e999a6946b20d89aa726ab&scene=21#wechat_redirect)</ins>

看完本文有收获？请转发分享给更多人

**推荐关注「数据分析与开发」，提升数据技能**

![](../../../_resources/0_wx_fmt_png_fc53a7663d334774b23c90343fc73f98.png)

**数据分析与开发**

「数据分析与开发」分享数据分析与开发相关技术文章、教程、工具

<a id="js_profile_article"></a>55篇原创内容

Official Account

点赞和在看就是最大的支持❤️

People who liked this content also liked

有了这篇 Docker 网络原理，彻底爱了~

高效运维

不看的原因

- 内容质量低
- 不看此公众号

告诉你怎么创建pandas数据框架（dataframe）

完美Excel

不看的原因

- 内容质量低
- 不看此公众号

12 个优化 Docker 镜像安全性的技巧

InfoQ

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___5dea4860d91f41ae9.bmp"/>

Scan to Follow