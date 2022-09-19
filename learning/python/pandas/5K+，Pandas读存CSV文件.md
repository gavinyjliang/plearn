# 5K+，Pandas读存CSV文件

<a id="copyright_logo"></a>Original <a id="js_author_name"></a>尤而小屋 <a id="profileBt"></a><a id="js_name"></a>尤而小屋 *2022-05-23 00:00* *Posted on <a id="js_ip_wording"></a>湖北*

收录于合集

<a id="js_article_tag_name__2392311813658066945"></a>#csv <a id="js_article_tag_num__2392311813658066945"></a>1 <a id="js_article_tag_tips__2392311813658066945"></a>个

<a id="js_article_tag_name__1999223975448477698"></a>#python <a id="js_article_tag_num__1999223975448477698"></a>80 <a id="js_article_tag_tips__1999223975448477698"></a>个

<a id="js_article_tag_name__1999223977226862596"></a>#pandas <a id="js_article_tag_num__1999223977226862596"></a>64 <a id="js_article_tag_tips__1999223977226862596"></a>个

<a id="js_article_tag_name__2000726468170940417"></a>#数据分析师 <a id="js_article_tag_num__2000726468170940417"></a>48 <a id="js_article_tag_tips__2000726468170940417"></a>个

<a id="js_article_tag_name__1999223975666581509"></a>#数据分析 <a id="js_article_tag_num__1999223975666581509"></a>129 <a id="js_article_tag_tips__1999223975666581509"></a>个

> 公众号：尤而小屋
> 作者：Peter
> 编辑：Peter

大家好，我是Peter~

文本记录的是如何使用Pandas来读取和保存CSV文件。

详细的参数请参考官网：https://pandas.pydata.org/docs/reference/api/pandas.read_csv.html

尤而小屋

，赞 24

关于Pandas操作Excel请参考：

