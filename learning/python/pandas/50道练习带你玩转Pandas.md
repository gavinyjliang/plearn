# 50道练习带你玩转Pandas

王大毛 <a id="profileBt"></a><a id="js_name"></a>机器学习初学者 *2020-01-11 11:30*

> 作者：王大毛，和鲸社区
> 
> 出处：https://www.kesci.com/home/project/5ddc974ef41512002cec1dca
> 
> 修改：黄海广

Pandas 是基于 NumPy 的一种数据处理工具，该工具为了解决数据分析任务而创建。Pandas 纳入了大量库和一些标准的数据模型，提供了高效地操作大型数据集所需的函数和方法。这些练习着重DataFrame和Series对象的基本操作，包括数据的索引、分组、统计和清洗。

本文的代码可以到github下载：

https://github.com/fengdu78/Data-Science-Notes/tree/master/3.pandas/4.Pandas50

## 基本操作

### 1.导入 Pandas 库并简写为 pd，并输出版本号

```
import pandas as pd
pd.__version__

```

```
'0.22.0'

```

### 2\. 从列表创建 Series

```
arr = [0, 1, 2, 3, 4]
df = pd.Series(arr) # 如果不指定索引，则默认从 0 开始
df

```

```
0    0
1    1
2    2
3    3
4    4
dtype: int64

```

### 3\. 从字典创建 Series

```
d = {'a':1,'b':2,'c':3,'d':4,'e':5}
df = pd.Series(d)
df

```

```
a    1
b    2
c    3
d    4
e    5
dtype: int64

```

### 4\. 从 NumPy 数组创建 DataFrame

```
import numpy as np
dates = pd.date_range('today', periods=6)  # 定义时间序列作为 index
num_arr = np.random.randn(6, 4)  # 传入 numpy 随机数组
columns = ['A', 'B', 'C', 'D']  # 将列表作为列名
df = pd.DataFrame(num_arr, index=dates, columns=columns)
df

```

|     | A   | B   | C   | D   |
| --- | --- | --- | --- | --- |
| 2020-01-10 22:46:01.642021 | 0.277099 | 0.665053 | 0.882637 | -0.598895 |
| 2020-01-11 22:46:01.642021 | 0.365233 | -2.529804 | -0.699849 | 0.159623 |
| 2020-01-12 22:46:01.642021 | -0.831850 | -2.099049 | -0.976407 | -0.342800 |
| 2020-01-13 22:46:01.642021 | 0.680800 | 1.682999 | 0.144469 | -2.503013 |
| 2020-01-14 22:46:01.642021 | -0.413880 | 0.876169 | -1.047877 | 0.996865 |
| 2020-01-15 22:46:01.642021 | 1.373956 | 0.029732 | -0.549268 | -0.287584 |

### 5\. 从CSV中创建 DataFrame，分隔符为“；”，编码格式为gbk

```
df = pd.read_csv('test.csv', encoding='gbk', sep=';')
```

```
6. 从字典对象创建DataFrame，并设置索引
```

```
import numpy as np
data = {
    'animal':
    ['cat', 'cat', 'snake', 'dog', 'dog', 'cat', 'snake', 'cat', 'dog', 'dog'],
    'age': [2.5, 3, 0.5, np.nan, 5, 2, 4.5, np.nan, 7, 3],
    'visits': [1, 3, 2, 3, 2, 3, 1, 1, 2, 1],
    'priority':
    ['yes', 'yes', 'no', 'yes', 'no', 'no', 'no', 'yes', 'no', 'no']
}
labels = ['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i', 'j']
df = pd.DataFrame(data, index=labels)
df

```

|     | age | animal | priority | visits |
| --- | --- | --- | --- | --- |
| a   | 2.5 | cat | yes | 1   |
| b   | 3.0 | cat | yes | 3   |
| c   | 0.5 | snake | no  | 2   |
| d   | NaN | dog | yes | 3   |
| e   | 5.0 | dog | no  | 2   |
| f   | 2.0 | cat | no  | 3   |
| g   | 4.5 | snake | no  | 1   |
| h   | NaN | cat | yes | 1   |
| i   | 7.0 | dog | no  | 2   |
| j   | 3.0 | dog | no  | 1   |

