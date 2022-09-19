# pandas100个骚操作：强大的 accessor 方法

<a id="copyright_logo"></a>Original 东哥起飞 <a id="profileBt"></a><a id="js_name"></a>Python数据科学 *2021-01-23 13:36*

<a id="js_article-tag-card__left"></a>收录于合集 #pandas骚操作 <a id="js_article-tag-card__right"></a>25个

关注上方“Python数据科学”，选择星标，

关键时间，第一时间送达！

<img width="657" height="334" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__07769bca5f6145fd8.png"/>

来源：Python数据科学

作者：东哥起飞

大家好，我是你们的东哥。

本篇是pandas100个骚操作系列的第 **6**篇：**强大的 accessor 方法**

系列全部内容请看文章标题下方的「**pandas100个骚操作**」话题，订阅后可更新可第一时间推送文章。

* * *

pandas有一种功能非常强大的方法，它就是`accessor`，可以将它理解为一种属性接口，通过它可以获得额外的方法。其实这样说还是很笼统，下面我们通过代码和实例来理解一下。

```
>>> pd.Series._accessors
{'cat', 'str', 'dt'}

```

对于Series数据结构使用`_accessors`方法，我们得到了3个对象：`cat`，`str`，`dt`。

- .cat：用于分类数据（Categorical data）
    
- .str：用于字符数据（String Object data）
    
- .dt：用于时间数据（datetime-like data）
    

下面我们依次看一下这三个对象是如何使用的。

## str对象的使用

> Series数据类型：str字符串

```
# 定义一个Series序列
>>> addr = pd.Series([
...     'Washington, D.C. 20003',
...     'Brooklyn, NY 11211-1755',
...     'Omaha, NE 68154',
...     'Pittsburgh, PA 15211'
... ]) 
>>> addr.str.upper()
0     WASHINGTON, D.C. 20003
1    BROOKLYN, NY 11211-1755
2            OMAHA, NE 68154
3       PITTSBURGH, PA 15211
dtype: object
>>> addr.str.count(r'\d') 
0    5
1    9
2    5
3    5
dtype: int64

```

关于以上str对象的2个方法说明：

- `Series.str.upper`：将Series中所有字符串变为大写；
    
- `Series.str.count`：对Series中所有字符串的个数进行计数；
    

其实不难发现，该用法的使用与Python中字符串的操作很相似。没错，在pandas中你一样可以这样简单的操作，而不同的是你操作的是一整列的字符串数据。仍然基于以上数据集，再看它的另一个操作：

```
>>> regex = (r'(?P<city>[A-Za-z ]+), '      # 一个或更多字母
...          r'(?P<state>[A-Z]{2}) '        # 两个大写字母
...          r'(?P<zip>\d{5}(?:-\d{4})?)')  # 可选的4个延伸数字
...
>>> addr.str.replace('.', '').str.extract(regex)
         city state         zip
0  Washington    DC       20003
1    Brooklyn    NY  11211-1755
2       Omaha    NE       68154
3  Pittsburgh    PA       15211

```

关于以上str对象的2个方法说明：

- `Series.str.replace`：将Series中指定字符串替换；
    
- `Series.str.extract`：通过正则表达式提取字符串中的数据信息；
    

这个用法就有点复杂了，因为很明显看到，这是一个链式的用法。通过`replace`将 `" . "` 替换为`""`，即为空，紧接着又使用了3个正则表达式（分别对应city，state，zip）通过`extract`对数据进行了提取，并由原来的Series数据结构变为了DataFrame数据结构。

当然，除了以上用法外，常用的属性和方法还有`.rstrip`，.`contains`，`split`等，我们通过下面代码查看一下`str`属性的完整列表：

