# 建议收藏：12个Pandas数据处理高频操作

<a id="copyright_logo"></a>Original 小白老表 <a id="profileBt"></a><a id="js_name"></a>简说Python *2022-03-06 20:09*

收录于话题

<a id="js_article_tag_name__1664647957402468355"></a>#Python <a id="js_article_tag_num__1664647957402468355"></a>78 <a id="js_article_tag_tips__1664647957402468355"></a>个

<a id="js_article_tag_name__2165943900622897156"></a>#Pandas <a id="js_article_tag_num__2165943900622897156"></a>1 <a id="js_article_tag_tips__2165943900622897156"></a>个

<a id="js_article_tag_name__2298062497515634689"></a>#Dataframe <a id="js_article_tag_num__2298062497515634689"></a>1 <a id="js_article_tag_tips__2298062497515634689"></a>个

<a id="js_article_tag_name__2060293929521987587"></a>#数据处理 <a id="js_article_tag_num__2060293929521987587"></a>4 <a id="js_article_tag_tips__2060293929521987587"></a>个

![](../../../_resources/0_wx_fmt_png_d6c6f5161b064077bca8c647cc586019.png)

**简说Python**

号主老表，自学，分享Python，SQL零基础入门、数据分析、数据挖掘、机器学习优质文章以及学习经验。

<a id="js_profile_article"></a>218篇原创内容

Official Account

大家好，我是老表～今天给大家分享几个自己近期常用的Pandas数据处理技巧，主打实用，所以你肯定能用的着，建议扫一遍，然后收藏起来，下次要用的时候再查查看即可。

- 简单说说
    
- 总结分享
    

- \> 1 统计一行/一列数据的负数出现的次数
    
- \> 2 让dataframe里面的正数全部变为0
    
- \> 3 统计某列中各元素出现次数
    
- \> 4 修改表头和索引
    
- \> 5 修改列所在位置insert+pop
    
- \> 6 常用查询方法query
    
- \> 7 数据存储时不要索引
    
- \> 8 按指定列排序sort_values
    
- \> 9 apply 函数运用
    
- \> 10 Pandas数据合并
    
- \> 11 Pandas Dataframe拷贝
    
- \> 12 对于列/行的操作
    

## 简单说说

Panda是一个快速、强大、灵活且易于使用的开源数据分析和操作工具，在Python环境下，我们可以通过pip直接进行安装。

```
`pip install pandas
`
```

在Python代码中使用pandas首先需要导入，：

```
`import pandas as pd
`
```

创建一个示例数据：

```
`# 统计一行/一列数据的负数出现的次数
df = pd.DataFrame(
    {'a':[1,-3,0,1,3],
     'b':[-1,0,1,5,1],
     'c':[0,-2,0,-9,0]})
df
`
```

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__47e5b73227a647c89.png)

## 总结分享

### \> 1 统计一行/一列数据的负数出现的次数

```
`# 获取到每一行的复数个数
# 要获取列的话，将axis改成0即可
num_list = (df < 0).astype(int).sum(axis=1)
num_list
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### \> 2 让dataframe里面的正数全部变为0

```
`# 直接了当
df[df>0] = 0
df
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### \> 3 统计某列中各元素出现次数

- 默认情况，直接统计出指定列各元素值出现的次数。
    

```
`# 默认情况，统计b列各元素出现次数
df['b'].value_counts()
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

- 最好奇的bins参数，按bins分割区间，统计落在各区间内元素个数
    

```
`# 按指定区间个数bin，元素起始值分割区间，统计表格中落在各区间内元素个数
df['b'].value_counts(bins=3)
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

- normalize参数，计算各元素出现次数占比
    

```
`# normalize参数 出现次数/总数据个数 
df['b'].value_counts(normalize=True)
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

还有sort和ascending，可以按指定方式对统计结果进行排序。

### \> 4 修改表头和索引

- 修改表头名称
    

```
`# 修改表头名称
columns = {'a': 'A', 'b': 'B'}
df.rename(columns=columns, inplace=True)
df
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

- 设置特殊索引
    

```
`# 设置特殊索引
df.index = ['a', 'b', 'c', 'd', 'e']
df
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

- 删除索引
    

```
`# 删除索引
df.reset_index(drop=True, inplace=True)
df
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### \> 5 修改列所在位置insert+pop

insert在指定位置插入某列值；pop按列名取出某列（同时会删掉该列）。

```
`# 将A列移到最后
# 新增列位置，新增列名，新增列的数值
df.insert(2,'A',df.pop('A'))
df
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### \> 6 常用查询方法query

- 直接查询
    

```
`# 找出c所有c值小于0的行
df.query("c<0")
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

- query+contains模糊查询
    

```
`# 插入一列
df.insert(0,'name',['张三', '张华', '李四', '王五', '李逵'])
# 查找名字里包含三、四、五的用户数据
df.query("name.str.contains('三|四|五')", engine='python')
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### \> 7 数据存储时不要索引

设置index为None即可。

```
`df.to_csv('测试数据.csv', encoding='utf-8-sig', index=None)
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### \> 8 按指定列排序sort_values

sort_values函数，通过by参数可以指定按哪些列进行排序，还可以设置ascending指定排序方式（升序或者降序，默认降序）

```
`# by 指定排序列 na_position nan值放的位置 开头还是尾部
df.sort_values(by=['name'],na_position='first')
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### \> 9 apply 函数运用

```
`# A B 两列都每个元素值都+1
df[['A', 'B']].apply(lambda x:x+1)
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