### 7\. 显示df的基础信息，包括行的数量；列名；每一列值的数量、类型

```
df.info()
# 方法二
# df.describe()

```

```
<class 'pandas.core.frame.DataFrame'>
Index: 10 entries, a to j
Data columns (total 4 columns):
age         8 non-null float64
animal      10 non-null object
priority    10 non-null object
visits      10 non-null int64
dtypes: float64(1), int64(1), object(2)
memory usage: 400.0+ bytes

```

### 8\. 展示df的前3行

```
df.iloc[:3]
# 方法二
#df.head(3)

```

|     | age | animal | priority | visits |
| --- | --- | --- | --- | --- |
| a   | 2.5 | cat | yes | 1   |
| b   | 3.0 | cat | yes | 3   |
| c   | 0.5 | snake | no  | 2   |

### 9\. 取出df的animal和age列

```
df.loc[:, ['animal', 'age']]
# 方法二
# df[['animal', 'age']]

```

|     | animal | age |
| --- | --- | --- |
| a   | cat | 2.5 |
| b   | cat | 3.0 |
| c   | snake | 0.5 |
| d   | dog | NaN |
| e   | dog | 5.0 |
| f   | cat | 2.0 |
| g   | snake | 4.5 |
| h   | cat | NaN |
| i   | dog | 7.0 |
| j   | dog | 3.0 |

### 10\. 取出索引为\[3, 4, 8\]行的animal和age列

```
df.loc[df.index[[3, 4, 8]], ['animal', 'age']]

```

|     | animal | age |
| --- | --- | --- |
| d   | dog | NaN |
| e   | dog | 5.0 |
| i   | dog | 7.0 |

### 11\. 取出age值大于3的行

```
df[df['age'] > 3]

```

|     | age | animal | priority | visits |
| --- | --- | --- | --- | --- |
| e   | 5.0 | dog | no  | 2   |
| g   | 4.5 | snake | no  | 1   |
| i   | 7.0 | dog | no  | 2   |

### 12\. 取出age值缺失的行

```
df[df['age'].isnull()]

```

|     | age | animal | priority | visits |
| --- | --- | --- | --- | --- |
| d   | NaN | dog | yes | 3   |
| h   | NaN | cat | yes | 1   |

### 13.取出age在2,4间的行（不含）

```
df[(df['age']>2) & (df['age']>4)]
# 方法二
# df[df['age'].between(2, 4)]

```

|     | age | animal | priority | visits |
| --- | --- | --- | --- | --- |
| e   | 5.0 | dog | no  | 2   |
| g   | 4.5 | snake | no  | 1   |
| i   | 7.0 | dog | no  | 2   |

### 14\. f 行的age改为1.5

```
df.loc['f', 'age'] = 1.5

```

### 15\. 计算visits的总和

```
df['visits'].sum()

```

```
19

```

### 16\. 计算每个不同种类animal的age的平均数

```
df.groupby('animal')['age'].mean()

```

```
animal
cat      2.333333
dog      5.000000
snake    2.500000
Name: age, dtype: float64

```

### 17\. 在df中插入新行k，然后删除该行

```
#插入
df.loc['k'] = [5.5, 'dog', 'no', 2]
# 删除
df = df.drop('k')
df

```

|     | age | animal | priority | visits |
| --- | --- | --- | --- | --- |
| a   | 2.5 | cat | yes | 1   |
| b   | 3.0 | cat | yes | 3   |
| c   | 0.5 | snake | no  | 2   |
| d   | NaN | dog | yes | 3   |
| e   | 5.0 | dog | no  | 2   |
| f   | 1.5 | cat | no  | 3   |
| g   | 4.5 | snake | no  | 1   |
| h   | NaN | cat | yes | 1   |
| i   | 7.0 | dog | no  | 2   |
| j   | 3.0 | dog | no  | 1   |

### 18\. 计算df中每个种类animal的数量

```
df['animal'].value_counts()

```

```
dog      4
cat      4
snake    2
Name: animal, dtype: int64

```

### 19\. 先按age降序排列，后按visits升序排列

```
df.sort_values(by=['age', 'visits'], ascending=[False, True])

```

