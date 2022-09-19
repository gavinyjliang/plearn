# pandas100个骚操作：变量类型自动转换

<a id="copyright_logo"></a>Original 东哥起飞 <a id="profileBt"></a><a id="js_name"></a>Python数据科学 *2021-01-17 13:46*

<a id="js_article-tag-card__left"></a>收录于合集 #pandas骚操作 <a id="js_article-tag-card__right"></a>25个

关注上方“Python数据科学”，选择星标，

关键时间，第一时间送达！

<img width="542" height="275" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__3dfffd1049714eaf8.png"/>

大家好，我是你们的东哥。

本篇是pandas100个骚操作的第一篇：变量**类型自动转换**

在用pandas进行数据清洗的过程中，变量的类型转换是一个必然会遇到的步骤。清洗初期查看`dtypes`经常出现`object`类型，但其实变量本身可能就是个字符串，或者是数字（但因存在空值，导致出现了`object`类型）。

通常大家所熟知的方法是使用`astype`进行类型转换，或者自己利用`astype`造个轮子，写个函数方法实现自动转换类型。

本次东哥介绍一个pandas里可实现自动转换变量类型的方法`convert_dtypes`。利用它可以一次性全部转换为最理想的类型。

## 一、使用方法

默认情况下，`convert_dtypes`将尝试将Series或DataFrame中的每个Series转换为支持的dtypes。它可以对Series和DataFrame都直接使用。

这个方法的参数如下。

```
# 是否应将对象dtypes转换为最佳类型
infer_objects bool，默认为True
# 对象dtype是否应转换为StringDtype()
convert_string bool，默认为True
# 如果可能，是否可以转换为整数扩展类型
convert_integer bool，默认为True
# 对象dtype是否应转换为BooleanDtypes()
convert_boolean bool，默认为True
# 如果可能，是否可以转换为浮动扩展类型。
# 如果convert_integer也为True，则如果可以将浮点数忠实地转换为整数，则将优先考虑整数dtype
convert_floating bool，默认为True

```

## 二、实例

下面看个例子。

首先创建一组数据，通过`dtype`规定每个变量的类型。

```
df = pd.DataFrame(
    {
        "a": pd.Series([1, 2, 3], dtype=np.dtype("int32")),
        "b": pd.Series(["x", "y", "z"], dtype=np.dtype("O")),
        "c": pd.Series([True, False, np.nan], dtype=np.dtype("O")),
        "d": pd.Series(["h", "i", np.nan], dtype=np.dtype("O")),
        "e": pd.Series([10, np.nan, 20], dtype=np.dtype("float")),
        "f": pd.Series([np.nan, 100.5, 200], dtype=np.dtype("float")),
    }
)

```

### DataFrame 变量类型转换

先从整个对dataframe操作开始。

```
>>> df
   a  b      c    d     e      f
0  1  x   True    h  10.0    NaN
1  2  y  False    i   NaN  100.5
2  3  z    NaN  NaN  20.0  200.0

```

```
>>> df.dtypes
a      int32
b     object
c     object
d     object
e    float64
f    float64
dtype: object

```

通过结果可以看到，变量都是是创建时默认的类型。但其实变量是有整数、字符串、布尔的，其中有的还存在空值。

```
>>> dfn = df.convert_dtypes()
>>> dfn
   a  b      c     d     e      f
0  1  x   True     h    10   <NA>
1  2  y  False     i  <NA>  100.5
2  3  z   <NA>  <NA>    20  200.0

```

下面使用`convert_dtypes`进行转换。

```
>>> dfn.dtypes
a      Int32
b     string
c    boolean
d     string
e      Int64
f    Float64
dtype: object

```

变量类型已经转换为我们想要的了。

### Series 变量类型转换

对Series的转换也是一样的。下面的Seires中由于存在`nan`空值所以类型为`object`。

```
s = pd.Series(["a", "b", np.nan])
>>> s
0      a
1      b
2    NaN
dtype: object

```

然后我们通过`convert_dtypes`成功转换为`String`。

```
>>> s.convert_dtypes()
0       a
1       b
2    <NA>
dtype: string

```

如果未来增加了新类型，`convert_dtypes`方法也会同步更新，并支持新的变量类型。

**原创不易，欢迎下方三连，留言和我讨论。**

```


![Image](https://mmbiz.qpic.cn/mmbiz/cZV2hRpuAPiaJQXWGyC9wrUzIicibgXayrgibTYarT3A1yzttbtaO0JlV21wMqroGYT3QtPq2C7HMYsvicSB2p7dTBg/640?wx_fmt=gif&wxfrom=5&wx_lazy=1 "音符")

我是东哥，最后给大家分享《100本Python电子书》，包括Python编程技巧、数据分析、爬虫、Web开发、机器学习、深度学习。

现在免费分享出来，有需要的读者可以下载学习，在下面的公众号「GitHuboy」里回复关键字：Python，就行。

![Image](https://mmbiz.qpic.cn/sz_mmbiz_jpg/NOM5HN2icXzzqzlg1zyGskHksTmufPmic9VDz6XE3JlQfpXrGs5O84t3OMjAakvyicW2uQyN6Jb2tI6UOd8Qic6mhw/640?wx_fmt=jpeg&wxfrom=5&wx_lazy=1&wx_co=1)

```


```


# 

```


爱点赞的人，运气都不会太差<img width="20" height="22" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_28402dca772f42e0b.jpg"/>


```




```


```




```

收录于合集 #<a id="js_album_keep_read_title"></a>pandas骚操作

 <a id="js_album_keep_read_size"></a>25个

<a id="js_album_next"></a>下一篇 <a id="js_album_keep_read_next_title"></a>pandas100个骚操作：再见 for 循环！速度提升315倍！

Modified on 2021-01-18

People who liked this content also liked

超强图解pandas，建议收藏(文末赠书)

...

Python数据科学

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___d4da8aa797a74f119.bmp"/>

Scan to Follow