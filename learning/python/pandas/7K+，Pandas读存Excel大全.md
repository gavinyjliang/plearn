# 7K+，Pandas读存Excel大全

<a id="copyright_logo"></a>Original <a id="js_author_name"></a>尤而小屋 <a id="profileBt"></a><a id="js_name"></a>尤而小屋 *2022-05-01 12:06* *Posted on <a id="js_ip_wording"></a>湖北*

收录于合集

<a id="js_article_tag_name__1999223977226862596"></a>#pandas <a id="js_article_tag_num__1999223977226862596"></a>64 <a id="js_article_tag_tips__1999223977226862596"></a>个

<a id="js_article_tag_name__1999223975448477698"></a>#python <a id="js_article_tag_num__1999223975448477698"></a>80 <a id="js_article_tag_tips__1999223975448477698"></a>个

<a id="js_article_tag_name__2378752767931924480"></a>#Excel <a id="js_article_tag_num__2378752767931924480"></a>1 <a id="js_article_tag_tips__2378752767931924480"></a>个

<a id="js_article_tag_name__1999223975666581509"></a>#数据分析 <a id="js_article_tag_num__1999223975666581509"></a>129 <a id="js_article_tag_tips__1999223975666581509"></a>个

> 公众号：尤而小屋
> 作者：Peter
> 编辑：Peter

大家好，我是Peter~祝大家五一节快乐<img width="20" height="20" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__973618b870ae464ba.png"/>

有粉丝问小编关于【尤而小屋】的资料如何学习，请参考Peter整理的**学习指南**：

