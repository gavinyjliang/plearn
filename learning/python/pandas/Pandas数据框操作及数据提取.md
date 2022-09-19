# Pandas数据框操作及数据提取

<a id="copyright_logo"></a>Original 工程师 <a id="profileBt"></a><a id="js_name"></a>Python工程师 *2022-03-04 07:30*

收录于话题

<a id="js_article_tag_name__2290579203408363526"></a>#字符串 <a id="js_article_tag_num__2290579203408363526"></a>1 <a id="js_article_tag_tips__2290579203408363526"></a>个

<a id="js_article_tag_name__2290243247811690499"></a>#pandas <a id="js_article_tag_num__2290243247811690499"></a>3 <a id="js_article_tag_tips__2290243247811690499"></a>个

<a id="js_article_tag_name__2290243248214343686"></a>#基础 <a id="js_article_tag_num__2290243248214343686"></a>2 <a id="js_article_tag_tips__2290243248214343686"></a>个

目录：

## 

1\. 数据框行列操作

2\. 数据读取与保存

3\. 提取指定行列的数据

4\. 提取重复值所在的行列数据

5\. 按指定条件提取元素值

6\. 提取含空值的行列

7.  提取某列不是数值或(包含)字符串的行

8. 其他提取操作

9. 小作业

```
#导包
import pandas as pd
import numpy as np

```

1

数据框行列操作

## 1.1 创建DataFrame

```
`data = {"col1":['Python', 'C', 'Java', 'R', 'SQL', 'PHP', 'Python', 'Java', 'C', 'Python'],
       "col2":[6, 2, 6, 4, 2, 5, 8, 10, 3, 4], 
       "col3":[4, 2, 6, 2, 1, 2, 2, 3, 3, 6]}
df = pd.DataFrame(data)
df
`
```

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__1cbee6314a5745b1a.png)

## 1.2 设置索引

```
`df['new_index'] = range(1,11)
df.set_index('new_index')
`
```

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__18a406ec68a44a3e8.png)

## 1.3 重置索引(行号)

```
`df.reset_index(drop = True, inplace = True) # drop = True：原有索引就不会成为新的列
df
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 1.4 更改列名

```
`#方法一：直接法
df.columns = ['grammer', 'score', 'cycle', 'id']
 
#方法二：(使用rename()函数：修改指定修改某列或某几列名字)
df.rename(columns={'col1':'grammer', 'col2':'score', 'col3':'cycle','new_index':'id'}, inplace=True)
`
```

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__c873e296fa434fcca.png)

## 1.5 调整列顺序

### (1) 将所有列倒序排列

```
`#方法一：
df.iloc[:, ::-1]
 
#方法二
df.iloc[:, [-1,-2,-3,-4]]
`
```

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__bc198b3820c04157a.png)

### (2) 交换两列位置

```
`temp = df['grammer']
df.drop(labels=['grammer'], axis=1, inplace=True)
df.insert(1, 'grammer', temp)
df
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 

### (3) 更改全部列顺序

```
`order = df.columns[[0, 3, 1, 2]] # 或者order = ['xx', 'xx',...] 具体列名
df = df[order]
df
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 1.6 删除行列

(1) 删除id这一列

```
`# 法一：
del df['id']
# 法二：
df['id'] = range(1,11)
df.drop('id',axis=1, inplace=True) #columns=['xxx']
`
```

### (2) 添加一行grammer='css'数据，并删除该行

```
`df.loc[len(df)] = [2, 'css', 3]
index = df[df['grammer'] == 'css'].index[0]
df.drop(labels=[index], inplace=True)
`
```

## 1.7 将grammer列和score列合并成新的一列

```
`df['new_col'] = df['grammer'] + df['score'].map(str) # score为int类型，需转换为字符串类型；
df
`
```

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__5d1996b314c8491aa.png)

## 1.8 将数据按行的方式逆序输出

```
`df.iloc[::-1, :]
# [::-1]表示步长为-1, 从后往前倒序输出
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

2

 数据读取与保存

## 2.1 读取excel文件

```
`excel = pd.read_excel('pandas120.xlsx')
excel.head()
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 2.2 读取csv文件

```
`csv = pd.read_csv('/drinks.csv')
csv.head()
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 2.3 读取tsv文件

```
`tsv = pd.read_csv('chipotle.tsv', sep = '\t')
tsv.head()
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 2.4 dataframe保存为csv文件

```
`df.to_csv('course.csv')
`
```

## 2.5 读取时设置显示行列的参数：pd.set_option()

### (1) 显示所有列

```
`pd.set_option('display.max_columns', None)
pd.set_option('display.max_columns', 5) #最多显示5列
`
```

### (2) 显示所有行

```
`pd.set_option('display.max_rows', None)
pd.set_option('display.max_rows', 10)#最多显示10行
`
```

### (3) 显示小数位数

```
`pd.set_option('display.float_format',lambda x: '%.2f'%x) #两位
`
```

### (4) 显示宽度

```
`pd.set_option('display.width', 100)
`
```

### (5) 设置小数点后的位数

```
`pd.set_option('precision', 1)
`
```

### (6) 是否换行显示

