# pandas100个骚操作：transform 数据转换的 4 个常用技巧！

<a id="copyright_logo"></a>Original 东哥起飞 <a id="profileBt"></a><a id="js_name"></a>Python数据科学 *2021-01-28 13:46*

<a id="js_article-tag-card__left"></a>收录于合集 #pandas骚操作 <a id="js_article-tag-card__right"></a>25个

关注上方“Python数据科学”，选择星标，

关键时间，第一时间送达！

<img width="657" height="334" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__98049b4dd9d3458d8.png"/>

来源：Python数据科学

作者：东哥起飞

大家好，我是东哥。

本篇是pandas100个骚操作系列的第 **8**篇：**transform 数据转换的 4 个常用技巧！**

系列内容，请看👉「[pandas100个骚操作](https://mp.weixin.qq.com/mp/appmsgalbum?__biz=MzUzODYwMDAzNA==&action=getalbum&album_id=1699019347278561282#wechat_redirect)」话题，订阅后文章更新可第一时间推送至订阅号。

* * *

本次给大家介绍一个功能超强的数据处理函数`transform`，相信很多朋友也用过，东哥这里再次进行详细分享下。

`transform`有4个比较常用的功能，总结如下：

- 转换数值
    
- 合并分组结果
    
- 过滤数据
    
- 结合分组处理缺失值
    

## 一. 转换数值

```
pd.transform(func, axis=0)

```

以上就是`transform`转换数值的基本用法，参数含义如下：

- `func`是指定用于处理数据的函数，它可以是`普通函数`、`字符串函数名称`、`函数列表`或`轴标签映射函数的字典`。
    
- `axis`是指要应用到哪个轴，`0`代表列，`1`代表行。
    

### 1\. 普通函数

`func`可以是我们正常使用的普通函数，像下面例子这样自定义一个函数。

```
df = pd.DataFrame({'A': [1,2,3], 'B': [10,20,30] })
def plus_10(x):
    return x+10
df.transform(plus_10)

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

或者，也可以用`lambda`函数简洁的实现，效果是一样的。

```
df.transform(lambda x: x+10)

```

### 2\. 字符串函数

也可以传递任何有效的`pandas`内置的字符串函数，例如`sqrt`：

```
df.transform('sqrt')

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 3\. 函数列表

`func`还可以是一个函数的列表。例如`numpy`的`sqrt`和`exp`函数的列表组合：

```
df.transform([np.sqrt, np.exp])

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 通过上面结果看到，两个函数分别作用于`A`和`B`每个列。

### 4\. 轴标签映射函数的字典

如果我们只想将指定函数作用于某一列，该如何操作？

`func`还可以是轴标签映射指定函数的字典。例如：

```
df.transform({
    'A': np.sqrt,
    'B': np.exp,
})

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

这样，就可以对`A`和`BL`两列分别使用相应函数了，互补干扰。

## 二、合并分组结果

这个功能是东哥最喜欢的，有点类似`SQL`的窗口函数，就是可以合并`grouby()`的分组结果。用一个例子说明：

```
df = pd.DataFrame({
  'restaurant_id': [101,102,103,104,105,106,107],
  'address': ['A','B','C','D', 'E', 'F', 'G'],
  'city': ['London','London','London','Oxford','Oxford', 'Durham', 'Durham'],
  'sales': [10,500,48,12,21,22,14]
})

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

我们可以看到，每个城市都有多家销售餐厅。我们现在想知道**每家餐厅在城市中所占的销售百分比是多少。** 预期输出为：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

传统方法是：先`groupby`分组，结合`apply`计算分组求和，再用`merge`合并原表，然后再`apply`计算百分比。

但其实用`transform`可以直接代替前面两个步骤（分组求和、合并），简单明了。

首先，用`transform`结合`groupby`按城市分组计算销售总和。

```
df['city_total_sales'] = df.groupby('city')['sales']
                           .transform('sum')

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

**可以看到，使用**`transfrom`**计算分组的求和并不会像**`apply`**一样改变原表的结构，而是直接在原表的基础上再增加一列。**

这样就可以一步到位，得到我们想要的格式。

然后，再计算百分比调整格式，搞定。

```
df['pct'] = df['sales'] / df['city_total_sales']
df['pct'] = df['pct'].apply(lambda x: format(x, '.2%'))

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 三、过滤数据

`transform`也可以用来过滤数据。仍用上个例子，我们希望获得城市总销售额超过40的记录，那么就可以这样使用。

```
df[df.groupby('city')['sales'].transform('sum') > 40]

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 上面结果来看，并没有生成新的列，而是通过汇总计算求和直接对原表进行了筛选，非常优雅。

## 四、结合分组处理缺失值

```
df = pd.DataFrame({
    'name': ['A', 'A', 'B', 'B', 'B', 'C', 'C', 'C'],
    'value': [1, np.nan, np.nan, 2, 8, 2, np.nan, 3]
})
```

在上面的示例中，数据可以按`name`分为三组A、B、C，每组都有缺失值。我们知道替换缺失值的常见的方法是用`mean`替换`NaN`。下面是每个组中的平均值。

```
df.groupby('name')['value'].mean()
name
A    1.0
B    5.0
C    2.5
Name: value, dtype: float64

```

我们可以通过`transform()`使用每组平均值来替换缺失值。用法如下：

```
df['value'] = df.groupby('name')
                .transform(lambda x: x.fillna(x.mean()))

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

以上就是本次关于`transform`的数据转换操作分享。

如果喜欢东哥的骚操作，请给我点个赞和在看![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

* * *

```


我是东哥，最后给大家分享《100本Python电子书》，包括Python编程技巧、数据分析、爬虫、Web开发、机器学习、深度学习。

```


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


```

收录于合集 #<a id="js_album_keep_read_title"></a>pandas骚操作

 <a id="js_album_keep_read_size"></a>25个

<a id="js_album_prev"></a>上一篇 <a id="js_album_keep_read_pre_title"></a>pandas100个骚操作：一行 pandas 代码搞定 Excel “条件格式”！ <a id="js_album_next"></a>下一篇 <a id="js_album_keep_read_next_title"></a>pandas100个骚操作：explode 列转行的 2 个常用技巧！

<a id="js_view_source"></a>Read more

People who liked this content also liked

超强图解pandas，建议收藏(文末赠书)

...

Python数据科学

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___c54dfc0d583f46eaa.bmp"/>

Scan to Follow