|     | age | animal | priority | visits |
| --- | --- | --- | --- | --- |
| i   | 7.0 | dog | no  | 2   |
| e   | 5.0 | dog | no  | 2   |
| g   | 4.5 | snake | no  | 1   |
| j   | 3.0 | dog | no  | 1   |
| b   | 3.0 | cat | yes | 3   |
| a   | 2.5 | cat | yes | 1   |
| f   | 1.5 | cat | no  | 3   |
| c   | 0.5 | snake | no  | 2   |
| h   | NaN | cat | yes | 1   |
| d   | NaN | dog | yes | 3   |

### 20\. 将priority列中的yes, no替换为布尔值True, False

```
df['priority'] = df['priority'].map({'yes': True, 'no': False})
df

```

|     | age | animal | priority | visits |
| --- | --- | --- | --- | --- |
| a   | 2.5 | cat | True | 1   |
| b   | 3.0 | cat | True | 3   |
| c   | 0.5 | snake | False | 2   |
| d   | NaN | dog | True | 3   |
| e   | 5.0 | dog | False | 2   |
| f   | 1.5 | cat | False | 3   |
| g   | 4.5 | snake | False | 1   |
| h   | NaN | cat | True | 1   |
| i   | 7.0 | dog | False | 2   |
| j   | 3.0 | dog | False | 1   |

### 21\. 将animal列中的snake替换为python

```
df['animal'] = df['animal'].replace('snake', 'python')
df

```

|     | age | animal | priority | visits |
| --- | --- | --- | --- | --- |
| a   | 2.5 | cat | True | 1   |
| b   | 3.0 | cat | True | 3   |
| c   | 0.5 | python | False | 2   |
| d   | NaN | dog | True | 3   |
| e   | 5.0 | dog | False | 2   |
| f   | 1.5 | cat | False | 3   |
| g   | 4.5 | python | False | 1   |
| h   | NaN | cat | True | 1   |
| i   | 7.0 | dog | False | 2   |
| j   | 3.0 | dog | False | 1   |

### 22\. 对每种animal的每种不同数量visits，计算平均age，即，返回一个表格，行是aniaml种类，列是visits数量，表格值是行动物种类列访客数量的平均年龄

```
df.pivot_table(index='animal', columns='visits', values='age', aggfunc='mean')

```

| visits | 1   | 2   | 3   |
| --- | --- | --- | --- |
| animal |     |     |     |
| --- | --- | --- | --- |
| cat | 2.5 | NaN | 2.25 |
| dog | 3.0 | 6.0 | NaN |
| python | 4.5 | 0.5 | NaN |

### 进阶操作

### 23\. 有一列整数列A的DatraFrame，删除数值重复的行

```
df = pd.DataFrame({'A': [1, 2, 2, 3, 4, 5, 5, 5, 6, 7, 7]})
print(df)
df1 = df.loc[df['A'].shift() != df['A']]
# 方法二
# df1 = df.drop_duplicates(subset='A')
print(df1)

```

```
 A
0   1
1   2
2   2
3   3
4   4
5   5
6   5
7   5
8   6
9   7
10  7
   A
0  1
1  2
3  3
4  4
5  5
8  6
9  7
```

### 24\. 一个全数值DatraFrame，每个数字减去该行的平均数

```
df = pd.DataFrame(np.random.random(size=(5, 3)))
print(df)
df1 = df.sub(df.mean(axis=1), axis=0)
print(df1)

```

```
 0         1         2
0  0.465407  0.152497  0.861174
1  0.623682  0.627339  0.495652
2  0.835176  0.862376  0.693047
3  0.319698  0.306709  0.654063
4  0.234855  0.194232  0.438597
          0         1         2
0 -0.027619 -0.340529  0.368148
1  0.041457  0.045115 -0.086572
2  0.038310  0.065509 -0.103819
3 -0.107125 -0.120114  0.227239
4 -0.054373 -0.094996  0.149368
```

### 25\. 一个有5列的DataFrame，求哪一列的和最小

```
df = pd.DataFrame(np.random.random(size=(5, 5)), columns=list('abcde'))
print(df)
df.sum().idxmin()

```

