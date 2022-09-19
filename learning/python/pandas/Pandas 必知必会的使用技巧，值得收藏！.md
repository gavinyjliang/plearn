# Pandas 必知必会的使用技巧，值得收藏！

<a id="profileBt"></a><a id="js_name"></a>数据分析与开发 *2020-12-23 11:20*

（给数据分析与开发加星标，提升数据技能）

> 来源：风控猎人

本期的主题是关于python的一个数据分析工具pandas的，归纳整理了一些工作中常用到的pandas使用技巧，方便更高效地实现数据分析。文章很短，不用收藏就能Get~

## Pandas技巧总结

## **1.计算变量缺失率**

```


df=pd.read_csv('titanic_train.csv')
def missing_cal(df):
    """
    df :数据集
    
    return：每个变量的缺失率
    """
    missing_series = df.isnull().sum()/df.shape[0]
    missing_df = pd.DataFrame(missing_series).reset_index()
    missing_df = missing_df.rename(columns={'index':'col',
                                            0:'missing_pct'})
    missing_df = missing_df.sort_values('missing_pct',ascending=False).reset_index(drop=True)
    return missing_df
missing_cal(df)


```

如果需要计算样本的缺失率分布，只要加上参数axis=1

## **2.获取分组里最大值所在的行方法**

分为分组中有重复值和无重复值两种。无重复值的情况。

```


df = pd.DataFrame({'Sp':['a','b','c','d','e','f'], 'Mt':['s1', 's1', 's2','s2','s2','s3'], 'Value':[1,2,3,4,5,6], 'Count':[3,2,5,10,10,6]})
df
df.iloc[df.groupby(['Mt']).apply(lambda x: x['Count'].idxmax())]



```

先按Mt列进行分组，然后对分组之后的数据框使用idxmax函数取出Count最大值所在的列，再用iloc位置索引将行取出。有重复值的情况

```


df["rank"] = df.groupby("ID")["score"].rank(method="min", ascending=False).astype(np.int64)
df[df["rank"] == 1][["ID", "class"]]



```

对ID进行分组之后再对分数应用rank函数，分数相同的情况会赋予相同的排名，然后取出排名为1的数据。

## **3.多列合并为一行**

```


df = pd.DataFrame({'id_part':['a','b','c','d'], 'pred':[0.1,0.2,0.3,0.4], 'pred_class':['women','man','cat','dog'], 'v_id':['d1','d2','d3','d1']})
df.groupby(['v_id']).agg({'pred_class': [', '.join],'pred': lambda x: list(x),
'id_part': 'first'}).reset_index()


```

## **4.删除包含特定字符串所在的行**

```


df = pd.DataFrame({'a':[1,2,3,4], 'b':['s1', 'exp_s2', 's3','exps4'], 'c':[5,6,7,8], 'd':[3,2,5,10]})
df[df['b'].str.contains('exp')]



```

## 5.组内排序

```


df = pd.DataFrame([['A',1],['A',3],['A',2],['B',5],['B',9]], columns = ['name','score'])



```

介绍两种高效地组内排序的方法。

```


df.sort_values(['name','score'], ascending = [True,False])
df.groupby('name').apply(lambda x: x.sort_values('score', ascending=False)).reset_index(drop=True)



```

## 6.选择特定类型的列

```


drinks = pd.read_csv('data/drinks.csv')
# 选择所有数值型的列
drinks.select_dtypes(include=['number']).head()
# 选择所有字符型的列
drinks.select_dtypes(include=['object']).head()
drinks.select_dtypes(include=['number','object','category','datetime']).head()
# 用 exclude 关键字排除指定的数据类型
drinks.select_dtypes(exclude=['number']).head()



```

## 7.字符串转换为数值

```


df = pd.DataFrame({'列1':['1.1','2.2','3.3'],
                  '列2':['4.4','5.5','6.6'],
                  '列3':['7.7','8.8','-']})
df
df.astype({'列1':'float','列2':'float'}).dtypes



```

用这种方式转换第三列会出错，因为这列里包含一个代表 0 的下划线，pandas 无法自动判断这个下划线。为了解决这个问题，可以使用 to_numeric() 函数来处理第三列，让 pandas 把任意无效输入转为 NaN。

