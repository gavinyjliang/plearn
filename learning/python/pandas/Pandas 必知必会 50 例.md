# Pandas 必知必会 50 例

<a id="profileBt"></a><a id="js_name"></a>数据STUDIO *2021-11-13 08:35*

The following article is from 关于数据分析与可视化 Author 俊欣

<a id="copyright_info"></a>[![](../../../_resources/0_35a012cee41c499b9cbaf2abd1e71210.jpg)<br>**关于数据分析与可视化** .<br>本公众号定期分享数据分析与可视化干货文章，并有时结合热点话题进行深入讨论，希望您会喜欢，要是哪里写的不好，也渴望倾听您的想法和意见，感谢！❤️](#)

大家好，我是云朵君！关注**『数据STUDIO』**，和云朵君一起学习数据分析与挖掘！

今天云朵君会介绍`pandas库`当中一些非常基础的方法与函数，希望大家看了之后会有所收获，另外呢，大家有什么想法可以**在评论区留言**，云朵君第一时间看到就会回复。

![](../../../_resources/0_wx_fmt_png_e67cdc365c8f4017b59060dbe1181ad0.png)

**数据STUDIO**

点击领取《Python学习手册》，后台回复「福利」获取。『数据STUDIO』专注于数据科学原创文章分享，内容以 Python 为核心语言，涵盖机器学习、数据分析、可视化、MySQL等领域干货知识总结及实战项目。

<a id="js_profile_article"></a>173篇原创内容

Official Account

👆点击关注｜设为星标｜干货速递👆

* * *

### 准备需要的数据集

我们先准备生成一些随机数，作为后面需要用到的数据集

```
`index = pd.date_range("1/1/2000", periods=8)
series = pd.Series(np.random.randn(5), index=["a", "b", "c", "d", "e"])
df = pd.DataFrame(np.random.randn(8, 3), index=index, columns=["A", "B", "C"])
`
```

### Head and tail

`head()`和`tail()`方法是用来查看数据集当中的前几行和末尾几行的，默认是查看5行，当然读者朋友也可以自行设定行数

```
`series2 = pd.Series(np.random.randn(100))
series2.head()
`
```

output

```
`0    0.733801
1   -0.740149
2   -0.031863
3    2.515542
4    0.615291
dtype: float64
`
```

同理

```
`series2.tail()
`
```

output

```
`95   -0.526625
96   -0.234975
97    0.744299
98    0.434843
99   -0.609003
dtype: float64
`
```

### 数据的统计分析

在`pandas`当中用`describe()`方法来对表格中的数据做一个概括性的统计分析，例如

```
`series2.describe()
`
```

output

```
`count    100.000000
mean       0.040813
std        1.003012
min       -2.385316
25%       -0.627874
50%       -0.029732
75%        0.733579
max        2.515542
dtype: float64
`
```

当然，我们也可以设置好输出的分位

```
`series2.describe(percentiles=[0.05, 0.25, 0.75, 0.95])
`
```

output

```
`count    100.000000
mean       0.040813
std        1.003012
min       -2.385316
5%        -1.568183
25%       -0.627874
50%       -0.029732
75%        0.733579
95%        1.560211
max        2.515542
dtype: float64
`
```

对于**离散型**的数据来说，`describe()`方法给出的结果则会简洁很多

```
`s = pd.Series(["a", "a", "b", "b", "a", "a", "d", "c", "d", "a"])
s.describe()
`
```

output

```
`count     10
unique     4
top        a
freq       5
dtype: object
`
```

要是表格中既包含了离散型数据，也包含了连续型的数据，默认的话，`describe()`是会针对**连续型数据**进行统计分析

```
`df2 = pd.DataFrame({"a": ["Yes", "Yes", "No", "No"], "b": np.random.randn(4)})
df2.describe()
`
```

output

```
`              b
count  4.000000
mean   0.336053
std    1.398306
min   -1.229344
25%   -0.643614
50%    0.461329
75%    1.440995
max    1.650898
`
```

当然我们也可以指定让其强制统计分析离散型数据或者连续型数据

```
`df2.describe(include=["object"])
`
```

output

```
`          a
count     4
unique    2
top     Yes
freq      2
`
```

同理，我们也可以指定连续型的数据进行统计分析

```
`df2.describe(include=["number"])
`
```

output

```
`              b
count  4.000000
mean  -0.593695
std    0.686618
min   -1.538640
25%   -0.818440
50%   -0.459147
75%   -0.234401
max    0.082155
`
```

如果我们都要去做统计分析，可以这么来执行

```
`df2.describe(include="all")
`
```

output

```
`          a         b
count     4  4.000000
unique    2       NaN
top     Yes       NaN
freq      2       NaN
mean    NaN  0.292523
std     NaN  1.523908
min     NaN -1.906221
25%     NaN -0.113774
50%     NaN  0.789560
75%     NaN  1.195858
max     NaN  1.497193
`
```

### 最大/最小值的位置

`idxmin()`和`idxmax()`方法是用来查找表格当中最大/最小值的**位置**，返回的是值的索引

```
`s1 = pd.Series(np.random.randn(5))
s1
`
```

output

```
`s1.idxmin(), s1.idxmax()
`
```

output

```
`(0, 3)
`
```

用在`DataFrame`上面的话，如下

```
`df1 = pd.DataFrame(np.random.randn(5, 3), columns=["A", "B", "C"])
df1.idxmin(axis=0)
`
```

output

```
`A    4
B    2
C    1
dtype: int64
`
```

同理，我们将`axis`参数改成`1`

```
`df1.idxmin(axis=1)
`
```

output

```
`0    C
1    C
2    C
3    B
4    A
dtype: object
`
```

### `value_counts()`方法

`pandas`当中的`value_counts()`方法主要用于数据表的计数以及排序，用来查看表格当中，指定列有多少个不同的数据值并且计算不同值在该列当中出现的**次数**，先来看一个简单的例子

```
`df = pd.DataFrame({'城市': ['北京', '广州', '上海', '上海', '杭州', '成都', '香港', '南京', '北京', '北京'],
                   '收入': [10000, 10000, 5500, 5500, 4000, 50000, 8000, 5000, 5200, 5600],
                   '年龄': [50, 43, 34, 40, 25, 25, 45, 32, 25, 25]})
df["城市"].value_counts()
`
```

output

```
`北京    3
上海    2
广州    1
杭州    1
成都    1
香港    1
南京    1
Name: 城市, dtype: int64
`
```

可以看到`北京`出现了3次，`上海`出现了2次，并且默认采用的是**降序**来排列的，下面我们来看一下用升序的方式来排列一下`收入`这一列

```
`df["收入"].value_counts(ascending=True)
`
```

output

```
`4000     1
50000    1
8000     1
5000     1
5200     1
5600     1
10000    2
5500     2
Name: 收入, dtype: int64
`
```

同时里面也还可以利用参数`normalize=True`，来计算不同值的计数占比

```
`df['年龄'].value_counts(ascending=True,normalize=True)
`
```

output

```
`50    0.1
43    0.1
34    0.1
40    0.1
45    0.1
32    0.1
25    0.4
Name: 年龄, dtype: float64
`
```

### 数据分组

我们可以使用`cut()`方法以及`qcut()`方法来对表格中的连续型数据分组，首先我们看一下`cut()`方法，假设下面这组数据代表的是小组每个成员的年龄

```
`ages = np.array([2,3,10,40,36,45,58,62,85,89,95,18,20,25,35,32])
pd.cut(ages, 5)
`
```

output

```
`[(1.907, 20.6], (1.907, 20.6], (1.907, 20.6], (39.2, 57.8], (20.6, 39.2], ..., (1.907, 20.6], (1.907, 20.6], (20.6, 39.2], (20.6, 39.2], (20.6, 39.2]]
Length: 16
Categories (5, interval[float64, right]): [(1.907, 20.6] < (20.6, 39.2] < (39.2, 57.8] <
                                           (57.8, 76.4] < (76.4, 95.0]]
`
```

由上可以看到用`cut()`方法将数据平分成了5个区间，且区间两边都有扩展以包含**最大值和最小值**，当然我们也可以给每一个区间加上标记

```
`pd.cut(ages, 5, labels=[u"婴儿",u"少年",u"青年",u"中年",u"老年"])
`
```

output

```
`['婴儿', '婴儿', '婴儿', '青年', '少年', ..., '婴儿', '婴儿', '少年', '少年', '少年']
Length: 16
Categories (5, object): ['婴儿' < '少年' < '青年' < '中年' < '老年']
`
```

而对于`qcut()`方法来说，我们可以指定区间来进行分组，例如

```
`pd.qcut(ages, [0,0.5,1], labels=['小朋友','大孩子'])
`
```

output

```
`['小朋友', '小朋友', '小朋友', '大孩子', '大孩子', ..., '小朋友', '小朋友', '小朋友', '小朋友', '小朋友']
Length: 16
Categories (2, object): ['小朋友' < '大孩子']
`
```

这里将年龄这组数据分成两部分\[0, 0.5, 1\]，一组是标上标记`小朋友`，另一组是`大孩子`，不过通常情况下，我们用的`cut()`方法比较多

### 引用函数

要是在表格当中引用其他的方法，或者是自建的函数，可以使用通过`pandas`当中的以下这几个方法

- `pipe()`
    
- `apply()`和`applymap()`
    
- `agg()`和`transform()`
    

#### `pipe()`方法

首先我们来看`pipe()`这个方法，我们可以将自己定义好的函数，以链路的形式一个接着一个传给我们要处理的数据集上

```
`def extract_city_name(df):
    df["state_name"] = df["state_and_code"].str.split(",").str.get(0)
    return df
def add_country_name(df, country_name=None):
    df["state_and_country"] = df["state_name"] + country_name
    return df
`
```

然后我们用`pip()`这个方法来将上面我们定义的函数串联起来

```
`df_p = pd.DataFrame({"city_and_code": ["Arizona, AZ"]})
df_p = pd.DataFrame({"state_and_code": ["Arizona, AZ"]})
df_p.pipe(extract_city_name).pipe(add_country_name, country_name="_USA")
`
```

output

```
`  state_and_code state_name state_and_country
0    Arizona, AZ    Arizona       Arizona_USA
`
```

#### `apply()`方法和`applymap()`方法

`apply()`方法可以对表格中的数据按照行或者是列方向进行处理，默认是按照列方向，如下

```
`df.apply(np.mean)
`
```

output

```
`A   -0.101751
B   -0.360288
C   -0.637433
dtype: float64
`
```

当然，我们也可以通过`axis`参数来进行调节

```
`df.apply(np.mean, axis = 1)
`
```

output

```
`0   -0.803675
1   -0.179640
2   -1.200973
3    0.156888
4    0.381631
5    0.049274
6    1.174923
7    0.612591
dtype: float64
`
```

除此之外，我们也可以直接调用匿名函数`lambda`的形式

```
`df.apply(lambda x: x.max() - x.min())
`
```

output

```
`A    1.922863
B    2.874672
C    1.943930
dtype: float64
`
```

也可以调用自己定义的函数方法

```
`df = pd.DataFrame(np.random.randn(5, 3), columns=["A", "B", "C"])
def normalize(x):
    return (x - x.mean()) / x.std()
`
```

我们用上`apply()`方法

```
`df.apply(normalize)
`
```

output

```
`          A         B         C
0  1.149795  0.390263 -0.813770
1  0.805843 -0.532374  0.859627
2  0.047824 -0.085334 -0.067179
3 -0.903319 -1.215023  1.149538
4 -1.100144  1.442467 -1.128216
`
```

`apply()`方法作用于数据集当中的**每个行或者是列**，而`applymap()`方法则是对数据集当中的**所有元素都进行处理**

```
`df = pd.DataFrame({'key1' : ['a', 'c', 'b', 'b', 'd'],
                   'key2' : ['one', 'two', 'three', 'two', 'one'],
                   'data1' : np.arange(1, 6),
                   'data2' : np.arange(10,15)})
`
```

output

```
`  key1   key2  data1  data2
0    a    one      1     10
1    c    two      2     11
2    b  three      3     12
3    b   four      4     13
4    d   five      5     14
`
```

我们来自定义一个函数

```
`def add_A(x):
    return "A" + str(x)
    
df.applymap(add_A)
`
```

output

```
`  key1    key2 data1 data2
0   Aa    Aone    A1   A10
1   Ac    Atwo    A2   A11
2   Ab  Athree    A3   A12
3   Ab   Afour    A4   A13
4   Ad   Afive    A5   A14
`
```

我们然后也可以通过`lambda()`自定义函数方法，然后来去除掉这个`A`

```
`df.applymap(add_A).applymap(lambda x: x.split("A")[1])
`
```

output

```
`  key1   key2 data1 data2
0    a    one     1    10
1    c    two     2    11
2    b  three     3    12
3    b   four     4    13
4    d   five     5    14
`
```

### `agg()`方法和`transform()`方法

`agg()`方法本意上是聚合函数，我们可以将用于统计分析的**一系列方法**都放置其中，并且放置多个

```
`df = pd.DataFrame(np.random.randn(5, 3), columns=["A", "B", "C"])
df.agg(np.sum)
`
```

output

```
`A    0.178156
B    3.233845
C   -0.859622
dtype: float64
`
```

当然，当中的`np.sum`部分也可以用字符串来表示，例如

```
`df.agg("sum")
`
```

output

```
`A   -0.606484
B   -1.491742
C   -1.732083
dtype: float64
`
```

我们尝试在当中放置多个统计分析的函数方法

```
`df.agg(["sum", "mean", "median"])
`
```

output

```
`               A         B         C
sum     1.964847  3.855801  0.630042
mean    0.392969  0.771160  0.126008
median  0.821005  0.714804 -0.273685
`
```

当然我们也可以和`lambda`匿名函数混合着搭配

```
`df.agg(["sum", lambda x: x.mean()])
`
```

output

```
`                 A         B         C
sum      -0.066486 -1.288341 -1.236244
<lambda> -0.013297 -0.257668 -0.247249
`
```

或者和自己定义的函数方法混合着用

```
`def my_mean(x):
    return x.mean()
    
df.agg(["sum", my_mean])
`
```

output

```
`                A         B         C
sum     -4.850201 -1.544773  0.429007
my_mean -0.970040 -0.308955  0.085801
`
```

与此同时，我们在`agg()`方法中添加字典，实现不同的列使用不同的函数方法

```
`df.agg({"A": "sum", "B": "mean"})
`
```

output

```
`A   -0.801753
B    0.097550
dtype: float64
`
```

例如

```
`df.agg({"A": ["sum", "min"], "B": "mean"})
`
```

output

```
`             A         B
sum   0.911243       NaN
min  -0.720225       NaN
mean       NaN  0.373411
`
```

而当数据集当中既有连续型变量，又有离散型变量的时候，用`agg()`方法则就会弄巧成拙了

```
`df = pd.DataFrame(
    {
        "A": [1, 2, 3],
        "B": [1.0, 2.0, 3.0],
        "C": ["test1", "test2", "test3"],
        "D": pd.date_range("20211101", periods=3),
    }
)
df.agg(["min", "sum"])
`
```

output

```
`     A    B                C          D
min  1  1.0            test1 2021-11-01
sum  6  6.0  test1test2test3        NaT
`
```

出来的结果可能并非是用户所想要的了，而至于`transform()`方法，其效果和用法都和`agg()`方法及其的相似，这边也就不多做赘述

### 索引和列名的重命名

针对索引和列名的重命名，我们可以通过`pandas`当中的`rename()`方法来实现，例如我们有这样一个数据集

```
`df1 = pd.DataFrame(np.random.randn(5, 3), columns=["A", "B", "C"],
                   index = ["a", "b", "c", "d", "e"])
`
```

output

```
`          A         B         C
a  0.343690  0.869984 -1.929814
b  1.025613  0.470155 -0.242463
c -0.400908 -0.362684  0.226857
d -1.339706 -0.302005 -1.784452
e -0.957026 -0.813600  0.215098
`
```

我们可以这样来操作

```
`df1.rename(columns={"A": "one", "B": "two", "C": "three"},
                 index={"a": "apple", "b": "banana", "c": "cat"})
`
```

output

```
`             one       two     three
apple   0.383813  0.588964 -0.162386
banana -0.462068 -2.938896  0.935492
cat    -0.059807 -1.987281  0.095432
d      -0.085230  2.013733 -1.324039
e      -0.678352  0.306776  0.808697
`
```

当然我们可以拆开来，单独对行或者是列进行重命名，对列的重命名可以这么来做

```
`df1.rename({"A": "one", "B": "two", "C": "three"}, axis = "columns")
`
```

output

```
`        one       two     three
a -0.997108 -1.383011  0.474298
b  1.009910  0.286303  1.120783
c  1.130700 -0.566922  1.841451
d -0.350438 -0.171079 -0.079804
e  0.988050 -0.524604  0.653306
`
```

对行的重命名则可以这么来做

```
`df1.rename({"a": "apple", "b": "banana", "c": "cat"}, axis = "index")
`
```

output

```
`               A         B         C
apple   0.590589 -0.311803 -0.782117
banana  1.528043 -0.944476 -0.337584
cat     1.326057 -0.087368  0.041444
d       1.079768 -0.098314 -0.210999
e       1.654869  1.170333  0.506194
`
```

### 排序

在`pandas`当中，我们可以针对数据集当中的值来进行排序

```
`df1 = pd.DataFrame(
    {"one": [2, 1, 1, 1], "two": [1, 3, 2, 4], "three": [5, 4, 3, 2]}
)
`
```

output

```
`   one  two  three
0    2    1      5
1    1    3      4
2    1    2      3
3    1    4      2
`
```

我们按照“three”这一列当中的数值来进行排序

```
`df1.sort_values(by = "three")
`
```

output

```
`   one  two  three
3    1    4      2
2    1    2      3
1    1    3      4
0    2    1      5
`
```

我们也可以依照多列进行排序

```
`df1.sort_values(by = ["one", "two"])
`
```

output

```
`   one  two  three
2    1    2      3
1    1    3      4
3    1    4      2
0    2    1      5
`
```

在“one”这一列相等的时候，比较“two”这一列数值的大小，在排序的过程当中，默认采用的都是升序，我们可以改成降序来进行编排

```
`df1.sort_values("two", ascending=False)
`
```

output

```
`   one  two  three
3    1    4      2
1    1    3      4
2    1    2      3
0    2    1      5
`
```

### 数据类型的转换

最后涉及到的是数据类型的转换，在这之前，我们先得知道如何来查看数据的类型，`pandas`当中有相应的方法可以处理

```
`df2 = pd.DataFrame(
    {
        "A": pd.Series(np.random.randn(5), dtype="float16"),
        "B": pd.Series(np.random.randn(5)),
        "C": pd.Series(np.array(np.random.randn(5), dtype="uint8")),
    }
)
`
```

output

```
`          A         B    C
0 -0.498779 -0.501512    0
1 -0.055817 -0.528227  254
2 -0.914551  0.763298    1
3 -0.916016  1.366833    0
4  1.993164  1.834457    0
`
```

我们通过`dtypes`属性来查看数据的类型

```
`A    float16
B    float64
C      uint8
dtype: object
`
```

而通过`astype()`方法来实现数据类型的转换

```
`df2["B"].astype("int64")
`
```

output

```
`0    0
1    0
2    0
3    2
4    1
Name: B, dtype: int64
`
```

### 根据数据类型来筛选

与此同时，我们也可以根据相对应的数据类型来进行筛选，运用`pandas`当中的`select_dtypes`方法，我们先来创建一个数据集包含了各种数据类型的

```
`df = pd.DataFrame(
    {
        "string_1": list("abcde"),
        "int64_1": list(range(1, 6)),
        "uint8_1": np.arange(3, 8).astype("u1"),
        "float64_1": np.arange(4.0, 9.0),
        "bool1": [True, False, True, True, False],
        "bool2": [False, True, False, False, True],
        "dates_1": pd.date_range("now", periods=5),
        "category_1": pd.Series(list("ABCDE")).astype("category"),
    }
)
`
```

output

```
`  string_1  int64_1  uint8_1  ...  bool2                      dates_1  category_1
0      a      1      3  ...  False 2021-11-10 10:43:05.957685         A
1      b      2      4  ...   True 2021-11-11 10:43:05.957685         B
2      c      3      5  ...  False 2021-11-12 10:43:05.957685         C
3      d      4      6  ...  False 2021-11-13 10:43:05.957685         D
4      e      5      7  ...   True 2021-11-14 10:43:05.957685         E
`
```

我们先来查看一下各个列的数据类型

```
`df.dtypes
`
```

output

```
`string_1              object
int64_1                int64
uint8_1                uint8
float64_1            float64
bool1                   bool
bool2                   bool
dates_1       datetime64[ns]
category_1          category
dtype: object
`
```

我们筛选类型为**布尔值**的数据

```
`df.select_dtypes(include=[bool])
`
```

output

```
`   bool1  bool2
0   True  False
1  False   True
2   True  False
3   True  False
4  False   True
`
```

筛选出数据类型为**整型**的数据

```
`df.select_dtypes(include=['int64'])
`
```

output

```
`   int64_1
0      1
1      2
2      3
3      4
4      5`
```

长按👇关注\- 数据STUDIO - 设为星标，干货速递

<img width="578" height="194" src="../../../_resources/640_wx_fmt_gif_wxfrom_5_wx_lazy__30e91ce4d8be4abda.gif"/>

<img width="19" height="19" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__58ae34992ee24e29a.png"/>

分享

<img width="19" height="20" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__d9764480fa1949809.png"/>

收藏

<img width="19" height="19" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__483f2de0b46b4432b.png"/>

点赞

<img width="19" height="20" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__004e7829f08e4e6f8.png"/>

在看

People who liked this content also liked

Pandas数据框操作及数据提取

Python工程师

不看的原因

- 内容质量低
- 不看此公众号

pandas module

远小数

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___5d8c15d3c2e54cde8.bmp"/>

Scan to Follow