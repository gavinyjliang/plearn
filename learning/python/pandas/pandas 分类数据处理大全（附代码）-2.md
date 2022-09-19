# pandas 分类数据处理大全（附代码）

<a id="profileBt"></a><a id="js_name"></a>小数志 *2022-04-21 12:00*

The following article is from Python数据科学 Author 东哥起飞

<a id="copyright_info"></a>[![](../../../_resources/0_076e4432748c48feb1acc244e7423c0d.jpg)<br>**Python数据科学** .<br>以Python为核心语言，专攻于「数据科学」领域，文章涵盖数据分析，数据挖掘，机器学习等干货内容，分享大量数据挖掘实战项目分析和讲解，以及海量的学习资源。](#)

`category`是`pandas`的一种**分类的定类数据类型**。和文本数据`.str.<methond>`一样，它也有访问器功能`.cat.<method>`。

本文将介绍：

- 什么是分类数据？
    
- 分类数据`cat`的处理方法
    
- 为什么要使用分类数据？
    
- 分类数据`cat`使用时的一些坑
    

## 什么是分类数据？

分类数据表达数值具有某种属性、类型和特征，也是我们理解的定类数据。比如，人口按性别分为男和女，按年龄分为老、中、少。

在计算机语言里，我们通常会用数字来表示，比如用1代表男，0代表女，但是0和1之间并没有大小关系，`pandas`中用`category`来表示分类数据。

**创建分类数据**

创建数据时可以用dtpye来指定类型，比如：

```
s = pd.Series(['a','b','c'],dtype='category')
s
------
0    a
1    b
2    c
dtype: category
Categories (3, object): ['a', 'b', 'c']

```

**自动创建分类数据**

在某些操作情况下会自动转变为分类类型，比如用`cut`进行分箱操作返回的分箱就是分类类型。

```
pd.Series(pd.cut(range(1,10,2),3))
-----------------
0    (0.992, 3.667]
1    (0.992, 3.667]
2    (3.667, 6.333]
3      (6.333, 9.0]
4      (6.333, 9.0]
dtype: category
Categories (3, interval[float64]): [(0.992, 3.667] < (3.667, 6.333] < (6.333, 9.0]]

```

**分类数据类型转换**

直接用`astype`方法转换即可，如：

```
s = pd.Series(['a','b','c'])
s
------
0    a
1    b
2    c
dtype: object
s.astype('category')
------
0    a
1    b
2    c
dtype: category
Categories (3, object): ['a', 'b', 'c']

```

**自定义分类数据**

除此之外，还可以通过`CategoricalDtype`自定义分类数据，自定义的类型适用于以上全部方法。

比如下面自定义了`abc`3个分类，并指定了顺序。然后就可以通过`dtype`指定自定义的数据类型了，`d`不在定义类型`abc`中，显示为空。

```
from pandas.api.types import CategoricalDtype
# 自定义分类数据，有序
c= CategoricalDtype(categories=['a','b','c'],ordered=True)
pd.Series(list('abcabd'),dtype=c)
--------
0      a
1      b
2      c
3      a
4      b
5    NaN
dtype: category
Categories (3, object): ['a' < 'b' < 'c']

```

## 分类数据的处理方法

### 修改分类

通过`.cat.rename_categories()`修改分类的名称。

```
s = pd.Series(['a','b','c'],dtype='category')
# 指定分类为x、y、z
s.cat.categories = ['x','y','z']
0    x
1    y
2    z
dtype: category
Categories (3, object): ['x', 'y', 'z']

```

```
# 列表形式：修改分类类型为mno
s.cat.rename_categories(['m','n','o'])
# 字典形式：
s.cat.rename_categories({'x':'m','y':'n','z':'o'})
0    m
1    n
2    o
dtype: category
Categories (3, object): ['m', 'n', 'o']

```

### 追加新分类

通过`.cat.add_categories()`追加分类。

```
s.cat.add_categories(['r','t'])
0    x
1    y
2    z
dtype: category
Categories (5, object): ['x', 'y', 'z', 'r', 't']

```

### 删除分类

同理，也可以删除分类。有两种方法`remove_categories`和`remove_unused_categories`。

```
# 删除指定的分类r和t
s.cat.remove_categories(['r','t'])
# 自动删除未使用的分类
s.cat.remove_unused_categories()

```

### 顺序

默认情况下分类数据不自动排序，可以通过前面`CategoricalDtype`设置顺序，或者通过`.cat.as_ordered`设置。

```
# 有序设置
s.cat.as_ordered()
0    x
1    y
2    z
dtype: category
Categories (3, object): ['x' < 'y' < 'z']
# 无序设置
s.cat.as_unordered()
# 重新排序
s.cat.reorder_categories(['y','x','z'], ordered=True)

```

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

这就是使用`category`的其中一个好处。

## 使用`category`的一些坑！

但爱之深，责之切呀，`category`有很多坑要注意，这里东哥总结出以下几点，供大家参考。

### 1、category列的操作

好吧，这部分应该才是大家较为关心的，因为经常会遇到一些莫名其妙的报错或者感觉哪里不对，又不知道问题出在哪里。

首先，说明一下：**使用****`category`的时候需要格外小心，因为如果姿势不对，它就很可能变回`object`** 。而变回`object`的结果就是，会降低代码的性能（因为强制转换类型成本很高），并会消耗内存。

日常面对`category`类型的数据，我们肯定是要对其进行操作的，比如做一些转换。下面看一个例子，我们要分别对`category`和`object`类型进行同样的字符串大写操作，使用accessor的`.str`方法。

在非category字符串上：

```
>> %timeit df1["species"].str.upper()
25.6 ms ± 2.07 ms per loop (mean ± std. dev. of 7 runs, 10 loops each)

```

在category字符串上：

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

### 2、与category列的合并

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

**把object列合并到category列上**

接着上面的例子。

```
>> df1.merge(df2_cat, on="species").dtypes
float_1     float64
species      object
habitat    category
dtype: object

```

左边的`df1`中`species`列为`object`,右边的`df2_cat`中`species`列为`category`。我们可以看到，当我们合并时，在结果中的合并列会得到**category+ object= object**。

这显然不行了，又回到原来那样了。我们再试下其他情况。

**两个category列的合并**

```
>> df1_cat.merge(df2_cat, on="species").dtypes
float_1     float64
species      object
habitat    category
dtype: object

```

结果是：**category+ category= object？**

有点想打人了，但是别急，我们看看为啥。

**在合并中，为了保存分类类型，两个****`category`类型必须是完全相同的。** 这个与`pandas`中的其他数据类型略有不同，例如所有`float64`列都具有相同的数据类型，就没有什么区分。

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

### 3、category列的分组

用category类列分组时，一旦误操作就会发生意外，结果是`Dataframe`会被填成空值，还有可能直接跑死。。

**当对****`category`列分组时，默认情况下，即使`category`类别的各个类不存在值，也会对每个类进行分组。**

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

在`groupby`中得到了一堆空值。**默认情况下，当按****`category`列分组时，即使数据不存在，`pandas`也会为该类别中的每个值返回结果**。略坑，如果数据类型包含很多不存在的，尤其是在多个不同的`category`列上进行分组，将会极其损害性能。

因此，**解决办法**是：可以传递`observed=True`到`groupby`调用中，这确保了我们仅获取数据中有值的组。

```
>> house_animals_df.groupby("species", observed=True)["float_1"].mean()
species
cat    0.501507
dog    0.501023
Name: float_1, dtype: float64

```

### 4、category列的索引

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

总结一下，`pandas`的`category`类型非常有用，可以带来一些良好的性能优势。但是它也很娇气，使用过程中要尤为小心，确保`category`类型在整个流程中保持不变，避免变回`object`。本文介绍的4个点注意点：

- **category列的变换操作**:直接对category本身操作而不是对它的值操作。这样可以保留分类性质并提高性能。
    
- **category列的合并**：合并时注意，要保留`category`类型，且每个`dataframe`的合并列中的分类类型必须完全匹配。
    
- **category列的分组**:默认情况下，获得数据类型中每个值的结果，即使数据中不存在该结果。可以通过设置`observed=True`调整。
    
- **category列的索引**：当索引为`category`类型的时候，注意是否可能与类别变量发生奇怪的交互作用。
    

以上就是本次分享内容

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

相关阅读：

- [写在1024：一名数据分析师的修炼之路](http://mp.weixin.qq.com/s?__biz=MzU0OTk5MDg3OQ==&mid=2247485017&idx=1&sn=1c4b44929e0df8e67d5f2460cf2fa37c&chksm=fba63ee9ccd1b7ffe08868d924d25e8bf32bfbe08536bfaa1904fcf049c48d487b4b15bfcda6&scene=21#wechat_redirect)
    
- [数据科学系列：sklearn库主要模块功能简介](http://mp.weixin.qq.com/s?__biz=MzU0OTk5MDg3OQ==&mid=2247484865&idx=1&sn=fcd8cfbf6adb777e693177ad1c169a39&chksm=fba63d71ccd1b4678adac1d357ed8ee70740676e8e637b43cb71eef7492863d8b154c03d43c5&scene=21#wechat_redirect)
    
- [数据科学系列：seaborn入门详细教程](http://mp.weixin.qq.com/s?__biz=MzU0OTk5MDg3OQ==&mid=2247484633&idx=1&sn=bd24e3a089587674a2a6991066ff41c1&chksm=fba63c69ccd1b57f4f48ea2c82b1c79e7fdd7bfaf7c11d65764ff793bdf5ec421e7430354326&scene=21#wechat_redirect)
    
- [数据科学系列：pandas入门详细教程](http://mp.weixin.qq.com/s?__biz=MzU0OTk5MDg3OQ==&mid=2247484410&idx=1&sn=b9654d33bc46795a3c296a6e5e0c6735&chksm=fba63b4accd1b25cb1c31527b3e9dad70917092b19c8f6ffe57404fee642b3e6a3577c8eb727&scene=21#wechat_redirect)
    
- [数据科学系列：matplotlib入门详细教程](http://mp.weixin.qq.com/s?__biz=MzU0OTk5MDg3OQ==&mid=2247484405&idx=1&sn=0e3ec6923c630965d0fbdf633341d0f5&chksm=fba63b45ccd1b253282f253fac55827f723439ca067472772e346343780343f8270a4c1a5298&scene=21#wechat_redirect)
    
- [数据科学系列：numpy入门详细教程](http://mp.weixin.qq.com/s?__biz=MzU0OTk5MDg3OQ==&mid=2247484400&idx=1&sn=f317d6c11ebda49eb3e197fa3f5a5379&chksm=fba63b40ccd1b256afe19cca78f013a65271e1f138d0cae45278e3c45c4fd50042ff9471c5a7&scene=21#wechat_redirect)
    

People who liked this content also liked

Python 全自动解密解码神器 — Ciphey

...

Python实用宝典

不看的原因

- 内容质量低
- 不看此公众号

PostgreSQL 并行框架分析

...

PostgreSQL中文社区

不看的原因

- 内容质量低
- 不看此公众号

Python 3.11比3.10 快60%：使用冒泡排序和递归函数对比测试

...

DeepHub IMBA

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___9649a0f66a7343a8b.bmp"/>

Scan to Follow