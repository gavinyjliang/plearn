# 24 个好用到爆的 Python 实用技巧！

<a id="copyright_logo"></a>Original <a id="js_author_name"></a>云朵君 <a id="profileBt"></a><a id="js_name"></a>数据STUDIO *2021-11-11 11:30*

收录于话题

<a id="js_article_tag_name__1974978822768771072"></a>#python <a id="js_article_tag_num__1974978822768771072"></a>45 <a id="js_article_tag_tips__1974978822768771072"></a>个

<a id="js_article_tag_name__1974989529669255169"></a>#精选总结 <a id="js_article_tag_num__1974989529669255169"></a>31 <a id="js_article_tag_tips__1974989529669255169"></a>个

大家好，我是云朵君！

作为一名数据工作者，我们每天都在使用 Python处理大多数工作。在此过程中，我们会不断学到了一些有用的技巧和窍门。

![](../../../_resources/0_wx_fmt_png_1b688944195049c99fec4d16a5985153.png)

**数据STUDIO**

点击领取《Python学习手册》，后台回复「福利」获取。『数据STUDIO』专注于数据科学原创文章分享，内容以 Python 为核心语言，涵盖机器学习、数据分析、可视化、MySQL等领域干货知识总结及实战项目。

<a id="js_profile_article"></a>173篇原创内容

Official Account

👆点击关注｜设为星标｜干货速递👆

* * *

在这里，我尝试以 A - Z 开头的格式分享这些技巧中的一些，并且在本文中简单介绍这些方法，如果你对其中一个或多个感兴趣，你可以通过文末参考资料查看官方文档。希望对你能有所帮助。

<img width="36" height="32" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__ad9111f847544eabb.png"/>

A - Z 

<img width="73" height="59" src="../../../_resources/640_wx_fmt_gif_wxfrom_5_wx_lazy__292b296be0a54fd79.gif"/>

### all or any

Python 语言如此流行的众多原因之一，是因为它具有很好的可读性和表现力。

人们经常开玩笑说 Python 是`可执行的伪代码`。当你可以像这样写代码时，就很难反驳。

```
x = [True, True, False]
if any(x):
    print("至少有一个True")
if all(x):
    print("全是True")
if any(x) and not all(x):
    print("至少一个True和一个False")

```

### bashplotlib

你有没有想过在控制台中绘制图形吗？

Bashplotlib 是一个 Python 库，他能够帮助我们在命令行(粗旷的环境)中绘制数据。

```
# 模块安装
pip install bashplotlib
# 绘制实例
import numpy as np
from bashplotlib.histpgram import plot_hist
arr = np.ramdom.normal(size=1000, loc=0, scale=1)
plot_hist(arr, bincount=50)

```

### collections

Python 有一些很棒的默认数据类型，但有时它们的行为并不完全符合你的期望。

幸运的是，Python 标准库提供了 collections 模块<sup>\[1\]</sup>。这个方便的附加组件为你提供了更多的数据类型。

```
from collections import OrderedDict, Counter
# 记住键的添加顺序！
x = OrderedDict(a=1, b=2, c=3)
# 统计每个字符出现的频率
y = Counter("Hello World!")

```

### dir

有没有想过如何查看 Python 对象内部并查看它具有哪些属性？在命令行中输入：

```
dir() 
dir("Hello World") 
dir(dir)

```

当以交互方式运行 Python 以及动态探索你正在使用的对象和模块时，这可能是一个非常有用的功能。在这里阅读更多functions<sup>\[2\]</sup>相关内容。

### emoji

emoji<sup>\[3\]</sup> 是日本在无线通信中所使用的视觉情感符号，绘指图画，文字指的则是字符，可用来代表多种表情，如笑脸表示笑、蛋糕表示食物等。在中国大陆，emoji通常叫做“小黄脸”，或者直称emoji。

```
# 安装模块
pip install emoji
# 做个尝试
from emoji import emojize
print(emojize(":thumbs_up:"))

```

👍

### from \_\_future\_\_ import

