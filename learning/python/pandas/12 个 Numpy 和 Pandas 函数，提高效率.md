# 12 个 Numpy 和 Pandas 函数，提高效率

<a id="profileBt"></a><a id="js_name"></a>数据分析与开发 *2020-03-28 21:54*

（给数据分析与开发加星标，提升数据技能）

> 来源：机器之心

> 我们都知道，Numpy 是 Python 环境下的扩展程序库，支持大量的维度数组和矩阵运算；Pandas 也是 Python 环境下的数据操作和分析软件包，以及强大的数据分析库。二者在日常的数据分析中都发挥着重要作用，如果没有 Numpy 和 Pandas 的支持，数据分析将变得异常困难。但有时我们需要加快数据分析的速度，有什么办法可以帮助到我们吗？

在本文中，数据和分析工程师 Kunal Dhariwal 为我们介绍了 **12 种 Numpy 和 Pandas 函数，这些高效的函数会令数据分析更为容易、便捷**。最后，读者也可以在 GitHub 项目中找到本文所用代码的 Jupyter Notebook。

<img width="677" height="381" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__8f4922b1d3604059b.jpg"/>

项目地址：https://github.com/kunaldhariwal/12-Amazing-Pandas-NumPy-Functions

**Numpy 的 6 种高效函数**

首先从 Numpy 开始。Numpy 是用于科学计算的 Python 语言扩展包，通常包含强大的 N 维数组对象、复杂函数、用于整合 C/C++和 Fortran 代码的工具以及有用的线性代数、傅里叶变换和随机数生成能力。

除了上面这些明显的用途，Numpy 还可以用作通用数据的高效多维容器（container），定义任何数据类型。这使得 Numpy 能够实现自身与各种数据库的无缝、快速集成。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

接下来一一解析 6 种 Numpy 函数。

**argpartition()**

借助于 argpartition()，Numpy 可以找出 N 个最大数值的索引，也会将找到的这些索引输出。然后我们根据需要对数值进行排序。

```
x = np.array([12, 10, 12, 0, 6, 8, 9, 1, 16, 4, 6, 0])index_val = np.argpartition(x, -4)[-4:]
index_val
array([1, 8, 2, 0], dtype=int64)np.sort(x[index_val])
array([10, 12, 12, 16])
```

**allclose()**

allclose() 用于匹配两个数组，并得到布尔值表示的输出。如果在一个公差范围内（within a tolerance）两个数组不等同，则 allclose() 返回 False。该函数对于检查两个数组是否相似非常有用。

```
array1 = np.array([0.12,0.17,0.24,0.29])
array2 = np.array([0.13,0.19,0.26,0.31])# with a tolerance of 0.1, it should return False:
np.allclose(array1,array2,0.1)
False# with a tolerance of 0.2, it should return True:
np.allclose(array1,array2,0.2)
True
```

**clip()**

Clip() 使得一个数组中的数值保持在一个区间内。有时，我们需要保证数值在上下限范围内。为此，我们可以借助 Numpy 的 clip() 函数实现该目的。给定一个区间，则区间外的数值被剪切至区间上下限（interval edge）。

```
x = np.array([3, 17, 14, 23, 2, 2, 6, 8, 1, 2, 16, 0])np.clip(x,2,5)
array([3, 5, 5, 5, 2, 2, 5, 5, 2, 2, 5, 2])
```

**extract()**

顾名思义，extract() 是在特定条件下从一个数组中提取特定元素。借助于 extract()，我们还可以使用 and 和 or 等条件。

```
# Random integers
array = np.random.randint(20, size=12)
array
array([ 0,  1,  8, 19, 16, 18, 10, 11,  2, 13, 14,  3])#  Divide by 2 and check if remainder is 1
cond = np.mod(array, 2)==1
cond
array([False,  True, False,  True, False, False, False,  True, False, True, False,  True])# Use extract to get the values
np.extract(cond, array)
array([ 1, 19, 11, 13,  3])# Apply condition on extract directly
np.extract(((array < 3) | (array > 15)), array)
array([ 0,  1, 19, 16, 18,  2])
```

**where()**