```
 a         b         c         d         e
0  0.653658  0.730994  0.223025  0.456730  0.288283
1  0.937546  0.640995  0.197359  0.671524  0.006035
2  0.392762  0.174955  0.053928  0.318634  0.464534
3  0.741499  0.197861  0.988105  0.633780  0.914250
4  0.469285  0.309043  0.162127  0.032480  0.863017
'c'
```

### 26\. 给定DataFrame，求A列每个值的前3大的B的值的和

```
df = pd.DataFrame({'A': list('aaabbcaabcccbbc'),
                   'B': [12,345,3,1,45,14,4,52,54,23,235,21,57,3,87]})
print(df)
df1 = df.groupby('A')['B'].nlargest(3).sum(level=0)
print(df1)

```

```
 A    B
0   a   12
1   a  345
2   a    3
3   b    1
4   b   45
5   c   14
6   a    4
7   a   52
8   b   54
9   c   23
10  c  235
11  c   21
12  b   57
13  b    3
14  c   87
A
a    409
b    156
c    345
Name: B, dtype: int64
```

### 27\. 给定DataFrame，有列A, B，A的值在1-100（含），对A列每10步长，求对应的B的和

```
df = pd.DataFrame({
    'A': [1, 2, 11, 11, 33, 34, 35, 40, 79, 99],
    'B': [1, 2, 11, 11, 33, 34, 35, 40, 79, 99]
})
print(df)
df1 = df.groupby(pd.cut(df['A'], np.arange(0, 101, 10)))['B'].sum()
print(df1)

```

```
 A   B
0   1   1
1   2   2
2  11  11
3  11  11
4  33  33
5  34  34
6  35  35
7  40  40
8  79  79
9  99  99
A
(0, 10]        3
(10, 20]      22
(20, 30]       0
(30, 40]     142
(40, 50]       0
(50, 60]       0
(60, 70]       0
(70, 80]      79
(80, 90]       0
(90, 100]     99
Name: B, dtype: int64
```

### 28\. 给定DataFrame，计算每个元素至左边最近的0（或者至开头）的距离，生成新列y

```
df = pd.DataFrame({'X': [7, 2, 0, 3, 4, 2, 5, 0, 3, 4]})
# 方法一
x = (df['X'] != 0).cumsum()
y = x != x.shift()
df['Y'] = y.groupby((y != y.shift()).cumsum()).cumsum()
print(df)

```

```
 X    Y
0  7  1.0
1  2  2.0
2  0  0.0
3  3  1.0
4  4  2.0
5  2  3.0
6  5  4.0
7  0  0.0
8  3  1.0
9  4  2.0
```

```
# 方法二
df['Y'] = df.groupby((df['X'] == 0).cumsum()).cumcount()
first_zero_idx = (df['X'] == 0).idxmax()
df['Y'].iloc[0:first_zero_idx] += 1
print(df)

```

```
 X  Y
0  7  1
1  2  2
2  0  0
3  3  1
4  4  2
5  2  3
6  5  4
7  0  0
8  3  1
9  4  2
```

### 29\. 一个全数值的DataFrame，返回最大3个值的坐标

```
df = pd.DataFrame(np.random.random(size=(5, 3)))
print(df)
df.unstack().sort_values()[-3:].index.tolist()

```

```
 0         1         2
0  0.974321  0.454025  0.018815
1  0.323491  0.468609  0.834424
2  0.340960  0.826835  0.503252
3  0.812414  0.202745  0.965168
4  0.633172  0.270281  0.915212
[(2, 4), (2, 3), (0, 0)]
```

### 30\. 给定DataFrame，将负值代替为同组的平均值

```
df = pd.DataFrame({
    'grps':
    list('aaabbcaabcccbbc'),
    'vals': [-12, 345, 3, 1, 45, 14, 4, -52, 54, 23, -235, 21, 57, 3, 87]
})
print(df)
def replace(group):
    mask = group < 0
    group[mask] = group[~mask].mean()
    return group
df['vals'] = df.groupby(['grps'])['vals'].transform(replace)
print(df)

```