Python 流行的结果之一，总是有新版本正在开发中。新版本意味着新功能 —— 除非你的版本已过时。

不过不要担心。使用该\_\_future\_\_模块<sup>\[4\]</sup>可以帮助你用Python的未来版本导入功能。从字面上看，这就像时间旅行、魔法或其他东西。

```
from __future__ import print_function
print("Hello World!")

```

### geogy

地理，对大多数程序员来说是一个具有挑战性的领域。在获取地理信息或者绘制地图时，也会遇到不少问题。这个geopy 模块<sup>\[5\]</sup>让地理相关内容变得非常容易。

```
pip install geopy

```

它通过抽象一系列不同地理编码服务的 API 来工作。通过它，你能够获得一个地方的完整街道地址、纬度、经度甚至海拔高度。

还有一个有用的距离类。它以你偏好的测量单位计算两个位置之间的距离。

```
from geopy import GoogleV3
place = "221b Baker Street, London"
location = GoogleV3().geocode(place)
print(location.address)
print(location.location)

```

### howdoi

当你使用terminal终端编程时，通过在遇到问题后会在StackOverflow上搜索答案，完后会回到终端继续编程，此时有时会不记得你之前查到的解决方案，此时需要重新查看StackOverflow，但又不想离开终端，那么此时你需要用到这个有用的命令行工具howdoi<sup>\[6\]</sup>。

```
pip install howdoi

```

无论你有什么问题，都可以问它，它会尽力回复。

```
howdoi vertical align css
howdoi for loop in java
howdoi undo commits in git

```

但请注意——它会从 StackOverflow 的最佳答案中抓取代码。它可能并不总是提供最有用的信息......

```
howdoi exit vim

```

### inspect

Python 的inspect模块<sup>\[7\]</sup>非常适合了解幕后发生的事情。你甚至可以调用它自己的方法！

下面的代码示例`inspect.getsource()` 用于打印自己的源代码。 `inspect.getmodule()` 还用于打印定义它的模块。

最后一行代码打印出它自己的行号。

```
import inspect
print(inspect.getsource(inspect.getsource))
print(inspect.getmodule(inspect.getmodule))
print(inspect.currentframe().f_lineno)

```

当然，除了这些微不足道的用途，inspect 模块可以证明对理解你的代码在做什么很有用。你还可以使用它来编写自文档化代码。

### Jedi

Jedi 库是一个自动完成和代码分析库。它使编写代码更快、更高效。

除非你正在开发自己的 IDE，否则你可能对使用Jedi <sup>\[8\]</sup>作为编辑器插件比较感兴趣。幸运的是，这已经有可用的负载！

### **kwargs

在学习任何语言时，都会有许多里程碑。使用 Python 并理解神秘的`**kwargs`语法可能算作一个重要的里程碑。

字典对象前面的双星号**kwargs<sup>\[9\]</sup>允许你将该字典的内容作为命名参数传递给函数。

字典的键是参数名称，值是传递给函数的值。你甚至不需要调用它`kwargs`！

```
dictionary = {"a": 1, "b": 2}
def someFunction(a, b):
    print(a + b)
    return
# 这些做同样的事情:
someFunction(**dictionary)
someFunction(a=1, b=2)

```

当你想编写可以处理未预先定义的命名参数的函数时，这很有用。

### 列表(list)推导式

关于 Python 编程，我最喜欢的事情之一是它的列表推导式<sup>\[10\]</sup>。

这些表达式可以很容易地编写非常顺畅的代码，几乎与自然语言一样。

```
numbers = [1,2,3,4,5,6,7]
evens = [x for x in numbers if x % 2 is 0]
odds = [y for y in numbers if y not in evens]
cities = ['London', 'Dublin', 'Oslo']
def visit(city):
    print("Welcome to "+city)
    
for city in cities:
    visit(city)

```

### map

Python 通过许多内置功能支持函数式编程。最有用的`map()`功能之一是函数——尤其是与lambda 函数<sup>\[11\]</sup>结合使用时。

```
x = [1, 2, 3] 
y = map(lambda x : x + 1, x)
# 打印出 [2,3,4]
print(list(y))

```

