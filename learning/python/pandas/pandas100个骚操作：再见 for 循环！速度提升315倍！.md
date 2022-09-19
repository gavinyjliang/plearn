# pandas100个骚操作：再见 for 循环！速度提升315倍！

<a id="copyright_logo"></a>Original 东哥起飞 <a id="profileBt"></a><a id="js_name"></a>Python数据科学 *2021-02-05 14:40*

<a id="js_article-tag-card__left"></a>收录于合集 #pandas骚操作 <a id="js_article-tag-card__right"></a>25个

关注上方“Python数据科学”，选择星标，

关键时间，第一时间送达！

<img width="657" height="334" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__a883c40e6b034e358.png"/>

来源：Python数据科学

作者：东哥起飞

大家好，我是东哥。

本篇是pandas100个骚操作系列的第 **11**篇：**再见 for 循环！速度提升315倍！**

系列内容，请看👉「[pandas100个骚操作](https://mp.weixin.qq.com/mp/appmsgalbum?__biz=MzUzODYwMDAzNA==&action=getalbum&album_id=1699019347278561282#wechat_redirect)」话题，订阅后文章更新可第一时间推送至订阅号。

* * *

上一篇分享了一个从时间处理上的加速方法「[使用 Datetime 提速 50 倍运行速度！](http://mp.weixin.qq.com/s?__biz=MzUzODYwMDAzNA==&mid=2247517523&idx=2&sn=ac56ba8d4bab2e944cbe524a8acfd76f&chksm=fad7f25ecda07b4877c10fe4997c255a109d8a40bb57f55f8570bb24a09220da72dd8f82cfc2&scene=21#wechat_redirect)」，本篇分享一个更常用的加速骚操作。

`for`是所有编程语言的基础语法，初学者为了快速实现功能，依懒性较强。但如果从运算时间性能上考虑可能不是特别好的选择。

本次东哥介绍几个常见的提速方法，一个比一个快，了解`pandas`本质，才能知道如何提速。

下面是一个例子，数据获取方式见文末。

```


>>> import pandas as pd
# 导入数据集
>>> df = pd.read_csv('demand_profile.csv')
>>> df.head()
     date_time  energy_kwh
0  1/1/13 0:00       0.586
1  1/1/13 1:00       0.580
2  1/1/13 2:00       0.572
3  1/1/13 3:00       0.596
4  1/1/13 4:00       0.592



```

基于上面的数据，我们现在要增加一个新的特征，但这个新的特征是基于一些时间条件生成的，根据时长（小时）而变化，如下：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

因此，如果你不知道如何提速，那正常第一想法可能就是用`apply`方法写一个函数，函数里面写好时间条件的逻辑代码。

```


def apply_tariff(kwh, hour):
    """计算每个小时的电费"""    
    if 0 <= hour < 7:
        rate = 12
    elif 7 <= hour < 17:
        rate = 20
    elif 17 <= hour < 24:
        rate = 28
    else:
        raise ValueError(f'Invalid hour: {hour}')
    return rate * kwh



```

然后使用`for`循环来遍历`df`，根据`apply`函数逻辑添加新的特征，如下：

```


>>> # 不赞同这种操作
>>> @timeit(repeat=3, number=100)
... def apply_tariff_loop(df):
...     """用for循环计算enery cost，并添加到列表"""
...     energy_cost_list = []
...     for i in range(len(df)):
...         # 获取用电量和时间（小时）
...         energy_used = df.iloc[i]['energy_kwh']
...         hour = df.iloc[i]['date_time'].hour
...         energy_cost = apply_tariff(energy_used, hour)
...         energy_cost_list.append(energy_cost)
...     df['cost_cents'] = energy_cost_list
... 
>>> apply_tariff_loop(df)
Best of 3 trials with 100 function calls per trial:
Function `apply_tariff_loop` ran in average of 3.152 seconds.



```

对于那些写`Pythonic`风格的人来说，这个设计看起来很自然。然而，这个循环将会严重影响效率。原因有几个：

首先，它需要初始化一个将记录输出的列表。

其次，它使用不透明对象范围`(0，len(df))`循环，然后再应用`apply_tariff()`之后，它必须将结果附加到用于创建新`DataFrame`列的列表中。另外，还使用`df.iloc [i]['date_time']`执行所谓的链式索引，这通常会导致意外的结果。

这种方法的最大问题是计算的时间成本。**对于8760行数据，此循环花费了3秒钟。**

接下来，一起看下优化的提速方案。

### 一、使用 iterrows循环

第一种可以通过`pandas`引入`iterrows`方法让效率更高。这些都是一次产生一行的`生成器`方法，类似`scrapy`中使用的`yield`用法。

`.itertuples`为每一行产生一个`namedtuple`，并且行的索引值作为元组的第一个元素。`nametuple`是`Python`的`collections`模块中的一种数据结构，其行为类似于`Python`元组，但具有可通过属性查找访问的字段。

`.iterrows`为`DataFrame`中的每一行产生`（index，series）`这样的元组。

在这个例子中使用`.iterrows`，我们看看这使用`iterrows`后效果如何。

```


>>> @timeit(repeat=3, number=100)
... def apply_tariff_iterrows(df):
...     energy_cost_list = []
...     for index, row in df.iterrows():
...         # 获取用电量和时间（小时）
...         energy_used = row['energy_kwh']
...         hour = row['date_time'].hour
...         # 添加cost列表
...         energy_cost = apply_tariff(energy_used, hour)
...         energy_cost_list.append(energy_cost)
...     df['cost_cents'] = energy_cost_list
...
>>> apply_tariff_iterrows(df)
Best of 3 trials with 100 function calls per trial:
Function `apply_tariff_iterrows` ran in average of 0.713 seconds.



```

这样的语法更明确，并且行值引用中的混乱更少，因此它更具可读性。

时间成本方面：**快了近5倍！**

但是，还有更多的改进空间，理想情况是可以用`pandas`内置更快的方法完成。

### 二、pandas的apply方法

我们可以使用`.apply`方法而不是`.iterrows`进一步改进此操作。`pandas`的`.apply`方法接受函数`callables`并沿`DataFrame`的轴(所有行或所有列)应用。下面代码中，`lambda`函数将两列数据传递给`apply_tariff()`：

```


>>> @timeit(repeat=3, number=100)
... def apply_tariff_withapply(df):
...     df['cost_cents'] = df.apply(
...         lambda row: apply_tariff(
...             kwh=row['energy_kwh'],
...             hour=row['date_time'].hour),
...         axis=1)
...
>>> apply_tariff_withapply(df)
Best of 3 trials with 100 function calls per trial:
Function `apply_tariff_withapply` ran in average of 0.272 seconds.



```

`apply`的语法优点很明显，行数少，代码可读性高。在这种情况下，所花费的时间大约是`iterrows`方法的一半。

但是，这还不是“非常快”。一个原因是`apply()`将在内部尝试循环遍历`Cython`迭代器。但是在这种情况下，传递的`lambda`不是可以在`Cython`中处理的东西，因此它在Python中调用并不是那么快。

如果我们使用`apply()`方法获取10年的小时数据，那么将需要大约15分钟的处理时间。如果这个计算只是大规模计算的一小部分，那么真的应该提速了。这也就是**矢量化操作**派上用场的地方。

### 三、矢量化操作：使用.isin选择数据

**什么是矢量化操作？**

如果你不基于一些条件，而是可以在一行代码中将所有电力消耗数据应用于该价格：`df ['energy_kwh'] * 28`，类似这种。那么这个特定的操作就是矢量化操作的一个例子，它是在`pandas`中执行的最快方法。

但是如何将条件计算应用为`pandas`中的矢量化运算？

一个技巧是：**根据你的条件，选择和分组****`DataFrame`****，然后对每个选定的组应用矢量化操作。**

在下面代码中，我们将看到如何使用`pandas`的`.isin()`方法选择行，然后在矢量化操作中实现新特征的添加。在执行此操作之前，如果将`date_time`列设置为`DataFrame`的索引，会更方便：

```


# 将date_time列设置为DataFrame的索引
df.set_index('date_time', inplace=True)
@timeit(repeat=3, number=100)
def apply_tariff_isin(df):
    # 定义小时范围Boolean数组
    peak_hours = df.index.hour.isin(range(17, 24))
    shoulder_hours = df.index.hour.isin(range(7, 17))
    off_peak_hours = df.index.hour.isin(range(0, 7))
    # 使用上面apply_traffic函数中的定义
    df.loc[peak_hours, 'cost_cents'] = df.loc[peak_hours, 'energy_kwh'] * 28
    df.loc[shoulder_hours,'cost_cents'] = df.loc[shoulder_hours, 'energy_kwh'] * 20
    df.loc[off_peak_hours,'cost_cents'] = df.loc[off_peak_hours, 'energy_kwh'] * 12



```

我们来看一下结果如何。

```


>>> apply_tariff_isin(df)
Best of 3 trials with 100 function calls per trial:
Function `apply_tariff_isin` ran in average of 0.010 seconds.



```

提示，上面`.isin()`方法返回的是一个布尔值数组，如下：

```


[False, False, False, ..., True, True, True]



```

布尔值标识了`DataFrame`索引`datetimes`是否落在了指定的小时范围内。然后把这些布尔数组传递给`DataFrame`的`.loc`，将获得一个与这些小时匹配的`DataFrame`切片。然后再将切片乘以适当的费率，这就是一种快速的矢量化操作了。

上面的方法完全取代了我们最开始自定义的函数`apply_tariff()`，代码大大减少，同时速度起飞。

**运行时间比Pythonic的for循环快315倍，比iterrows快71倍，比apply快27倍！**

### 四、还能更快？

太刺激了，我们继续加速。

在上面`apply_tariff_isin`中，我们通过调用`df.loc`和`df.index.hour.isin`三次来进行一些**手动调整**。如果我们有更精细的时间范围，你可能会说这个解决方案是不可扩展的。但在这种情况下，我们可以使用`pandas`的`pd.cut()`函数来自动完成切割：

```


@timeit(repeat=3, number=100)
def apply_tariff_cut(df):
    cents_per_kwh = pd.cut(x=df.index.hour,
                           bins=[0, 7, 17, 24],
                           include_lowest=True,
                           labels=[12, 20, 28]).astype(int)
    df['cost_cents'] = cents_per_kwh * df['energy_kwh']



```

上面代码`pd.cut()`会根据`bin`列表应用分组。

其中`include_lowest`参数表示第一个间隔是否应该是包含左边的。

这是一种完全矢量化的方法，它在时间方面是最快的：

```


>>> apply_tariff_cut(df)
Best of 3 trials with 100 function calls per trial:
Function `apply_tariff_cut` ran in average of 0.003 seconds.



```

到目前为止，使用pandas处理的时间上基本快达到极限了！只需要花费不到一秒的时间即可处理完整的10年的小时数据集。

但是，最后一个其它选择，就是使用`NumPy`，还可以更快！

### 五、使用Numpy继续加速

使用`pandas`时不应忘记的一点是`Pandas`的`Series`和`DataFrames`是在`NumPy`库之上设计的。并且，`pandas`可以与`NumPy`阵列和操作无缝衔接。

下面我们使用`NumPy`的 `digitize()`函数更进一步。它类似于上面`pandas`的`cut()`，因为数据将被分箱，但这次它将由一个索引数组表示，这些索引表示每小时所属的`bin`。然后将这些索引应用于价格数组：

```


@timeit(repeat=3, number=100)
def apply_tariff_digitize(df):
    prices = np.array([12, 20, 28])
    bins = np.digitize(df.index.hour.values, bins=[7, 17, 24])
    df['cost_cents'] = prices[bins] * df['energy_kwh'].values



```

与`cut`函数一样，这种语法非常简洁易读。

```


>>> apply_tariff_digitize(df)
Best of 3 trials with 100 function calls per trial:
Function `apply_tariff_digitize` ran in average of 0.002 seconds.



```

**0.002秒！ **虽然仍有性能提升，但已经很边缘化了。

以上就是本次加速的技巧分享。样本数据可在公众号回复：加速 获取。

如果喜欢东哥的骚操作，请给我点个赞和在看![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

* * *

```


我是东哥，最后给大家分享《100本Python电子书》，包括Python编程技巧、数据分析、爬虫、Web开发、机器学习、深度学习。

```


现在免费分享出来，有需要的读者可以下载学习，在下面的公众号「GitHuboy」里回复关键字：Python，就行。

```


![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

```


```


# 

```


爱点赞的人，运气都不会太差![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)


```




```


```




```


```


```

收录于合集 #<a id="js_album_keep_read_title"></a>pandas骚操作

 <a id="js_album_keep_read_size"></a>25个

<a id="js_album_prev"></a>上一篇 <a id="js_album_keep_read_pre_title"></a>pandas100个骚操作：变量类型自动转换 <a id="js_album_next"></a>下一篇 <a id="js_album_keep_read_next_title"></a>pandas100个骚操作：JSON自动解析为DataFrame

People who liked this content also liked

超强图解pandas，建议收藏(文末赠书)

...

Python数据科学

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___008dd7697190433db.bmp"/>

Scan to Follow