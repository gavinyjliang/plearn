# pandas module

远小数 <a id="profileBt"></a><a id="js_name"></a>远小数 *2022-03-02 21:01*

Data import and analysis can be performed in Python with the **pandas** data analysis package.**pandas** is built on the **numpy** module,which provides good support for big data analysis.The pandas data analysis package contains a large number of libraries and standard data models, a large number of functions and methods to process data quickly.

<img width="657" height="289" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_1c3d19895a584093b.jpg"/>

All data types in python can be used in pandas.

We usually specify the pandas alias when we introduce the pandas package, and the introduction command is as follows.

```
`import pandas as pd
`
```

## Series Type

The series type is composed of a set of data and a set of data corresponding to the index.The Series type index is generally used as a row index.

Application Example

```
`import pandas as pd
#Create series type objects with series()
a=pd.Series([11,12,13,np.nan,15,18,19])
print(a)
`
```

where the value of np.nan is NaN, indicating that the data value is missing.

Running results:

```
`0    11.0
1    12.0
2    13.0
3     NaN
4    15.0
5    18.0
6    19.0
dtype: float64
`
```

## DataFrame Type

DataFrame is a tabular data structure with an ordered set of columns, each of which can be of a different value type, and with both rows and columns indexed. DataFrame types can usually be thought of as dictionary types consisting of Series types.

Application Example

```
`from operator import index
import pandas as pd
import numpy as np
a=np.random.randn(3,5)
print(a)
date1=pd.date_range('20220302',periods=3)
print(date1)
b=pd.DataFrame(a,index=date1)
print(b)
`
```

Running results:

```
`[[-0.18233464  0.68320663 -1.86089498  1.24787913 -0.09038954] 
 [-0.25515928 -0.73568351 -0.36083917  0.42447515 -0.22841278] 
 [-1.31412265  1.2288802   0.70799535  1.11683947 -2.22600035]]
DatetimeIndex(['2022-03-02', '2022-03-03', '2022-03-04'], dtype='datetime64[ns]', freq='D')
                   0         1         2         3         4
2022-03-02 -0.182335  0.683207 -1.860895  1.247879 -0.090390
2022-03-03 -0.255159 -0.735684 -0.360839  0.424475 -0.228413
2022-03-04 -1.314123  1.228880  0.707995  1.116839 -2.226000
`
```

Dictionary as input to DataFrame() to create an example of a two-dimensional table.

```
`import pandas as pd
import numpy as np
a=pd.DataFrame({"xh":[82454515,15151654,87894855,45451556,12316699],
"xm":['Jhon','Davas','Cancas','Sandy','Hurry'],
"cj":[90,89,88,78,92]})
print(a)
`
```

Running results:

```
`         xh      xm  cj
0  82454515    Jhon  90
1  15151654   Davas  89
2  87894855  Cancas  88
3  45451556   Sandy  78
4  12316699   Hurry  92
`
```

After defining the DateFrame() object, you can use some functions to view the information about the data table.

**Dataset base information query:**

```
`data.shape # Number of rows and columns
data.dtypes # the data types of all columns
data['id'].dtype # the data type of a column
data.ndim # the dimension of the data
data.index # row index
data.columns # Column indexes
data.values # object values
`
```

**Overall data set query:**

```
`data.head() # Show the head rows (default 5 rows)
data.tail() # Show the last few rows (default 5 rows)
data.info() # Overview of dataset-related information: indexing, column data types, non-null values, memory usage
data.describe() # Quick synthesis of statistical results
data.isnull() # See the null values for the entire data set
data['department'].isnull() # See the null value of a column
`
```

Using the dataset created above, output the basic descriptive statistics for each column.

results:

```
`                 xh         cj
count  5.000000e+00   5.000000
mean   4.865386e+07  87.400000
std    3.583079e+07   5.458938
min    1.231670e+07  78.000000
25%    1.515165e+07  88.000000
50%    4.545156e+07  89.000000
75%    8.245452e+07  90.000000
max    8.789486e+07  92.000000
`
```

## Importing external data

Use pandas to read and access local data files in .csv,.xlsx,.txt format

#### Importing .csv files

```
`a=pd.read_csv()
`
```

The following describes the common basic parameters of the syntax .

| Parameters | Functions |
| --- | --- |
| filepath\_or\_buffer | Specify the path to the file |
| sep | Specify the separator, default is ',' |
| header | Specify which line as the table header, the default setting is 0 (the first line as the table header), if there is no table header then you need to modify the parameters, set to 'header=None' |
| names | Specifies the name of the column, represented as a list. The column name can be added with this parameter when 'header=None' |
| index_col | Specify which column of data to use as a row index, either one or more columns. If it is multiple columns, you will see a hierarchical index. |
| nrows | Number of lines to be read (counting from the file header) |

#### Importing .xls files

```
`a=pd.read_excel()
`
```

#### Importing .txt files

```
`a=pd.read_table()
`
```

## Data pre-processing

In the process of big data analysis and modeling in practical applications, the use of pandas allows for efficient and fast data cleaning and preparation.

#### View Missing Values

NaN (np.nan) is usually used in pandas to represent missing numeric data.

```
`print(a.isnull())
`
```

#### Delete missing values

Missing values can be removed using the dropna() function. For DataFrame objects, the dropna() function deletes rows containing missing values by default. If you want to delete columns containing missing values, you need to pass "axis=1" as an argument. If you want to delete all rows or columns with missing values, you need to pass "how=all" as an argument.

#### Fill missing values

You can use "ObjectName.fillna()" to fill in the missing values. When actually working with data, it is common to use the number 0 to fill in missing values or to use the average value of the data to fill in missing values.

**using the number 0 to fill**

```
`a.fillna(value=0)
`
```

**Fill missing values with data averages**

```
`a.fillna(a.mean())
`
```

#### Repeat value processing

You can use "object.drop_duplicates()" to delete duplicate rows, by default, only the first row of the duplicate content will be kept, and the other rows will be deleted; you can also use the "keep='last'" parameter to specify that the last row of the duplicate content will be kept, and the other rows will be deleted.

#### Merge Data

You can use the pd.merge() function of the pandas library to merge the data of two data objects. Here we focus on setting the value of the parameter how.

The how parameter can have four types of values: inner, outer, left, right. how parameter defaults to inner, which matches and merges according to the same fields, resulting in the intersection of two data objects. When outer is used, the merged set is taken and filled with NaN. The outer join can be thought of as a concatenation of the left join and the right join. left is the left DataFrame that takes all the data, and the right DataFrame matches the left DataFrame. right is the right DataFrame that takes all the data, and the left DataFrame matches the left DataFrame.

```
`df_outer=pd.merge(df1,df2,how='outer')
`
```

#### Data Statistics

In Python, you can use the code "b.sample(n=3)" to achieve the simplest random sampling of data, and the code "b.sample(n=3,replace=False)" to set it to random sampling Use the code "b.sample(n=3,replace=True)" to set the data sampling for random sampling without putting back, and use the code "b.sample(n=3,replace=True)" to set the data sampling for random sampling after putting back.

People who liked this content also liked

spark--RDD

星海云测

不看的原因

- 内容质量低
- 不看此公众号

一文精通mysql的索引优化

程序JAVA圈

不看的原因

- 内容质量低
- 不看此公众号

MySQL管理之道，性能调优，高可用与监控（第二版）

Java全栈布道师

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___2b60f3814ac944728.bmp"/>

Scan to Follow