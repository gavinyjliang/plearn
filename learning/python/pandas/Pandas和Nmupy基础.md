# Pandas和Nmupy基础

<a id="copyright_logo"></a>Original 工程师 <a id="profileBt"></a><a id="js_name"></a>Python工程师 *2022-03-01 11:10*

收录于话题

<a id="js_article_tag_name__2290243247811690499"></a>#pandas <a id="js_article_tag_num__2290243247811690499"></a>3 <a id="js_article_tag_tips__2290243247811690499"></a>个

<a id="js_article_tag_name__2290243247761358855"></a>#numpy <a id="js_article_tag_num__2290243247761358855"></a>2 <a id="js_article_tag_tips__2290243247761358855"></a>个

<a id="js_article_tag_name__2290243248214343686"></a>#基础 <a id="js_article_tag_num__2290243248214343686"></a>2 <a id="js_article_tag_tips__2290243248214343686"></a>个

## Numpy简介

Numpy(Numerical Python) 是 Python语言的一个第三方库，支持大量的维度数组与矩阵运算，此外也针对数组运算提供大量的数学函数库。

Numpy是一个运行速度非常快的数学库，主要用于数组计算。

## Pandas

Pandas是基于NumPy数组构建的，也是Python语言的第三方库，Pandas使数据预处理、清洗、分析工作变得更快更简单，主要用于数据分析。

Pandas是专门为处理表格和混杂数据设计的，相当于Python的Excel，而Numpy更适合处理统一的数组数据。

Numpy和Pandas都是第三方库，需要预先安装好后才能导入使用，如果安装了Anaconda，则不必另外安装（因为Anaconda会自动安装很多数据分析用的第三方库）。

## 导包

```
import pandas as pd
import numpy as np
```

## 1.将字典创建为DataFrame

```
data = {"grammer":['Python', 'C', 'Java', 'GO', np.NaN, 'SQL', 'PHP', 'Python'],
       "score":[1.0, 2.0, np.NaN, 4.0, 5.0, 6.0, 7.0, 10.0]}
df = pd.DataFrame(data)
df

```

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__fa61f209da144c6fb.png)

## 2.提取含有字符串“Python”的行

```
# 方法1：
df[df['grammer']=='Python']

```

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__49b390f7dc0748d0a.png)

```
# 方法2：
results = df['grammer'].str.contains('Python')
results.fillna(value=False,inplace=True)
df[results]
```

## 3.输出df的所有列名

```
df.columns

```

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__bb25e3816edf45fd8.png)

##  4.修改第二列列名为popularity

```
df.rename(columns={'score':'popularity'},inplace=True)
df

```

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__7c275317e7af49298.png)

## 5.统计grammer列中每种编程语言出现的次数

```
df['grammer'].value_counts()

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 6.将空值用上下值的平均值填充

```
df['popularity'] = df['popularity'].fillna(df['popularity'].interpolate())
df

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 7.提取popularity列值值大于3的行

```
df[df['popularity']>3]

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 8.按照grammer列进行去除重复值

```
df.drop_duplicates(['grammer'])

```

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__8cdf4039548b4695a.png)

## 9.计算popularity列平均值

```
df['popularity'].mean()
# 4.75

```

## 10.将grammer列转换为list

```
df['grammer'].to_list()
# ['Python', 'C', 'Java', 'GO', nan, 'SQL', 'PHP', 'Python']

```

## 11.将DataFrame保存为csv

```
df.to_csv('test.csv')

```

## 12.查看数据行列数

```
df.shape
# (8, 2)

