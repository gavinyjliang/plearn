# Python 初学者进阶的九大技能

<a id="profileBt"></a><a id="js_name"></a>数据STUDIO *2022-04-18 08:35*

![Image](../../../_resources/640_wx_fmt_gif_wxfrom_5_wx_lazy__e9a19ba965424b03a.gif)

<a id="js_appmsg_search_keywords_head"></a>![](../../../_resources/0_wx_fmt_png_999a9d48aa73498e9742c3c0e7f1063c.png)<a id="appmsg_search_keywords_nickname"></a>数据STUDIO<a id="appmsg_search_keywords_title"></a>推荐搜索<a id="appmsg_search_keywords_tips"></a>关键词列表：<a id="appmsg_search_keywords_keywords_list"></a>Python数据挖掘数据可视化SQL

<img width="677" height="103" src="../../../_resources/640_wx_fmt_gif_wxfrom_5_wx_lazy__eec54a12ca1b47888.gif"/>

Python是一种很棒的语言，语法简单，无需在代码中搜索分号。对于初学者来说，Python是入门最简单的语言之一。

Python有大量的库支持，你还可以安装其他库来增加自己的编程经验。

学了一阵子之后，你可能会觉得：为如此简单的操作写大量的代码有些令人困惑。实际上，事情并没有你想得那么糟。理解其背后的逻辑比写几行代码更为重要。短代码更好，但如果逻辑有问题，那么无论如何你的代码都会有问题。随着经验和创造力的增长，最终你的代码将会变得更短、更好。

#### 初学者与中级程序员

那么，对于Python程序员而言，初学者和进阶者有什么区别呢？

本文将重点介绍以下方面：

- 解决问题和提出问题；
    
- XY问题；
    
- 理解代码为何起作用（或不起作用）；
    
- 使用字符串；
    
- 使用列表；
    
- 使用循环；
    
- 使用函数（并正确谈论函数）；
    
- 面向对象编程；
    
- 尊重PEP。
    

## 1.解决问题和提出问题

程序员缺乏解决问题能力的话，代码出色也是枉然。

如果你解决问题的思维不够发达，可能就无法为你要解决的问题找到最佳的解决方案。编程不仅仅是编写代码，需要解决问题才能有机会出初学者行列。

提出编程相关的问题也很重要。如果不经尝试，就让别人解决你的问题，可能也会出局。这很难，但如果不尝试自己解决问题，你将对解决方案一无所得。

如果想要了解更多关于编程提问的技能，我另有一篇文章，链接如下：How to Ask Questions About Programming<sup>\[1\]</sup>。

## 2\. XY问题

> “我需要从字符串中提取最后3个字符。”
> “不，你不需要。只需文件扩展名。”

XY问题很有趣。你有个X问题，当你调用服务中心时，会寻求Y问题的解决方案，以解决X问题。

上面的案例就是极好的例子。如果想要文件名中的文件扩展名，很容易假设你需要的是最后3个字母。

如何写代码：

```
def extract_ext(filename):
    return filename[-3:]
print (extract_ext('photo_of_sasquatch.png'))
>>> png

```

好极了，现在换成 `photo_of_lochness.jpeg`：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

用户从一开始应该会索要扩展名，最后3个字母是Y问题，而X问题是我们想要扩展名。

```
def extract_ext(filename):
    return filename.split('.')[-1]
print (extract_ext('photo_of_sasquatch.png'))
print (extract_ext('photo_of_lochness.jpeg'))
>>> png
>>> jpeg

```

**成功了！**

你也可以使用标准库 `os.path.splitext()` ，这里查看：os.path.splitext()<sup>\[2\]</sup>。

## 3\. 理解代码为何起作用

#### （或不起作用）

作为新手，你可能要花几天来对付一小段代码。如果这段代码突然起作用了，你可能会感觉放心，然后继续下一段代码。这是最糟糕的事情之一。不理解原因只管运行的做法，可能比不理解代码的为什么不运行更加危险。

不理解为何代码不运行的情况总会发生，当进行故障排除并搞清楚其原因时，思考代码不运行的原因和最终使其运行的因素非常重要。这次学到的知识会带到下一个程序中。

例如，如果多个缩进级别的代码中出现了缩进错误，可以尝试随机调整代码块，然后在最终运行时为自己庆祝。

切记，在大多数IDE中，可以折叠循环和if语句，从而更容易查看正在使用的部分。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