```
 grps  vals
0     a   -12
1     a   345
2     a     3
3     b     1
4     b    45
5     c    14
6     a     4
7     a   -52
8     b    54
9     c    23
10    c  -235
11    c    21
12    b    57
13    b     3
14    c    87
   grps        vals
0     a  117.333333
1     a  345.000000
2     a    3.000000
3     b    1.000000
4     b   45.000000
5     c   14.000000
6     a    4.000000
7     a  117.333333
8     b   54.000000
9     c   23.000000
10    c   36.250000
11    c   21.000000
12    b   57.000000
13    b    3.000000
14    c   87.000000
```

### 31\. 计算3位滑动窗口的平均值，忽略NAN

```
df = pd.DataFrame({
    'group': list('aabbabbbabab'),
    'value': [1, 2, 3, np.nan, 2, 3, np.nan, 1, 7, 3, np.nan, 8]
})
print(df)
g1 = df.groupby(['group'])['value']
g2 = df.fillna(0).groupby(['group'])['value']
s = g2.rolling(3, min_periods=1).sum() / g1.rolling(3, min_periods=1).count()
s.reset_index(level=0, drop=True).sort_index()

```

```
 group  value
0      a    1.0
1      a    2.0
2      b    3.0
3      b    NaN
4      a    2.0
5      b    3.0
6      b    NaN
7      b    1.0
8      a    7.0
9      b    3.0
10     a    NaN
11     b    8.0
0     1.000000
1     1.500000
2     3.000000
3     3.000000
4     1.666667
5     3.000000
6     3.000000
7     2.000000
8     3.666667
9     2.000000
10    4.500000
11    4.000000
Name: value, dtype: float64
```

## Series 和 Datetime索引

### 32\. 创建Series s，将2015所有工作日作为随机值的索引

```
dti = pd.date_range(start='2015-01-01', end='2015-12-31', freq='B')
s = pd.Series(np.random.rand(len(dti)), index=dti)
s.head(10)

```

```
2015-01-01    0.503458
2015-01-02    0.194185
2015-01-05    0.550930
2015-01-06    0.174309
2015-01-07    0.316911
2015-01-08    0.288385
2015-01-09    0.293285
2015-01-12    0.340436
2015-01-13    0.630009
2015-01-14    0.076130
Freq: B, dtype: float64

```

### 33\. 所有礼拜三的值求和

```
s[s.index.weekday == 2].sum()

```

```
27.272318047689705

```

### 34\. 求每个自然月的平均数

```
s.resample('M').mean()

```

```
2015-01-31    0.375417
2015-02-28    0.551560
2015-03-31    0.540772
2015-04-30    0.450957
2015-05-31    0.369119
2015-06-30    0.588625
2015-07-31    0.584358
2015-08-31    0.609751
2015-09-30    0.511285
2015-10-31    0.555546
2015-11-30    0.528777
2015-12-31    0.574317
Freq: M, dtype: float64

```

### 35\. 每连续4个月为一组，求最大值所在的日期

```
s.groupby(pd.Grouper(freq='4M')).idxmax()

```

```
2015-01-31   2015-01-15
2015-05-31   2015-02-04
2015-09-30   2015-06-02
2016-01-31   2015-12-08
dtype: datetime64[ns]

```

### 36\. 创建2015-2016每月第三个星期四的序列

```
pd.date_range('2015-01-01', '2016-12-31', freq='WOM-3THU')
#数据清洗
df = pd.DataFrame({'From_To': ['LoNDon_paris', 'MAdrid_miLAN', 'londON_StockhOlm',
                               'Budapest_PaRis', 'Brussels_londOn'],
              'FlightNumber': [10045, np.nan, 10065, np.nan, 10085],
              'RecentDelays': [[23, 47], [], [24, 43, 87], [13], [67, 32]],
                   'Airline': ['KLM(!)', '<Air France> (12)', '(British Airways. )',
                               '12. Air France', '"Swiss Air"']})
df

```

|     | Airline | FlightNumber | From_To | RecentDelays |
| --- | --- | --- | --- | --- |
| 0   | KLM(!) | 10045.0 | LoNDon_paris | \[23, 47\] |
| 1   | &lt;Air France&gt; (12) | NaN | MAdrid_miLAN | \[\] |
| 2   | (British Airways. ) | 10065.0 | londON_StockhOlm | \[24, 43, 87\] |
| 3   | 12\. Air France | NaN | Budapest_PaRis | \[13\] |
| 4   | "Swiss Air" | 10085.0 | Brussels_londOn | \[67, 32\] |

