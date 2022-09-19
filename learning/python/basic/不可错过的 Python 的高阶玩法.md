# 不可错过的 Python 的高阶玩法

<a id="profileBt"></a><a id="js_name"></a>Python编程时光 *2022-05-17 09:02* *Posted on <a id="js_ip_wording"></a>福建*

<img width="661" height="37" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__e61c466b745d4d08b.png"/>

**Python**是世界上最流行的编程语言（TIOBE Index for April 2022），它易于上手且多才多艺，除了用于神经网络的构建外, 还能用来创建Web应用、桌面应用、游戏和运维脚本等多种多样的程序。

Python语言语法简洁，易于上手, 但当你深入研究时, 会发现Python有很多高级用法，这些高级用法可以大幅度提高代码的可读性和运行效率。

此外, Python包含了海量的高质量第三方库, 许多重要的库已经成为Python开发不可或缺的内容。

**《高阶Python：代码精进之路》**一书可以帮你掌握Python语言的高级特性，以及Python科学计算基石——numpy的使用方法（numpy的API设计非常优秀，深度学习框架TensorFlow、PyTorch都使用了numpy相似的API，并且可以和numpy混用）。

<img width="261" height="261" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__16f5ef9bdab64d6a8.png"/>

**下面介绍几个Python的高级用法。**

01

**索引和切片**

Python列表的索引和切片是非常强大的功能, 它们可以让你在Python中获取列表中的任意元素。除了支持常见的正索引外, Python还支持负索引和切片。

**正索引**

```
 `a_list = [100, 200, 300, 400, 500, 600]`` print(a_list[0])      # 输出 100. `` print(a_list[1])      # 输出 200. `` print(a_list[2])      # 输出 300.`
```

**负索引**

```
 `a_list = [100, 200, 300, 400, 500, 600]`` print(a_list[-1])     # 输出 600. `` print(a_list[-3])     # 输出 400.`
```

**切片**

**![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)**

以下是列表切片的一些示例：

```
 `a_list = [1, 2, 5, 10, 20, 30]`  `b_list = a_list[1:3]      # 生成 [2, 5]`` c_list = a_list[4:]       # 生成 [20, 30] `` d_list = a_list[-4:-1]        # 生成 [5, 10, 20] `` e_list = a_list[-1:]          # 生成 [30]`
```

02

**字符串对齐**

字符串格式化在命令行工具开发中非常重要, str类包含基础的，用于文本对齐的方法：左对齐，右对齐或居中对齐。

```
`str.ljust(width [, fillchar])    # 左对齐``str.rjust(width [, fillchar])    # 右对齐` `str.center(width [, fillchar])   # 中间对齐` `digit_str.zfill(width)           # 用“0”填充`
```

下面是一些例子：

```
`new_str = 'Help!'.center(10, '#')` `print(new_str)`
```

该例的输出为：

```
##Help!###
```

* * *

```
`new_str = '750'.rjust(6, '0')` `print(new_str)`
```

此例的输出为：

```
000750
```

上例只是一个简单的字符串格式化样例，《高阶Python：代码精进之路》一书中还介绍了许多更复杂的格式化方法。

03

**列表推导式&字典推导式**

Python 2.0版本引入的最重要的功能之一就是列表推导式。它提供了一种从列表中生成一系列值的紧凑语法。它也可以应用于字典，集合（set）和其他类型的集合。

假设你要创建一个包含a_list中每个元素的平方的新列表，一种可能的实现方式如下：

```
 `b_list = [ ]`` for i in a_list: `` b_list.append(i * i)`
```

如果a\_list包含元素\[1，2，3\]，则这些语句的结果是创建一个包含\[1，4，9\]的新列表，并将此列表分配给变量b\_list。在这种情况下，相应的列表推导式如下所示：

```
 b_list = [i * i   for i in a_list]
```

假设想讲一个元组列表转换为字典,元组列表如下:

```
 vals_list = [ ('pi', 3.14), ('phi', 1.618) ]
```

字典可以用下面的代码生成：

```
 my_dict = { i[0]: i[1] for i in vals_list }
```

注意在键值表达式（i\[0\]：i\[1\]）中冒号(:)的使用。

04

**可变长参数列表**

Python最通用的功能之一就是能够访问可变长度参数的列表。借助此功能，你的函数可以处理任意数量的参数，就像内置的print函数一样。可变长参数的特性也可以扩展到命名参数。

```
 `def func_name([ordinary_args,] *args):`` statements`
```

这里的中括号表示*args前面可以有任意数量的普通参数，在此表示为ordinary_args。此类参数是可选的。下面是示例代码：

```
 `def my_var_func(*args):`` print('The number of args is', len(args)) `` for item in args: `` print(items)`