右侧是折叠了if/else语句的ATOM

另一种办法是将你的代码通过 `www.pythontutor.com` 可视化，就可以逐行查看代码运行的方式了。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

使用pythontutors执行代码

## 4\. 使用字符串

这部分内容其实与字符串不完全相关，与挖掘Python优雅的库有更大关系。

我们很早就在Python中学过，字符串也可以看作是一串字符。你也可以使用索引访问字符串中的字符。

```
word = 'supergreat'
print (f'{word[0]}') 
>>> s
print (f'{word[0:5]}')
>>> super

```

敏锐的学习者会查看`str()`所提供的内容，但也可以不查看 `str()`文档继续编程。

查看函数或过程文档可以通过调用 `help(str)` 或者`dir(str)`来实现。执行此操作时，你可能会发现一些并不知道的方法，也许你在查看`str()`时，找到有个名叫 `endswith()` 的方法，或许能用在某处。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

下面是一些以两种不同方式执行相同操作的代码案例，一种用到了我们才谈过的拆分，还有一种用到了我们刚刚学到的 `endswith()` ：

```
filenames = ['lochness.png' , 'e.t.jpeg' , 'conspiracy_theories_CONFIRMED.zip']
# 1: Using ENDSWITH
for files in filenames:
    if files.endswith('zip'):
        print(f'{files} is a zip file')
    else:
        print (f'{files} is NOT a zip file')
# 2: Using SPLIT
for files in filenames:
    if files.split('.')[-1] == 'zip':
        print(f'{files} is a zip file (using split)')
    else:
        print (f'{files} is NOT a zip file (using split)')

```

大多程序员是从来不会把所有文档读遍来学习全部内容的。作为一名程序员，部分工作就是要搜索如何解决问题的信息。

## 5\. 使用列表

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

列表很棒，用途也很广泛

下面的案例中，我们将整数和字符串混合在了一起：

```
my_list = ['a' , 'b' , 'n' , 'x' , 1 , 2 , 3, 'a' , 'n' , 'b']
for item in my_list:
    print (f'current item: {item}, Type: {type(item)}')

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

注意我们是怎么将字符串和整数混合在一起的，如果尝试对其排序，就会报错：

```
print (my_list.sort())

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

如果我们想把整数与字母分开要怎么做？一种方式是通过循环来实现，我们可以遍历列表中的所有项目。初学者很早就会使用循环了，循环对于编程也很重要。

代码可能是下面这样的：

```
my_list = ['a' , 'b' , 'n' , 'x' , 1 , 2 , 3 , 'a' , 33.3 , 'n' , 'b']
number_list = []
string_list = []
for item in my_list:
    print (f'current item: {item}, Type: {type(item)}')
    if not isinstance(item,str):
        number_list.append(item)
    else:
        string_list.append(item)
my_list = string_list

```

即便有些混乱，这也是一种有效的方式，可以运行，不过经过重构可以用单行来表示！

如果想要生活多些乐趣，请学习Python的列表解析式，下面是同样问题通过列表解析式得出的：

```
my_list = [letter for letter in my_list if isinstance(letter,str)]

```

就是这样！

还没结束！使用过滤器也可以获得同样的结果：

```
def get_numbers(input_char):
    if not isinstance(input_char,str):
        return True
    return False
my_list = [1,2,3,'a','b','c']
check_list = filter(get_numbers, my_list)
for items in check_list:
    print(items)

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

现在你可能明白了，实现同样的结果有很多方法，你必须找出适合你或你团队的那个。

### 额外知识点

- 反向列表（或字符串）：
    

```
names = ['First' , 'Middle' , 'Last']
print(names[::-1])
>>> ['Last', 'Middle', 'First']

```

- 在列表中加入元素：
    

```
names = ['First' , 'Middle' , 'Last']
full_name = ' '.join(names)
print(f'Full Name:\n{full_name}')
>>> First Middle Last6. 使用循环：

```

是否在Python中见过这样的代码？

```
greek_gods = ['Zeus' , 'Hera' , 'Poseidon' , 'Apollo' , 'Bob']
for index in range(0,len(greek_gods)):
    print (f'at index {index} , we have : {greek_gods[index]}')

```

你可能发现了，它来自其他语言，这不是Python的风格。在Python中，你可以使用for-each循环：

```
for name in greek_gods:
    print (f'Greek God: {name}')