```
>>> [i for i in dir(pd.Series.str) if not i.startswith('_')]
['capitalize',
 'cat',
 'center',
 'contains',
 'count',
 'decode',
 'encode',
 'endswith',
 'extract',
 'extractall',
 'find',
 'findall',
 'get',
 'get_dummies',
 'index',
 'isalnum',
 'isalpha',
 'isdecimal',
 'isdigit',
 'islower',
 'isnumeric',
 'isspace',
 'istitle',
 'isupper',
 'join',
 'len',
 'ljust',
 'lower',
 'lstrip',
 'match',
 'normalize',
 'pad',
 'partition',
 'repeat',
 'replace',
 'rfind',
 'rindex',
 'rjust',
 'rpartition',
 'rsplit',
 'rstrip',
 'slice',
 'slice_replace',
 'split',
 'startswith',
 'strip',
 'swapcase',
 'title',
 'translate',
 'upper',
 'wrap',
 'zfill']

```

属性有很多，对于具体的用法，如果感兴趣可以自己进行摸索练习。

## dt对象的使用

> Series数据类型：datetime

因为数据需要`datetime`类型，所以下面使用pandas的`date_range()`生成了一组日期`datetime`演示如何进行`dt`对象操作。

```
>>> daterng = pd.Series(pd.date_range('2017', periods=9, freq='Q'))
>>> daterng
0   2017-03-31
1   2017-06-30
2   2017-09-30
3   2017-12-31
4   2018-03-31
5   2018-06-30
6   2018-09-30
7   2018-12-31
8   2019-03-31
dtype: datetime64[ns]
>>>  daterng.dt.day_name()
0      Friday
1      Friday
2    Saturday
3      Sunday
4    Saturday
5    Saturday
6      Sunday
7      Monday
8      Sunday
dtype: object
>>> # 查看下半年
>>> daterng[daterng.dt.quarter > 2]
2   2017-09-30
3   2017-12-31
6   2018-09-30
7   2018-12-31
dtype: datetime64[ns]
>>> daterng[daterng.dt.is_year_end]
3   2017-12-31
7   2018-12-31
dtype: datetime64[ns]

```

以上关于dt的3种方法说明：

- `Series.dt.day_name()`：从日期判断出所处星期数；
    
- `Series.dt.quarter`：从日期判断所处季节；
    
- `Series.dt.is_year_end`：从日期判断是否处在年底；
    

其它方法也都是基于datetime的一些变换，并通过变换来查看具体微观或者宏观日期。

## cat对象的使用

> Series数据类型：Category

在说cat对象的使用前，先说一下`Category`这个数据类型，它的作用很强大。虽然我们没有经常性的在内存中运行上g的数据，但是我们也总会遇到执行几行代码会等待很久的情况。使用Category数据的一个好处就是：可以很好的节省在时间和空间的消耗。下面我们通过几个实例来学习一下。

```
>>> colors = pd.Series([
...     'periwinkle',
...     'mint green',
...     'burnt orange',
...     'periwinkle',
...     'burnt orange',
...     'rose',
...     'rose',
...     'mint green',
...     'rose',
...     'navy'
... ])
...
>>> import sys
>>> colors.apply(sys.getsizeof)
0    59
1    59
2    61
3    59
4    61
5    53
6    53
7    59
8    53
9    53
dtype: int64

```

上面我们通过使用`sys.getsizeof`来显示内存占用的情况，数字代表字节数。还有另一种计算内容占用的方法：`memory_usage()`，后面会使用。

现在我们将上面colors的不重复值映射为一组整数，然后再看一下占用的内存。

```
>>> mapper = {v: k for k, v in enumerate(colors.unique())}
>>> mapper
{'periwinkle': 0, 'mint green': 1, 'burnt orange': 2, 'rose': 3, 'navy': 4}
>>> as_int = colors.map(mapper)
>>> as_int
0    0
1    1
2    2
3    0
4    2
5    3
6    3
7    1
8    3
9    4
dtype: int64
>>> as_int.apply(sys.getsizeof)
0    24
1    28
2    28
3    24
4    28
5    28
6    28
7    28
8    28
9    28
dtype: int64

```

> 注：对于以上的整数值映射也可以使用更简单的pd.factorize()方法代替。

我们发现上面所占用的内存是使用`object`类型时的一半。其实，这种情况就类似于`Category data`类型内部的原理。

> 内存占用区别：Categorical所占用的内存与Categorical分类的数量和数据的长度成正比，相反，object所占用的内存则是一个常数乘以数据的长度。

下面是`object`内存使用和`category`内存使用的情况对比。