其他更高级应用，可以查看之前分享的文章[Pandas数据分析，你不能不知道的技能](https://mp.weixin.qq.com/s?__biz=MzUyOTAwMzI4NA==&mid=2247515110&idx=1&sn=e9db6e7f73f210ca21ce61c1de7130d1&chksm=fa655d9dcd12d48bc19172701e1ccf9fea9efe0eb4311297be8baa889f771116cfd4f46778a0&scene=21#wechat_redirect)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

```
`DataFrame.apply(func, 
axis=0, broadcast=False, 
raw=False, reduce=None, args=(), **kwds)
`
```

### \> 10 Pandas数据合并

进行数据合并前，首先需要确定合并的数据的表头都是一致的，然后将他们依次加入一个列表，最终使用concat函数即可进行数据合并。

```
`# 现将表构成list，然后再作为concat的输入
df1 = df[0:1]
df2 = df[2:4]
df3 = df[3:5]
frames = [df1, df2, df3]
df4 = pd.concat(frames)
df4
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### \> 11 Pandas Dataframe拷贝

- 深拷贝，df1改变，df不会变
    

```
`# 深拷贝，df1改变，df不会变
df1 = df.copy(deep=True)
print(f"df\n{df}\ndf1\n{df1}")
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

- 将原数据df的name列的第一个元素改为`zs`，会发现，df改动，不会影响df1。
    

```
`df['name'][0] = 'zs'
print(f"df\n{df}\ndf1\n{df1}")
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

- 浅拷贝，df2改变，df也会变  等同df2 = df
    

```
`# 浅拷贝，df2改变，df也会变  等同df2 = df
df2 = df.copy(deep=False)
print(f"df\n{df}\ndf2\n{df2}")
`
```

- 将原数据df的name列的第一个元素改为`张三`，会发现，df改动，df2也会一起改动。
    

```
`df['name'][0] = '张三'
print(f"df\n{df}\ndf2\n{df2}")
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

关于深浅拷贝相关介绍和应用，大家可以查看之前的分享[别再弄不清Python 深拷贝和浅拷贝了！](https://mp.weixin.qq.com/s?__biz=MzUyOTAwMzI4NA==&mid=2247518842&idx=1&sn=5cd83a8a5247cf6a44fca799c2a6d007&chksm=fa656c01cd12e51718c14fe7ecd12c217026418c1f7edfe89fce63ad39bde27dae2b8d831168&scene=21#wechat_redirect)

### \> 12 对于列/行的操作

- 删除指定行/列
    

```
`# 行索引/列索引 多行/多列可以用列表
# axis=0表示行 axis=1表示列 inplace是否在原列表操作 
# 删除df中的c列
df.drop('c', axis=1, inplace=True)
df
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

- 取出指定列/行
    

```
`# 不知道列名，取出表格最后两列
df3 = df.iloc[:, -2:]  
# 知道列名，取出name和A两列
df4 = df.loc[:, ['name', 'A']]  
print(f"df3\n{df3}\ndf4\n{df4}")
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

```
`# 重新设置下索引
df.index = ['a1', 'a2', 'a3', 'a4', 'a5']
# 不知道行索引，取出表格前两行
df5 = df.iloc[:2, :]  
# 知道行索引，取出a1和a3两行
df6 = df.loc[['a1', 'a3'], :]  
print(f"df5\n{df5}\ndf6\n{df6}")
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

- 交换两列指定值
    

```
`# 将B列中小于0的元素和A列交换
# 筛选出B列中小于0的行
flag = df['B'].astype(int).map(lambda x: x<0)
# 通过布尔提取交换两列数据
df.loc[flag, 'B'], df.loc[flag, 'A'] = df.loc[flag, 'A'], df.loc[flag, 'B']
df
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

好啦，今天的分享就到这里啦，下会有新的积累，再分享给大家，也欢迎大家留言区留言说说你平时pandas用的比较多的操作呀～互相学习，才能一起进步，更快的进步。

你的每一个点赞、在看，每一条留言，每一次转发，都对我很重要，原创不易，感谢支持。
好的，那么下期见，我是爱猫爱技术，更爱思思的老表⁽⁽ଘ( ˙꒳˙ )ଓ⁾⁾

**近期阅读学习推荐**：

[服务器被黑客攻击，用来挖矿！怎么办？](http://mp.weixin.qq.com/s?__biz=MzUyOTAwMzI4NA==&mid=2247526053&idx=1&sn=6f35fc3cfdb7a13a9b962be20677d21f&chksm=fa6580decd1209c8538ed5eadb5456fd61c9eb381e8cbf7432f3e06ae978c25a6cac1a110954&scene=21#wechat_redirect)

[怎么才能写出好看的Python代码？这五个工具你得用上](http://mp.weixin.qq.com/s?__biz=MzUyOTAwMzI4NA==&mid=2247526231&idx=1&sn=594edcb077ef5bcdf145251467ff549f&chksm=fa65812ccd12083ae89f4325d6f4176213e2ef2622cc72d8caedf05f82e1179d581d5e4ad2db&scene=21#wechat_redirect)

[Python超好用的命令行界面实现工具](http://mp.weixin.qq.com/s?__biz=MzUyOTAwMzI4NA==&mid=2247526118&idx=1&sn=eb0314365ab125b2c3c3c360d408c92d&chksm=fa65809dcd12098b1b3bdc7c6a328e342af1982a2f6b0badbd2d506f0b4fc6bbc71f02febf33&scene=21#wechat_redirect)

**如何找到我**：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

People who liked this content also liked

Linux 性能优化的全景指南，可能都在这里了，建议收藏~

高效运维

不看的原因

- 内容质量低
- 不看此公众号

在Python中使用Greppo构建的地理空间仪表板

大邓和他的Python

不看的原因

- 内容质量低
- 不看此公众号

RISC-V又一开源SoC-zqh_riscv

OpenFPGA

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___76fdbc1a617646f28.bmp"/>

Scan to Follow