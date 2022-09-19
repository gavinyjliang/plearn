# 10个 Python 小技巧，覆盖了90%的数据分析需求！

<a id="profileBt"></a><a id="js_name"></a>小数志 *2022-03-08 12:00*

The following article is from Python学习与数据挖掘 Author 喜欢就关注呀

<a id="copyright_info"></a>[![](../../../_resources/0_5ab8dcc7f2564748917aec04fc7d0263.jpg)<br>**Python学习与数据挖掘** .<br>点我关注，持续获取Python/数据分析/数据挖掘原创干货！](#)

数据分析师日常工作会涉及各种任务，比如数据预处理、数据分析、机器学习模型创建、模型部署。

在本文中，我将分享10个 Python 操作，它们可覆盖90%的数据分析问题。**有所收获点赞、收藏、关注。**

#### 1、阅读数据集

阅读数据是数据分析的组成部分，了解如何从不同的文件格式读取数据是数据分析师的第一步。下面是如何使用 pandas 读取包含 Covid-19 数据的 csv 文件的示例。

```
`import pandas as pd 
# reading the countries_data file along with the location within read_csv function.
countries_df = pd.read_csv('C:/Users/anmol/Desktop/Courses/Python for Data Science/Code/countries_data.csv') 
# showing the first 5 rows of the dataframe 
countries_df.head()
`
```

以下是 countries_df.head() 的输出，我们可以使用它查看数据框的前 5 行：<img width="657" height="165" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__05286057f20845768.png"/>

#### 2、汇总统计

下一步就是通过查看数据汇总来了解数据，例如 NewConfirmed、TotalConfirmed 等数字列的计数、均值、标准偏差、分位数以及国家代码等分类列的频率、最高出现值

```
`countries_df.describe()
`
```

使用 describe 函数，我们可以得到数据集连续变量的摘要，如下所示：<img width="657" height="260" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__2cbb009e05024fb0b.png"/>在 describe() 函数中，我们可以设置参数"include = 'all'"来获取连续变量和分类变量的摘要

```
`countries_df.describe(include = 'all')
`
```

<img width="657" height="226" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__c85a2de3e3744ac7b.png"/>

#### 3、数据选择和过滤

分析其实不需要数据集的所有行和列，只需要选择感兴趣的列并根据问题过滤一些行。

例如，我们可以使用以下代码选择 Country 和 NewConfirmed 列：

```
`countries_df[['Country','NewConfirmed']]
`
```

我们还可以将数据过滤Country，使用 loc，我们可以根据一些值过滤列，如下所示：

```
`countries_df.loc[countries_df['Country'] == 'United States of America']
`
```

<img width="657" height="57" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__77286878fc8e49e1b.png"/>

#### 4、聚合

计数、总和、均值等数据聚合，是数据分析最常执行的任务之一。

我们可以使用聚合找到各国的 NewConfimed 病例总数。使用 groupby 和 agg 函数执行聚合。

```
`countries_df.groupby(['Country']).agg({'NewConfirmed':'sum'})
`
```

#### 5、Join

使用 Join 操作将 2 个数据集组合成一个数据集。

例如：一个数据集可能包含不同国家/地区的 Covid-19 病例数，另一个数据集可能包含不同国家/地区的纬度和经度信息。

现在我们需要结合这两个信息，那么我们可以执行如下所示的连接操作

```
`countries_lat_lon = pd.read_excel('C:/Users/anmol/Desktop/Courses/Python for Data Science/Code/countries_lat_lon.xlsx')
# joining the 2 dataframe : countries_df and countries_lat_lon
# syntax : pd.merge(left_df, right_df, on = 'on_column', how = 'type_of_join')
joined_df = pd.merge(countries_df, countries_lat_lon, on = 'CountryCode', how = 'inner')
joined_df
`
```

#### 6、内建函数

了解数学内建函数，如 min()、max()、mean()、sum() 等，对于执行不同的分析非常有帮助。

我们可以通过调用它们直接在数据帧上应用这些函数，这些函数可以在列上或在聚合函数中独立使用，如下所示：

```
`# finding sum of NewConfirmed cases of all the countries 
countries_df['NewConfirmed'].sum()
# Output : 6,631,899
# finding the sum of NewConfirmed cases across different countries 
countries_df.groupby(['Country']).agg({'NewConfirmed':'sum'})
# Output 
#          NewConfirmed
#Country 
#Afghanistan    75
#Albania       168
#Algeria       247
#Andorra        0
#Angola        53
`
```

#### 7、用户自定义函数

我们自己编写的函数是用户自定义函数。我们可以在需要时通过调用该函数来执行这些函数中的代码。例如，我们可以创建一个函数来添加 2 个数字，如下所示：

```
`# User defined function is created using 'def' keyword, followed by function definition - 'addition()'
# and 2 arguments num1 and num2
def addition(num1, num2):
    return num1+num2
# calling the function using function name and providing the arguments 
print(addition(1,2))
#output : 3
`
```

#### 8、Pivot

Pivot 是将一列行内的唯一值转换为多个新列，这是很棒的数据处理技术。

在 Covid-19 数据集上使用 pivot_table() 函数，我们可以将国家名称转换为单独的新列：

```
`# using pivot_table to convert values within the Country column into individual columns and 
# filling the values corresponding to these columns with numeric variable - NewConfimed 
pivot_df = pd.pivot_table(countries_df,  columns = 'Country', values = 'NewConfirmed')
pivot_df
`
```

#### 9、遍历数据框

很多时候需要遍历数据框的索引和行，我们可以使用 iterrows 函数遍历数据框：

```
`# iterating over the index and row of a dataframe using iterrows() function 
for index, row in countries_df.iterrows():
    print('Index is ' + str(index))
    print('Country is '+ str(row['Country']))
  
# Output : 
# Index is 0
# Country is Afghanistan
# Index is 1
# Country is Albania
# .......
`
```

#### 10、字符串操作

很多时候我们处理数据集中的字符串列，在这种情况下，了解一些基本的字符串操作很重要。

例如如何将字符串转换为大写、小写以及如何找到字符串的长度。

```
`# country column to upper case
countries_df['Country_upper'] = countries_df['Country'].str.upper()
# country column to lower case
countries_df['CountryCode_lower']=countries_df['CountryCode'].str.lower()
# finding length of characters in the country column 
countries_df['len'] = countries_df['Country'].str.len()
countries_df.head()
`
```

```


* * *

交流群限时推广：

【小数志】公众号唯一读者交流群还有100个左右坑位，广泛招募热衷技术交流的大佬和萌新，欢迎添加个人微信：<ins>luanhz</ins>，我会拉你入群。名额有限，错过不再。




```

<img width="578" height="21" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__1c892ca37e104a33b.png"/>

相关阅读：

- [写在1024：一名数据分析师的修炼之路](http://mp.weixin.qq.com/s?__biz=MzU0OTk5MDg3OQ==&mid=2247485017&idx=1&sn=1c4b44929e0df8e67d5f2460cf2fa37c&chksm=fba63ee9ccd1b7ffe08868d924d25e8bf32bfbe08536bfaa1904fcf049c48d487b4b15bfcda6&scene=21#wechat_redirect)
    
- [数据科学系列：sklearn库主要模块功能简介](http://mp.weixin.qq.com/s?__biz=MzU0OTk5MDg3OQ==&mid=2247484865&idx=1&sn=fcd8cfbf6adb777e693177ad1c169a39&chksm=fba63d71ccd1b4678adac1d357ed8ee70740676e8e637b43cb71eef7492863d8b154c03d43c5&scene=21#wechat_redirect)
    
- [数据科学系列：seaborn入门详细教程](http://mp.weixin.qq.com/s?__biz=MzU0OTk5MDg3OQ==&mid=2247484633&idx=1&sn=bd24e3a089587674a2a6991066ff41c1&chksm=fba63c69ccd1b57f4f48ea2c82b1c79e7fdd7bfaf7c11d65764ff793bdf5ec421e7430354326&scene=21#wechat_redirect)
    
- [数据科学系列：pandas入门详细教程](http://mp.weixin.qq.com/s?__biz=MzU0OTk5MDg3OQ==&mid=2247484410&idx=1&sn=b9654d33bc46795a3c296a6e5e0c6735&chksm=fba63b4accd1b25cb1c31527b3e9dad70917092b19c8f6ffe57404fee642b3e6a3577c8eb727&scene=21#wechat_redirect)
    
- [数据科学系列：matplotlib入门详细教程](http://mp.weixin.qq.com/s?__biz=MzU0OTk5MDg3OQ==&mid=2247484405&idx=1&sn=0e3ec6923c630965d0fbdf633341d0f5&chksm=fba63b45ccd1b253282f253fac55827f723439ca067472772e346343780343f8270a4c1a5298&scene=21#wechat_redirect)
    
- [数据科学系列：numpy入门详细教程](http://mp.weixin.qq.com/s?__biz=MzU0OTk5MDg3OQ==&mid=2247484400&idx=1&sn=f317d6c11ebda49eb3e197fa3f5a5379&chksm=fba63b40ccd1b256afe19cca78f013a65271e1f138d0cae45278e3c45c4fd50042ff9471c5a7&scene=21#wechat_redirect)
    

People who liked this content also liked

C++17常用新特性(四)---聚合体扩展

CPP开发前沿

不看的原因

- 内容质量低
- 不看此公众号

StarRocks的SQL指纹应用

雷雷DBA

不看的原因

- 内容质量低
- 不看此公众号

为什么DevOps很好，但80%的公司却无法落地？

大魏分享

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___40441660eedd4df4a.bmp"/>

Scan to Follow