```

你很快就能发现，这里我们不包含索引。如果想用索引打印要怎么做？在Python中，你可以使用枚举（enumerate参数），这是一种访问所需内容的绝佳方案。

```
for index, name in enumerate(greek_gods):
    print (f'at index {index} , we have : {name}')

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 7\. 使用函数

#### （并正确谈论函数）

我在从事动画工作时，总是说如果同一个操作重复5次，就应该考虑是否需要写个程序。有些时候花上两周开发一款工具可以节省你六个礼拜的工作时间。

编写代码时，如果发现同一动作执行了不止一次，应该考虑这是过程还是函数，还不只是写写代码。函数会返回内容，过程则只是运行代码，第一个案例是个过程，第二个是函数。

这样说可能会令人困惑，下面是其工作原理的示意图：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

注意print和return的差异，看起来也许很相似，但如果你查看输出结果，函数只会返回发送的名称。

下一个要了解的语法是parameters和arguments，在过程或函数中定义时（红色部分）被称为形参（parameters），当发送名称到过程或函数中（绿色部分）时就叫实参（arguments）了。

下面是些案例：

### 案例1

```
def print_list(input_list):
    for each in input_list:
        print(f'{each}')
    print() #just to separate output
greek_gods = ['Zeus' , 'Hera' , 'Poseidon' , 'Apollo' , 'Bob']
grocery_list = ['Apples' , 'Milk' , 'Bread']
print_list(greek_gods)
print_list(grocery_list)
print_list(['a' , 'b' , 'c'])

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

无需把循环写上3次，只需在过程中写上一次，然后在需要时调用即可。在案例2中，你可以发现代码是如何返回反向列表的。

### 案例2

```
def reverse_list(list_input):
    return list_input[::-1]
my_list = ['a', 'b' , 'c']
print (reverse_list(my_list))
>>> ['c', 'b', 'a']

```

## 8\. 面向对象编程

Python是一种面向对象的语言，其强大之处在于对象。将对象视为蓝图，如果使用蓝图，你可以创建该蓝图的实例。也就是说，你可以创建需要的多个蓝图实例，但不会损毁你使用的蓝图。

面向对象编程（OOP）是一个庞大的话题，因此我们不会在本节中涵盖所有你需要了解的内容，但可以通过几个简单的示例帮你入门。

如果你之前读过面向对象编程的相关内容，可能已经厌倦了学生（student）类，但我们又来了。从定义一个名为student的类开始，student会拥有一个名称和一个 `subject_list`：

```
class Student():
    def __init__(self,name):
        self._name = name
        self._subject_list = []
# 如果想要创建一个student，可以像这样将其分配给变量：
student1 = Student('Martin Aaberge')

```

如果需要更多student，可以使用同一个类并添加另外的姓名：

```
student2 = Student('Ninja Henderson')

```

`student1`和`student2`都是student类的实例，它们共享同一个蓝图，但彼此之间并无关系。此时，我们对学生们能做的不多，但我们确实增加了一个主题列表。要填充此列表，我们需要创建方法，你可以调用方法来实现与该类实例的交互。

我们更新：

```
class Student():
    def __init__(self,name):
        self._name = name
        self._subject_list = []
    def add_subject(self, subject_name):
        self._subject_list.append(subject_name)
    def get_student_data(self):
        print (f'Student: {self._name} is assigned to:')
        for subject in self._subject_list:
            print (f'{subject}')
        print()

```

这个类可以用于创建、编辑学生信息，并获取我们存在其中的信息：

```
#create students:
student1 = Student('Martin Aaberge')
student2 = Student('Heidi Hummelvold')
#add subjects to student1
student1.add_subject('psychology_101')
student1.add_subject('it_security_101')
#add subject to student2
student2.add_subject('leadership_101')
#print current data on students
student1.get_student_data()
student2.get_student_data()

```

将类保存在单独的文件中并导入主代码的操作很常见，在我们的案例中，我们会在student.py文件中创建一个`student`类，并将其导入我们的main.py文件（本案例中，它们都位于同一个文件夹中）。

```
from student import Student
student1 = Student('Martin')
student1.add_subject('biomechanics_2020')
student1.get_student_data()

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

student类和main.py在使用它

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 9\. 尊重PEP

我们经常看到人们在写Python代码时并不尊重PEP（Python增强提案：Python Enhancement Proposals），但我自己会尊重。