[尤而小屋学习指南！](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247510942&idx=1&sn=34fc66c5fddeb8d5e733166ca0243c1e&chksm=cf12bb44f8653252a609dba04bcd6423ad8a7e95879abac3a0386ac924181f52fd446380731c&scene=21#wechat_redirect)

关于近期的免费送书活动，欢迎积极参与（文末赠书）：

[实战项目：Python制作词云跳舞](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247513496&idx=1&sn=ef05b0a613d31bad7bd743534387e98a&chksm=cf12a542f8652c54ddaa4d916f979a94d58c8aacc81aa6a23879837181c277f2d9de954ae3c7&scene=21#wechat_redirect)

* * *

本文记录的**是如何通过Pandas来读取Excel文件，以及如何将DataFrame保存到Excel文件中**。

官网参数详解：https://pandas.pydata.org/docs/reference/api/pandas.read_excel.html

<img width="657" height="252" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_10808c591eb84815b.jpg"/>

## 参数

read_excel函数能够读取的格式包含：xls, xlsx, xlsm, xlsb, odf, ods 和 odt 文件扩展名。支持读取单一sheet或几个sheet。

下面记录的官方文档中提供的全部参数信息：

```
pandas.read_excel(
  io,    
  sheet_name=0, 
  header=0, 
  names=None, 
  index_col=None, 
  usecols=None, 
  squeeze=None, 
  dtype=None, 
  engine=None, 
  converters=None, 
  true_values=None, 
  false_values=None, 
  skiprows=None, 
  nrows=None, 
  na_values=None,
  keep_default_na=True, 
  na_filter=True, 
  verbose=False, 
  parse_dates=False, 
  date_parser=None, 
  thousands=None, 
  decimal='.', 
  comment=None, 
  skipfooter=0, 
  convert_float=None, 
  mangle_dupe_cols=True, 
  storage_options=None
)

```

下面解释常用参数的含义：

- io：文件路径，支持 str, bytes, ExcelFile, xlrd.Book, path object, or file-like object。默认读取第一个sheet的内容。案例："/desktop/student.xlsx"
    
- sheet\_name：sheet表名，支持 str, int, list, or None；默认是0，索引号从0开始，表示第一个sheet。案例：sheet\_name=1, sheet\_name="sheet1"，sheet\_name=\[1,2,"sheet3"\]。None 表示引用所有sheet
    
- header：表示用第几行作为表头，支持 int, list of int；默认是0，第一行的数据当做表头。header=None表示不使用数据源中的表头，Pandas自动使用0,1,2,3…的自然数作为索引。
    
- names：表示自定义表头的名称，此时需要传递数组参数。
    
- index_col：指定列属性为行索引列，支持 int, list of int, 默认是None，也就是索引为0,1,2,3等自然数的列用作DataFrame的行标签。如果传入的是列表形式，则行索引会是多层索引
    
- usecols：待解析的列，支持 int, str, list-like, or callable ，默认是 None，表示解析全部的列。
    
- dtype：指定列属性的字段类型。案例：{‘a’: np.float64, ‘b’: np.int32}；默认为None，也就是不改变数据类型。
    
- engine：解析引擎；可以接受的参数有"xlrd"、"openpyxl"、"odf"、"pyxlsb"，用于使用第三方的库去解析excel文件
    

- “xlrd”支持旧式 Excel 文件 (.xls)
    
- “openpyxl”支持更新的 Excel 文件格式
    
- “odf”支持 OpenDocument 文件格式(.odf、.ods、.odt)
    
- “pyxlsb”支持二进制 Excel 文件
    

- converters：对指定列进行指定函数的处理，传入参数为列名与函数组成的字典，和usecols参数连用。key 可以是列名或者列的序号，values是函数，可以自定义的函数或者Python的匿名lambda函数
    
- skiprows：跳过指定的行（可选参数），类型为：list-like, int, or callable
    
- nrows：指定读取的行数，通常用于较大的数据文件中。类型int, 默认是None，读取全部数据
    
- na_values：指定列的某些特定值为NaN
    
- keep\_default\_na：是否导入空值，默认是导入，识别为NaN
    

## 模拟数据

现在本次模拟了两个数据**：Pandas\_Excel.xls 和 Pandas\_Excel.xlsx**

Pandas_Excel.xls 文件中包含两个sheet，**第二个数据只比第一个多个index的信息**

1、sheet1的内容

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

image-20220423115151077

2、sheet2的内容

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

3、Pandas_Excel.xlsx的内容，模拟的完整信息：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

```
import pandas as pd

```

## 默认情况

此时文件刚好在当前目录下，读取的时候指定文件名即可，可以看到读取的是第一个sheet

```
df = pd.read_excel("Pandas-Excel.xls")
df

```

|     | name | age | sex | address | date |
| --- | --- | --- | --- | --- | --- |
| 0   | 张三  | 23  | 男   | 深圳  | 2022-04-01 |
| 1   | 李四  | 16  | 男   | 广州  | 2022-04-02 |
| 2   | 小明  | 26  | 未知  | 深圳  | 2022-04-05 |
| 3   | 张飞  | 28  | 女   | 苏州  | 2021-09-08 |
| 4   | 小苏  | 20  | 女   | NaN | 2022-06-07 |
| 5   | 小王  | 0   | 男   | 南京  | 2022-05-09 |

## 参数io

填写完整的文件路径作为io的取值。也可以使用相对路径

```
pd.read_excel(r"/Users/peter/Desktop/pandas/Pandas-Excel.xls")

```

|     | name | age | sex | address | date |
| --- | --- | --- | --- | --- | --- |
| 0   | 张三  | 23  | 男   | 深圳  | 2022-04-01 |
| 1   | 李四  | 16  | 男   | 广州  | 2022-04-02 |
| 2   | 小明  | 26  | 未知  | 深圳  | 2022-04-05 |
| 3   | 张飞  | 28  | 女   | 苏州  | 2021-09-08 |
| 4   | 小苏  | 20  | 女   | NaN | 2022-06-07 |
| 5   | 小王  | 0   | 男   | 南京  | 2022-05-09 |

## 参数sheet_name

```
# pd.read_excel("Pandas-Excel.xls", sheet_name=0) # 效果同上
# 直接指定sheet的名字
pd.read_excel("Pandas-Excel.xls", sheet_name="Sheet1") # 效果同上

```

|     | name | age | sex | address | date |
| --- | --- | --- | --- | --- | --- |
| 0   | 张三  | 23  | 男   | 深圳  | 2022-04-01 |
| 1   | 李四  | 16  | 男   | 广州  | 2022-04-02 |
| 2   | 小明  | 26  | 未知  | 深圳  | 2022-04-05 |
| 3   | 张飞  | 28  | 女   | 苏州  | 2021-09-08 |
| 4   | 小苏  | 20  | 女   | NaN | 2022-06-07 |
| 5   | 小王  | 0   | 男   | 南京  | 2022-05-09 |

换成读取第二个sheet：名称是Sheet2

```
pd.read_excel("Pandas-Excel.xls", sheet_name="Sheet2") 

```

|     | index | name | age | sex | address | date |
| --- | --- | --- | --- | --- | --- | --- |
| 0   | 1   | 张三  | 23  | 男   | 深圳  | 2022-04-01 |
| 1   | 2   | 李四  | 16  | 男   | 广州  | 2022-04-02 |
| 2   | 3   | 小明  | 26  | 未知  | 深圳  | 2022-04-05 |
| 3   | 4   | 张飞  | 28  | 女   | 苏州  | 2021-09-08 |
| 4   | 5   | 小苏  | 20  | 女   | NaN | 2022-06-07 |
| 5   | 6   | 小王  | 0   | 男   | 南京  | 2022-05-09 |

结果中多了一列index的取值

## 参数header

```
# 和默认情况相同
pd.read_excel("Pandas-Excel.xls", header=[0]) 

```

|     | name | age | sex | address | date |
| --- | --- | --- | --- | --- | --- |
| 0   | 张三  | 23  | 男   | 深圳  | 2022-04-01 |
| 1   | 李四  | 16  | 男   | 广州  | 2022-04-02 |
| 2   | 小明  | 26  | 未知  | 深圳  | 2022-04-05 |
| 3   | 张飞  | 28  | 女   | 苏州  | 2021-09-08 |
| 4   | 小苏  | 20  | 女   | NaN | 2022-06-07 |
| 5   | 小王  | 0   | 男   | 南京  | 2022-05-09 |

```
pd.read_excel("Pandas-Excel.xls", header=[1])  # 单个元素

```

第一行的数据当做列属性：

|     | 张三  | 23  | 男   | 深圳  | 2022-04-01 00:00:00 |
| --- | --- | --- | --- | --- | --- |
| 0   | 李四  | 16  | 男   | 广州  | 2022-04-02 |
| 1   | 小明  | 26  | 未知  | 深圳  | 2022-04-05 |
| 2   | 张飞  | 28  | 女   | 苏州  | 2021-09-08 |
| 3   | 小苏  | 20  | 女   | NaN | 2022-06-07 |
| 4   | 小王  | 0   | 男   | 南京  | 2022-05-09 |

传入多个元素会形成多层索引：

```
pd.read_excel("Pandas-Excel.xls", header=[0,1])   # 多个元素

```

|     | name | age | sex | address | date |
| --- | --- | --- | --- | --- | --- |
|     | 张三  | 23  | 男   | 深圳  | 2022-04-01 00:00:00 |
| --- | --- | --- | --- | --- | --- |
| 0   | 李四  | 16  | 男   | 广州  | 2022-04-02 |
| 1   | 小明  | 26  | 未知  | 深圳  | 2022-04-05 |
| 2   | 张飞  | 28  | 女   | 苏州  | 2021-09-08 |
| 3   | 小苏  | 20  | 女   | NaN | 2022-06-07 |
| 4   | 小王  | 0   | 男   | 南京  | 2022-05-09 |

## 参数names

```
# 指定列名称
pd.read_excel("Pandas-Excel.xls", names=["a","b","c","d","e"])   

```

|     | a   | b   | c   | d   | e   |
| --- | --- | --- | --- | --- | --- |
| 0   | 张三  | 23  | 男   | 深圳  | 2022-04-01 |
| 1   | 李四  | 16  | 男   | 广州  | 2022-04-02 |
| 2   | 小明  | 26  | 未知  | 深圳  | 2022-04-05 |
| 3   | 张飞  | 28  | 女   | 苏州  | 2021-09-08 |
| 4   | 小苏  | 20  | 女   | NaN | 2022-06-07 |
| 5   | 小王  | 0   | 男   | 南京  | 2022-05-09 |

## 参数index_col

```
# 指定单个元素作为索引
pd.read_excel("Pandas-Excel.xls", index_col=[0]) 

```

|     | age | sex | address | date |
| --- | --- | --- | --- | --- |
| name |     |     |     |     |
| --- | --- | --- | --- | --- |
| 张三  | 23  | 男   | 深圳  | 2022-04-01 |
| 李四  | 16  | 男   | 广州  | 2022-04-02 |
| 小明  | 26  | 未知  | 深圳  | 2022-04-05 |
| 张飞  | 28  | 女   | 苏州  | 2021-09-08 |
| 小苏  | 20  | 女   | NaN | 2022-06-07 |
| 小王  | 0   | 男   | 南京  | 2022-05-09 |

```
# 多个元素
pd.read_excel("Pandas-Excel.xls", index_col=[0,1])   

```

|     |     | sex | address | date |
| --- | --- | --- | --- | --- |
| name | age |     |     |     |
| --- | --- | --- | --- | --- |
| 张三  | 23  | 男   | 深圳  | 2022-04-01 |
| 李四  | 16  | 男   | 广州  | 2022-04-02 |
| 小明  | 26  | 未知  | 深圳  | 2022-04-05 |
| 张飞  | 28  | 女   | 苏州  | 2021-09-08 |
| 小苏  | 20  | 女   | NaN | 2022-06-07 |
| 小王  | 0   | 男   | 南京  | 2022-05-09 |

## 参数usecols

```
pd.read_excel("Pandas-Excel.xls", usecols=[0])   # 单个字段

```

|     | name |
| --- | --- |
| 0   | 张三  |
| 1   | 李四  |
| 2   | 小明  |
| 3   | 张飞  |
| 4   | 小苏  |
| 5   | 小王  |

```
pd.read_excel("Pandas-Excel.xls", usecols=[0,2,4])   # 多个字段

```

|     | name | sex | date |
| --- | --- | --- | --- |
| 0   | 张三  | 男   | 2022-04-01 |
| 1   | 李四  | 男   | 2022-04-02 |
| 2   | 小明  | 未知  | 2022-04-05 |
| 3   | 张飞  | 女   | 2021-09-08 |
| 4   | 小苏  | 女   | 2022-06-07 |
| 5   | 小王  | 男   | 2022-05-09 |

```
# 直接指定名称
    
pd.read_excel("Pandas-Excel.xls", usecols=["age","sex"])  

```

|     | age | sex |
| --- | --- | --- |
| 0   | 23  | 男   |
| 1   | 16  | 男   |
| 2   | 26  | 未知  |
| 3   | 28  | 女   |
| 4   | 20  | 女   |
| 5   | 0   | 男   |

```
# 传入匿名函数，字段中包含a，结果sex没有了
pd.read_excel("Pandas-Excel.xls", usecols=lambda x: "a" in x)

```

|     | name | age | address | date |
| --- | --- | --- | --- | --- |
| 0   | 张三  | 23  | 深圳  | 2022-04-01 |
| 1   | 李四  | 16  | 广州  | 2022-04-02 |
| 2   | 小明  | 26  | 深圳  | 2022-04-05 |
| 3   | 张飞  | 28  | 苏州  | 2021-09-08 |
| 4   | 小苏  | 20  | NaN | 2022-06-07 |
| 5   | 小王  | 0   | 南京  | 2022-05-09 |

## 参数dtype

```
df.dtypes  

```

```
name               object
age                 int64
sex                object
address            object
date       datetime64[ns]
dtype: object

```

从上面的结果中看到age字段，在默认情况下读取的是int64类型：

```
# 指定数据类型
df1 = pd.read_excel("Pandas-Excel.xls", dtype={"age":"float64"})
# 查看字段信息
df1.dtypes

```

```
name               object
age               float64  # 修改
sex                object
address            object
date       datetime64[ns]
dtype: object

```

## 参数engine

```
# xls 结尾
pd.read_excel("Pandas-Excel.xls", engine="xlrd")

```

|     | name | age | sex | address | date |
| --- | --- | --- | --- | --- | --- |
| 0   | 张三  | 23  | 男   | 深圳  | 2022-04-01 |
| 1   | 李四  | 16  | 男   | 广州  | 2022-04-02 |
| 2   | 小明  | 26  | 未知  | 深圳  | 2022-04-05 |
| 3   | 张飞  | 28  | 女   | 苏州  | 2021-09-08 |
| 4   | 小苏  | 20  | 女   | NaN | 2022-06-07 |
| 5   | 小王  | 0   | 男   | 南京  | 2022-05-09 |

```
# xlsx 结尾
pd.read_excel("Pandas-Excel.xlsx", engine="openpyxl")

```

|     | name | age | sex | address | date |
| --- | --- | --- | --- | --- | --- |
| 0   | 张三  | 23  | 男   | 深圳  | 2022-04-01 |
| 1   | 李四  | 16  | 男   | 广州  | 2022-04-02 |
| 2   | 小明  | 26  | 男   | 深圳  | 2022-04-05 |
| 3   | 张飞  | 28  | 女   | 苏州  | 2021-09-08 |
| 4   | 小苏  | 20  | 女   | 杭州  | 2022-06-07 |
| 5   | 小王  | 25  | 男   | 南京  | 2022-05-09 |

## 参数converters

```
pd.read_excel("Pandas-Excel.xlsx")  # 默认操作

```

|     | name | age | sex | address | date |
| --- | --- | --- | --- | --- | --- |
| 0   | 张三  | 23  | 男   | 深圳  | 2022-04-01 |
| 1   | 李四  | 16  | 男   | 广州  | 2022-04-02 |
| 2   | 小明  | 26  | 男   | 深圳  | 2022-04-05 |
| 3   | 张飞  | 28  | 女   | 苏州  | 2021-09-08 |
| 4   | 小苏  | 20  | 女   | 杭州  | 2022-06-07 |
| 5   | 小王  | 25  | 男   | 南京  | 2022-05-09 |

```
pd.read_excel("Pandas-Excel.xlsx", 
              usecols=[1,3],  # 1-age  3-address 数值为原索引号
              converters={0:lambda x: x+5,  # 0代表上面[1,3]中的索引号
                          1:lambda x: x + "市"
                         })

```

|     | age | address |
| --- | --- | --- |
| 0   | 28  | 深圳市 |
| 1   | 21  | 广州市 |
| 2   | 31  | 深圳市 |
| 3   | 33  | 苏州市 |
| 4   | 25  | 杭州市 |
| 5   | 30  | 南京市 |

## 参数skiprows

```
pd.read_excel("Pandas-Excel.xls")   # 默认情况

```

|     | name | age | sex | address | date |
| --- | --- | --- | --- | --- | --- |
| 0   | 张三  | 23  | 男   | 深圳  | 2022-04-01 |
| 1   | 李四  | 16  | 男   | 广州  | 2022-04-02 |
| 2   | 小明  | 26  | 未知  | 深圳  | 2022-04-05 |
| 3   | 张飞  | 28  | 女   | 苏州  | 2021-09-08 |
| 4   | 小苏  | 20  | 女   | NaN | 2022-06-07 |
| 5   | 小王  | 0   | 男   | 南京  | 2022-05-09 |

把张三和李四所在的行直接跳过：

```
pd.read_excel("Pandas-Excel.xls", skiprows=2)

```

|     | 李四  | 16  | 男   | 广州  | 2022-04-02 00:00:00 |
| --- | --- | --- | --- | --- | --- |
| 0   | 小明  | 26  | 未知  | 深圳  | 2022-04-05 |
| 1   | 张飞  | 28  | 女   | 苏州  | 2021-09-08 |
| 2   | 小苏  | 20  | 女   | NaN | 2022-06-07 |
| 3   | 小王  | 0   | 男   | 南京  | 2022-05-09 |

```
# 跳过偶数行
pd.read_excel("Pandas-Excel.xls", skiprows=lambda x: x%2 == 0)

```

|     | 张三  | 23  | 男   | 深圳  | 2022-04-01 00:00:00 |
| --- | --- | --- | --- | --- | --- |
| 0   | 小明  | 26  | 未知  | 深圳  | 2022-04-05 |
| 1   | 小苏  | 20  | 女   | NaN | 2022-06-07 |

## 参数nrows

```
# 指定读取的行数
pd.read_excel("Pandas-Excel.xls", nrows=2)

```

|     | name | age | sex | address | date |
| --- | --- | --- | --- | --- | --- |
| 0   | 张三  | 23  | 男   | 深圳  | 2022-04-01 |
| 1   | 李四  | 16  | 男   | 广州  | 2022-04-02 |

## 参数na_values

```
pd.read_excel("Pandas-Excel.xls")  # 默认

```

|     | name | age | sex | address | date |
| --- | --- | --- | --- | --- | --- |
| 0   | 张三  | 23  | 男   | 深圳  | 2022-04-01 |
| 1   | 李四  | 16  | 男   | 广州  | 2022-04-02 |
| 2   | 小明  | 26  | 未知  | 深圳  | 2022-04-05 |
| 3   | 张飞  | 28  | 女   | 苏州  | 2021-09-08 |
| 4   | 小苏  | 20  | 女   | NaN | 2022-06-07 |
| 5   | 小王  | 0   | 男   | 南京  | 2022-05-09 |

```
pd.read_excel("Pandas-Excel.xls", 
              na_values={"sex":"未知"})

```

sex字段中的未知显示成了NaN：

|     | name | age | sex | address | date |
| --- | --- | --- | --- | --- | --- |
| 0   | 张三  | 23  | 男   | 深圳  | 2022-04-01 |
| 1   | 李四  | 16  | 男   | 广州  | 2022-04-02 |
| 2   | 小明  | 26  | NaN | 深圳  | 2022-04-05 |
| 3   | 张飞  | 28  | 女   | 苏州  | 2021-09-08 |
| 4   | 小苏  | 20  | 女   | NaN | 2022-06-07 |
| 5   | 小王  | 0   | 男   | 南京  | 2022-05-09 |

## 参数keep\_default\_na

```
pd.read_excel("Pandas-Excel.xls")  # 默认keep_default_na=True

```

|     | name | age | sex | address | date |
| --- | --- | --- | --- | --- | --- |
| 0   | 张三  | 23  | 男   | 深圳  | 2022-04-01 |
| 1   | 李四  | 16  | 男   | 广州  | 2022-04-02 |
| 2   | 小明  | 26  | 未知  | 深圳  | 2022-04-05 |
| 3   | 张飞  | 28  | 女   | 苏州  | 2021-09-08 |
| 4   | 小苏  | 20  | 女   | NaN | 2022-06-07 |
| 5   | 小王  | 0   | 男   | 南京  | 2022-05-09 |

```
pd.read_excel("Pandas-Excel.xls", keep_default_na=False)

```

|     | name | age | sex | address | date |
| --- | --- | --- | --- | --- | --- |
| 0   | 张三  | 23  | 男   | 深圳  | 2022-04-01 |
| 1   | 李四  | 16  | 男   | 广州  | 2022-04-02 |
| 2   | 小明  | 26  | 未知  | 深圳  | 2022-04-05 |
| 3   | 张飞  | 28  | 女   | 苏州  | 2021-09-08 |
| 4   | 小苏  | 20  | 女   |     | 2022-06-07 |
| 5   | 小王  | 0   | 男   | 南京  | 2022-05-09 |

## 输出到excel文件

简单模拟一份数据：

```
df2 = pd.DataFrame({"num1":[1,2,3],
                   "num2":[4,5,6],
                   "num3":[7,8,9]})
df2

```

|     | num1 | num2 | num3 |
| --- | --- | --- | --- |
| 0   | 1   | 4   | 7   |
| 1   | 2   | 5   | 8   |
| 2   | 3   | 6   | 9   |

```
df2.to_excel("newdata_1.xlsx")

```

效果如下：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

```
df2.to_excel("newdata_2.xlsx",index=False)

```

不会带上索引号

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

推荐阅读

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

[机器学习-从文本分析看《解忧杂货店》](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247511150&idx=1&sn=e52c6d5a54932d8075b6250f3eda11ad&chksm=cf12bcb4f86535a2da19bf1f9a25b1e9c67e5dfa04fc35ffbfb9fd499c9d1f83c480e15ec85e&scene=21#wechat_redirect)

[可视化神器Plotly绘制桑基图](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247511083&idx=1&sn=c9e5026ab7420f171f17bc3762cb082d&chksm=cf12bcf1f86535e75041f330a766f24ceff1ceaef7d32975ba6eeb831192cd1056c78965202a&scene=21#wechat_redirect)

[Python关联规则挖掘情侣、基友、渣男和狗](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247510933&idx=1&sn=54a7dd43e340d72054205f88de0924eb&chksm=cf12bb4ff86532593aaf5f8ba6e0ce31f0d1614766943ce593c322fd054c027168771681a14f&scene=21#wechat_redirect)

[尤而小屋学习指南！](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247510942&idx=1&sn=34fc66c5fddeb8d5e733166ca0243c1e&chksm=cf12bb44f8653252a609dba04bcd6423ad8a7e95879abac3a0386ac924181f52fd446380731c&scene=21#wechat_redirect)

[基于不均衡样本的信用卡欺诈分��](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247513735&idx=1&sn=fa92c4ce51df48638d66ca0bb44c1ab7&chksm=cf12a65df8652f4b006ae446956a381edf2beadbf11eaa418b31880ff3840589d4438047586e&scene=21#wechat_redirect)

[实战项目：Python制作词云跳舞](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247513496&idx=1&sn=ef05b0a613d31bad7bd743534387e98a&chksm=cf12a542f8652c54ddaa4d916f979a94d58c8aacc81aa6a23879837181c277f2d9de954ae3c7&scene=21#wechat_redirect)

[图解10大机器学习算法](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247510418&idx=1&sn=206ebfc5741f5b6e7eaac7a92444816d&chksm=cf12b948f865305e527692d3773975229270b310294deab74efa92a2e54496adf0095af324ac&scene=21#wechat_redirect)

[kaggle实战：可视化深度探索苹果AppStores](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247509347&idx=1&sn=e3601b69be31d0299ddb56295b902b58&chksm=cf12b5b9f8653caf4d7dce76e8b4587b3cc9bc6c67a96592d67f4295d9a4921a3d1b4752d1a8&scene=21#wechat_redirect)

> 尤而小屋，一个温馨的小屋。小屋主人，一手代码谋求生存，一手掌勺享受生活，欢迎你的光临

![](../../../_resources/0_wx_fmt_png_90757d240e044b7aba2e1edb158ab791.png)

**尤而小屋**

尤而小屋，一个温馨且有爱的小屋🏡 小屋主人，一手代码谋求生存，一手掌勺享受生活，欢迎你的光临~

<a id="js_profile_article"></a>247篇原创内容

Official Account

收录于合集 #<a id="js_album_keep_read_title"></a>pandas

 <a id="js_album_keep_read_size"></a>64个

<a id="js_album_prev"></a>上一篇 <a id="js_album_keep_read_pre_title"></a>Pandas技巧：缺失值精准定位 <a id="js_album_next"></a>下一篇 <a id="js_album_keep_read_next_title"></a>基于不均衡样本的信用卡欺诈分析

People who liked this content also liked

5K+，Pandas读存CSV文件

...

尤而小屋

不看的原因

- 内容质量低
- 不看此公众号

​有了这份小抄，你还学不会Python？再也不怕不记得Python的语法了

...

编程小小

不看的原因

- 内容质量低
- 不看此公众号

整理20个Pandas统计函数

...

算法进阶

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___3a52cee5ef7f42e19.bmp"/>

Scan to Follow