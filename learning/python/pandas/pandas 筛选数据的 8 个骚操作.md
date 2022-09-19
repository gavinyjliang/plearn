# pandas 筛选数据的 8 个骚操作

<a id="profileBt"></a><a id="js_name"></a>数据分析与开发 *2021-09-08 12:10*

The following article is from Python数据科学 Author 东哥起飞

<a id="copyright_info"></a>[![](../../../_resources/0_45e7cb03f4a0438e9fa2b037cdab927e.jpg)<br>**Python数据科学** .<br>以Python为核心语言，专攻于「数据科学」领域，文章涵盖数据分析，数据挖掘，机器学习等干货内容，分享大量数据挖掘实战项目分析和讲解，以及海量的学习资源。](#)

日常用`Python`做数据分析最常用到的就是查询筛选了，按各种条件、各种维度以及组合挑出我们想要的数据，以方便我们分析挖掘。东哥总结了日常查询和筛选常用的种骚操作，供各位学习参考。本文采用`sklearn`的`boston`数据举例介绍。

```
`from sklearn import datasets
import pandas as pd
boston = datasets.load_boston()
df = pd.DataFrame(boston.data, columns=boston.feature_names)
`
```

<img width="637" height="162" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__b61f90a3a17146b8b.png"/>

## 1\. \[\]

第一种是最快捷方便的，直接在dataframe的`[]`中写筛选的条件或者组合条件。比如下面，想要筛选出大于`NOX`这变量平均值的所有数据，然后按`NOX`降序排序。

```
`df[df['NOX']>df['NOX'].mean()].sort_values(by='NOX',ascending=False).head()
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

当然，也可以使用组合条件，条件之间使用逻辑符号`& |`等。比如下面这个例子除了上面条件外再加上且条件`CHAS为1`，注意逻辑符号分开的条件要用`()`隔开。

```
`df[(df['NOX']>df['NOX'].mean())& (df['CHAS'] ==1)].sort_values(by='NOX',ascending=False).head()
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 2\. loc/iloc

除`[]`之外，`loc/iloc`应该是最常用的两种查询方法了。`loc`按标签值（列名和行索引取值）访问，`iloc`按数字索引访问，均支持单值访问或切片查询。除了可以像`[]`按条件筛选数据以外，`loc`还可以指定返回的列变量，**从行和列两个维度筛选。**比如下面这个例子，按条件筛选出数据，并筛选出指定变量，然后赋值。

```
`df.loc[(df['NOX']>df['NOX'].mean()),['CHAS']] = 2
`
```

<img width="637" height="166" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__d24a5497884a46259.png"/>

## 3\. isin

上面我们筛选条件`< > == !=`都是个范围，但很多时候是需要锁定某些具体的值的，这时候就需要`isin`了。比如我们要限定`NOX`取值只能为`0.538,0.713,0.437`中时。

```
`df.loc[df['NOX'].isin([0.538,0.713,0.437]),:].sample(5)
`
```

<img width="637" height="166" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__27992c432dc54b998.png"/>

当然，也可以做取反操作，在筛选条件前加`~`符号即可。

```
`df.loc[~df['NOX'].isin([0.538,0.713,0.437]),:].sample(5)
`
```

<img width="637" height="162" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__2c7bd3c4431b4d8a8.png"/>

## 4\. str.contains

上面的举例都是**数值大小比较**的筛选条件，除数值以外当然也有**字符串的查询需求**。`pandas`里实现字符串的模糊筛选，可以用`.str.contains()`来实现，有点像在SQL语句里用的是`like`。下面利用titanic的数据举例，筛选出人名中包含`Mrs`或者`Lily`的数据，`|`或逻辑符号在引号内。

```
`train.loc[train['Name'].str.contains('Mrs|Lily'),:].head()
`
```

<img width="637" height="114" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__8fcd9742a8f94c47b.png"/>

`.str.contains()`中还可以设置正则化筛选逻辑。

- case=True：使用case指定区分大小写
    
- na=True：就表示把有NAN的转换为布尔值True
    
- flags=re.IGNORECASE：标志传递到re模块，例如re.IGNORECASE
    
- regex=True：regex ：如果为True，则假定第一个字符串是正则表达式，否则还是字符串
    

## 5\. where/mask

在SQL里，我们知道`where`的功能是要把满足条件的筛选出来。pandas中`where`也是筛选，但用法稍有不同。`where`接受的条件需要是**布尔类型**的，如果不满足匹配条件，就被赋值为默认的`NaN`或其他指定值。举例如下，将`Sex`为`male`当作筛选条件，`cond`就是一列布尔型的Series，非male的值就都被赋值为默认的`NaN`空值了。

```
`cond = train['Sex'] == 'male'
train['Sex'].where(cond, inplace=True)
train.head()
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

也可以用`other`赋给指定值。

```
`cond = train['Sex'] == 'male'
train['Sex'].where(cond, other='FEMALE', inplace=True)
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

甚至还可以写组合条件。

```
`train['quality'] = ''
traincond1 = train['Sex'] == 'male'
cond2 = train['Age'] > 25
train['quality'].where(cond1 & cond2, other='低质量男性', inplace=True)
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

`mask`和`where`是一对操作，与`where`正好反过来。

```
`train['quality'].mask(cond1 & cond2, other='低质量男性', inplace=True)
`
```

<img width="637" height="189" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__695b7e64c72d4215b.png"/>

## 6\. query

这是一种非常优雅的筛选数据方式。所有的筛选操作都在`''`之内完成。

```
`# 常用方式
train[train.Age > 25]
# query方式
train.query('Age > 25')
`
```

上面的两种方式效果上是一样的。再比如复杂点的，加入上面的`str.contains`用法的组合条件，注意条件里有`''`时，两边要用`""`包住。

```
`train.query("Name.str.contains('William') & Age > 25")
`
```

<img width="637" height="211" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__a4bc2eb5a51842dfa.png"/>

在`query`里还可以通过`@`来设定变量。

```
`name = 'William'
train.query("Name.str.contains(@name)")
`
```

## 7\. filter

`filter`是另外一个独特的筛选功能。`filter`不筛选具体数据，而是筛选特定的行或列。它支持三种筛选方式：

- items：固定列名
    
- regex：正则表达式
    
- like：以及模糊查询
    
- axis：控制是行index或列columns的查询
    

下面举例介绍下。

```
`train.filter(items=['Age', 'Sex'])
`
```

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__02ed47877b904fa3a.png)

