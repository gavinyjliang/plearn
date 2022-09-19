# pandas 与 lambda 完美结合指南

<a id="profileBt"></a><a id="js_name"></a>数据分析与开发 *2022-02-13 20:00*

The following article is from 关于数据分析与可视化 Author 俊欣

<a id="copyright_info"></a>[![](../../../_resources/0_03ada6e1d718479ca949764a48de0401.jpg)<br>**关于数据分析与可视化** .<br>本公众号定期分享数据分析与可视化干货文章，并有时结合热点话题进行深入讨论，希望您会喜欢，要是哪里写的不好，也渴望倾听您的想法和意见，感谢！❤️](#)

这篇文章来讲讲`lambda`方法以及它在`pandas`模块当中的运用，熟练掌握可以极大地提高数据分析与挖掘的效率

### 导入模块与读取数据

我们第一步需要导入模块以及数据集

```
`import pandas as pd
df = pd.read_csv("IMDB-Movie-Data.csv")
df.head()
`
```

### 创建新的列

一般我们是通过在现有两列的基础上进行一些简单的数学运算来创建新的一列，例如

```
`df['AvgRating'] = (df['Rating'] + df['Metascore']/10)/2
`
```

但是如果要新创建的列是经过相当复杂的计算得来的，那么`lambda`方法就很多必要被运用到了，我们先来定义一个函数方法

```
`def custom_rating(genre,rating):
    if 'Thriller' in genre:
        return min(10,rating+1)
    elif 'Comedy' in genre:
        return max(0,rating-1)
    elif 'Drama' in genre:
        return max(5, rating-1)
    else:
        return rating
`
```

我们对于不同类别的电影采用了不同方式的评分方法，例如对于“**惊悚片**”，评分的方法则是在“**原来的评分+1**”和10分当中**取一个最小的**，而对于“**喜剧**”类别的电影，则是在0分和“**原来的评分-1**”当中**取一个最大的**，然后我们通过`apply`方法和`lambda`方法将这个自定义的函数应用在这个`DataFrame`数据集当中

```
`df["CustomRating"] = df.apply(lambda x: custom_rating(x['Genre'], x['Rating']), axis = 1)
`
```

我们这里需要说明一下`axis`参数的作用，其中`axis=1`代表跨列而`axis=0`代表跨行，如下图所示

<img width="618" height="318" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__66d7ad0c798b456a8.png"/>

### 筛选数据

在`pandas`当中筛选数据相对来说比较容易，可以用到`& | ~`这些操作符，代码如下

```
`# 单个条件，评分大于5分的
df_gt_5 = df[df['Rating']>5]
# 多个条件: AND - 同时满足评分高于5分并且投票大于100000的
And_df = df[(df['Rating']>5) & (df['Votes']>100000)]
# 多个条件: OR - 满足评分高于5分或者投票大于100000的
Or_df = df[(df['Rating']>5) | (df['Votes']>100000)]
# 多个条件：NOT - 将满足评分高于5分或者投票大于100000的数据排除掉
Not_df = df[~((df['Rating']>5) | (df['Votes']>100000))]
`
```

这些都是非常简单并且是常见的例子，但是要是我们想要筛选出`电影的影名长度大于5`的部分，要是也采用上面的方式就会报错

```
`df[len(df['Title'].split(" "))>=5]
`
```

output

```
`AttributeError: 'Series' object has no attribute 'split'
`
```

这里我们还是采用`apply`和`lambda`相结合，来实现上面的功能

```
`#创建一个新的列来存储每一影片名的长度
df['num_words_title'] = df.apply(lambda x : len(x['Title'].split(" ")),axis=1)
#筛选出影片名长度大于5的部分
new_df = df[df['num_words_title']>=5]
`
```

当然要是大家觉得上面的方法有点繁琐的话，也可以一步到位

```
`new_df = df[df.apply(lambda x : len(x['Title'].split(" "))>=5,axis=1)]
`
```

例如我们想要**筛选出那些影片的票房低于当年平均水平的数据**，可以这么来做。

我们先要对**每年票房的的平均值**做一个归总，代码如下

```
`year_revenue_dict = df.groupby(['Year']).agg({'Revenue(Millions)':np.mean}).to_dict()['Revenue(Millions)']
`
```

然后我们定义一个函数来判断**是否存在该影片的票房低于当年平均水平**的情况，返回的是布尔值

```
def bool_provider(revenue, year):
    return revenue<year_revenue_dict[year]

```

然后我们通过结合`apply`方法和`lambda`方法应用到数据集当中去

```
`new_df = df[df.apply(lambda x : bool_provider(x['Revenue(Millions)'],
                                              x['Year']),axis=1)]
`
```

我们筛选数据的时候，主要是用`.loc`方法，它同时也可以和`lambda`方法联用，例如我们想要筛选出**评分在5-8分之间的电影以及它们的票房**，代码如下

```
`df.loc[lambda x: (x["Rating"] > 5) & (x["Rating"] < 8)][["Title", "Revenue (Millions)"]]
`
```

### 转变指定列的数据类型

通常我们转变指定列的数据类型，都是调用`astype`方法来实现的，例如我们将“Price”这一列的数据类型**转变成整型**的数据，代码如下

```
`df['Price'].astype('int')
`
```

会出现如下所示的报错信息

```
`ValueError: invalid literal for int() with base 10: '12,000'
`
```

因此当出现类似“12,000”的数据的时候，调用`astype`方法实现数据类型转换就会报错，因此我们还需要将到`apply`和`lambda`结合进行数据的清洗，代码如下

```
`df['Price'] = df.apply(lambda x: int(x['Price'].replace(',', '')),axis=1)
`
```

### 方法调用过程的可视化

有时候我们在处理数据集比较大的时候，调用函数方法需要比较长的时间，这个时候就需要有一个要是有一个**进度条**，时时刻刻向我们展示数据处理的进度，就会直观很多了。

这里用到的是`tqdm`模块，我们将其导入进来

```
`from tqdm import tqdm, tqdm_notebook
tqdm_notebook().pandas()
`
```

然后将`apply`方法替换成`progress_apply`即可，代码如下

```
`df["CustomRating"] = df.progress_apply(lambda x: custom_rating(x['Genre'],x['Rating']),axis=1)
`
```

output

<img width="618" height="64" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__615c1e69d84f4655b.png"/>

### 当`lambda`方法遇到`if-else`

当然我们也可以将`if-else`运用在`lambda`自定义函数当中，代码如下

```
`Bigger = lambda x, y : x if(x > y) else y
Bigger(2, 10)
`
```

output

```
`10
`
```

当然很多时候我们可能有多组`if-else`，这样写起来就有点麻烦了，代码如下

```
`df['Rating'].apply(lambda x:"低分电影" if x < 3 else ("中等电影" if x>=3 and x < 5 else("高分电影" if x>=8 else "值得观看")))
`
```

看上去稍微有点凌乱了，这个时候，这里还是推荐大家自定义函数，然后通过`apply`和`lambda`方法搭配使用。

\- EOF -

推荐阅读  点击标题可跳转

<ins>1、[详解 20 个 pandas 读与写函数！](http://mp.weixin.qq.com/s?__biz=MzA5ODM5MDU3MA==&mid=2650879973&idx=2&sn=96e85755f3f0e3934c6c0afc78b7b891&chksm=8b67d0a0bc1059b684e114957e5342ee6f4cc0c861ad04b657bc97ed02e47778c4fbd072a000&scene=21#wechat_redirect)</ins>

<ins>2、[20 个短小精悍的 pandas 骚操作](http://mp.weixin.qq.com/s?__biz=MzA5ODM5MDU3MA==&mid=2650878611&idx=2&sn=74bab894acc2518efadaa15b8c8e9666&chksm=8b67d5d6bc105cc084f5d068edd05a63259afc7c971a8b7bfb503032ccbb9ce60eb04d51267b&scene=21#wechat_redirect)</ins>

<ins>3、[8000 字，Pandas 必知必会 50 例](http://mp.weixin.qq.com/s?__biz=MzA5ODM5MDU3MA==&mid=2650877876&idx=2&sn=7dfc677948b5ed0b7822d296cd2c451e&chksm=8b67c8f1bc1041e745bee78f96ad436b6552fd8e06500cb8c8fb3e8fc21b18709f93ebe1a585&scene=21#wechat_redirect)</ins>

看完本文有收获？请转发分享给更多人

**推荐关注「数据分析与开发」，提升数据技能**

![](../../../_resources/0_wx_fmt_png_75f3688a099c404abed3b824576a4946.png)

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

C函数指针别再停留在语法，得上升到软件设计~

嵌入式资讯精选

不看的原因

- 内容质量低
- 不看此公众号

详解 seaborn，快速实现统计数据可视化

大邓和他的Python

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___ad67fad7f7b44b4c9.bmp"/>

Scan to Follow