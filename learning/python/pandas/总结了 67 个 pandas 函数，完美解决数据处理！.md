# 总结了 67 个 pandas 函数，完美解决数据处理！

<a id="profileBt"></a><a id="js_name"></a>数据分析与开发 *2021-05-03 11:52*

The following article is from 快学Python Author 黄伟呢

<a id="copyright_info"></a>[![](../../../_resources/0_f0fbb05412384451b914fb26759e62d4.png)<br>**快学Python** .<br>Python可视化、自动化办公、数据分析、爬虫、Web开发！人生苦短，快学Python！](#)

不管是业务数据分析 ，还是数据建模。数据处理都是及其重要的一个步骤，它对于最终的结果来说，至关重要。

今天，就为大家总结一下 “Pandas数据处理” 几个方面重要的知识，拿来即用，随查随查。

- 导⼊数据
    
- 导出数据
    
- 查看数据
    
- 数据选取
    
- 数据处理
    
- 数据分组和排序
    
- 数据合并
    

```


# 在使用之前，需要导入pandas库
import pandas as pd



```

## **导⼊数据**

这里我为大家总结7个常见用法。

```


pd.DataFrame() # 自己创建数据框，用于练习
pd.read_csv(filename) # 从CSV⽂件导⼊数据
pd.read_table(filename) # 从限定分隔符的⽂本⽂件导⼊数据
pd.read_excel(filename) # 从Excel⽂件导⼊数据
pd.read_sql(query,connection_object) # 从SQL表/库导⼊数据
pd.read_json(json_string) # 从JSON格式的字符串导⼊数据
pd.read_html(url) # 解析URL、字符串或者HTML⽂件，抽取其中的tables表格



```

## **导出数据**

这里为大家总结5个常见用法。

```


df.to_csv(filename) #导出数据到CSV⽂件
df.to_excel(filename) #导出数据到Excel⽂件
df.to_sql(table_name,connection_object) #导出数据到SQL表
df.to_json(filename) #以Json格式导出数据到⽂本⽂件
writer=pd.ExcelWriter('test.xlsx',index=False) 
df1.to_excel(writer,sheet_name='单位')和writer.save()，将多个数据帧写⼊同⼀个⼯作簿的多个sheet(⼯作表)



```

## **查看数据**

这里为大家总结11个常见用法。

```


df.head(n) # 查看DataFrame对象的前n⾏
df.tail(n) # 查看DataFrame对象的最后n⾏
df.shape() # 查看⾏数和列数
df.info() # 查看索引、数据类型和内存信息
df.columns() # 查看字段（⾸⾏）名称
df.describe() # 查看数值型列的汇总统计
s.value_counts(dropna=False) # 查看Series对象的唯⼀值和计数
df.apply(pd.Series.value_counts) # 查看DataFrame对象中每⼀列的唯⼀值和计数
df.isnull().any() # 查看是否有缺失值
df[df[column_name].duplicated()] # 查看column_name字段数据重复的数据信息
df[df[column_name].duplicated()].count() # 查看column_name字段数据重复的个数



```

## **数据选取**

这里为大家总结10个常见用法。

```


df[col] # 根据列名，并以Series的形式返回列
df[[col1,col2]] # 以DataFrame形式返回多列
s.iloc[0] # 按位置选取数据
s.loc['index_one'] # 按索引选取数据
df.iloc[0,:] # 返回第⼀⾏
df.iloc[0,0] # 返回第⼀列的第⼀个元素
df.loc[0,:] # 返回第⼀⾏（索引为默认的数字时，⽤法同df.iloc），但需要注意的是loc是按索引,iloc参数只接受数字参数
df.ix[[:5],["col1","col2"]] # 返回字段为col1和col2的前5条数据，可以理解为loc和
iloc的结合体。
df.at[5,"col1"] # 选择索引名称为5，字段名称为col1的数据
df.iat[5,0] # 选择索引排序为5，字段排序为0的数据



```

## **数据处理**

这里为大家总结16个常见用法。

```


df.columns= ['a','b','c'] # 重命名列名（需要将所有列名列出，否则会报错）
pd.isnull() # 检查DataFrame对象中的空值，并返回⼀个Boolean数组
pd.notnull() # 检查DataFrame对象中的⾮空值，并返回⼀个Boolean数组
df.dropna() # 删除所有包含空值的⾏
df.dropna(axis=1) # 删除所有包含空值的列
df.dropna(axis=1,thresh=n) # 删除所有⼩于n个⾮空值的⾏
df.fillna(value=x) # ⽤x替换DataFrame对象中所有的空值，⽀持
df[column_name].fillna(x)
s.astype(float) # 将Series中的数据类型更改为float类型
s.replace(1,'one') # ⽤‘one’代替所有等于1的值
s.replace([1,3],['one','three']) # ⽤'one'代替1，⽤'three'代替3
df.rename(columns=lambdax:x+1) # 批量更改列名
df.rename(columns={'old_name':'new_ name'}) # 选择性更改列名
df.set_index('column_one') # 将某个字段设为索引，可接受列表参数，即设置多个索引
df.reset_index("col1") # 将索引设置为col1字段，并将索引新设置为0,1,2...
df.rename(index=lambdax:x+1) # 批量重命名索引



```

## **数据分组、排序、透视**

这里为大家总结13个常见用法。

```


df.sort_index().loc[:5] # 对前5条数据进⾏索引排序
df.sort_values(col1) # 按照列col1排序数据，默认升序排列
df.sort_values(col2,ascending=False) # 按照列col1降序排列数据
df.sort_values([col1,col2],ascending=[True,False]) # 先按列col1升序排列，后按col2降序排列数据
df.groupby(col) # 返回⼀个按列col进⾏分组的Groupby对象
df.groupby([col1,col2]) # 返回⼀个按多列进⾏分组的Groupby对象
df.groupby(col1)[col2].agg(mean) # 返回按列col1进⾏分组后，列col2的均值,agg可以接受列表参数，agg([len,np.mean])
df.pivot_table(index=col1,values=[col2,col3],aggfunc={col2:max,col3:[ma,min]}) # 创建⼀个按列col1进⾏分组，计算col2的最⼤值和col3的最⼤值、最⼩值的数据透视表
df.groupby(col1).agg(np.mean) # 返回按列col1分组的所有列的均值,⽀持
df.groupby(col1).col2.agg(['min','max'])
data.apply(np.mean) # 对DataFrame中的每⼀列应⽤函数np.mean
data.apply(np.max,axis=1) # 对DataFrame中的每⼀⾏应⽤函数np.max
df.groupby(col1).col2.transform("sum") # 通常与groupby连⽤，避免索引更改



```

## **数据合并**

这里为大家总结5个常见用法。

```


df1.append(df2) # 将df2中的⾏添加到df1的尾部
df.concat([df1,df2],axis=1,join='inner') # 将df2中的列添加到df1的尾部,值为空的对应⾏与对应列都不要
df1.join(df2.set_index(col1),on=col1,how='inner') # 对df1的列和df2的列执⾏SQL形式的join，默认按照索引来进⾏合并，如果df1和df2有共同字段时，会报错，可通过设置lsuffix,rsuffix来进⾏解决，如果需要按照共同列进⾏合并，就要⽤到set_index(col1)
pd.merge(df1,df2,on='col1',how='outer') # 对df1和df2合并，按照col1，⽅式为outer
pd.merge(df1,df2,left_index=True,right_index=True,how='outer') # 与 df1.join(df2, how='outer')效果相同



```

\- EOF -

推荐阅读  点击标题可跳转

<ins>1、[详解16个 pandas 函数，让你的 “数据清洗” 能力提高100倍！](http://mp.weixin.qq.com/s?__biz=MzA5ODM5MDU3MA==&mid=2650871841&idx=1&sn=d4f313df510b391de79479a765d231e2&chksm=8b67ff64bc107672f2d7531d7d8534d23a4748328c273f52fc6cb5fdab46d4e4662b1f56f487&scene=21#wechat_redirect)</ins>

<ins>2、[解一道反常的 pandas 题（附源数据和代码）](http://mp.weixin.qq.com/s?__biz=MzA5ODM5MDU3MA==&mid=2650871650&idx=1&sn=c69378749d1cdb268e9ed74ee4badec2&chksm=8b67f027bc107931c46d2b3f2a08066e573625ed09cf4595393a3fe13b47f1e80ff567203ad0&scene=21#wechat_redirect)</ins>

<ins>3、[Pandas 必知必会的使用技巧，值得收藏！](http://mp.weixin.qq.com/s?__biz=MzA5ODM5MDU3MA==&mid=2650869810&idx=2&sn=cea7c191f87235d091c30191eda28abc&chksm=8b67f777bc107e615e747b266937c032624708209d0339086d0edff9fe69fc843ee1aa696b4d&scene=21#wechat_redirect)</ins>

看完本文有收获？请转发分享给更多人

**推荐关注「数据分析与开发」，提升数据技能**

![](../../../_resources/0_wx_fmt_png_a5d3c517de74410a9a28c173965a27f4.png)

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

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___feab6c9b11e44fd9a.bmp"/>

Scan to Follow