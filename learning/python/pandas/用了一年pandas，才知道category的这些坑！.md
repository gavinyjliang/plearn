# 用了一年pandas，才知道category的这些坑！

<a id="copyright_logo"></a>Original 东哥起飞 <a id="profileBt"></a><a id="js_name"></a>Python数据科学 *2021-04-23 14:31*

<a id="js_article-tag-card__left"></a>收录于合集 #pandas骚操作 <a id="js_article-tag-card__right"></a>25个

大家好，我是东哥。

本篇是pandas100个骚操作系列的第 **15** 篇：关于category的一些坑。

系列内容，请看👉「[pandas100个骚操作](https://mp.weixin.qq.com/mp/appmsgalbum?__biz=MzUzODYwMDAzNA==&action=getalbum&album_id=1699019347278561282&scene=21#wechat_redirect)」话题，订阅后文章更新可第一时间推送至订阅号。

* * *

`pandas`有一个特别的数据类型叫`category`，如其名一样，是一种**分类的数据类型**。`category`很娇气，使用的时候稍有不慎就会进坑，因此本篇东哥将介绍在`pandas`中，

**1\. 为什么要使用`category`？**

**2\. 以及使用`category`时需要注意的一些坑！**

> 文中使用的`pandas`版本为1.2.3，于今年2021年3月发布的。

## 为什么使用category数据类型？

总结一下，使用`category`有以下一些好处：

- **内存使用情况**:对于重复值很多的字符串列，`category`可以大大减少将数据存储在内存中所需的内存量;
    
- **运行性能**:进行了一些优化，可以提高某些操作的执行速度
    
- **算法库的适用**：在某些情况下，一些算法模型需要`category`这种类型。比如，我们知道`lightgbm`相对于`xgboost`优化的一个点就是可以处理分类变量，而在构建模型时我们需要指定哪些列是分类变量，并将它们调整为`category`作为超参数传给模型。
    

一个简单的例子。

```
df_size = 100_000
df1 = pd.DataFrame(
    {
        "float_1": np.random.rand(df_size),
        "species": np.random.choice(["cat", "dog", "ape", "gorilla"], size=df_size),
    }
)
df1_cat = df1.astype({"species": "category"})

```

创建了两个`DataFrame`，其中df1包含了species并且为`object`类型，df1_cat复制了df1，但指定了species为`category`类型。

```
>> df1.memory_usage(deep=True)
Index          128
float_1     800000
species    6100448
dtype: int64

```

就内存使用而言，我们可以直接看到包含字符串的列的成本是多高。**species列的字符串大约占用了6MB，如果这些字符串较长，则将会更多。**

```
>> df1_cat.memory_usage(deep=True)
Index         128
float_1    800000
species    100416
dtype: int64

```

再看转换为`category`类别后的内存使用情况。**有了相当大的改进，使用的内存减少了大约60倍**。没有对比，就没有伤害。

这就是使用`category`的其中一个好处。但爱之深，责之切呀，使用它要格外小心。

## 使用`category`的一些坑！

### 一、category列的操作

好吧，这部分应该才是大家较为关心的，因为经常会遇到一些莫名其妙的报错或者感觉哪里不对，又不知道问题出在哪里。

首先，说明一下：**使用`category`的时候需要格外小心，因为如果姿势不对，它就很可能变回`object`**。而变回`object`的结果就是，会降低代码的性能（因为强制转换类型成本很高），并会消耗内存。

日常面对`category`类型的数据，我们肯定是要对其进行操作的，比如做一些转换。下面看一个例子，我们要分别对`category`和`object`类型进行同样的字符串大写操作，使用accessor的`.str`方法。

### 在非category字符串上：

```
>> %timeit df1["species"].str.upper()
25.6 ms ± 2.07 ms per loop (mean ± std. dev. of 7 runs, 10 loops each)

```

### 在category字符串上：

```
>> %timeit df1_cat["species"].str.upper()
1.85 ms ± 41.1 µs per loop (mean ± std. dev. of 7 runs, 1000 loops each)

```

结果很明显了。在这种情况下，速度提高了大约14倍（因为内部优化会让`.str.upper()`仅对分类的唯一类别值调用一次，然后根据结果构造一个seires，而不是对结果中的每个值都去调用一次）。

怎么理解？假设现有一个列叫animal，其类别有`cat`和`dog`两种，假设样本为10000个，4000个`cat`和6000个`dog`。那么如果我用对category本身处理，意味着我只分别对`cat`和`dog`两种类别处理一次，一共两次就解决。如果对每个值处理，那就需要样本数量10000次的处理。

尽管从时间上有了一些优化，然而这种方法的使用也是有一些问题的。。。看一下内存使用情况。

```
>> df1_cat["species"].str.upper().memory_usage(deep=True)
6100576

```

意外的发现`category`类型丢了。。结果竟是一个`object`类型，数据压缩的效果也没了，现在的结果再次回到刚才的6MB内存占用。

这是因为使用`str`会直接让原本的`category`类型强制转换为`object`，所以内存占用又回去了，这是我为什么最开始说要格外小心。

**解决方法就是：直接对category本身操作而不是对它的值操作。** 要直接使用cat的方法来完成转换操作，如下。

```
%timeit df1_cat["species"].cat.rename_categories(str.upper)
239 µs ± 13.9 µs per loop (mean ± std. dev. of 7 runs, 1000 loops each)

```

可以看到，这个速度就更快了，**因为省去了将category类别转换为object的时间，并且内存占用也非常少**。因此，这才是最优的做法。

### 二、与category列的合并

还是上面那个例子，但是这次增加了`habitat`一列，并且`species`中增加了`sanke`。

```
df2 = pd.DataFrame(
    {
        "species": ["cat", "dog", "ape", "gorilla", "snake"],
        "habitat": ["house", "house", "jungle", "jungle", "jungle"],
    }
)
df2_cat = df2.astype({"species": "category", "habitat": "category"})

```

和前面一样，创建该数据集的一个`category`版本，并创建了一个带有`object`字符串的版本。如果将两个`object`列合并在一起的，没什么意思，因为大家都知道会发生什么，`object+ object= object`而已。

### 把object列合并到category列上

还是一个例子。

```
>> df1.merge(df2_cat, on="species").dtypes
float_1     float64
species      object
habitat    category
dtype: object

```

左边的`df1`中`species`列为`object`,右边的`df2_cat`中`species`列为`category`。我们可以看到，当我们合并时，在结果中的合并列会得到**category+ object= object**。

这显然不行了，又回到原来那样了。我们再试下其他情况。

### 两个category列的合并

```
>> df1_cat.merge(df2_cat, on="species").dtypes
float_1     float64
species      object
habitat    category
dtype: object

```

结果是：**category+ category= object？**

有点想打人了，但是别急，我们看看为啥。

**在合并中，为了保存分类类型，两个`category`类型必须是完全相同的。** 这个与`pandas`中的其他数据类型略有不同，例如所有`float64`列都具有相同的数据类型，就没有什么区分。

而当我们讨论`category`数据类型时，该数据类型实际上是由该特定类别中存在的一组值来描述的，因此一个类别包含`["cat", "dog", "mouse"]`与类别包含`["cheese", "milk", "eggs"]`是不一样的。上面的例子之所以没成功，是因为多加了一个`snake`。

因此，我们可以得出结论：

- `category1+ category2=object`
    
- `category1+ category1=category1`
    

因此，**解决办法就是：两个category类别一模一样，让其中一个等于另外一个**。

```
>> df1_cat.astype({"species": df2_cat["species"].dtype}).merge(
       df2_cat, on="species"
   ).dtypes
float_1     float64
species    category
habitat    category
dtype: object

```

### 三、category列的分组

用category类列分组时，一旦误操作就会发生意外，结果是`Dataframe`会被填成空值，还有可能直接跑死。。

**当对`category`列分组时，默认情况下，即使`category`类别的各个类不存在值，也会对每个类进行分组。**

一个例子来说明。

```
habitat_df = (
    df1_cat.astype({"species": df2_cat["species"].dtype})
           .merge(df2_cat, on="species")
)
house_animals_df = habitat_df.loc[habitat_df["habitat"] == "house"]

```

这里采用`habitat_df`，从上面例子得到的，筛选`habitat`为`house`的，只有`dog`和`cat`是`house`，看下面分组结果。

```
>> house_animals_df.groupby("species")["float_1"].mean()
species
ape             NaN
cat        0.501507
dog        0.501023
gorilla         NaN
snake           NaN
Name: float_1, dtype: float64

```

在`groupby`中得到了一堆空值。**默认情况下，当按`category`列分组时，即使数据不存在，`pandas`也会为该类别中的每个值返回结果**。略坑，如果数据类型包含很多不存在的，尤其是在多个不同的`category`列上进行分组，将会极其损害性能。

因此，**解决办法**是：可以传递`observed=True`到`groupby`调用中，这确保了我们仅获取数据中有值的组。

```
>> house_animals_df.groupby("species", observed=True)["float_1"].mean()
species
cat    0.501507
dog    0.501023
Name: float_1, dtype: float64

```

### 四、category列的索引

仍以上面例子举例，使用`groupby-unstack`实现了一个交叉表，`species`作为列，`habitat`作为行，均为`category`类型。

```
>> species_df = habitat_df.groupby(["habitat", "species"], observed=True)["float_1"].mean().unstack()
>> species_df
species       cat       ape       dog   gorilla
habitat                                        
house    0.501507       NaN  0.501023       NaN
jungle        NaN  0.501284       NaN  0.501108

```

这好像看似也没什么毛病，我们继续往下看。为这个交叉表添加一个新列`new_col`，值为1。

```
>> species_df["new_col"] = 1
TypeError: 'fill_value=new_col' is not present in this Categorical's categories

```

正常情况下，上面这段代码是完全可以的，但这里报错了，为什么？

**原因是**：`species`和`habitat`现在均为`category`类型。使用`.unstack()`会把`species`索引移到列索引中（类似`pivot`交叉表的操作）。而当添加的新列不在`species`的分类索引中时，就会报错。

> 虽然平时使用时可能很少用分类作为索引，但是万一恰巧用到了，就要注意一下了。

## 总结

总结一下，`pandas`的`category`类型非常有用，可以带来一些良好的性能优势。但是它也很娇气，使用过程中要尤为小心，确保`category`类型在整个流程中保持不变，避免变回`object`。本文介绍的4个点注意点：

- **category列的变换操作**:直接对category本身操作而不是对它的值操作。这样可以保留分类性质并提高性能。
    
- **category列的合并**：合并时注意，要保留`category`类型，且每个`dataframe`的合并列中的分类类型必须完全匹配。
    
- **category列的分组**:默认情况下，获得数据类型中每个值的结果，即使数据中不存在该结果。可以通过设置`observed=True`调整。
    
- **category列的索引**：当索引为`category`类型的时候，注意是否可能与类别变量发生奇怪的交互作用。
    

**下一篇将介绍关于category的一些骚操作，****原创不易，欢迎点赞、留言、分享，支持我继续写下去![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)**

* * *

我是东哥，最近正在原创👉「[pandas100个骚操作](https://mp.weixin.qq.com/mp/appmsgalbum?__biz=MzUzODYwMDAzNA==&action=getalbum&album_id=1699019347278561282#wechat_redirect)」系列话题，欢迎订阅。订阅后，文章更新可第一时间推送至订阅号，每篇都不错过。

最后给大家**分享《100本Python电子书》**，包括Python编程技巧、数据分析、爬虫、Web开发、机器学习、深度学习。

现在免费分享出来，有需要的读者可以下载学习，在下面的公众号「**GitHuboy**」里回复关键字：**Python**，就行。

![](../../../_resources/0_wx_fmt_png_00b6beabe56648239925fb27fb9697c2.png)

**机器学习专栏**

专注于分享机器学习、深度学习、NLP、CV、PyTorch、TensorFlow、Python实战等干货文章。

<a id="js_profile_article"></a>24篇原创内容

Official Account

收录于合集 #<a id="js_album_keep_read_title"></a>pandas骚操作

 <a id="js_album_keep_read_size"></a>25个

<a id="js_album_prev"></a>上一篇 <a id="js_album_keep_read_pre_title"></a>pandas100个骚操作：groupby 8 个常用技巧！ <a id="js_album_next"></a>下一篇 <a id="js_album_keep_read_next_title"></a>pandas100个骚操作：concat 5 个常用技巧！

<a id="js_view_source"></a>Read more

People who liked this content also liked

超强图解pandas，建议收藏(文末赠书)

...

Python数据科学

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___77ee909ee6524db48.bmp"/>

Scan to Follow