```

此函数my\_var\_func可接受任意长度的参数列表。

```
 `>>> my_var_func(10, 20, 30, 40)`` The number of args is 4 `` 10 `` 20 `` 30 `` 40`
```

可变长参数列表还支持关键字参数,如下所示：

```
 `def pr_named_vals(**kwargs):`` for k in kwargs: `` print(k, ':', kwargs[k])`
```

上面的函数遍历了kwargs表示的字典参数，打印出传入参数的键（对应于参数名称）和对应的值。

```
 `For example:`` >>> pr_named_vals(a=10, b=20, c=30) `` a : 10 `` b : 20 `` c : 30`
```

args 和 kwargs可以组合使用,下面是一个例子。

```
 `def pr_vals_2(*args, **kwargs):`` for i in args: `` print(i) `` for k in kwargs: ``print(k, ':', kwargs[k])`  `pr_vals_2(1, 2, 3, -4, a=100, b=200)`
```

运行时，此程序将打印以下内容：

```
 `1`` 2 `` 3 ``-4` `   a : 100 ` `b : 200`
```

05

**使用numpy进行线性代数运算**

线性代数运算在深度学习中非常重要，numpy库为Python提供了高效的线性代数运算模块。

numpy的线性代数模块非常完备，以计算点积为例进行介绍。

使用numpy时，可以使用点积函数dot计算点积。

```
numpy.dot(A, B, out=None)
```

A和B是要进行点积运算的两个数组；out参数（如果已指定）是用于存储结果的正确形状的数组，“正确形状”取决于A和B的形状。

两个一维数组的点积很简单。数组的长度必须相同。点积计算是将A中的每个元素与其B中的对应元素相乘，然后对这些乘积求和，得出一个标量值。

```
D. P. = A[0]*B[0] + A[1]*B[1] + ... + A[N-1] * B[N-1]
```

例子：

```
`import numpy as np` `A = np.ones(5)` `B = np.arange(5)` `print(A, B)` `[1. 1. 1. 1. 1.] [0 1 2 3 4]` `np.dot(A, A)` `5.0` `np.dot(A, B)` `10.0` `np.dot(B, B)` `30`
```

二维矩阵之间的点积比较复杂。与数组之间的普通乘法一样，两个数组的形状必须兼容，但这只需要在其中一个维度上相等即可。
下面是描述点积应用到二维数组通用模式：

```
(A, B) * (B, C) => (A, C)
```

思考下面的2×3数组，再结合一个3×2数组，其点积是2×2数组。

```
`A = np.arange(6).reshape(2,3)` `B = np.arange(6).reshape(3,2)` `C = np.dot(A, B)` `print(A, B, sep='\n\n')` `print('\nDot product:\n', C)``[[0 1 2]` `[3 4 5]]``[[0 1]` `[2 3]` `[4 5]]``Dot product:` `[[10 13]` `[28 40]]`
```

以上内容节选自**《高阶Python：代码精进之路》**一书，欢迎阅读本书学习更多Python的高级技巧。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg== "高阶Python：代码精进之路.jpg")

****▊******《****高阶Python：代码精进之路》**

\[美\] Brian，Overland（布赖恩・欧弗兰），John，Bennett（约翰・班纳特） 著

李辉 译

- 本书开发了一个“RPN脚本解释器”项目，该项目贯穿本书的各个章节，通过对该项目的学习，你也可以开发出自己的“语言”
    

本书详细地介绍了Python语言的一些高级功能以及常见数据类型的高级用法，非常适合有一定基础的读者深入学习Python编程。本书的主要内容包括常见内置类型（数值、字符串和集合等）的高级用法和潜在的陷阱，用于文本处理的格式化方法和正则表达式，用于数值计算和大规模数据处理的math包和numpy包等。此外，文件存储、随机数生成和图表绘制也是本书的重要内容。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg== "高阶python二维码 &#40;3&#41;.png")

（扫码查看本书详情！）

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

```


如果喜欢本文

欢迎 在看丨留言丨分享至朋友圈 三连

## 留言赠书

赠书规则：给本文留言，然后 留言集赞数 排名前3的朋友，可获得《高阶 Python 代码精进之路》赠书。

开奖时间：5月20日 20:00

注意：中奖者24小时内，必须添加我微信（stromwbm）领取，逾期不候！为了大家都有机会中奖，本月已经中过书的朋友，再次中奖将不在赠书。

本批书籍由 博文视点 电子工业出版社 赞助，再次致谢。也欢迎大家自行前往购买支持。




```

People who liked this content also liked

支持300+常用功能的开源GO语言工具函数库

...

GoCN

不看的原因

- 内容质量低
- 不看此公众号

像Python一样玩C/C++

...

光城

不看的原因

- 内容质量低
- 不看此公众号

只需一个文件，Python 实现迷你 Web 框架！

...

Python猫

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___4bf219984a5449f4b.bmp"/>

Scan to Follow