### 37\. FlightNumber列中有些值缺失了，他们本来应该是每一行增加10，填充缺失的数值，并且令数据类型为整数

```
df['FlightNumber'] = df['FlightNumber'].interpolate().astype(int)
df

```

|     | Airline | FlightNumber | From_To | RecentDelays |
| --- | --- | --- | --- | --- |
| 0   | KLM(!) | 10045 | LoNDon_paris | \[23, 47\] |
| 1   | &lt;Air France&gt; (12) | 10055 | MAdrid_miLAN | \[\] |
| 2   | (British Airways. ) | 10065 | londON_StockhOlm | \[24, 43, 87\] |
| 3   | 12\. Air France | 10075 | Budapest_PaRis | \[13\] |
| 4   | "Swiss Air" | 10085 | Brussels_londOn | \[67, 32\] |

### 38\. 将From\_To列从\_分开，分成From, To两列，并删除原始列

```
temp = df.From_To.str.split('_', expand=True)
temp.columns = ['From', 'To']
df = df.join(temp)
df = df.drop('From_To', axis=1)
df

```

|     | Airline | FlightNumber | RecentDelays | From | To  |
| --- | --- | --- | --- | --- | --- |
| 0   | KLM(!) | 10045 | \[23, 47\] | LoNDon | paris |
| 1   | &lt;Air France&gt; (12) | 10055 | \[\] | MAdrid | miLAN |
| 2   | (British Airways. ) | 10065 | \[24, 43, 87\] | londON | StockhOlm |
| 3   | 12\. Air France | 10075 | \[13\] | Budapest | PaRis |
| 4   | "Swiss Air" | 10085 | \[67, 32\] | Brussels | londOn |

### 39\. 将From, To大小写统一首字母大写其余小写

```
df['From'] = df['From'].str.capitalize()
df['To'] = df['To'].str.capitalize()
df

```

|     | Airline | FlightNumber | RecentDelays | From | To  |
| --- | --- | --- | --- | --- | --- |
| 0   | KLM(!) | 10045 | \[23, 47\] | London | Paris |
| 1   | &lt;Air France&gt; (12) | 10055 | \[\] | Madrid | Milan |
| 2   | (British Airways. ) | 10065 | \[24, 43, 87\] | London | Stockholm |
| 3   | 12\. Air France | 10075 | \[13\] | Budapest | Paris |
| 4   | "Swiss Air" | 10085 | \[67, 32\] | Brussels | London |

### 40\. Airline列，有一些多余的标点符号，需要提取出正确的航司名称。举例：'(British Airways. )' 应该改为 'British Airways'.

```
df['Airline'] = df['Airline'].str.extract(
    '([a-zA-Z\s]+)', expand=False).str.strip()
df

```

|     | Airline | FlightNumber | RecentDelays | From | To  |
| --- | --- | --- | --- | --- | --- |
| 0   | KLM | 10045 | \[23, 47\] | London | Paris |
| 1   | Air France | 10055 | \[\] | Madrid | Milan |
| 2   | British Airways | 10065 | \[24, 43, 87\] | London | Stockholm |
| 3   | Air France | 10075 | \[13\] | Budapest | Paris |
| 4   | Swiss Air | 10085 | \[67, 32\] | Brussels | London |

### 41\. Airline列，数据被以列表的形式录入，但是我们希望每个数字被录入成单独一列，delay\_1, delay\_2, ...没有的用NAN替代。

```
delays = df['RecentDelays'].apply(pd.Series)
delays.columns = ['delay_{}'.format(n) for n in range(1, len(delays.columns)+1)]
df = df.drop('RecentDelays', axis=1).join(delays)
df

```