```


df = df.apply(pd.to_numeric, errors='coerce').fillna(0)


```

## 8.优化 DataFrame 对内存的占用

方法一：只读取切实所需的列，使用usecols参数

```


cols = ['beer_servings','continent']
small_drinks = pd.read_csv('data/drinks.csv', usecols=cols)



```

方法二：把包含类别型数据的 object 列转换为 Category 数据类型，通过指定 dtype 参数实现。

```


dtypes ={'continent':'category'}
smaller_drinks = pd.read_csv('data/drinks.csv',usecols=cols, dtype=dtypes)



```

## 9.根据最大的类别筛选 DataFrame

```


movies = pd.read_csv('data/imdb_1000.csv')
counts = movies.genre.value_counts()
movies[movies.genre.isin(counts.nlargest(3).index)].head()



```

## 10.把字符串分割为多列

```


df = pd.DataFrame({'姓名':['张 三','李 四','王 五'],
                   '所在地':['北京-东城区','上海-黄浦区','广州-白云区']})
df
df.姓名.str.split(' ', expand=True)



```

## 11.把 Series 里的列表转换为 DataFrame

```


df = pd.DataFrame({'列1':['a','b','c'],'列2':[[10,20], [20,30], [30,40]]})
df
df_new = df.列2.apply(pd.Series)
pd.concat([df,df_new], axis='columns')



```

## 12.用多个函数聚合

```


orders = pd.read_csv('data/chipotle.tsv', sep='\t')
orders.groupby('order_id').item_price.agg(['sum','count']).head()


```

## **13.分组聚合**

```


import pandas as pd
df = pd.DataFrame({'key1':['a', 'a', 'b', 'b', 'a'],
    'key2':['one', 'two', 'one', 'two', 'one'],
    'data1':np.random.randn(5),
     'data2':np.random.randn(5)})
df
for name, group in df.groupby('key1'):
    print(name)
    print(group)
dict(list(df.groupby('key1')))



```

通过字典或Series进行分组

```


people = pd.DataFrame(np.random.randn(5, 5),
     columns=['a', 'b', 'c', 'd', 'e'],
     index=['Joe', 'Steve', 'Wes', 'Jim', 'Travis'])
mapping = {'a':'red', 'b':'red', 'c':'blue',
     'd':'blue', 'e':'red', 'f':'orange'}
by_column = people.groupby(mapping, axis=1)
by_column.sum()



```

\- EOF -

推荐阅读  点击标题可跳转

<ins>1、[用 pandas-profiling 做出更好的探索性数据分析](http://mp.weixin.qq.com/s?__biz=MzA5ODM5MDU3MA==&mid=2650866936&idx=2&sn=af894fc31c49cf35006c091638f9816f&chksm=8b67e3bdbc106aab0be79c89d3cb319bbb7038a48aa8b07dacd94a5e03ef8f2e61785b305dc6&scene=21#wechat_redirect)</ins>

<ins>2、[<ins>12 个 Numpy 和 Pandas 函数，提高效率</ins>](http://mp.weixin.qq.com/s?__biz=MzA5ODM5MDU3MA==&mid=2650865906&idx=1&sn=afb32717f2f095ca442ca9cd15cf828f&chksm=8b67e7b7bc106ea119d12a21f453f36318444479e562a8d5143c25035cd93b8f9b1822e7d4bc&scene=21#wechat_redirect)</ins>

<ins>3、[<ins>Pandas 可视化综合指南：手把手从零教你绘制数据图表</ins>](http://mp.weixin.qq.com/s?__biz=MzA5ODM5MDU3MA==&mid=2650865469&idx=1&sn=e6e6ea23a0c0e77586c0eb318e9ecee3&chksm=8b661878bc11916e5eae2ca895ec54d711437d799023c5b9382589b8ddd0e0f8783f09975380&scene=21#wechat_redirect)</ins>

看完本文有收获？请转发分享给更多人

**推荐关注「数据分析与开发」，提升数据技能**

![Image](../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_f485335b9dcc4e99a.jpg)

点赞和在看就是最大的支持❤️

People who liked this content also liked

我学习 Python 的三个神级网站

数据分析与开发

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___efbd6b51861044f6a.bmp"/>

Scan to Follow