```

## 13.提取popularity列值大于3小于7的行

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 14.交换两列位置

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 15.提取popularity列最大值所在行

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__7842be79626c4f5ca.png)

## 16.随机生成20个0-100的随机整数 Numpy.random.randint()

<img width="660" height="96" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__553732ba0d0948c7a.png"/>

## 17\. 生成20个0-100固定步长的数：Numpy.arange()

<img width="660" height="196" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__6b2e38decff8465aa.png"/>

## 18.生成20个指定分布（如标准正态分布）的数：Numpy.random.normal()

<img width="660" height="106" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__c56a699732ec47be8.png"/>

## 19.提取score列中可以整除5的数字位置：Numpy.argwhere()

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 20.查看两列值相等的行号：Numpy.where()

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 21.查找第一列的局部最大值位置：Numpy.sign()

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 22.对第二列计算移动平均值：Numpy.convolve()

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 23.计算两列欧式距离：Numpy.linalg.norm()

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 24.查找某列数据中第三大值的行号：Numpy.argsort()

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 25\. numpy其他一些统计基础函数

```
np.min([1,2,3]) # 最小值
np.mean([1,2,3]) # 均值
np.median([1,2,3]) # 中位数
np.var([1,2,3]) # 方差
np.max([1,2,3]) # 最大值
np.ptp([1,2,3]) # 极差
np.std([1,2,3]) # 标准差
np.cov([1,2,3]) # 协方差
np.log1p([1,2,3]) # log(x + 1)
np.log2([1,2,3]) # 以2为底的对数
np.expm1([1,2,3]) # e的x次幂-1
np.exp([1,2,3]) # e的次数幂
np.log([1,2,3]) # 取对数
np.sqrt([1,2,3]) # 开根号
np.exp2([1,2,3]) # 平方

```

## 小作业

已知10位同学的学号以及语数英三科成绩如下：（都是数值型数据）

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__8bd65df4cd1d4d8d8.png)

\# 要求：计算出每位同学的

#总成绩(SumScore)、

#平均成绩(MeanScore)，

#最高成绩(MaxScore)、

#最低成绩(MinScore)、

#最高成绩与最低成绩的极差(PtpScore)、

#成绩方差(VarScore)；

#并将所有数据保存到score数据框中；将多列数据(包括学生的ID)合并到一列中，列名设置为answer，

\# 最终只保留索引id（从0到100）和answer两列，统一保留整数；

<img width="660" height="242" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__4f528f2fcde944d58.png"/>

构造数据

```
data = {
    "Id":[202001, 202002,202003,202004,202005,202006, 202007,202008,202009,202010],
    "Chinese" :[98,67,84,88,78,90,93,75,82,87],
    'Math'    :[92,80,73,76,88,78,90,82,77,69],
    'English' :[88,79,90,73,79,83,81,91,71,78],
}
StudentScore = pd.DataFrame(data)
StudentScore

```

求值

```
temp = df[["Chinese","Math","English"]]
StudentScore['SumScore'] = temp.sum(axis=1)
StudentScore['MeanScore'] = temp.mean(axis=1).astype(int)
StudentScore['MaxScore'] = temp.max(axis=1)
StudentScore['MinScore'] = temp.min(axis=1)
StudentScore['PtpScore'] = temp.max(axis=1)-temp.min(axis=1)
StudentScore['VarScore'] = temp.var(axis=1).astype(int)
StudentScore

```

保存到csv

```
df.to_csv('answer_1.csv', index=False, encoding='utf-8-sig')

```

\- EOF -

推荐阅读  点击标题可跳转

<ins>1、[Python 中有 3 个不可思议的返回功能](http://mp.weixin.qq.com/s?__biz=MzkzMDMyMzIzNw==&mid=2247483816&idx=1&sn=8f5880bd73fc5b419cb1d93e2a860ab7&chksm=c27d4fe2f50ac6f4addd7f675e509a6466993f579d68467943545ed8de852dba5f64fd39a5da&scene=21#wechat_redirect)</ins>

<ins>2、</ins><ins>10行python代码做出哪些酷炫的事情？</ins>

<ins>3、</ins><ins>相见恨晚的 Python 内置库：itertools</ins>

觉得本文对你有帮助？请分享给更多人

点赞和在看就是最大的支持❤️

People who liked this content also liked

程序猿生涯中的第一次PR！冲冲冲！

Java4ye

不看的原因

- 内容质量低
- 不看此公众号

「干货」SQL常用函数及避坑点汇总『Hive系列1』

小火龙说数据

不看的原因

- 内容质量低
- 不看此公众号

Z-Wave 700应用程序框架第三章 - Z-Wave架构

Smartlabs

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___2d254a2eaf114190a.bmp"/>

Scan to Follow