[7K+，Pandas读存Excel大全](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247513808&idx=1&sn=c6d7d6e19cefb385e1b5ae698f77c079&chksm=cf12a60af8652f1c5acd4f05be077d16a68a3a943550bf4654efd0c91b89e55b772c114f3ee9&scene=21#wechat_redirect)

## 官方参数

<img width="657" height="349" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_02170ce082a1418c9.jpg"/>

```
read_csv(filepath_or_buffer, 
         sep=NoDefault.no_default, 
         delimiter=None, 
         header='infer', 
         names=NoDefault.no_default, 
         index_col=None, 
         usecols=None, 
         squeeze=None, 
         prefix=NoDefault.no_default, 
         mangle_dupe_cols=True, 
         dtype=None, 
         engine=None, 
         converters=None, 
         true_values=None, 
         false_values=None, 
         skipinitialspace=False, 
         skiprows=None, 
         skipfooter=0, 
         nrows=None, 
         na_values=None, 
         keep_default_na=True,
         na_filter=True, 
         verbose=False,
         skip_blank_lines=True,
         parse_dates=None,
         infer_datetime_format=False,
         keep_date_col=False,
         date_parser=None,
         dayfirst=False, 
         cache_dates=True,
         iterator=False, 
         chunksize=None, 
         compression='infer', 
         thousands=None, 
         decimal='.', 
         lineterminator=None,
         quotechar='"',
         quoting=0,
         doublequote=True, 
         escapechar=None, 
         comment=None, 
         encoding=None, 
         encoding_errors='strict',
         dialect=None,
         error_bad_lines=None, 
         warn_bad_lines=None,
         on_bad_lines=None, 
         delim_whitespace=False,
         low_memory=True, 
         memory_map=False,
         float_precision=None, 
         storage_options=None)

```

下面是对部分参数的解释：

1.  filepath\_or\_buffer：文件路径或者在线缓存的数据
    
2.  sep：指定的分隔符，默认是`,`
    
3.  delimiter：作用和sep是类似的
    
4.  header：文件头，默认是数据第一行；如果指定为None，则用自然数0,1,2,3等表示
    
5.  names：指定文件列名
    
6.  index_col：指定将哪列作为行索引
    
7.  usecols：指定读取的列，可以是数字或列名
    
8.  squeeze：如果将squeeze设置为True，文件中只包含一列，则返回一个Series型的数据；如果有多列，则返回DataFrame型数据
    
9.  prefix：如果原始数据没有列名，通过该参数加上前缀，此时headers参数为None
    
10. dtype：指定数据类型
    
11. engine：解析引擎，一般是c或者python
    
12. converters：针对列进行处理。指定列名和处理的函数，最终通过字典的形式传入
    
13. nrows：指定读取的行
    
14. skip\_blank\_lines：是否跳过空白行，默认是True
    

在一般情况下，会将读取到的数据返回成一个Pandas的DataFrame形式的数据。

## 导入库

```
import pandas as pd
import numpy as np
import io

```

## 参数1：filepath\_or\_buffer

### 本地数据

- 读取本地csv文件
    
- 路径可以是相对路径，也可以是绝对路径
    

In \[3\]:

```
# 绝对路径：当前路径
df = pd.read_csv("Pandas_for_CSV.csv")
# 完整的绝对路径
df1 = pd.read_csv(r"/Users/peter/Desktop/pandas/Pandas_for_CSV.csv")
df1.head()

```

<img width="657" height="229" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_aea72387f1c14307b.jpg"/>

### 在线数据

读取某个URL地址下的在线公开数据集：

```
url = "https://raw.githubusercontent.com/hunkim/DeepLearningZeroToAll/master/data-03-diabetes.csv"
# 方式1
data = pd.read_csv(url,header=None)
# 方式2
import io
import requests
# 先发送请求
r = requests.get(url).content.decode('utf-8')
# 通过 io.StringIO 函数进行转换
data = pd.read_csv(io.StringIO(r),header=None)
data.head()

```

<img width="657" height="186" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_5b9a58a4c5d244c58.jpg"/>

## 参数2：sep

指定分割的符号。CSV的文件默认是英文的逗号来分割的：`,`。另外常见的还有制表符（\\t）、空格等，我们可以根据数据的实际情况来进行传值。

sep的默认取值也是英文的逗号

In \[9\]:

```
df = pd.read_csv("Pandas_for_CSV.csv", sep=",")  # 默认
df.head()

```

<img width="657" height="240" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_aab3a0db243945c28.jpg"/>

其他分隔符：

```
# 数据分隔符默认是逗号，可以指定为其他符号  
# pd.read_csv(data, sep='\t') # 制表符分隔tab   
# pd.read_csv(data, sep='|') # 制表符分隔tab  

```

还可以同时使用多个符号，传入的是正则表达式。

下面的例子中表示的是使用`|`或者`,`来进行分割；当使用的是正则表达式，engine参数一定是python

In \[11\]:

```
# pd.read_csv(data,sep=r'/|,', engine='python') # 使用正则表达式

```

## 参数3：delimiter

备选的分隔符，作用和sep是类似的；如果指定了delimiter参数，则sep参数会失效

In \[12\]:

```
df = pd.read_csv("Pandas_for_CSV.csv", delimiter=",")
df.head()

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 参数4：header

指定读取的文件头；默认是第一行当做表头，如果是None则没有表头，使用自然数当做表头。

支持单个整数或者整数组成的列表：

默认情况下将数据的第一行当做文件的头：

In \[13\]:

```
# 默认情况读取
data = pd.read_csv(url)
data.head(3)

```

<img width="657" height="134" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_e3017b2707fd4aabb.jpg"/>

指定为None之后，第一行不再是表头，而是使用自然数做表头：

In \[14\]:

```
# 指定header为None
data = pd.read_csv(url,header=None)
data.head(3)

```

Out\[14\]:

<img width="657" height="152" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_c80efffe94334c7aa.jpg"/>

```
# 读取本地文件
df = pd.read_csv("Pandas_for_CSV.csv",header=[0])
df.head()

```

<img width="657" height="228" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_e2897b299d244f06b.jpg"/>

下面的例子是将多行当做表头，由数字组成的列表：

In \[16\]:

```
df = pd.read_csv("Pandas_for_CSV.csv",header=[0,2])
df.head()

```

Out\[16\]:

<img width="657" height="220" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_c1cb6cd7c161437f9.jpg"/>

## 参数5：names

用来指定文件的列名，也是一个列表对象。

<img width="657" height="386" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_01dfab78a89b4f64b.jpg"/>

在线数据的读取也可以指定names参数：

<img width="657" height="191" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_f779c872489e440f9.jpg"/>

## 参数6：index_col

指定行索引列

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

image-20220508104414894

指定多层索引：

```
df = pd.read_csv("Pandas_for_CSV.csv",index_col=[0,1])
# 效果同上
# df = pd.read_csv("Pandas_for_CSV.csv",index_col=["id","name"])
df.head()

```

<img width="657" height="530" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_37bdadb3c0ab495ea.jpg"/>

## 参数7：usecols

指定查看的列属性信息，可以是索引号或者直接使用列名：

<img width="657" height="487" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_7e5c621a610741529.jpg"/>

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 参数8：squeeze

如果将squeeze设置为True，文件中只包含一列，则返回一个Series型的数据；如果包含有多列，则还是返回DataFrame型数据

In \[27\]:

```
# 默认返回的是DataFrame
df = pd.read_csv("Pandas_for_CSV.csv",usecols=[0])
df.head()

```

Out\[27\]:

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

In \[28\]:

```
# 加上squeeze参数，返回的是Series型数据
df = pd.read_csv("Pandas_for_CSV.csv",usecols=[0],squeeze=True)
df.head()

```

Out\[28\]:

```
0    S001
1    S002
2    S003
3    S004
4    S005
Name: id, dtype: object

```

In \[29\]:

```
df = pd.read_csv("Pandas_for_CSV.csv",usecols=[0,1],squeeze=True)
df.head()

```

Out\[29\]:

![Image](../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_99826e0c878c4256a.jpg)

## 参数9：prefix

如果原始数据没有列名，通过prefix参数给表头加上一个前缀；此时header参数一定是None

In \[30\]:

```
df = pd.read_csv(url, prefix="n", header=None)
df.head()

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 参数10：mangle\_dupe\_cols

针对列名中有重复名称的处理，可以将重复的列名自动解析为col.1,col.2,col.3等

In \[31\]:

```
df = pd.read_csv("Pandas_for_CSV_1.csv",mangle_dupe_cols=False)
df

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 参数11：dtype

指定读取字段的数据类型，通常是以字典的形式出现

In \[33\]:

```
df = pd.read_csv("Pandas_for_CSV.csv")
# 查看列名的字段类型
df.dtypes

```

Out\[33\]:

```
id         object
name       object
age         int64
sex        object
address    object
time        int64
year        int64
month       int64
dtype: object

```

In \[34\]:

```
df = pd.read_csv("Pandas_for_CSV.csv", dtype={"age":"float32"})
# 查看列名的字段类型：已经变成了float32
df.dtypes

```

Out\[34\]:

```
id          object
name        object
age        float32
sex         object
address     object
time         int64
year         int64
month        int64
dtype: object

```

## 参数12：engine

读取数据时的解析引擎，一般是C、PyArrow、或者Python；C和PyArrow的速度是相对快的，但是Python的功能最强。

补充知识点：

> Apache arrow是高性能的，用于内存计算的，列式数据存储格式。PyArrow是apache arrow的python库，PyArrow与NumPy、pandas和内置的Python对象有很好的集成。它们是基于Arrow的C++实现。

In \[35\]:

```
# pd.read_csv("Pandas_for_CSV.csv", engine="c")
# pd.read_csv("Pandas_for_CSV.csv", engine="python")

```

## 参数13：converters

针对列进行处理。指定列名和处理的函数，最终通过字典的形式传入

In \[36\]:

```
# 默认
df = pd.read_csv("Pandas_for_CSV.csv")
df.head()

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

实施改变1：名称name的首字母变成大写

In \[37\]:

```
# Python中的title实现
# 1-表示索引号
df = pd.read_csv("Pandas_for_CSV.csv", 
                 converters={1: lambda x:x.title()}  
                )
df.head()

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

实施改变2：同时传入列名和对应的函数

In \[38\]:

```
# 自定义改变姓名内容的函数
def change_sex(x):
    if x == "male":
        return 0
    else:
        return 1

```

In \[39\]:

```
df = pd.read_csv(
 "Pandas_for_CSV.csv", 
  usecols=[1,3],
  converters={
   1: lambda x: x.title(), # 首字母大写
   3: lambda x: change_sex(x)  # 传入自定义函数
  })
df.head()

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 参数14：true\_values、false\_values

将指定内容的文本转成True或者False

In \[40\]:

```
df = pd.read_csv("Pandas_for_CSV.csv",
                 true_values=['male'], 
                 false_values=['female']
                )
df.head()

```

Out\[40\]:

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 参数15：skiprows

跳过指定的行，可以是数字，列表，也可以是函数表达式

In \[41\]:

```
# pd.read_csv("Pandas_for_CSV.csv", skiprows=[0])
# pd.read_csv("Pandas_for_CSV.csv", skiprows=[0,1,2])
pd.read_csv("Pandas_for_CSV.csv", skiprows=lambda x: x % 2 == 0)

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 参数16：skip\_blank\_lines

是否跳过空白行。如果设置成True，则跳过空白行，否则不跳过，显示为NaN

## 参数17：skipfooter

最后的N行数据不进行显示

In \[42\]:

```
# pd.read_csv("Pandas_for_CSV.csv", skipfooter=1)  最后1行不显示
# pd.read_csv("Pandas_for_CSV.csv", skipfooter=2)  最后2行不显示
# pd.read_csv("Pandas_for_CSV.csv", skipfooter=3)  最后3行不显示

```

## 参数18：skipinitialspace

忽略分隔符后面的空白符

## 参数19：nrows

用来表示读取指定行数的数据

In \[43\]:

```
pd.read_csv("Pandas_for_CSV.csv",nrows=4)   # 读取4行数据

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 参数20：parse_dates

自动解析数据的时间信息

In \[44\]:

```
df = pd.read_csv("Pandas_for_CSV.csv") 
df.dtypes

```

Out\[44\]:

```
id         object
name       object
age         int64
sex        object
address    object
time        int64
year        int64
month       int64
dtype: object

```

In \[45\]:

```
df = pd.read_csv("Pandas_for_CSV.csv", 
                 infer_datetime_format=True,
                 parse_dates=True) 
df.dtypes

```

Out\[45\]:

```
id         object
name       object
age         int64
sex        object
address    object
time        int64
year        int64
month       int64
dtype: object

```

In \[46\]:

```
df = pd.read_csv("Pandas_for_CSV.csv", 
                 parse_dates=["time","year"]  # 指定字段解析
                ) 
df.dtypes

```

Out\[46\]:

```
id                 object
name               object
age                 int64
sex                object
address            object
time       datetime64[ns]
year       datetime64[ns]
month               int64
dtype: object

```

In \[47\]:

```
# 将6-year和7-month合并成一个字段date
df = pd.read_csv("Pandas_for_CSV.csv", 
                 parse_dates={"date":[6,7]}  
                ) 
df.dtypes

```

Out\[47\]:

```
date       datetime64[ns]
id                 object
name               object
age                 int64
sex                object
address            object
time                int64
dtype: object

```

## 参数21：keep\_date\_col

在上面的例子中生成了新的字段date，但是原来的year和mont删除了。可以通过keep\_date\_col来设置保留或者取出

In \[48\]:

```
# 将6-year和7-month合并成一个字段date
df = pd.read_csv("Pandas_for_CSV.csv", 
                 parse_dates={"date":[6,7]},
                 keep_date_col=True  # 保留原字段
                ) 
df.dtypes

```

Out\[48\]:

```
date       datetime64[ns]
id                 object
name               object
age                 int64
sex                object
address            object
time                int64
year               object
month              object
dtype: object

```

## 保存CSV文件

这个保存到CSV的功能是将DataFrame保存到Excel的功能是类似的：

```
df.to_csv("new_df.csv")  # 会带上index索引号
df.to_csv("new_df.csv", index=False)  # 不保留索引号

```

尤而小屋

，赞 11

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

推荐阅读

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

[机器学习-从文本分析看《解忧杂货店》](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247511150&idx=1&sn=e52c6d5a54932d8075b6250f3eda11ad&chksm=cf12bcb4f86535a2da19bf1f9a25b1e9c67e5dfa04fc35ffbfb9fd499c9d1f83c480e15ec85e&scene=21#wechat_redirect)

[可视化神器Plotly绘制桑基图](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247511083&idx=1&sn=c9e5026ab7420f171f17bc3762cb082d&chksm=cf12bcf1f86535e75041f330a766f24ceff1ceaef7d32975ba6eeb831192cd1056c78965202a&scene=21#wechat_redirect)

[Python关联规则挖掘情侣、基友、渣男和狗](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247510933&idx=1&sn=54a7dd43e340d72054205f88de0924eb&chksm=cf12bb4ff86532593aaf5f8ba6e0ce31f0d1614766943ce593c322fd054c027168771681a14f&scene=21#wechat_redirect)

[尤而小屋学习指南！](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247510942&idx=1&sn=34fc66c5fddeb8d5e733166ca0243c1e&chksm=cf12bb44f8653252a609dba04bcd6423ad8a7e95879abac3a0386ac924181f52fd446380731c&scene=21#wechat_redirect)

[Jupyter Notebook奇技淫巧](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247515068&idx=2&sn=89e79f9d2a07f0522cb91fa8662fd15a&chksm=cf12ab66f86522702525686851f1899d0908c44966b768988010796f67b301c33fd6f12aa934&scene=21#wechat_redirect)

[图解10大机器学习算法](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247510418&idx=1&sn=206ebfc5741f5b6e7eaac7a92444816d&chksm=cf12b948f865305e527692d3773975229270b310294deab74efa92a2e54496adf0095af324ac&scene=21#wechat_redirect)

[kaggle实战：可视化深度探索苹果AppStores](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247509347&idx=1&sn=e3601b69be31d0299ddb56295b902b58&chksm=cf12b5b9f8653caf4d7dce76e8b4587b3cc9bc6c67a96592d67f4295d9a4921a3d1b4752d1a8&scene=21#wechat_redirect)

> 尤而小屋，一个温馨的小屋。小屋主人，一手代码谋求生存，一手掌勺享受生活，欢迎你的光临

![](../../../_resources/0_wx_fmt_png_b3e0e002600f4ff4b6bf2057c1f86952.png)

**尤而小屋**

尤而小屋，一个温馨且有爱的小屋🏡 小屋主人，一手代码谋求生存，一手掌勺享受生活，欢迎你的光临~

<a id="js_profile_article"></a>247篇原创内容

Official Account

收录于合集 #<a id="js_album_keep_read_title"></a>python

 <a id="js_album_keep_read_size"></a>80个

<a id="js_album_prev"></a>上一篇 <a id="js_album_keep_read_pre_title"></a>数据科学：Python 和 R 针锋相对！ <a id="js_album_next"></a>下一篇 <a id="js_album_keep_read_next_title"></a>Python高级用法，代码精进! (文末赠书）

People who liked this content also liked

牛！一个名叫“简单作图”的R包，瞧瞧它到底有多简单？！

...

R语言和统计

不看的原因

- 内容质量低
- 不看此公众号

7K+，Pandas读存Excel大全

...

尤而小屋

不看的原因

- 内容质量低
- 不看此公众号

一文弄懂Python中的 if \_\_name\_\_==\_\_main\_\_

...

AI算法之道

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___42c355a464454ba7b.bmp"/>

Scan to Follow