|     | Airline | FlightNumber | From | To  | delay_1 | delay_2 | delay_3 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 0   | KLM | 10045 | London | Paris | 23.0 | 47.0 | NaN |
| 1   | Air France | 10055 | Madrid | Milan | NaN | NaN | NaN |
| 2   | British Airways | 10065 | London | Stockholm | 24.0 | 43.0 | 87.0 |
| 3   | Air France | 10075 | Budapest | Paris | 13.0 | NaN | NaN |
| 4   | Swiss Air | 10085 | Brussels | London | 67.0 | 32.0 | NaN |

## 层次化索引

### 42\. 用 letters = \['A', 'B', 'C'\]和 numbers = list(range(10))的组合作为系列随机值的层次化索引

```
letters = ['A', 'B', 'C']
numbers = list(range(4))
mi = pd.MultiIndex.from_product([letters, numbers])
s = pd.Series(np.random.rand(12), index=mi)
s

```

```
A  0    0.250785
   1    0.146978
   2    0.596062
   3    0.064608
B  0    0.709660
   1    0.515778
   2    0.483163
   3    0.524490
C  0    0.360434
   1    0.987620
   2    0.527151
   3    0.636960
dtype: float64

```

### 43\. 检查s是否是字典顺序排序的

```
s.index.is_lexsorted()
# 方法二
# s.index.lexsort_depth == s.index.nlevels

```

```
True

```

### 44\. 选择二级索引为1, 3的行

```
s.loc[:, [1, 3]]

```

```
A  1    0.146978
   3    0.064608
B  1    0.515778
   3    0.524490
C  1    0.987620
   3    0.636960
dtype: float64

```

### 45\. 对s进行切片操作，取一级索引至B，二级索引从2开始到最后

```
s.loc[pd.IndexSlice[:'B', 2:]]
# 方法二
# s.loc[slice(None, 'B'), slice(2, None)]

```

```
A  2    0.596062
   3    0.064608
B  2    0.483163
   3    0.524490
dtype: float64

```

### 46\. 计算每个一级索引的和（A, B, C每一个的和）

```
s.sum(level=0)
#方法二
#s.unstack().sum(axis=0)

```

```
A    1.058433
B    2.233091
C    2.512164
dtype: float64

```

### 47\. 交换索引等级，新的Series是字典顺序吗？不是的话请排序

```
new_s = s.swaplevel(0, 1)
print(new_s)
print(new_s.index.is_lexsorted())
new_s = new_s.sort_index()
print(new_s)

```

```
0  A    0.250785
1  A    0.146978
2  A    0.596062
3  A    0.064608
0  B    0.709660
1  B    0.515778
2  B    0.483163
3  B    0.524490
0  C    0.360434
1  C    0.987620
2  C    0.527151
3  C    0.636960
dtype: float64
False
0  A    0.250785
   B    0.709660
   C    0.360434
1  A    0.146978
   B    0.515778
   C    0.987620
2  A    0.596062
   B    0.483163
   C    0.527151
3  A    0.064608
   B    0.524490
   C    0.636960
dtype: float64

```

```
## 可视化
import matplotlib.pyplot as plt
df = pd.DataFrame({"xs": [1, 5, 2, 8, 1], "ys": [4, 2, 1, 9, 6]})
plt.style.use('ggplot')

```

### 48\. 画出df的散点图

```
df.plot.scatter("xs", "ys", color = "black", marker = "x")

```

```
<matplotlib.axes._subplots.AxesSubplot at 0x1f188ddacc0>

```

<img width="657" height="461" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__67fd7c6287654f359.png"/>

### 49\. 可视化指定4维DataFrame

```
df = pd.DataFrame({
    "productivity": [5, 2, 3, 1, 4, 5, 6, 7, 8, 3, 4, 8, 9],
    "hours_in": [1, 9, 6, 5, 3, 9, 2, 9, 1, 7, 4, 2, 2],
    "happiness": [2, 1, 3, 2, 3, 1, 2, 3, 1, 2, 2, 1, 3],
    "caffienated": [0, 0, 1, 1, 0, 0, 0, 0, 1, 1, 0, 1, 0]
})
df.plot.scatter(
    "hours_in", "productivity", s=df.happiness * 100, c=df.caffienated)

```

```
<matplotlib.axes._subplots.AxesSubplot at 0x1f18aea4c18>

```

<img width="657" height="461" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__8dd4792a10364338a.png"/>

