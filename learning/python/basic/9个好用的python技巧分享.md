# 9个好用的python技巧分享

<a id="copyright_logo"></a>Original AI_Way1024 <a id="profileBt"></a><a id="js_name"></a>AI算法之道 *2021-12-11 12:09*

收录于话题

<a id="js_article_tag_name__1958994820572545028"></a>#python <a id="js_article_tag_num__1958994820572545028"></a>96 <a id="js_article_tag_tips__1958994820572545028"></a>个

<a id="js_article_tag_name__1990948735710822402"></a>#编程技巧 <a id="js_article_tag_num__1990948735710822402"></a>39 <a id="js_article_tag_tips__1990948735710822402"></a>个

<img width="444" height="307" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_61745a1c4f6f4fceb.jpg"/>

01

**引言**

本文是Python生态系统中一些有用技巧的分享。大多数技巧只是使用标准库中的包，但其他一些技巧会涉及一些第三方包。

在开始阅读本文内容之前，我们首先来回顾一下Python中的Iterables的概念。

根据Python标准文档，Iterable的概念如下：

> 一种能够一次返回一个成员的对象。

iterables的示例包括：

- 所有序列类型（如list、str和tuple）
    
- 一些非序列类型，如dict、文件对象以及类的实现中定义了\_\_iter\_\_()方法
    

Iterables是一个需要我们牢记的概念，因为接下来我们展示的许多技巧都使用itertools包。

itertools模块提供了一些函数，用于接收Iterable对象，而不仅仅是打印逐个对象。

02

**Trick 1**

在工作学习中，我们经常会需要使用一个简单的函数来实现从一个list来生成新的list,set或dict.此时我们就会用到iterables概念。举例来说：

生成List:

```
`names = ['John', 'Bard', 'Jessica' 'Andres']``lower_names = [name.lower() for name in names]`
```

生成Set:

```
`names = ['John', 'Bard', 'Jessica' 'Andres']``lower_names = {name.lower() for name in names}`
```

生成Dict:

```
`names = ['John', 'Bard', 'Jessica' 'Andres']``lower_names = {name:name.lower() for name in names}`
```

个人建议：

> 仅当for语句、函数调用和方法调用的数量较少时使用。

03

**Trick 2**

有时，我们需要获得两个列表对象之间的所有可能组合。我们首先想到的实现可能如下：

```
`l1 = [1, 2, 3]``l2 = [4, 5, 6]``combinations = []``for e1 in l1:` `for e2 in l2:` `combinations.append((e1, e2))`
```

或者简化一下，如下：

```
combinations = [(e1, e2) for e1 in l1 for e2 in l1]
```

上述实现已经很简洁了，但标准库itertools提供product函数，从而提供了相同的结果。如下所示：

```
`from itertools import product``l1 = [1, 2, 3]``l2 = [4, 5, 6]``combinatios = product(l1, l2)`
```

04

**Trick 3**

假设有一个元素列表，我们需要在每对相邻元素之间比较或应用一些操作，这有时称为2个元素的滑动窗口。我们可以采用以下方式：

```
`from itertools import tee``from typing import Iterable``def window2(iterable: Iterable):` `it, offset = tee(iter(iterable))` `next(offset)` `return zip(it, offset)``l = [1, 2, 3, 4, 5, 6]``dd = window2(l)``for a in dd:` `print(a)`
```

运行结果如下：

```
`(1, 2)``(2, 3)``(3, 4)``(4, 5)``(5, 6)`
```

05

**Trick 4**

有时我们会需要一个类来存储信息，但是如果我们觉得创建一个类并定义其\_\_init\_\_()函数太麻烦时，我们不妨选择使用dataclass。如下所示：

```
`from dataclasses import dataclass``@dataclass``class Person:` `name: str` `age: int` `address: str`
```

上述代码创建了一个具有默认构造函数的类，该类以与声明相同的顺序接收相应字段的赋值。

```
person = Person(name='John', age=12, address='nanjing street')
```

dataclass的另一个优点是，默认情况下，会生成特殊方法，如\_\_str\_\_、\_\_repr\_\_、\_\_eq\_\_等。

> 值得一提的是我们在类中声明的成员变量的类型注释（str、int等）并不强制在构造函数中传递的值属于这种类型。也就是说dataclasses构造对象时并不执行数据类型的检查。

关于dataclass的更多用法，可以参考官网。

06

**Trick 5**

我们有时希望将一个对象上的操作视为tuple上的操作，一种选择是使用collections.namedtuple,但也存在更类似于dataclass的实现。如下：

```
`from typing import NamedTuple``class Coordinate(NamedTuple):` `x: int` `y: int`
```

上述定义了一个标准的类可以被当做tuple来使用，如下：

```
`coordinate = Coordinate(10, 15)``coordinate.x == coordinate[0] // True` `coordinate.y == coordinate[1] // True`
```

07

**Trick 6**

假如我们有一个dataclass，需要验证输入数据是否符合类型注释。在这种情况下，安装第三方软件包pydantic并将

from dataclasses import dataclass

替换为

from pydantic.dataclasses import dataclass 即可，如下所示：

```
`from pydantic.dataclasses import dataclass``@dataclass``class Person:` `name: str` `age: int` `address: str`
```

这将生成一个类，该类具有根据成员变量声明的类型进行输入数据的解析和类型验证。

> Pydantic在运行时强制执行类型提示，并在数据无效时提供友好的错误提醒。

08

**Trick 7**

在某些情况下，我们需要生成一些容器中元素频率的基本统计信息。在这种情况下，您可以使用标准结构Counter来接收iterable并根据元素的频率生成相应的统计信息。

```
`from collections import Counter``l = [1, 1, 2, 3, 4, 4]``frequencys = Counter(l)``print(frequencys[1])    // Ouput: 2``print(frequencys[2])    // Ouput: 1``print(frequencys[2323]) // Ouput: 0`
```

> Counter也提供了一些其他方法，比如如most_common，用于检索最常见的元素。

09

**Trick 8**

如果我们相对两个list中的元素对做相应的函数处理，我们最容易想到的方法如下：

```
`l1 = [1, 2, 3]``l2 = [4, 5, 6]``for (e1, e2) in zip(l1, l2):` `f(e1, e2)`
```

但是使用函数map可以让代码更加简洁一些。

```
`l1 = [1, 2, 3]``l2 = [4, 5, 6]``map(f, l1, l2)`
```

10

**Trick 9**

有时候我们需要从一个list中随机选择一个元素，此时我们使用random.choice.如下所示：

```
`from random import choice``l = [1, 2, 3]``random = choice(l)`
```

如果我们需要随机选择多个元素呢？当然是使用random.choices.

```
`from random import choices``l = [1, 2, 3, 4, 5]``random_elements = choices(l, k=3)`
```

上述代码中的参数k为我们随机选择元素的个数。

11

**总结
**

本文重点介绍了在python中9个和迭代相关的使用技巧，可以方便提升大家的工作效率。

您学废了吗?

People who liked this content also liked

python计算机考试中易错考点

水文挖掘

不看的原因

- 内容质量低
- 不看此公众号

python数据可视化--matplotlib绘制气泡图

AI小白笔记

不看的原因

- 内容质量低
- 不看此公众号

Python基础(下)

啊颜的学习场所

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___b7660c77a7be44eb9.bmp"/>

Scan to Follow