```
`train.filter(regex='S', axis=1) # 列名包含S的
`
```

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__d7dde7ac157e4757a.png)

```
`train.filter(like='2', axis=0) # 索引中有2的
`
```

<img width="637" height="110" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__9d65086cf8814219b.png"/>

```
`train.filter(regex='^2', axis=0).filter(like='S', axis=1)
`
```

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__88c3ae8ca87c45808.png)

## 8\. any/all

`any`方法意思是，如果至少有一个值为`True`结果便为`True`，`all`需要所有值为`True`结果才为`True`，比如下面这样。

```
`>> train['Cabin'].all()
>> False
>> train['Cabin'].any()
>> True
`
```

`any`和`all`一般是需要和其它操作配合使用的，比如查看每列的空值情况。

```
`train.isnull().any(axis=0)
`
```

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__b59d5a1e0d394e00a.png)

再比如查看含有空值的行数。

```
`>>> train.isnull().any(axis=1).sum()
>>> 708
`
```

参考：

\[1\] https://pandas.pydata.org/

\[2\] https://www.gairuo.com/p/pandas-selecting-data

\[3\] https://blog.csdn.net/weixin_43484764/article/details/89847241

\- EOF -

推荐阅读  点击标题可跳转

<ins>1、[好习惯！pandas 8 个常用的 index 设置](http://mp.weixin.qq.com/s?__biz=MzA5ODM5MDU3MA==&mid=2650873993&idx=2&sn=34e9dfe6081f4bdc48853f19f8c3bb61&chksm=8b67c7ccbc104eda797978c617fc673f5ce631972f08e51ccbc47acba369a1b4127728e2b550&scene=21#wechat_redirect)</ins>

<ins>2、[简单实用的 pandas 技巧：如何将内存占用降低90%](http://mp.weixin.qq.com/s?__biz=MzA5ODM5MDU3MA==&mid=2650873961&idx=1&sn=34233ae4367cfd78a385a367698cc84c&chksm=8b67c72cbc104e3a6eeb581e05b1dc40cae61b93e5ae468cb6a9f88a31925c983be642f77cef&scene=21#wechat_redirect)</ins>

<ins>3、[数据分析打工人的 Pandas 75 个高频操作（收藏查询用~）](http://mp.weixin.qq.com/s?__biz=MzA5ODM5MDU3MA==&mid=2650873957&idx=2&sn=9315785842178161f0ea8febc9ba756d&chksm=8b67c720bc104e369dd9c4b787cbc3c53bdfb0bed33921d00873c90ef5f899e7aa97161e3c0e&scene=21#wechat_redirect)</ins>

看完本文有收获？请转发分享给更多人

**推荐关注「数据分析与开发」，提升数据技能**

![](../../../_resources/0_wx_fmt_png_0e6aa110eac54ff9bb1ee03c3335bd64.png)

**数据分析与开发**

「数据分析与开发」分享数据分析与开发相关技术文章、教程、工具

<a id="js_profile_article"></a>55篇原创内容

Official Account

点赞和在看就是最大的支持❤️

People who liked this content also liked

我学习 Python 的三个神级网站

数据分析与开发

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___8ccccada2228411f8.bmp"/>

Scan to Follow