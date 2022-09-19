# 【Python】Python lambda 函数深度总结

<a id="profileBt"></a><a id="js_name"></a>机器学习初学者 *2022-06-01 12:01* *Posted on <a id="js_ip_wording"></a>浙江*

The following article is from 萝卜大杂烩 Author 周萝卜

<a id="copyright_info"></a>[![](../../../_resources/0_d1e43ae61aea4a0b8a97f1531310c9a2.jpg)<br>**萝卜大杂烩** .<br>Do it by yourself！爱好Python，测试，NLP，数据分析，小程序，k8s等技术，期待与你一同成长](#)

今天我们来学习 Python 中的 lambda 函数，并探讨使用它的优点和局限性

Let's do it！

<img width="657" height="411" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_2cbc267003214e51b.jpg"/>

## 什么是 Python 中的 Lambda 函数

lambda 函数是一个匿名函数（即，没有名称定义），它可以接受任意数量的参数，但与普通函数不同，它只计算并返回一个表达式

Python 中的 lambda 函数使用以下语法表达：

`lambda 参数：表达式`

lambda 函数包括三个元素：

- 关键字 lambda：与普通函数中 def 类似
    
- 参数：支持传递位置和关键字参数，与普通函数一样
    
- 正文：处理定参数的表达式
    

需要注意的是，普通函数不同，这里不需要用括号将 lambda 函数的参数括起来，如果 lambda 函数有两个或更多参数，我们用逗号列出它们

我们使用 lambda 函数只计算一个短表达式（理想情况下，单行）并且只计算一次，这意味着我们以后不会再复用这个函数。通常来说我们会将 lambda 函数作为参数传递给高阶函数（接受其他函数作为参数的函数），例如 Python 内置函数，如 filter()、map() 或 reduce()等

## Python 中的 Lambda 函数如何工作

让我们看一个简单的 lambda 函数示例：

```
`lambda x: x + 1
`
```

Output:

```
`<function __main__.<lambda>(x)>
`
```

上面的 lambda 函数接受一个参数，将其递增 1，然后返回结果

它是以下带有 def 和 return 关键字的普通函数的更简单版本：

```
`def increment_by_one(x):
    return x + 1
`
```

到目前我们的 lambda 函数 `lambda x: x + 1` 只创建一个函数对象，不返回任何内容，这是因为我们没有为其参数 x 提供任何值（参数）。让我们先分配一个变量，将它传递给 lambda 函数，看看这次我们得到了什么：

```
`a = 2
print(lambda x: a + 1)
`
```

Output:

```
`<function <lambda> at 0x00000250CB0A5820>
`
```

我们的 lambda 函数没有像我们预期的那样返回 3，而是返回了函数对象本身及其内存位置，可以看出这不是调用 lambda 函数的正确方法。要将参数传递给 lambda 函数，执行它并返回结果，我们应该使用以下语法：

```
`(lambda x: x + 1)(2)
`
```

Output:

```
`3
`
```

虽然我们的 lambda 函数的参数没有用括号括起来，但当我们调用它时，我们会在 lambda 函数的整个构造以及我们传递给它的参数周围添加括号

上面代码中要注意的另一件事是，使用 lambda 函数，我们可以在创建函数后立即执行该函数并接收结果。这就是所谓的立即调用函数执行（或 IIFE）

我们可以创建一个带有多个参数的 lambda 函数，在这种情况下，我们用逗号分隔函数定义中的参数。当我们执行这样一个 lambda 函数时，我们以相同的顺序列出相应的参数，并用逗号分隔它们：

```
`(lambda x, y, z: x + y + z)(3, 8, 1)
`
```

Output:

```
`12
`
```

也可以使用 lambda 函数来执行条件操作。下面是一个简单 if-else 函数的 lambda 模拟：

```
`print((lambda x: x if(x > 10) else 10)(5))
print((lambda x: x if(x > 10) else 10)(12))
`
```

Output:

```
`10
12
`
```

如果存在多个条件（if-elif-...-else），我们必须嵌套它们：

```
`(lambda x: x * 10 if x > 10 else (x * 5 if x < 5 else x))(11)
`
```

Output:

```
`110
`
```

但是上面的写法，又令代码变得难以阅读

在这种情况下，具有 if-elif-...-else 条件集的普通函数将是比 lambda 函数更好的选择。实际上，我们可以通过以下方式编写上面示例中的 lambda 函数：

```
`def check_conditions(x):
    if x > 10:
        return x * 10
    elif x < 5:
        return x * 5
    else:
        return x
check_conditions(11)
`
```

Output:

```
`110
`
```

尽管上面的函数比相应的 lambda 函数增加了更多行，但它更容易阅读

我们可以将 lambda 函数分配给一个变量，然后将该变量作为普通函数调用：

```
`increment = lambda x: x + 1
increment(2)
`
```

Output:

```
`3
`
```

但是根据 Python 代码的 PEP 8 样式规则，这是一种不好的做法

> 赋值语句的使用消除了 lambda 表达式相对于显式 def 语句所能提供的唯一好处（即，它可以嵌入到更大的表达式中）

因此如果我们确实需要存储一个函数以供进一步使用，我们最好定义一个等效的普通函数，而不是将 lambda 函数分配给变量

## Lambda 函数在 Python 中的应用

### 带有 filter() 函数的 Lambda

Python 中的 filter() 函数需要两个参数：

- 定义过滤条件的函数
    
- 函数在其上运行的可迭代对象
    

运行该函数，我们得到一个过滤器对象：

```
`lst = [33, 3, 22, 2, 11, 1]
filter(lambda x: x > 10, lst)
`
```

Output:

```
`<filter at 0x250cb090520>
`
```

为了从过滤器对象中获取一个新的迭代器，并且原始迭代器中的所有项都满足预定义的条件，我们需要将过滤器对象传递给 Python 标准库的相应函数：list()、tuple()、set ()、frozenset() 或 sorted()（返回排序列表）

让我们过滤一个数字列表，只选择大于 10 的数字并返回一个按升序排序的列表：

```
`lst = [33, 3, 22, 2, 11, 1]
sorted(filter(lambda x: x > 10, lst))
`
```

Output:

```
`[11, 22, 33]
`
```

我们不必创建与原始对象相同类型的新可迭代对象，此外我们可以将此操作的结果存储在一个变量中：

```
`lst = [33, 3, 22, 2, 11, 1]
tpl = tuple(filter(lambda x: x > 10, lst))
tpl
`
```

Output:

```
`(33, 22, 11)
`
```

### 带有 map() 函数的 Lambda

我们使用 Python 中的 map() 函数对可迭代的每个项目执行特定操作。它的语法与 filter() 相同：一个要执行的函数和一个该函数适用的可迭代对象。

map() 函数返回一个 map 对象，我们可以通过将该对象传递给相应的 Python 函数来从中获取一个新的迭代：list()、tuple()、set()、frozenset() 或 sorted()

与 filter() 函数一样，我们可以从 map 对象中提取与原始类型不同类型的可迭代对象，并将其分配给变量。

下面是使用 map() 函数将列表中的每个项目乘以 10 并将映射值作为分配给变量 tpl 的元组输出的示例：

```
`lst = [1, 2, 3, 4, 5]
print(map(lambda x: x * 10, lst))
tpl = tuple(map(lambda x: x * 10, lst))
tpl
`
```

Output:

```
`<map object at 0x00000250CB0D5F40>
(10, 20, 30, 40, 50)
`
```

map() 和 filter() 函数之间的一个重要区别是第一个函数总是返回与原始函数相同长度的迭代。因此由于 pandas Series 对象也是可迭代的，我们可以在 DataFrame 列上应用 map() 函数来创建一个新列：

```
`import pandas as pd
df = pd.DataFrame({'col1': [1, 2, 3, 4, 5], 'col2': [0, 0, 0, 0, 0]})
print(df)
df['col3'] = df['col1'].map(lambda x: x * 10)
df
`
```

Output:

```
 `col1  col2
0     1     0
1     2     0
2     3     0
3     4     0
4     5     0
   col1  col2  col3
0     1     0    10
1     2     0    20
2     3     0    30
3     4     0    40
4     5     0    50`
```

当然要在上述情况下获得相同的结果，也可以使用 apply() 函数：

```
`df['col3'] = df['col1'].apply(lambda x: x * 10)
df
`
```

Output:

```
 `col1  col2  col3
0     1     0    10
1     2     0    20
2     3     0    30
3     4     0    40
4     5     0    50`
```

我们还可以根据某些条件为另一列创建一个新的 DataFrame 列，对于下面的代码，我们可以互换使用 map() 或 apply() 函数：

```
`df['col4'] = df['col3'].map(lambda x: 30 if x < 30 else x)
df
`
```

Output:

```
 `col1  col2  col3  col4
0     1     0    10    30
1     2     0    20    30
2     3     0    30    30
3     4     0    40    40
4     5     0    50    50`
```

### 带有 reduce() 函数的 Lambda

reduce() 函数与 functools Python 模块相关，它的工作方式如下：

- 对可迭代对象的前两项进行操作并保存结果
    
- 对保存的结果和可迭代的下一项进行操作
    
- 以这种方式在值对上进行，直到所有项目使用可迭代的
    

该函数与前两个函数具有相同的两个参数：一个函数和一个可迭代对象。但是与前面的函数不同的是，这个函数不需要传递给任何其他函数，直接返回结果标量值：

```
`from functools import reduce
lst = [1, 2, 3, 4, 5]
reduce(lambda x, y: x + y, lst)
`
```

Output:

```
`15
`
```

上面的代码展示了我们使用 reduce() 函数计算列表总和时的作用

需要注意的是，reduce() 函数总是需要一个带有两个参数的 lambda 函数，而且我们必须首先从 functools Python 模块中导入它

## Python 中 Lambda 函数的优缺点

### 优点

- 它是评估单个表达式的理想选择，应该只评估一次
    
- 它可以在定义后立即调用
    
- 与相应的普通语法相比，它的语法更紧凑
    
- 它可以作为参数传递给高阶函数，例如 filter()、map() 和 reduce()
    

### 缺点

- 它不能执行多个表达式
    
- 它很容易变得麻烦，可读性差，例如当它包括一个 if-elif-...-else 循环
    
- 它不能包含任何变量赋值（例如，lambda x: x=0 将抛出一个语法错误）
    
- 我们不能为 lambda 函数提供文档字符串
    

## 总结

总而言之，我们已经详细讨论了在 Python 中定义和使用 lambda 函数的许多方面：

- lambda 函数与普通 Python 函数有何不同
    
- Python 中 lambda 函数的语法和剖析
    
- 何时使用 lambda 函数
    
- lambda 函数的工作原理
    
- 如何调用 lambda 函数
    
- 调用函数执行（IIFE）的定义
    
- 如何使用 lambda 函数执行条件操作，如何嵌套多个条件，以及为什么我们应该避免它
    
- 为什么我们应该避免将 lambda 函数分配给变量
    
- 如何将 lambda 函数与 filter() 函数一起使用
    
- 如何将 lambda 函数与 map() 函数一起使用
    
- 我们如何在 pandas DataFrame 中使用
    
- 带有传递给它的 lambda 函数的 map() 函数 - 以及在这种情况下使用的替代功能
    
- 如何将 lambda 函数与 reduce() 函数一起使用
    
- 在普通 Python 上使用 lambda 函数的优缺点
    

希望今天的讨论可以使 Python 中看似令人生畏的 lambda 函数概念更清晰、更易于应用，更希望小伙伴们能够喜欢，喜欢就点个**赞**吧！

```


![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

```


```


```


```


```


往期精彩回顾

- [适合初学者入门人工智能的路线及资料下载](http://mp.weixin.qq.com/s?__biz=MzIwODI2NDkxNQ==&mid=2247484737&idx=1&sn=27c52b4bc4ca98d3ab817344b84226cc&chksm=97048efda07307eb78d4f4ec0039a386a658404156b051af0cb715fafa8d2ae66cbe49343bf3&scene=21#wechat_redirect)
    
- [(图文+视频)机器学习入门系列下载](https://mp.weixin.qq.com/mp/appmsgalbum?__biz=MzIwODI2NDkxNQ==&action=getalbum&album_id=2259163844755406853#wechat_redirect)
    
- [中国大学慕课《机器学习》（黄海广主讲）](http://mp.weixin.qq.com/s?__biz=MzIwODI2NDkxNQ==&mid=2247502323&idx=1&sn=598d7231681ce2f316503201dd615c86&chksm=9707424fa070cb59f861a47f9eb4218cc5d3b835be49e93e67bb086dcc4c3b555a3546ebe9c5&scene=21#wechat_redirect)
    
- [机器学习及深度学习笔记等资料打印](http://mp.weixin.qq.com/s?__biz=MzIwODI2NDkxNQ==&mid=2247488304&idx=1&sn=581944f63eab1822ca53b9a4eeedad79&chksm=9704988ca073119a38a534adbedd51ca0b5705cdd6a104fed74b265bb092485e97c91bb5b347&scene=21#wechat_redirect)
    
- [《统计学习方法》的代码复现专辑](https://mp.weixin.qq.com/mp/appmsgalbum?action=getalbum&album_id=1337257945842778113&__biz=MzIwODI2NDkxNQ==#wechat_redirect)
    
- 机器学习交流qq群955171419，加入微信群请扫码
    




```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)


```


```


```


```






```

People who liked this content also liked

关于大龄读博的几点回答？

...

机器学习初学者

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___6820b0642b274f628.bmp"/>

Scan to Follow