在上面的示例中，`map()`将一个简单的 `lambda` 函数应用于`x`. 它返回一个映射对象，该对象可以转换为一些可迭代对象，例如列表或元组。

### newspaper3k

如果你还没有看过它，那么准备好被Python newspaper module <sup>\[12\]</sup>模块震撼到。它使你可以从一系列领先的国际出版物中检索新闻文章和相关的元数据。你可以检索图像、文本和作者姓名。它甚至有一些内置的 NLP 功能<sup>\[13\]</sup>。

因此，如果你正在考虑在下一个项目中使用 `BeautifulSoup` 或其他一些 DIY 网页抓取库，使用本模块可以为你自己节省不少时间和精力。

```
pip install newspaper3k

```

### Operator overloading

Python 提供对运算符重载的<sup>\[14\]</sup>支持，这是让你听起来像一个合法的计算机科学家的术语之一。

这实际上是一个简单的概念。有没有想过为什么 Python 允许你使用`+`运算符来添加数字以及连接字符串？这就是操作符重载的作用。

你可以定义以自己的特定方式使用 Python 的标准运算符符号的对象。并且你可以在与你正在使用的对象相关的上下文中使用它们。

```
class Thing:
    def __init__(self, value):
        self.__value = value
    def __gt__(self, other):
        return self.__value > other.__value
    def __lt__(self, other):
        return self.__value < other.__value
something = Thing(100)
nothing = Thing(0)
# True
something > nothing
# False
something < nothing
# Error
something + nothing

```

### pprint

Python 的默认`print`函数完成了它的工作。但是如果尝试使用`print`函数打印出任何大的嵌套对象，其结果相当难看。这个标准库的漂亮打印模块pprint<sup>\[15\]</sup>可以以易于阅读的格式打印出复杂的结构化对象。

这算是任何使用非平凡数据结构的 Python 开发人员的必备品。

```
import requests
import pprint
url = 'https://randomuser.me/api/?results=1'
users = requests.get(url).json()
pprint.pprint(users)

```

### Queue

Python 标准库的 Queue 模块实现支持多线程。这个模块让你实现队列数据结构。这些是允许你根据特定规则添加和检索条目的数据结构。

“先进先出”（FIFO）队列让你可以按添加顺序检索对象。“后进先出”(LIFO) 队列让你可以首先访问最近添加的对象。

最后，优先队列让你可以根据对象的排序顺序检索对象。

这是一个如何在 Python 中使用队列Queue<sup>\[16\]</sup>进行多线程编程的示例。

### \_\_repr\_\_

在 Python 中定义类或对象时，提供一种将该对象表示为字符串的“官方”方式很有用。例如：

```
>>> file = open('file.txt', 'r') 
>>> print(file) 
<open file 'file.txt', mode 'r' at 0x10d30aaf0>

```

这使得调试代码更加容易。将其添加到你的类定义中，如下所示：

```
class someClass: 
    def __repr__(self): 
        return "<some description here>"
someInstance = someClass()
# 打印 <some description here>
print(someInstance)

```

### sh

Python 是一种很棒的脚本语言。有时使用标准的 os 和 subprocess 库可能有点头疼。

该SH库<sup>\[17\]</sup>让你可以像调用普通函数一样调用任何程序——对于自动化工作流和任务非常有用。

```
import sh
sh.pwd()
sh.mkdir('new_folder')
sh.touch('new_file.txt')
sh.whoami()
sh.echo('This is great!')

```

### Type hints

Python 是一种动态类型语言。定义变量、函数、类等时不需要指定数据类型。这允许快速的开发时间。但是，没有什么比由简单的输入问题引起的运行时错误更烦人的了。

从 Python 3.5<sup>\[18\]</sup> 开始，你可以选择在定义函数时提供类型提示。

```
def addTwo(x : Int) -> Int:
    return x + 2

```

你还可以定义类型别名。