当你在开发环境中工作时，遵守标准非常重要——如果不是PEP标准，也至少要遵守公司的标准。

PEP是代码的一组准则，下面是PEP-8的链接<sup>\[3\]</sup>，读起来很棒。请确保你通读过一次，了解大概内容。一个典型的案例是`snake_case`，Python是以`snake_case`来写的，这代表着我们用下划线来区分词组，即便大学里也会犯错，因此别难过，只要别这样做就行了。

这样写是对的：

```
chocolate_cake = 'yummy'

```

这样是错的：

```
chocolateCake = 'Yummy'

```

## 结论

入门是了不起的体验，需要艰苦钻研，但你的学习曲线急遽上升，用新的经验填满你。

也许新手状态很难摆脱，了解你要关注什么是很困难的，下一步呢？

也许本文将你向正确的方向推进了一步，也许只是一堆你已经知道的胡言乱语。如果你不确定下一步该做什么，不要害怕提问。确保你用好了那些比你更有经验的人，对各种意见持开放态度，看看哪些对你有用。如果还没准备好使用某些编程方式，请继续让代码能够运行，同时学些新的和更好的方法。

### 参考资料

\[1\]

How to Ask Questions About Programming: *https://medium.com/better-programming/how-to-ask-questions-about-programming-dcd948fcd2bd*

\[2\]

os.path.splitext(): *https://www.geeksforgeeks.org/python-os-path-splitext-method/*

\[3\]

PEP-8的链接: *https://www.python.org/dev/peps/pep-0008/*

> 原文：medium.com/better-programming/9-skills-that-separate-a-beginner-from-an-intermediate-python-programmer-8bbde735c246
> 
> 作者：Martin Andersson Aaberge 来源：CSDN

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

##### 🏴‍☠️宝藏级🏴‍☠️ 原创公众号『**数据STUDIO**』内容超级硬核。公众号以Python为核心语言，垂直于数据科学领域，包括可戳👉[**Python**](https://mp.weixin.qq.com/mp/appmsgalbum?__biz=Mzk0OTI1OTQ2MQ==&action=getalbum&album_id=1974978822768771072&scene=173&from_msgid=2247519294&from_itemidx=1&count=3&nolastread=1#wechat_redirect)**｜**[**MySQL**](https://mp.weixin.qq.com/mp/appmsgalbum?__biz=Mzk0OTI1OTQ2MQ==&action=getalbum&album_id=2023684574089658370&scene=173&from_msgid=2247519619&from_itemidx=2&count=3&nolastread=1#wechat_redirect)**｜**[**数据分析**](https://mp.weixin.qq.com/mp/appmsgalbum?__biz=Mzk0OTI1OTQ2MQ==&action=getalbum&album_id=1974978820940054530&scene=173&from_msgid=2247518366&from_itemidx=1&count=3&nolastread=1#wechat_redirect)**｜**[**数据可视化**](https://mp.weixin.qq.com/mp/appmsgalbum?__biz=Mzk0OTI1OTQ2MQ==&action=getalbum&album_id=1974991176839544834&scene=173&from_msgid=2247519244&from_itemidx=1&count=3&nolastread=1#wechat_redirect)**｜**[**机器学习与数据挖掘**](https://mp.weixin.qq.com/mp/appmsgalbum?__biz=Mzk0OTI1OTQ2MQ==&action=getalbum&album_id=1963494160565354497&scene=173&from_msgid=2247512171&from_itemidx=1&count=3&nolastread=1#wechat_redirect)**｜**[**爬虫**](https://mp.weixin.qq.com/mp/appmsgalbum?__biz=Mzk0OTI1OTQ2MQ==&action=getalbum&album_id=2318258648965644288&scene=173&from_msgid=2247518366&from_itemidx=1&count=3&nolastread=1#wechat_redirect) 等，从入门到进阶！

长按👇关注\- 数据STUDIO -设为星标，干货速递

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

People who liked this content also liked

现代C++测试工具链(是时候抛弃gtest/google bench了)

...

程序喵大人

不看的原因

- 内容质量低
- 不看此公众号

推荐七个Python效率工具

...

Python丹卿

不看的原因

- 内容质量低
- 不看此公众号

Python高级用法，代码精进! (文末赠书）

...

尤而小屋

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___2d0fb6a672cf43d1b.bmp"/>

Scan to Follow