```
`pd.set_option('expand_frame_repr', False)
# True就是可以换行显示。设置成False的时候不允许换行
`
```

3

 提取指定行列的数据

```
`# 读取pandas120数据文件
df = pd.read_excel('pandas120.xlsx')
df.head()
`
```

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__c5a00fca37824334b.png)

## 3.1 提取第32行数据

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__d9fa553997894bb79.png)

## 3.2 提取education这一列数据

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 3.3 提取后两列(education, salary)数据

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 3.4 提取第一列位置在1,10,15上的值

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

4

提取重复值所在的行列数据

## 4.1 判断createTime列数据是否重复

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 4.2 判断数据框中所有行是否存在重复

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 4.3 判断education列和salary列数据是否重复(多列组合查询)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 4.4 判断重复索引所在行列数据

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

5

 按指定条件提取元素值

```
`import random
df['value'] = [random.randint(1,100) for i in range(len(df))]
df.head()
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 5.1 提取value列元素值大于90的行

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 5.2 提取value列元素值大于60小于70的行

<img width="660" height="358" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__548cd7766f284a18b.png"/>

## 5.3 提取某列最大值所在的行

<img width="660" height="123" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__a88b924beec9462da.png"/>

## 5.4 提取value和value1之和大于150的最后三行

<img width="660" height="259" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__5e51e343a5b5440c9.png"/>

6

提取含空值的行列

```
`df.loc[[2,10,45,87], 'value'] = np.nan
df.loc[[19,30,55,97,114], 'value1'] = np.nan
df.loc[[24,52,67,120], 'education'] = 111
df.loc[[8,26,84], 'salary'] = '--'
`
```

## 6.1 提取value列含有空值的行

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 6.2 提取每列缺失值的具体行数

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

7

提取某列不是数值或(包含)字符串的行

## 7.1 提取education列数值类型不是字符串的行

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 7.2 提取salary列包含字符串('--')的行

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 7.3 提取education列值为'硕士'的行

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

8

其他提取操作

## 8.1 提取学历为本科和硕士的数据，只显示学历和薪资两列

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 8.2 提取salary列以'25k'开头的行

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 8.3 提取value列中不在value1列出现的数字

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 8.4 提取value列和value1列出现频率最高的数字

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 8.5 提取value列中可以整除10的数字位置

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__b8ba77fb39f74b999.png)

9

小作业

```
`# 读取pandas120数据文件
df = pd.read_excel('pandas120.xlsx')
df.head()
`
```

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__969d60e548454059b.png)

## 9.1 提取学历为本科，工资在25k-35k的数据

```
`df1 = df[(df['education']=='本科') & (df['salary']=='25k-35k')]
df1
`
```

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__6ced13bf479341ea9.png)

## 9.2  提取salary列中以'40k'结尾的数据

```
`df2 = df[df['salary'].str.endswith('40k')]
df2
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 9.3  提取薪资区间中最低薪资与最高薪资的平均值大于30k的行，只需提取原始字段('createTime', 'education', 'salary')即可

```
`df3 = df['salary'].str.replace("k","").str.split("-",expand=True)
results = ((df3[0].map(int)+df3[1].map(int))/2>30)
results.fillna(value=False, inplace=True)
df3 = df[results]
df3
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 9.4  将以上三题提取出来的行按照相同列进行合并，汇总到一个数据框中

```
`answer_2 = pd.concat([df1, df2, df3], axis=0)
answer_2
`
```

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__ae03e25c7e27461e9.png)

## 9.5  将三列数据合并成一列，并设置列名为answer，最后保留id(数据行数、answer)

```
`data = pd.concat([answer_2.iloc[:,0],answer_2.iloc[:,1],answer_2.iloc[:,2]])
df = pd.DataFrame(data, columns=['answer'])
df['id'] = range(len(df))
df = df[['id', 'answer']]
df
`
```

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__0423377d97284cd9b.png)

## 结果保存为 csv 文件

```
`df.to_csv('answer_2.csv', index=False, encoding='utf-8-sig')
`
```

\- EOF -

推荐阅读  点击标题可跳转

<ins>1、[Pandas和Nmupy基础](http://mp.weixin.qq.com/s?__biz=MzkzMDMyMzIzNw==&mid=2247483912&idx=1&sn=41aa73c82e71466483adbb1d8c2b35a6&chksm=c27d4c42f50ac5548dfbef79a5b7d85011964ee36579e64e807606badbdf5395320913d15ce1&scene=21#wechat_redirect)</ins>

<ins>2、</ins><ins>Python 中有 3 个不可思议的返回功能</ins>

<ins>3、</ins><ins>10行python代码做出哪些酷炫的事情？</ins>

觉得本文对你有帮助？请分享给更多人

点赞和在看就是最大的支持❤️

People who liked this content also liked

Proxy 和 Reflect

前端很美

不看的原因

- 内容质量低
- 不看此公众号

python数据可视化--matplotlib绘制折线图(2)

AI小白笔记

不看的原因

- 内容质量低
- 不看此公众号

Elasticsearch入门与实战

爪哇缪斯

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___683bee4bfbe748418.bmp"/>

Scan to Follow