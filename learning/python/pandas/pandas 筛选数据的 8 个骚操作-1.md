# pandas 筛选数据的 8 个骚操作

<a id="copyright_logo"></a>Original 东哥起飞 <a id="profileBt"></a><a id="js_name"></a>Python数据科学 *2021-08-15 16:03*

<a id="js_article-tag-card__left"></a>收录于合集 #pandas骚操作 <a id="js_article-tag-card__right"></a>25个

大家好，我是东哥。

本篇是pandas骚操作系列的第 **20** 篇：**pandas筛选数据的 8 个骚操作**。系列内容，请看👉「[pandas100个骚操作](https://mp.weixin.qq.com/mp/appmsgalbum?__biz=MzUzODYwMDAzNA==&action=getalbum&album_id=1699019347278561282&scene=21#wechat_redirect)」话题。

另外，最近收到出版社送的一本新书 **《深入浅出pandas》**，内容非常赞，目前已上架各商城。当然，东哥**再****给大家争取了5本，免费包邮送出去**，参与方式见文末~

* * *

日常用`Python`做数据分析最常用到的就是查询筛选了，按各种条件、各种维度以及组合挑出我们想要的数据，以方便我们分析挖掘。

东哥总结了日常查询和筛选常用的种骚操作，供各位学习参考。本文采用`sklearn`的`boston`数据举例介绍。

```
from sklearn import datasets
import pandas as pd
boston = datasets.load_boston()
df = pd.DataFrame(boston.data, columns=boston.feature_names)

```

<img width="657" height="167" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__e097520c122542c0b.png"/>

## 1\. \[\]

第一种是最快捷方便的，直接在dataframe的`[]`中写筛选的条件或者组合条件。比如下面，想要筛选出大于`NOX`这变量平均值的所有数据，然后按`NOX`降序排序。

```
df[df['NOX']>df['NOX'].mean()].sort_values(by='NOX',ascending=False).head()

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

当然，也可以使用组合条件，条件之间使用逻辑符号`& |`等。比如下面这个例子除了上面条件外再加上且条件`CHAS为1`，注意逻辑符号分开的条件要用`()`隔开。

```
df[(df['NOX']>df['NOX'].mean())& (df['CHAS'] ==1)].sort_values(by='NOX',ascending=False).head()

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 2\. loc/iloc

除`[]`之外，`loc/iloc`应该是最常用的两种查询方法了。`loc`按标签值（列名和行索引取值）访问，`iloc`按数字索引访问，均支持单值访问或切片查询。除了可以像`[]`按条件筛选数据以外，`loc`还可以指定返回的列变量，**从行和列两个维度筛选。**

比如下面这个例子，按条件筛选出数据，并筛选出指定变量，然后赋值。

```
df.loc[(df['NOX']>df['NOX'].mean()),['CHAS']] = 2

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 3\. isin

上面我们筛选条件`< > == !=`都是个范围，但很多时候是需要锁定某些具体的值的，这时候就需要`isin`了。比如我们要限定`NOX`取值只能为`0.538,0.713,0.437`中时。

```
df.loc[df['NOX'].isin([0.538,0.713,0.437]),:].sample(5)

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

当然，也可以做取反操作，在筛选条件前加`~`符号即可。

```
df.loc[~df['NOX'].isin([0.538,0.713,0.437]),:].sample(5)

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 4\. str.contains

上面的举例都是**数值大小比较**的筛选条件，除数值以外当然也有**字符串的查询需求**。`pandas`里实现字符串的模糊筛选，可以用`.str.contains()`来实现，有点像在SQL语句里用的是`like`。

下面利用titanic的数据举例，筛选出人名中包含`Mrs`或者`Lily`的数据，`|`或逻辑符号在引号内。

```
train.loc[train['Name'].str.contains('Mrs|Lily'),:].head()

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

`.str.contains()`中还可以设置正则化筛选逻辑。

- case=True：使用case指定区分大小写
    
- na=True：就表示把有NAN的转换为布尔值True
    
- flags=re.IGNORECASE：标志传递到re模块，例如re.IGNORECASE
    
- regex=True：regex ：如果为True，则假定第一个字符串是正则表达式，否则还是字符串
    

## 5\. where/mask

在SQL里，我们知道`where`的功能是要把满足条件的筛选出来。pandas中`where`也是筛选，但用法稍有不同。

`where`接受的条件需要是**布尔类型**的，如果不满足匹配条件，就被赋值为默认的`NaN`或其他指定值。举例如下，将`Sex`为`male`当作筛选条件，`cond`就是一列布尔型的Series，非male的值就都被赋值为默认的`NaN`空值了。

```
cond = train['Sex'] == 'male'
train['Sex'].where(cond, inplace=True)
train.head()

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

也可以用`other`赋给指定值。

```
cond = train['Sex'] == 'male'
train['Sex'].where(cond, other='FEMALE', inplace=True)

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

甚至还可以写组合条件。

```
train['quality'] = ''
traincond1 = train['Sex'] == 'male'
cond2 = train['Age'] > 25
train['quality'].where(cond1 & cond2, other='低质量男性', inplace=True)

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

`mask`和`where`是一对操作，与`where`正好反过来。

```
train['quality'].mask(cond1 & cond2, other='低质量男性', inplace=True)

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 6\. query

这是一种非常优雅的筛选数据方式。所有的筛选操作都在`''`之内完成。

```
# 常用方式
train[train.Age > 25]
# query方式
train.query('Age > 25')

```

上面的两种方式效果上是一样的。再比如复杂点的，加入上面的`str.contains`用法的组合条件，注意条件里有`''`时，两边要用`""`包住。

```
train.query("Name.str.contains('William') & Age > 25")

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

在`query`里还可以通过`@`来设定变量。

```
name = 'William'
train.query("Name.str.contains(@name)")

```

## 7\. filter

`filter`是另外一个独特的筛选功能。`filter`不筛选具体数据，而是筛选特定的行或列。它支持三种筛选方式：

- items：固定列名
    
- regex：正则表达式
    
- like：以及模糊查询
    
- axis：控制是行index或列columns的查询
    

下面举例介绍下。

```
train.filter(items=['Age', 'Sex'])

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

```
train.filter(regex='S', axis=1) # 列名包含S的

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

```
train.filter(like='2', axis=0) # 索引中有2的

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

```
train.filter(regex='^2', axis=0).filter(like='S', axis=1)

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 8\. any/all

`any`方法意思是，如果至少有一个值为`True`结果便为`True`，`all`需要所有值为`True`结果才为`True`，比如下面这样。

```
>> train['Cabin'].all()
>> False
>> train['Cabin'].any()
>> True

```

`any`和`all`一般是需要和其它操作配合使用的，比如查看每列的空值情况。

```
train.isnull().any(axis=0)

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

再比如查看含有空值的行数。

```
>>> train.isnull().any(axis=1).sum()
>>> 708

```

**原创不易，欢迎点赞、留言、分享，支持我继续写下去。**

参考：

\[1\] https://pandas.pydata.org/

\[2\] https://www.gairuo.com/p/pandas-selecting-data

\[3\] https://blog.csdn.net/weixin_43484764/article/details/89847241

**赠书福利**

**书籍：**赠送 **5** 本《**深入浅出pandas：利用Python进行数据处理与分析**》，由「**机械工业出版社华****章公司**」赞助提供，pandas学习的经典之作 ，超赞！推荐入手一本。更多介绍和目录可以点击下面链接了解。

**赠送规则：**「**转发本文至朋友圈**」+「**留言**」，文章内容相关的优质留言或感想才可上墙。留言点赞数量最多**前5位**读者将获得一本。

**开奖****时间：**「**8月17日20:00」**

**领书须知：**提供转发截图+ 集赞截图

**注意事项：**禁止刷赞，发现后将进入黑名单，取消上墙资格。最终获赠者请在24小时以内添加我的微信，备注：赠书👇

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

```


爱点赞的人，运气都不会太差![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)


```

收录于合集 #<a id="js_album_keep_read_title"></a>pandas骚操作

 <a id="js_album_keep_read_size"></a>25个

<a id="js_album_prev"></a>上一篇 <a id="js_album_keep_read_pre_title"></a>20 个短小精悍的 pandas 骚操作 <a id="js_album_next"></a>下一篇 <a id="js_album_keep_read_next_title"></a>Pandas 骚操作：如何将运行内存占用降低 90%！

People who liked this content also liked

超强图解pandas，建议收藏(文末赠书)

...

Python数据科学

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___f18cbd9264a74c49a.bmp"/>

Scan to Follow