```
from typing import List
Vector = List[float]
Matrix = List[Vector]
def addMatrix(a : Matrix, b : Matrix) -> Matrix:
  result = []
  for i,row in enumerate(a):
    result_row =[]
    for j, col in enumerate(row):
      result_row += [a[i][j] + b[i][j]]
    result += [result_row]
  return result
x = [[1.0, 0.0], [0.0, 1.0]]
y = [[2.0, 1.0], [0.0, -2.0]]
z = addMatrix(x, y)

```

尽管不是强制性的，但类型注释可以使你的代码更易于理解。

它们还允许你使用类型检查工具，在运行前捕获那些杂散的 TypeError。如果你正在处理大型、复杂的项目，这是很有用的！

### uuid

通过Python 标准库的 uuid 模块<sup>\[19\]</sup>生成通用唯一 ID（或“UUID”）的一种快速简便的方法。

```
import uuid
user_id = uuid.uuid4()
print(user_id)

```

这将创建一个随机的 128 位数字，该数字几乎肯定是唯一的。事实上，可以生成超过 2¹²² 种可能的 UUID。这超过了五个十进制 （或 5,000,000,000,000,000,000,000,000,000,000,000,000）。

在给定的集合中发现重复的概率极低。即使有一万亿个 UUID，重复存在的可能性也远低于十亿分之一。

### Virtual environments

你可能同时在多个 Python 项目上工作。不幸的是，有时两个项目将依赖于相同依赖项的不同版本。你在你的系统上安装了什么?

幸运的是，Python支持对 虚拟环境<sup>\[20\]</sup> 的让你可以两全其美。从命令行：

```
python -m venv my-project 
source my-project/bin/activate 
pip install all-the-modules

```

现在，你可以在同一台机器上运行 Python 的独立版本和安装。

### wikipedia

维基百科有一个很棒的 API，它允许用户以编程方式访问无与伦比的完全免费的知识和信息。在wikipedia模块<sup>\[21\]</sup>使访问该API非常方便。

```
import wikipedia
result = wikipedia.page('freeCodeCamp')
print(result.summary)
for link in result.links:
    print(link)

```

和真实站点一样，该模块提供了多语言支持、页面消歧、随机页面检索，甚至还有一个`donate()`方法。

### xkcd

幽默是 Python 语言的一个关键特征，它是以英国喜剧小品剧Python飞行马戏团<sup>\[22\]</sup>命名的。Python 的许多官方文档都引用了该节目最著名的草图。不过，Python 的幽默并不仅限于文档。试试运行下面的行：

```
import antigravity

```

### YAML

YAML<sup>\[23\]</sup>指的是 “ 非标记语言” 。它是一种数据格式化语言，是 JSON 的超集。

与 JSON 不同，它可以存储更复杂的对象并引用它自己的元素。你还可以编写注释，使其特别适合编写配置文件。该PyYAML模块<sup>\[24\]</sup>可让你使用YAML使用Python。

安装并然后导入到你的项目中：

```
pip install pyyaml
import yaml

```

PyYAML 允许你存储任何数据类型的 Python 对象，以及任何用户定义类的实例。

### zip

压轴出场的也是很棒的一个模块。你曾经遇到过需要从两个列表中形成字典吗？

```
keys = ['a', 'b', 'c']
vals = [1, 2, 3]
zipped = dict(zip(keys, vals))

```

该`zip()`内置函数需要一系列可迭代的对象，并返回一个元组列表中。每个元组按位置索引对输入对象的元素进行分组。

你还可以通过调用对象来“解压缩”对象`*zip()`。

## 写在最后

Python 是一种非常多样化且发展良好的语言，因此肯定会有许多我没有考虑的功能。如果你想了解更多的python模块，可以参考awesome-python<sup>\[25\]</sup>。

### 参考资料

\[1\] 

collections 模块: *https://docs.python.org/3/library/collections.html*

\[2\] 

functions: *https://docs.python.org/3/library/functions.html#dir*

\[3\] 

emoji: *https://pypi.org/project/emoji/*

\[4\] 

\_\_future\_\_模块: *https://docs.python.org/2/library/**future**.html*

\[5\] 