Where() 用于从一个数组中返回满足特定条件的元素。比如，它会返回满足特定条件的数值的索引位置。Where() 与 SQL 中使用的 where condition 类似，如以下示例所示：

```
y = np.array([1,5,6,8,1,7,3,6,9])# Where y is greater than 5, returns index position
np.where(y>5)
array([2, 3, 5, 7, 8], dtype=int64),)# First will replace the values that match the condition, 
# second will replace the values that does not
np.where(y>5, "Hit", "Miss")
array(['Miss', 'Miss', 'Hit', 'Hit', 'Miss', 'Hit', 'Miss', 'Hit', 'Hit'],dtype='<U4')
```

**percentile()**

Percentile() 用于计算特定轴方向上数组元素的第 n 个百分位数。

```
a = np.array([1,5,6,8,1,7,3,6,9])print("50th Percentile of a, axis = 0 : ",  
      np.percentile(a, 50, axis =0))
50th Percentile of a, axis = 0 :  6.0b = np.array([[10, 7, 4], [3, 2, 1]])print("30th Percentile of b, axis = 0 : ",  
      np.percentile(b, 30, axis =0))
30th Percentile of b, axis = 0 :  [5.1 3.5 1.9]
```

这就是 Numpy 扩展包的 6 种高效函数，相信会为你带来帮助。接下来看一看 Pandas 数据分析库的 6 种函数。

**Pandas 数据统计包的 6 种高效函数**

Pandas 也是一个 Python 包，它提供了快速、灵活以及具有显著表达能力的数据结构，旨在使处理结构化 (表格化、多维、异构) 和时间序列数据变得既简单又直观。

<img width="677" height="423" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_249de4f216164c2d8.jpg"/>

Pandas 适用于以下各类数据:

- 具有异构类型列的表格数据，如 SQL 表或 Excel 表；
    
- 有序和无序 (不一定是固定频率) 的时间序列数据；
    
- 带有行/列标签的任意矩阵数据（同构类型或者是异构类型）；
    
- 其他任意形式的统计数据集。事实上，数据根本不需要标记就可以放入 Pandas 结构中。
    

Pandas 擅长处理的类型如下所示：

- 容易处理浮点数据和非浮点数据中的 缺失数据（用 NaN 表示）；
    
- 大小可调整性: 可以从 DataFrame 或者更高维度的对象中插入或者是删除列；
    
- 显式数据可自动对齐: 对象可以显式地对齐至一组标签内，或者用户可以简单地选择忽略标签，使 Series、 DataFrame 等自动对齐数据；
    
- 灵活的分组功能，对数据集执行拆分-应用-合并等操作，对数据进行聚合和转换；
    
- 简化将数据转换为 DataFrame 对象的过程，而这些数据基本是 Python 和 NumPy 数据结构中不规则、不同索引的数据；
    
- 基于标签的智能切片、索引以及面向大型数据集的子设定；
    
- 更加直观地合并以及连接数据集；
    
- 更加灵活地重塑、转置（pivot）数据集；
    
- 轴的分级标记 (可能包含多个标记)；
    
- 具有鲁棒性的 IO 工具，用于从平面文件 (CSV 和 delimited)、 Excel 文件、数据库中加在数据，以及从 HDF5 格式中保存 / 加载数据；
    
- 时间序列的特定功能: 数据范围的生成以及频率转换、移动窗口统计、数据移动和滞后等。
    

**read_csv(nrows=n)**

大多数人都会犯的一个错误是，在不需要.csv 文件的情况下仍会完整地读取它。如果一个未知的.csv 文件有 10GB，那么读取整个.csv 文件将会非常不明智，不仅要占用大量内存，还会花很多时间。我们需要做的只是从.csv 文件中导入几行，之后根据需要继续导入。

```
import io
import requests# I am using this online data set just to make things easier for you guys
url = "https://raw.github.com/vincentarelbundock/Rdatasets/master/csv/datasets/AirPassengers.csv"
s = requests.get(url).content# read only first 10 rows
df = pd.read_csv(io.StringIO(s.decode('utf-8')),nrows=10 , index_col=0)
```

**map()**

