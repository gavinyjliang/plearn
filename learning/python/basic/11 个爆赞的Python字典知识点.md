# 11 个爆赞的Python字典知识点

<a id="profileBt"></a><a id="js_name"></a>数据STUDIO *2022-02-18 08:35*

The following article is from 快学Python Author 快快

<a id="copyright_info"></a>[![](../../../_resources/0_36a0e8a29866473697a6885cc8f282f8.png)<br>**快学Python** .<br>Python可视化、自动化办公、数据分析、爬虫、Web开发！人生苦短，快学Python！](#)

大家好，我是云朵君！
**关注和星标『数据STUDIO』**，和云朵君一起学习数据分析与挖掘！

![](../../../_resources/0_wx_fmt_png_7bbf70f865c74df9bc181d131e0eac12.png)

**数据STUDIO**

点击领取《Python学习手册》，后台回复「福利」获取。『数据STUDIO』专注于数据科学原创文章分享，内容以 Python 为核心语言，涵盖机器学习、数据分析、可视化、MySQL等领域干货知识总结及实战项目。

<a id="js_profile_article"></a>173篇原创内容

Official Account

👆点击关注｜设为星标｜干货速递👆

* * *

关于Python字典，算是Python中相当重要的数据类型了。在你学会基础知识后，`字典`这个概念，将会伴随着你后面的学习和工作。

因此，这里有几个相当重要的知识点，大家有必要知道。

## 字典是否是无序的

关于这个概念，很多朋友不一定清楚。

在 Python 2.7 中，字典是无序结构。字典项目的顺序是混乱的。这意味着项目的顺序是确定性和可重复的。

```


>>> # Python 2.7
>>> a_dict = {'color': 'blue', 'fruit': 'apple', 'pet': 'dog'}
>>> a_dict
{'color': 'blue', 'pet': 'dog', 'fruit': 'apple'}
>>> a_dict
{'color': 'blue', 'pet': 'dog', 'fruit': 'apple'}



```

在 Python 3.5 中，字典仍然是无序的，但这次是随机的数据结构。这意味着每次重新运行字典时，您都会得到不同的项目顺序。

```


>>> # Python 3.5
>>> a_dict = {'color': 'blue', 'fruit': 'apple', 'pet': 'dog'}
>>> a_dict
{'color': 'blue', 'pet': 'dog', 'fruit': 'apple'}
>>> a_dict
{'color': 'blue', 'pet': 'dog', 'fruit': 'apple'}



```

在 Python 3.6 及更高版本中，字典是有序的数据结构，这意味着它们保持元素的顺序与它们被引入时的顺序相同。

```


>>> a_dict = {'color': 'blue', 'fruit': 'apple', 'pet': 'dog'}
>>> a_dict
{'color': 'blue', 'fruit': 'apple', 'pet': 'dog'}
>>> a_dict
{'color': 'blue', 'fruit': 'apple', 'pet': 'dog'}



```

## 键值互换

假设您有一本字典，由于某种原因需要将键转换为值，值转换为键，应该怎么做呢？

```


>>> a_dict = {'one': 1, 'two': 2, 'thee': 3, 'four': 4}
>>> new_dict = {}
>>> for key, value in a_dict.items():
...     new_dict[value] = key
...
>>> new_dict
{1: 'one', 2: 'two', 3: 'thee', 4: 'four'}



```

## 依据某种条件，过滤字典

有时候，你需要根据某种条件来过滤字典。那么配合if条件语句，是一个很好的选择。

```


>>> a_dict = {'one': 1, 'two': 2, 'thee': 3, 'four': 4}
>>> new_dict = {}  # Create a new empty dictionary
>>> for key, value in a_dict.items():
...     if value <= 2:
...         new_dict[key] = value
...
>>> new_dict
{'one': 1, 'two': 2}



```

## 利用字典中的值，做一些计算

在Python中遍历字典时。需要进行一些计算也是很常见的。假设您已将公司销售额的数据存储在字典中，现在您想知道一年的总收入。

```


>>> incomes = {'apple': 5600.00, 'orange': 3500.00, 'banana': 5000.00}
>>> total_income = 0.00
>>> for value in incomes.values():
...     total_income += value  # Accumulate the values in total_income
...
>>> total_income
14100.0



```

## 字典推导式

字典推导式，是一个和列表推导式一样，具有很强大功能的知识点。因此，大家一定要掌握。

例如，假设您有两个数据列表，您需要根据它们创建一个新字典。

```


>>> objects = ['blue', 'apple', 'dog']
>>> categories = ['color', 'fruit', 'pet']
>>> a_dict = {key: value for key, value in zip(categories, objects)}
>>> a_dict
{'color': 'blue', 'fruit': 'apple', 'pet': 'dog'}



```

## 利用字典推导式，实现键值转换

你会发现，使用字典推导式，是一个更简单、高效的操作。

```


>>> a_dict = {'one': 1, 'two': 2, 'thee': 3, 'four': 4}
>>> new_dict = {value: key for key, value in a_dict.items()}
>>> new_dict
{1: 'one', 2: 'two', 3: 'thee', 4: 'four'}



```

## 利用字典推导式，过滤字典

```


>>> a_dict = {'one': 1, 'two': 2, 'thee': 3, 'four': 4}
>>> new_dict = {k: v for k, v in a_dict.items() if v <= 2}
>>> new_dict
{'one': 1, 'two': 2}



```

## 利用字典推导式，做一些计算

```


>>> incomes = {'apple': 5600.00, 'orange': 3500.00, 'banana': 5000.00}
>>> total_income = sum([value for value in incomes.values()])
>>> total_income
14100.0



```

## 字典排序

从 Python 3.6 开始，字典是有序的数据结构，因此如果您使用 Python 3.6（及更高版本），您将能够通过使用sorted()并借助字典理解对任何字典的键，进行排序。

```


>> incomes = {'apple': 5600.00, 'orange': 3500.00, 'banana': 5000.00}
>>> sorted_income = {k: incomes[k] for k in sorted(incomes)}
>>> sorted_income
{'apple': 5600.0, 'banana': 5000.0, 'orange': 3500.0}



```

## 内置函数，与字典配合使用

Python 提供了一些内置函数，这些函数在您处理集合（如字典）时可能会很有用。

#### map()函数

假设您有一个包含一堆产品价格的字典，并且您需要对它们应用折扣。

```


>>> prices = {'apple': 0.40, 'orange': 0.35, 'banana': 0.25}
>>> def discount(current_price):
...     return (current_price[0], round(current_price[1] * 0.95, 2))
...
>>> new_prices = dict(map(discount, prices.items()))
>>> new_prices
{'apple': 0.38, 'orange': 0.33, 'banana': 0.24}



```

#### filter()函数

假设您想知道单价低于0.40的产品。

```


>>> prices = {'apple': 0.40, 'orange': 0.35, 'banana': 0.25}
>>> def has_low_price(price):
...     return prices[price] < 0.4
...
>>> low_price = list(filter(has_low_price, prices.keys()))
>>> low_price
['orange', 'banana']



```

## 字典解包运算符

这是很多人不清楚的概念，Python 3.5 带来了一个有趣的新特性，因此大家需要着重学习。

您可以使用字典解包运算符 ( **) 将两个字典合并为一个新字典。

```


>>> vegetable_prices = {'pepper': 0.20, 'onion': 0.55}
>>> fruit_prices = {'apple': 0.40, 'orange': 0.35, 'pepper': .25}
>>> {**vegetable_prices, **fruit_prices}
{'pepper': 0.25, 'onion': 0.55, 'apple': 0.4, 'orange': 0.35}



```

如果您尝试合并的字典，具有重复或公共键，则最右侧字典的值将补充上。

**往期推荐 点击查看**

[Python 绘制惊艳的桑基图](https://mp.weixin.qq.com/s?__biz=Mzk0OTI1OTQ2MQ==&mid=2247513400&idx=1&sn=d8225f1f055dc778bf7915cb364c0dfc&chksm=c359e892f42e61845ee900ca8cd6baf076e817acf7bbc9f7dfdb1ebb15f4a42b016125fddfb9&scene=21#wechat_redirect)

[Python时间序列分析和预测实战](https://mp.weixin.qq.com/s?__biz=Mzk0OTI1OTQ2MQ==&mid=2247512759&idx=1&sn=90234bcb1c4fa611dd9f3e13af0e329e&chksm=c359ef1df42e660b873dc9023b936c94a3de82d40b29482507a7e6d2bf97d37864e94e056f73&scene=21#wechat_redirect)

[Python中最常用的 14 种数据可视化类型的概念与代码](https://mp.weixin.qq.com/s?__biz=Mzk0OTI1OTQ2MQ==&mid=2247512690&idx=1&sn=8cc6adb2da980ce6d21ff5f70cef9e3f&chksm=c359efd8f42e66cea0f74b1f57448d9235c9d4f00ba0757b402aa8dce104d30ddb4380c0a8ec&scene=21#wechat_redirect)

[Python 绘制惊艳的瀑布图](https://mp.weixin.qq.com/s?__biz=Mzk0OTI1OTQ2MQ==&mid=2247511167&idx=1&sn=d3688e5c8c6b7e679f78439e858ff85c&chksm=c359f1d5f42e78c31c4c0273e6aaa1faa2e89a5d164eb54012018ebf3cf57c9da1d59bcec912&scene=21#wechat_redirect)

[50个量身定制的Python学习资源，建议收藏！](https://mp.weixin.qq.com/s?__biz=Mzk0OTI1OTQ2MQ==&mid=2247511215&idx=2&sn=96bfc63fb43db9aa42ad071c33293f4a&chksm=c359f105f42e781343adbfa235b13635349d8382f4780960fc1e8bef1e89e3e3e47c1382059b&scene=21#wechat_redirect)

[Python 使用和高性能技巧总结](https://mp.weixin.qq.com/s?__biz=Mzk0OTI1OTQ2MQ==&mid=2247510427&idx=2&sn=ed94f6db2452d28ee4139305a18e688f&chksm=c359f431f42e7d2784d4c35816a1e3593cb2e07231409a16db3e153d268fb4c5923c22028faa&scene=21#wechat_redirect)

[Python 加速运行技巧](https://mp.weixin.qq.com/s?__biz=Mzk0OTI1OTQ2MQ==&mid=2247511167&idx=2&sn=eb97c38aa44399b1397fe775c6b3bb98&chksm=c359f1d5f42e78c3d8b07817053666d2842cc8d01e82e427d60816c3fdc3bd3ca3c1c8c869fc&scene=21#wechat_redirect)

[这个插件竟打通了Python和Excel，还能自动生成代码！](https://mp.weixin.qq.com/s?__biz=Mzk0OTI1OTQ2MQ==&mid=2247509192&idx=1&sn=3a38f58bbd91e62e381acc1c47b7d9eb&chksm=c359f962f42e7074d69a2348c8ad698a96cb39f9029d1eec69cd4f4b7b02a47a3ca90dc3a6e7&scene=21#wechat_redirect)

[再见 Seaborn！Altair 数据可视化已超神](https://mp.weixin.qq.com/s?__biz=Mzk0OTI1OTQ2MQ==&mid=2247508705&idx=1&sn=a535acb7f859325ad43cdb22b7ec0951&chksm=c359ff4bf42e765dd2291dbaf8d747d48f69e4fa9d6ba3b5b07b069d492b4cae7f2b4d088208&scene=21#wechat_redirect)

[用一行Python代码创建高级财务图表](https://mp.weixin.qq.com/s?__biz=Mzk0OTI1OTQ2MQ==&mid=2247507142&idx=1&sn=730166685a5099d994a700a95b6f31d2&chksm=c359816cf42e087a221a7b5b4922d1d17ef3d86540eeae0b610e9627167fe948417559ff569d&scene=21#wechat_redirect)

长按👇关注\- 数据STUDIO -设为星标，干货速递

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

People who liked this content also liked

使用python绘制散点密度图（用颜色标识密度）

Chemocoder

不看的原因

- 内容质量低
- 不看此公众号

学Python 新手做到这7点，提升编程能力真不难！

Python编程大全

不看的原因

- 内容质量低
- 不看此公众号

Python其实很简单！从零基础到大佬，超详细知识点汇总，附教程

编程学习部

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___934280ce8e2549b6b.bmp"/>

Scan to Follow