```
>>> colors.memory_usage(index=False, deep=True)
650
>>> colors.astype('category').memory_usage(index=False, deep=True)
495

```

上面结果是使用`object`和`Category`两种情况下内存的占用情况。

我们发现效果并没有我们想象中的那么好。但是注意`Category`内存是成比例的，如果数据集的数据量很大，但不重复分类（unique）值很少的情况下，那么`Category`的内存占用可以节省达到10倍以上，比如下面数据量增大的情况：

```
>>> manycolors = colors.repeat(10)
>>> len(manycolors) / manycolors.nunique() 
20.0
>>> manycolors.memory_usage(index=False, deep=True)
6500
>>> manycolors.astype('category').memory_usage(index=False, deep=True)
585

```

可以看到，在数据量增加10倍以后，使用`Category`所占内容节省了10倍以上。

除了占用内存节省外，另一个额外的好处是计算效率有了很大的提升。因为对于`Category`类型的`Series`，str字符的操作发生在`.cat.categories`的非重复值上，而并非原Series上的所有元素上。也就是说对于每个非重复值都只做一次操作，然后再向与非重复值同类的值映射过去。

对于`Category`的数据类型，可以使用`accessor`的`cat`对象，以及相应的属性和方法来操作Category数据。

```
>>> ccolors = colors.astype('category')
>>> ccolors.cat.categories
Index(['burnt orange', 'mint green', 'navy', 'periwinkle', 'rose'], dtype='object')

```

实际上，对于开始的整数类型映射，我们可以先通过`reorder_categories`进行重新排序，然后再使用`cat.codes`来实现对整数的映射，来达到同样的效果。

```
>>> ccolors.cat.reorder_categories(mapper).cat.codes
0    0
1    1
2    2
3    0
4    2
5    3
6    3
7    1
8    3
9    4
dtype: int8

```

`dtype`类型是Numpy的`int8（-127~128）`。可以看出以上只需要一个单字节就可以在内存中包含所有的值。我们开始的做法默认使用了int64类型，然而通过pandas的使用可以很智能的将`Category`数据类型变为最小的类型。

让我们来看一下`cat`还有什么其它的属性和方法可以使用。下面`cat`的这些属性基本都是关于查看和操作`Category`数据类型的。

```
>>> [i for i in dir(ccolors.cat) if not i.startswith('_')]
['add_categories',
 'as_ordered',
 'as_unordered',
 'categories',
 'codes',
 'ordered',
 'remove_categories',
 'remove_unused_categories',
 'rename_categories',
 'reorder_categories',
 'set_categories']

```

但是`Category`数据的使用不是很灵活。例如，插入一个之前没有的值，首先需要将这个值添加到`.categories`的容器中，然后再添加值。

```
>>> ccolors.iloc[5] = 'a new color'
# ...
ValueError: Cannot setitem on a Categorical with a new category,
set the categories first
>>> ccolors = ccolors.cat.add_categories(['a new color'])
>>> ccolors.iloc[5] = 'a new color'  

```

如果你想设置值或重塑数据，而非进行新的运算操作，那么`Category`类型不是那么有用。

* * *

```


我是东哥，最后给大家分享《100本Python电子书》，包括Python编程技巧、数据分析、爬虫、Web开发、机器学习、深度学习。

现在免费分享出来，有需要的读者可以下载学习，在下面的公众号「GitHuboy」里回复关键字：Python，就行。

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

收录于合集 #<a id="js_album_keep_read_title"></a>pandas骚操作

 <a id="js_album_keep_read_size"></a>25个

<a id="js_album_prev"></a>上一篇 <a id="js_album_keep_read_pre_title"></a>pandas100个骚操作：生成器\_\_iter\_\_分析数据样本 <a id="js_album_next"></a>下一篇 <a id="js_album_keep_read_next_title"></a>pandas100个骚操作：一行 pandas 代码搞定 Excel “条件格式”！

<a id="js_view_source"></a>Read more

People who liked this content also liked

超强图解pandas，建议收藏(文末赠书)

...

Python数据科学

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___856794e92e2447b3a.bmp"/>

Scan to Follow