map( ) 函数根据相应的输入来映射 Series 的值。用于将一个 Series 中的每个值替换为另一个值，该值可能来自一个函数、也可能来自于一个 dict 或 Series。

```
# create a dataframe
dframe = pd.DataFrame(np.random.randn(4, 3), columns=list('bde'), index=['India', 'USA', 'China', 'Russia'])#compute a formatted string from each floating point value in frame
changefn = lambda x: '%.2f' % x# Make changes element-wise
dframe['d'].map(changefn)
```

**apply()**

apply() 允许用户传递函数，并将其应用于 Pandas 序列中的每个值。

```
# max minus mix lambda fn
fn = lambda x: x.max() - x.min()# Apply this on dframe that we've just created above
dframe.apply(fn)
```

**isin()**

lsin () 用于过滤数据帧。Isin () 有助于选择特定列中具有特定（或多个）值的行。

```
# Using the dataframe we created for read_csv
filter1 = df["value"].isin([112]) 
filter2 = df["time"].isin([1949.000000])df [filter1 & filter2]
```

**copy()**

Copy () 函数用于复制 Pandas 对象。当一个数据帧分配给另一个数据帧时，如果对其中一个数据帧进行更改，另一个数据帧的值也将发生更改。为了防止这类问题，可以使用 copy () 函数。

```
# creating sample series 
data = pd.Series(['India', 'Pakistan', 'China', 'Mongolia'])# Assigning issue that we face
data1= data
# Change a value
data1[0]='USA'
# Also changes value in old dataframe
data# To prevent that, we use
# creating copy of series 
new = data.copy()# assigning new values 
new[1]='Changed value'# printing data 
print(new) 
print(data)
```

**select_dtypes()**

select_dtypes() 的作用是，基于 dtypes 的列返回数据帧列的一个子集。这个函数的参数可设置为包含所有拥有特定数据类型的列，亦或者设置为排除具有特定数据类型的列。

```
# We'll use the same dataframe that we used for read_csv
framex =  df.select_dtypes(include="float64")# Returns only time column
```

最后，pivot\_table( ) 也是 Pandas 中一个非常有用的函数。如果对 pivot\_table( ) 在 excel 中的使用有所了解，那么就非常容易上手了。

```
# Create a sample dataframe
school = pd.DataFrame({'A': ['Jay', 'Usher', 'Nicky', 'Romero', 'Will'], 
      'B': ['Masters', 'Graduate', 'Graduate', 'Masters', 'Graduate'], 
      'C': [26, 22, 20, 23, 24]})# Lets create a pivot table to segregate students based on age and course
table = pd.pivot_table(school, values ='A', index =['B', 'C'], 
                         columns =['B'], aggfunc = np.sum, fill_value="Not Available") 
table
```

*原文链接：https://towardsdatascience.com/12-amazing-pandas-numpy-functions-22e5671a45b8*

推荐阅读  点击标题可跳转

[70 道 NumPy 测试题](http://mp.weixin.qq.com/s?__biz=MzA5ODM5MDU3MA==&mid=2650865665&idx=1&sn=70ad8c6e1c0b319c0f84f7d4e29f8db8&chksm=8b67e744bc106e52aca01bd8466ccd02edf823951a09741723cf14a9b6d37000df27fd6998ec&scene=21#wechat_redirect)

[Pandas技巧：万能转格式、轻松合并、压缩数据，让数据分析更高效](http://mp.weixin.qq.com/s?__biz=MzA5ODM5MDU3MA==&mid=2650865901&idx=2&sn=b227a2b0fc91637261b5b82bf7c2cc5d&chksm=8b67e7a8bc106ebe6122c1c4ac9f04cd301b3cb0eba2949f446e1d03b8e1d882cf76400e08ca&scene=21#wechat_redirect)

看完本文有收获？请转发分享给更多人

**关注「数据分析与开发」加星标，提升数据技能**

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__180cd9f03a7343d0b.jpg)

好文章，我在看❤️

People who liked this content also liked

我学习 Python 的三个神级网站

数据分析与开发

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___9c63c1188d294655a.bmp"/>

Scan to Follow