### 50\. 在同一个图中可视化2组数据，共用X轴，但y轴不同

```
df = pd.DataFrame({
    "revenue": [57, 68, 63, 71, 72, 90, 80, 62, 59, 51, 47, 52],
    "advertising":
    [2.1, 1.9, 2.7, 3.0, 3.6, 3.2, 2.7, 2.4, 1.8, 1.6, 1.3, 1.9],
    "month":
    range(12)
})
ax = df.plot.bar("month", "revenue", color="green")
df.plot.line("month", "advertising", secondary_y=True, ax=ax)
ax.set_xlim((-1, 12))

```

```
(-1, 12)

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

本文的代码可以到github下载：

https://github.com/fengdu78/Data-Science-Notes/tree/master/3.pandas/4.Pandas50

<img width="677" height="394" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__915b557229034cc8b.jpg"/>

**备注：公众号菜单包含了整理了一本****AI小抄**，**非常适合在通勤路上用学习**。

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__dd6248b9f38d4c05a.png)

```


往期精彩回顾

- [2019年公众号文章精选](http://mp.weixin.qq.com/s?__biz=MzIwODI2NDkxNQ==&mid=2247487296&idx=1&sn=05c7c8195920f45c317f02b017e9be1f&chksm=970484fca0730deadd26f0edbcb9c0976920c64f1d245fd99c8359791a0c1842c6821845fbf7&scene=21#wechat_redirect)
    
- [适合初学者入门人工智能的路线及资料下载](http://mp.weixin.qq.com/s?__biz=MzIwODI2NDkxNQ==&mid=2247484737&idx=1&sn=27c52b4bc4ca98d3ab817344b84226cc&chksm=97048efda07307eb78d4f4ec0039a386a658404156b051af0cb715fafa8d2ae66cbe49343bf3&scene=21#wechat_redirect)
    
- [机器学习在线手册](http://mp.weixin.qq.com/s?__biz=MzIwODI2NDkxNQ==&mid=2247486187&idx=1&sn=3c86ade48695ce102e93fa813c55f126&chksm=97048157a07308419d900bec1ff4699092c62c7ed331a334734a4225db0b2274a7b134153037&scene=21#wechat_redirect)
    
- [深度学习在线手册](http://mp.weixin.qq.com/s?__biz=MzIwODI2NDkxNQ==&mid=2247486718&idx=1&sn=0d4db93f3dab930b5b0876d982407251&chksm=97048742a0730e544fd65df66a6ed516c2a5eca8f9501d23c40b15226d53f7f84b37943db094&scene=21#wechat_redirect)
    
- [AI基础下载（](http://mp.weixin.qq.com/s?__biz=MzIwODI2NDkxNQ==&mid=2247487183&idx=2&sn=689b5d6f5bfc8b589575a500d0591fcb&chksm=97048573a0730c65f6cc42a4df7c1e30410c833af01d1690b336def67b0db7421cde40ed6922&scene=21#wechat_redirect)[第一部分](http://mp.weixin.qq.com/s?__biz=MzIwODI2NDkxNQ==&mid=2247487183&idx=2&sn=689b5d6f5bfc8b589575a500d0591fcb&chksm=97048573a0730c65f6cc42a4df7c1e30410c833af01d1690b336def67b0db7421cde40ed6922&scene=21#wechat_redirect)[）](http://mp.weixin.qq.com/s?__biz=MzIwODI2NDkxNQ==&mid=2247487183&idx=2&sn=689b5d6f5bfc8b589575a500d0591fcb&chksm=97048573a0730c65f6cc42a4df7c1e30410c833af01d1690b336def67b0db7421cde40ed6922&scene=21#wechat_redirect)
    

备注：加入本站微信群或者qq群，请回复“加群”

## 加入知识星球（4500+用户，ID：92416895），请回复“知识星球”






```

**喜欢文章，点个****在看<img width="20" height="22" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_aff9ebeddbff431bb.jpg"/>**

People who liked this content also liked

【深度学习】离开英伟达仅19个月，他交出了一块国产全功能GPU

机器学习初学者

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___250ba57570b34652b.bmp"/>

Scan to Follow