# 22个案例详解Pandas数据分析/预处理时的实用技巧，超简单

<a id="profileBt"></a><a id="js_name"></a>AI科技大本营 *2022-02-18 18:00*

The following article is from 关于数据分析与可视化 Author 俊欣

<a id="copyright_info"></a>[![](../../../_resources/0_3d2d7595a9c34aaeb1b14e3c5d510f3f.jpg)<br>**关于数据分析与可视化** .<br>本公众号定期分享数据分析与可视化干货文章，并有时结合热点话题进行深入讨论，希望您会喜欢，要是哪里写的不好，也渴望倾听您的想法和意见，感谢！❤️](#)

<img width="641" height="113" src="../../../_resources/640_wx_fmt_gif_wxfrom_5_wx_lazy__c2fc1bf9d2924e448.gif"/>

作者 | 俊欣

来源 | 关于数据分析与可视化

今天小编打算来讲一讲数据分析方面的内容，整理和总结一下`Pandas`在数据预处理和数据分析方面的硬核干货，我们大致会说

- `Pandas`计算交叉列表
    
- `Pandas`将字符串与数值转化成时间类型
    
- `Pandas`将字符串转化成数值类型
    

### `Pandas`当中的交叉列表

首先我们来讲一下`Pandas`模块当中的`crosstab()`函数，它的作用主要是进行分组之后的信息统计，里面会用到聚合函数，默认的是统计行列组合出现的次数，参数如下

```
`pandas.crosstab(index, columns,
                values=None,
                rownames=None,
                colnames=None,
                aggfunc=None,
                margins=False,
                margins_name='All',
                dropna=True,
                normalize=False)
`
```

下面小编来解释一下里面几个常用的函数

- index: 指定了要分组的类目，作为行
    
- columns: 指定了要分组的类目，作为列
    
- rownames/colnames: 行/列的名称
    
- aggfunc: 指定聚合函数
    
- values: 最终在聚合函数之下，行与列一同计算出来的值
    
- normalize: 标准化统计各行各列的百分比
    

我们通过几个例子来进一步理解`corss_tab()`函数的作用，我们先导入要用到的模块并且读取数据集

```
`import pandas as pd
df = pd.read_excel(
    io="supermarkt_sales.xlsx",
    engine="openpyxl",
    sheet_name="Sales",
    skiprows=3,
    usecols="B:R",
    nrows=1000,
)
`
```

output

我们先简单来看几个`corsstab()`函数的例子，代码如下

```
`pd.crosstab(df['城市'], df['顾客类型'])
`
```

output

```
`顾客类型   会员   普通
省份            
上海    124  115
北京    116  127
四川     26   35
安徽     28   12
广东     30   36
.......
`
```

这里我们将省份指定为行索引，将会员类型指定为列，其中顾客类型有“会员”、“普通”两种，举例来说，四川省的会员顾客有26名，普通顾客有35名。

当然我们这里只是指定了一个列，也可以指定多个，代码如下

```
`pd.crosstab(df['省份'], [df['顾客类型'], df["性别"]])
`
```

output

```
`顾客类型  会员      普通    
性别    女性  男性  女性  男性
省份                  
上海    67  57  53  62
北京    53  63  59  68
四川    17   9  16  19
安徽    17  11   9   3
广东    18  12  15  21
.....
`
```

这里我们将顾客类型进行了细分，有女性会员、男性会员等等，那么同理，对于行索引我们也可以指定多个，这里也就不过多进行演示。

有时候我们想要改变行索引的名称或者是列方向的名称，我们则可以这么做

```
`pd.crosstab(df['省份'], df['顾客类型'],
            colnames = ['顾客的类型'],
            rownames = ['各省份名称'])
`
```

output

```
`顾客的类型  会员   普通
各省份名称            
上海    124  115
北京    116  127
四川     26   35
安徽     28   12
广东     30   36
`
```

要是我们想在行方向以及列方向上加一个汇总的列，就需要用到`crosstab()`方法当中的`margin`参数，如下

```
`pd.crosstab(df['省份'], df['顾客类型'], margins = True)
`
```

output

```
`顾客类型   会员   普通   All
省份                  
上海    124  115   239
北京    116  127   243
.....
江苏     18   15    33
浙江    119  111   230
黑龙江    14   17    31
All   501  499  1000
`
```

你也可以给汇总的那一列重命名，用到的是`margins_name`参数，如下

```
`pd.crosstab(df['省份'], df['顾客类型'],
            margins = True, margins_name="汇总")
`
```

output

```
`顾客类型   会员   普通   汇总
省份                  
上海    124  115   239
北京    116  127   243
.....
江苏     18   15    33
浙江    119  111   230
黑龙江    14   17    31
汇总   501  499  1000
`
```

而如果我们需要的数值是百分比的形式，那么就需要用到`normalize`参数，如下

```
`pd.crosstab(df['省份'], df['顾客类型'],
            normalize=True)
`
```

output

```
`顾客类型     会员     普通
省份                
上海    0.124  0.115
北京    0.116  0.127
四川    0.026  0.035
安徽    0.028  0.012
广东    0.030  0.036
.......
`
```

要是我们更加倾向于是百分比，并且保留两位小数，则可以这么来做

```
`pd.crosstab(df['省份'], df['顾客类型'],
            normalize=True).style.format('{:.2%}')
`
```

output

```
`顾客类型  会员   普通
省份                
上海     12.4%   11.5%
北京     11.6%   12.7%
四川     26%     35%
安徽     28%     12%
广东     30%     36%
.......
`
```

下面我们指定聚合函数，并且作用在我们指定的列上面，用到的参数是`aggfunc`参数以及`values`参数，代码如下

```
`pd.crosstab(df['省份'], df['顾客类型'],
            values = df["总收入"],
            aggfunc = "mean")
`
```

output

```
`顾客类型         会员         普通
省份                        
上海    15.648738  15.253248
北京    14.771259  14.354390
四川    20.456135  14.019029
安徽    10.175893  11.559917
广东    14.757083  18.331903
.......
`
```

如上所示，我们所要计算的是地处“上海”并且是“会员”顾客的总收入的平均值，除了平均值之外，还有其他的聚合函数，如`np.sum`加总或者是`np.median`求取平均值。

我们还可以指定保留若干位的小数，使用`round()`函数

```
`df_1 = pd.crosstab(df['省份'], df['顾客类型'],
                   values=df["总收入"],
                   aggfunc="mean").round(2)
`
```

output

```
`顾客类型     会员     普通
省份                
上海    15.65  15.25
北京    14.77  14.35
四川    20.46  14.02
安徽    10.18  11.56
广东    14.76  18.33
.......
`
```

### 时间类型数据的转化

对于很多数据分析师而言，在进行数据预处理的时候，需要将不同类型的数据转换成时间格式的数据，我们来看一下具体是怎么来进行

首先是将整形的时间戳数据转换成时间类型，看下面的例子

```
`df = pd.DataFrame({'date': [1470195805, 1480195805, 1490195805],
                   'value': [2, 3, 4]})
pd.to_datetime(df['date'], unit='s')
`
```

output

```
`0   2016-08-03 03:43:25
1   2016-11-26 21:30:05
2   2017-03-22 15:16:45
Name: date, dtype: datetime64[ns]
`
```

上面的例子是精确到秒，我们也可以精确到天，代码如下

```
`df = pd.DataFrame({'date': [1470, 1480, 1490],
                   'value': [2, 3, 4]})
pd.to_datetime(df['date'], unit='D')
`
```

output

```
`0   1974-01-10
1   1974-01-20
2   1974-01-30
Name: date, dtype: datetime64[ns]
`
```

下面则是将字符串转换成时间类型的数据，调用的也是`pd.to_datetime()`方法

```
`pd.to_datetime('2022/01/20', format='%Y/%m/%d')
`
```

output

```
`Timestamp('2022-01-20 00:00:00')
`
```

亦或是

```
`pd.to_datetime('2022/01/12 11:20:10',
               format='%Y/%m/%d %H:%M:%S')
`
```

output

```
`Timestamp('2022-01-12 11:20:10')
`
```

这里着重介绍一下`Python`当中的时间日期格式化符号

- %y 两位数的年份表示(00-99)
    
- %Y 四位数的年份表示(000-9999)
    
- %m 表示的是月份(01-12)
    
- %d 表示的是一个月当中的一天(0-31)
    
- %H 表示的是24小时制的小时数
    
- %I 表示的是12小时制的小时数
    
- %M 表示的是分钟数 (00-59)
    
- %S 表示的是秒数(00-59)
    
- %w 表示的是星期数，一周当中的第几天，从星期天开始算
    
- %W 表示的是一年中的星期数
    

当然我们进行数据类型转换遇到错误的时候，`pd.to_datetime()`方法当中的`errors`参数就可以派上用场，

```
`df = pd.DataFrame({'date': ['3/10/2000', 'a/11/2000', '3/12/2000'],
                   'value': [2, 3, 4]})
# 会报解析错误
df['date'] = pd.to_datetime(df['date'])
`
```

output

<img width="618" height="185" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__4bdb652a451b48f18.png"/>

我们来看一下`errors`参数的作用，代码如下

```
`df['date'] = pd.to_datetime(df['date'], errors='ignore')
df
`
```

output

```
` date      value
0 3/10/2000   2
1 a/11/2000   3
2 3/12/2000   4
`
```

或者将不准确的值转换成`NaT`，代码如下

```
`df['date'] = pd.to_datetime(df['date'], errors='coerce')
df
`
```

output

```
` date       value
0 2000-03-10   2
1 NaT          3
2 2000-03-12   4
`
```

### 数值类型的转换

接下来我们来看一下其他数据类型往数值类型转换所需要经过的步骤，首先我们先创建一个`DataFrame`数据集，如下

```
`df = pd.DataFrame({
    'string_col': ['1','2','3','4'],
    'int_col': [1,2,3,4],
    'float_col': [1.1,1.2,1.3,4.7],
    'mix_col': ['a', 2, 3, 4],
    'missing_col': [1.0, 2, 3, np.nan],
    'money_col': ['£1,000.00','£2,400.00','£2,400.00','£2,400.00'],
    'boolean_col': [True, False, True, True],
    'custom': ['Y', 'Y', 'N', 'N']
  })
`
```

output

<img width="618" height="161" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__2ca32c51ebe644688.png"/>

我们先来查看一下每一列的数据类型

```
`df.dtypes
`
```

output

```
`string_col      object
int_col          int64
float_col      float64
mix_col         object
missing_col    float64
money_col       object
boolean_col       bool
custom          object
dtype: object
`
```

可以看到有各种类型的数据，包括了布尔值、字符串等等，或者我们可以调用`df.info()`方法来调用，如下

```
`df.info()
`
```

output

```
`<class 'pandas.core.frame.DataFrame'>
RangeIndex: 4 entries, 0 to 3
Data columns (total 8 columns):
 #   Column       Non-Null Count  Dtype  
---  ------       --------------  -----  
 0   string_col   4 non-null      object 
 1   int_col      4 non-null      int64  
 2   float_col    4 non-null      float64
 3   mix_col      4 non-null      object 
 4   missing_col  3 non-null      float64
 5   money_col    4 non-null      object 
 6   boolean_col  4 non-null      bool   
 7   custom       4 non-null      object 
dtypes: bool(1), float64(2), int64(1), object(4)
memory usage: 356.0+ bytes
`
```

我们先来看一下从字符串到整型数据的转换，代码如下

```
`df['string_col'] = df['string_col'].astype('int')
df.dtypes
`
```

output

```
`string_col       int32
int_col          int64
float_col      float64
mix_col         object
missing_col    float64
money_col       object
boolean_col       bool
custom          object
dtype: object
`
```

看到数据是被转换成了`int32`类型，当然我们指定例如`astype('int16')`、`astype('int8')`或者是`astype('int64')`，当我们碰到量级很大的数据集时，会特别的有帮助。

那么类似的，我们想要转换成浮点类型的数据，就可以这么来做

```
`df['string_col'] = df['string_col'].astype('float')
df.dtypes
`
```

output

```
`string_col     float64
int_col          int64
float_col      float64
mix_col         object
missing_col    float64
money_col       object
boolean_col       bool
custom          object
dtype: object
`
```

同理我们也可以指定转换成`astype('float16')`、`astype('float32')`或者是`astype('float128')`

而如果数据类型的混合的，既有整型又有字符串的，正常来操作就会报错，如下

```
`df['mix_col'] = df['mix_col'].astype('int')
`
```

output

<img width="618" height="191" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__d794a4d927c440228.png"/>

当中有一个字符串的数据"a"，这个时候我们可以调用`pd.to_numeric()`方法以及里面的`errors`参数，代码如下

```
`df['mix_col'] = pd.to_numeric(df['mix_col'], errors='coerce')
df.head()
`
```

output

<img width="618" height="165" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__863665ca07984381a.png"/>

我们来看一下各列的数据类型

```
`df.dtypes
`
```

output

```
`string_col     float64
int_col          int64
float_col      float64
mix_col        float64
missing_col    float64
money_col       object
boolean_col       bool
custom          object
dtype: object
`
```

"mix_col"这一列的数据类型被转换成了`float64`类型，要是我们想指定转换成我们想要的类型，例如

```
`df['mix_col'] = pd.to_numeric(df['mix_col'], errors='coerce').astype('Int64')
df['mix_col'].dtypes
`
```

output

```
`Int64Dtype()
`
```

而对于"money_col"这一列，在字符串面前有一个货币符号，并且还有一系列的标签符号，我们先调用`replace()`方法将这些符号给替换掉，然后再进行数据类型的转换

```
`df['money_replace'] = df['money_col'].str.replace('£', '').str.replace(',','')
df['money_replace'] = pd.to_numeric(df['money_replace'])
df['money_replace']
`
```

output

```
`0    1000.0
1    2400.0
2    2400.0
3    2400.0
Name: money_replace, dtype: float64
`
```

要是你熟悉正则表达式的话，也可以通过正则表达式的方式来操作，通过调用`regex=True`的参数，代码如下

```
`df['money_regex'] = df['money_col'].str.replace('[\£\,]', '', regex=True)
df['money_regex'] = pd.to_numeric(df['money_regex'])
df['money_regex']
`
```

另外我们也可以通过`astype()`方法，对多个列一步到位进行数据类型的转换，代码如下

```
`df = df.astype({
    'string_col': 'float16',
    'int_col': 'float16'
})
`
```

或者在第一步数据读取的时候就率先确定好数据类型，代码如下

```
`df = pd.read_csv(
    'dataset.csv', 
    dtype={
        'string_col': 'float16',
        'int_col': 'float16'
    }
)
`
```

<img width="562" height="112" src="../../../_resources/640_wx_fmt_gif_wxfrom_5_wx_lazy__0da146a3adbd4602a.gif"/>

<img width="562" height="292" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__c0f06c38cbe74ddc9.png"/>

往

期

回

顾

资讯

[冬奥会夺金背后的杀手锏，是他](http://mp.weixin.qq.com/s?__biz=Mzg4NDQwNTI0OQ==&mid=2247555474&idx=1&sn=6ed705215bb3625a048b489455518252&chksm=cfbafcfcf8cd75ea33af095a6e07bc25e0739ffaf85dcc2f4684d0eaf1dd014f3e2e22db8750&scene=21#wechat_redirect)

资讯

[首个深度强化学习AI，控制核聚变](http://mp.weixin.qq.com/s?__biz=Mzg4NDQwNTI0OQ==&mid=2247555474&idx=2&sn=7f291fc2f8328432af13302ae5d9e85d&chksm=cfbafcfcf8cd75ea68b313492972cef0d5cd2fc22b714ef9bc0b753cc66f8eb60d7e350c8814&scene=21#wechat_redirect)

技术

[真香，邮件分类准确识别垃圾邮件](http://mp.weixin.qq.com/s?__biz=Mzg4NDQwNTI0OQ==&mid=2247555434&idx=1&sn=dc36edd92235a0dbde9c6e55a5f01b2d&chksm=cfbafc04f8cd7512d6b46aa2fec8ba543a4769b1915e2600c2322cc93225731ebc9d549a00fa&scene=21#wechat_redirect)

技术

[程序员元宵节的正确打开方式](http://mp.weixin.qq.com/s?__biz=Mzg4NDQwNTI0OQ==&mid=2247555352&idx=1&sn=99b583ae487835dcf1cb9575dfe342c5&chksm=cfbafc76f8cd75604c0b575f33bd51df3bffeac46693fffaa3f0ba361efbf547bbffaceac2b4&scene=21#wechat_redirect)

<img width="30" height="28" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__c0c568accc1a48409.png"/>

**分享**

<img width="30" height="27" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__ec417ee171084c629.png"/>

**点收藏**

<img width="30" height="28" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__76f9dac4ad6648639.png"/>

**点点赞**

<img width="30" height="28" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__76518f4f2e0f47669.png"/>

**点在看**

People who liked this content also liked

22个Python绘图包，极简总结

Python丹卿

不看的原因

- 内容质量低
- 不看此公众号

比ArcGIS更好用的地图制作软件

GIS二师兄

不看的原因

- 内容质量低
- 不看此公众号

SQL Server tempdb 闩锁争用

SQLServer

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___9c147dae3b8f4b65a.bmp"/>

Scan to Follow