geopy 模块: *https://geopy.readthedocs.io/en/latest/*

\[6\] 

howdoi: *https://github.com/gleitz/howdoi*

\[7\] 

inspect模块: *https://docs.python.org/3/library/inspect.html*

\[8\] 

Jedi : *https://jedi.readthedocs.io/en/latest/docs/usage.html*

\[9\] 

**kwargs: *https://docs.python.org/3/tutorial/controlflow.html#keyword-arguments*

\[10\] 

列表推导式: *https://docs.python.org/3/tutorial/datastructures.html#list-comprehensions*

\[11\] 

lambda 函数: *https://docs.python.org/3/tutorial/controlflow.html#lambda-expressions*

\[12\] 

Python newspaper module : *https://pypi.org/project/newspaper3k/*

\[13\] 

内置的 NLP 功能: *https://newspaper.readthedocs.io/en/latest/user_guide/quickstart.html#performing-nlp-on-an-article*

\[14\] 

运算符重载的: *https://docs.python.org/3/reference/datamodel.html#special-method-names*

\[15\] 

pprint: *https://docs.python.org/3/library/pprint.html*

\[16\] 

Queue: *https://www.tutorialspoint.com/python3/python_multithreading.htm*

\[17\] 

SH库: *http://amoffat.github.io/sh/*

\[18\] 

Python 3.5: *https://docs.python.org/3/library/typing.html*

\[19\] 

uuid 模块: *https://docs.python.org/3/library/uuid.html*

\[20\] 

虚拟环境: *https://docs.python.org/3/tutorial/venv.html*

\[21\] 

wikipedia模块: *https://wikipedia.readthedocs.io/en/latest/quickstart.html*

\[22\] 

Python飞行马戏团: *https://en.wikipedia.org/wiki/Monty\_Python's\_Flying_Circus*

\[23\] 

YAML: *http://yaml.org/*

\[24\] 

PyYAML模块: *https://pyyaml.org/wiki/PyYAMLDocumentation*

\[25\] 

awesome-python: *https://awesome-python.com/*

<img width="16" height="23" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__0705379bf04e4b408.png"/>

机器学习研习院

<img width="19" height="20" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__7038eda46b9d41398.png"/>

最后给大家分享**吴恩达的机器学习视频教程、吴恩达的机器学习《Machine Learning Yearning》完整中文版，**有需要的读者可以关注👇下面的公众号『机器学习研习院』，后台消息栏回复：【吴恩达机器学习】，免费下载学习。

![](../../../_resources/0_wx_fmt_png_3db4392e68dd49c2a8db6a3fb5000cb7.png)

**机器学习研习院**

专注分享机器学习领域原创文章，一起研习，共同进步！

<a id="js_profile_article"></a>23篇原创内容

Official Account

长按👇关注\- 数据STUDIO - 设为星标，干货速递

<img width="677" height="227" src="../../../_resources/640_wx_fmt_gif_wxfrom_5_wx_lazy__6efb816ee78a49f39.gif"/>

<img width="19" height="19" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__19f83fd9e68041d5b.png"/>

分享

<img width="19" height="20" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__b2608055645c45639.png"/>

收藏

<img width="19" height="19" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__668539f6fa374b018.png"/>

点赞

<img width="19" height="20" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__9172231500d748a28.png"/>

在看

收录于话题 #<a id="js_album_keep_read_title"></a>精选总结

 <a id="js_album_keep_read_size"></a>31个

<a id="js_album_prev"></a>上一篇 <a id="js_album_keep_read_pre_title"></a>数据科学家提高效率的 40 个 Python 技巧 <a id="js_album_next"></a>下一篇 <a id="js_album_keep_read_next_title"></a>深入浅出经典贝叶斯统计

People who liked this content also liked

Python 程序设计方法

笙临天下 Sec

不看的原因

- 内容质量低
- 不看此公众号

Python从入门到放弃day1

我要当咸鱼

不看的原因

- 内容质量低
- 不看此公众号

python小记（4）

找Bug

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___f267f6bdc12541468.bmp"/>

Scan to Follow