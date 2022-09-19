# Python 加速运行技巧

<a id="profileBt"></a><a id="js_name"></a>数据STUDIO *2022-01-17 11:30*

<a id="js_article-tag-card__left"></a>收录于话题 #python <a id="js_article-tag-card__right"></a>45个

大家好，我是云朵君！
**关注和星标『数据STUDIO』**，和云朵君一起学习数据分析与挖掘！

![](../../../_resources/0_wx_fmt_png_32e58b63e81c43b194e26c1e63e4ad3c.png)

**数据STUDIO**

点击领取《Python学习手册》，后台回复「福利」获取。『数据STUDIO』专注于数据科学原创文章分享，内容以 Python 为核心语言，涵盖机器学习、数据分析、可视化、MySQL等领域干货知识总结及实战项目。

<a id="js_profile_article"></a>173篇原创内容

Official Account

👆点击关注｜设为星标｜干货速递👆

* * *

Python 是一种脚本语言，相比 C/C++ 这样的编译语言，在效率和性能方面存在一些不足。但是，有很多时候，Python 的效率并没有想象中的那么夸张。本文对一些 Python 代码加速运行的技巧进行整理。

## 代码优化原则

本文会介绍不少的 Python 代码加速运行的技巧。在深入代码优化细节之前，需要了解一些代码优化基本原则。

#### 第一个基本原则：不要过早优化

很多人一开始写代码就奔着性能优化的目标，“让正确的程序更快要比让快速的程序正确容易得多”。因此，优化的前提是代码能正常工作。过早地进行优化可能会忽视对总体性能指标的把握，在得到全局结果前不要主次颠倒。

#### 第二个基本原则：权衡优化的代价

优化是有代价的，想解决所有性能的问题是几乎不可能的。通常面临的选择是时间换空间或空间换时间。另外，开发代价也需要考虑。

#### 第三个原则：不要优化那些无关紧要的部分

如果对代码的每一部分都去优化，这些修改会使代码难以阅读和理解。如果你的代码运行速度很慢，首先要找到代码运行慢的位置，通常是内部循环，专注于运行慢的地方进行优化。在其他地方，一点时间上的损失没有什么影响。

## 避免全局变量

```
# 不推荐写法。代码耗时：26.8秒
import math
size = 10000
for x in range(size):
    for y in range(size):
        z = math.sqrt(x) + math.sqrt(y)

```

许多程序员刚开始会用 Python 语言写一些简单的脚本，当编写脚本时，通常习惯了直接将其写为全局变量，例如上面的代码。但是，由于全局变量和局部变量实现方式不同，定义在全局范围内的代码运行速度会比定义在函数中的慢不少。通过将脚本语句放入到函数中，通常可带来 15% - 30% 的速度提升。

```
# 推荐写法。代码耗时：20.6秒
import math
def main():  # 定义到函数中，以减少全部变量使用
    size = 10000
    for x in range(size):
        for y in range(size):
            z = math.sqrt(x) + math.sqrt(y)
main()

```

## 避免

### 2.1 避免模块和函数属性访问

```
# 不推荐写法。代码耗时：14.5秒
import math
def computeSqrt(size: int):
    result = []
    for i in range(size):
        result.append(math.sqrt(i))
    return result
def main():
    size = 10000
    for _ in range(size):
        result = computeSqrt(size)
main()

```

每次使用`.`（属性访问操作符时）会触发特定的方法，如`__getattribute__()`和`__getattr__()`，这些方法会进行字典操作，因此会带来额外的时间开销。通过`from import`语句，可以消除属性访问。

```
# 第一次优化写法。代码耗时：10.9秒
from math import sqrt
def computeSqrt(size: int):
    result = []
    for i in range(size):
        result.append(sqrt(i))  # 避免math.sqrt的使用
    return result
def main():
    size = 10000
    for _ in range(size):
        result = computeSqrt(size)
main()

```

在第 1 节中我们讲到，局部变量的查找会比全局变量更快，因此对于频繁访问的变量`sqrt`，通过将其改为局部变量可以加速运行。

```
# 第二次优化写法。代码耗时：9.9秒
import math
def computeSqrt(size: int):
    result = []
    sqrt = math.sqrt  # 赋值给局部变量
    for i in range(size):
        result.append(sqrt(i))  # 避免math.sqrt的使用
    return result
def main():
    size = 10000
    for _ in range(size):
        result = computeSqrt(size)
main()

```

除了`math.sqrt`外，`computeSqrt`函数中还有`.`的存在，那就是调用`list`的`append`方法。通过将该方法赋值给一个局部变量，可以彻底消除`computeSqrt`函数中`for`循环内部的`.`使用。

```
# 推荐写法。代码耗时：7.9秒
import math
def computeSqrt(size: int):
    result = []
    append = result.append
    sqrt = math.sqrt    # 赋值给局部变量
    for i in range(size):
        append(sqrt(i))  # 避免 result.append 和 math.sqrt 的使用
    return result
def main():
    size = 10000
    for _ in range(size):
        result = computeSqrt(size)
main()

```

### 2.2 避免类内属性访问

```
# 不推荐写法。代码耗时：10.4秒
import math
from typing import List
class DemoClass:
    def __init__(self, value: int):
        self._value = value
    
    def computeSqrt(self, size: int) -> List[float]:
        result = []
        append = result.append
        sqrt = math.sqrt
        for _ in range(size):
            append(sqrt(self._value))
        return result
def main():
    size = 10000
    for _ in range(size):
        demo_instance = DemoClass(size)
        result = demo_instance.computeSqrt(size)
main()

```

避免`.`的原则也适用于类内属性，访问`self._value`的速度会比访问一个局部变量更慢一些。通过将需要频繁访问的类内属性赋值给一个局部变量，可以提升代码运行速度。

```
# 推荐写法。代码耗时：8.0秒
import math
from typing import List
class DemoClass:
    def __init__(self, value: int):
        self._value = value
    
    def computeSqrt(self, size: int) -> List[float]:
        result = []
        append = result.append
        sqrt = math.sqrt
        value = self._value
        for _ in range(size):
            append(sqrt(value))  # 避免 self._value 的使用
        return result
def main():
    size = 10000
    for _ in range(size):
        demo_instance = DemoClass(size)
        demo_instance.computeSqrt(size)
main()

```

## 避免不必要的抽象

```
# 不推荐写法，代码耗时：0.55秒
class DemoClass:
    def __init__(self, value: int):
        self.value = value
    @property
    def value(self) -> int:
        return self._value
    @value.setter
    def value(self, x: int):
        self._value = x
def main():
    size = 1000000
    for i in range(size):
        demo_instance = DemoClass(size)
        value = demo_instance.value
        demo_instance.value = i
main()

```

任何时候当你使用额外的处理层（比如装饰器、属性访问、描述器）去包装代码时，都会让代码变慢。大部分情况下，需要重新进行审视使用属性访问器的定义是否有必要，使用`getter/setter`函数对属性进行访问通常是 C/C++ 程序员遗留下来的代码风格。如果真的没有必要，就使用简单属性。

```
# 推荐写法，代码耗时：0.33秒
class DemoClass:
    def __init__(self, value: int):
        self.value = value  # 避免不必要的属性访问器
def main():
    size = 1000000
    for i in range(size):
        demo_instance = DemoClass(size)
        value = demo_instance.value
        demo_instance.value = i
main()

```

## 避免数据复制

### 4.1 避免无意义的数据复制

```
# 不推荐写法，代码耗时：6.5秒
def main():
    size = 10000
    for _ in range(size):
        value = range(size)
        value_list = [x for x in value]
        square_list = [x * x for x in value_list]
main()

```

上面的代码中`value_list`完全没有必要，这会创建不必要的数据结构或复制。

```
# 推荐写法，代码耗时：4.8秒
def main():
    size = 10000
    for _ in range(size):
        value = range(size)
        square_list = [x * x for x in value]  # 避免无意义的复制
main()

```

另外一种情况是对 Python 的数据共享机制过于偏执，并没有很好地理解或信任 Python 的内存模型，滥用 `copy.deepcopy()`之类的函数。通常在这些代码中是可以去掉复制操作的。

### 4.2 交换值时不使用中间变量

```
# 不推荐写法，代码耗时：0.07秒
def main():
    size = 1000000
    for _ in range(size):
        a = 3
        b = 5
        temp = a
        a = b
        b = temp
main()

```

上面的代码在交换值时创建了一个临时变量`temp`，如果不借助中间变量，代码更为简洁、且运行速度更快。

```
# 推荐写法，代码耗时：0.06秒
def main():
    size = 1000000
    for _ in range(size):
        a = 3
        b = 5
        a, b = b, a  # 不借助中间变量
main()

```

### 4.3 字符串拼接用join而不是+

```
# 不推荐写法，代码耗时：2.6秒
import string
from typing import List
def concatString(string_list: List[str]) -> str:
    result = ''
    for str_i in string_list:
        result += str_i
    return result
def main():
    string_list = list(string.ascii_letters * 100)
    for _ in range(10000):
        result = concatString(string_list)
main()

```

当使用`a + b`拼接字符串时，由于 Python 中字符串是不可变对象，其会申请一块内存空间，将`a`和`b`分别复制到该新申请的内存空间中。因此，如果要拼接`n`个字符串，会产生 `n-1`个中间结果，每产生一个中间结果都需要申请和复制一次内存，严重影响运行效率。而使用`join()`拼接字符串时，会首先计算出需要申请的总的内存空间，然后一次性地申请所需内存，并将每个字符串元素复制到该内存中去。

```
# 推荐写法，代码耗时：0.3秒
import string
from typing import List
def concatString(string_list: List[str]) -> str:
    return ''.join(string_list)  # 使用 join 而不是 +
def main():
    string_list = list(string.ascii_letters * 100)
    for _ in range(10000):
        result = concatString(string_list)
main()
```

## 利用 if 条件的短路特性

```
# 不推荐写法，代码耗时：0.05秒
from typing import List
def concatString(string_list: List[str]) -> str:
    abbreviations = {'cf.', 'e.g.', 'ex.', 'etc.', 'flg.', 'i.e.', 'Mr.', 'vs.'}
    abbr_count = 0
    result = ''
    for str_i in string_list:
        if str_i in abbreviations:
            result += str_i
    return result
def main():
    for _ in range(10000):
        string_list = ['Mr.', 'Hat', 'is', 'Chasing', 'the', 'black', 'cat', '.']
        result = concatString(string_list)
main()

```

`if` 条件的短路特性是指对`if a and b`这样的语句， 当`a`为`False`时将直接返回，不再计算`b`；对于`if a or b`这样的语句，当`a`为`True`时将直接返回，不再计算`b`。因此， 为了节约运行时间，对于`or`语句，应该将值为`True`可能性比较高的变量写在`or`前，而`and`应该推后。

```
# 推荐写法，代码耗时：0.03秒
from typing import List
def concatString(string_list: List[str]) -> str:
    abbreviations = {'cf.', 'e.g.', 'ex.', 'etc.', 'flg.', 'i.e.', 'Mr.', 'vs.'}
    abbr_count = 0
    result = ''
    for str_i in string_list:
        if str_i[-1] == '.' and str_i in abbreviations:  # 利用 if 条件的短路特性
            result += str_i
    return result
def main():
    for _ in range(10000):
        string_list = ['Mr.', 'Hat', 'is', 'Chasing', 'the', 'black', 'cat', '.']
        result = concatString(string_list)
main()

```

## 循环优化

### 6.1 用`for`循环代替`while`循环

```
# 不推荐写法。代码耗时：6.7秒
def computeSum(size: int) -> int:
    sum_ = 0
    i = 0
    while i < size:
        sum_ += i
        i += 1
    return sum_
def main():
    size = 10000
    for _ in range(size):
        sum_ = computeSum(size)
main()

```

Python 的`for`循环比`while`循环快不少。

```
# 推荐写法。代码耗时：4.3秒
def computeSum(size: int) -> int:
    sum_ = 0
    for i in range(size):  # for 循环代替 while 循环
        sum_ += i
    return sum_
def main():
    size = 10000
    for _ in range(size):
        sum_ = computeSum(size)
main()

```

### 6.2 使用隐式`for`循环代替显式`for`循环

针对上面的例子，更进一步可以用隐式`for`循环来替代显式`for`循环

```
# 推荐写法。代码耗时：1.7秒
def computeSum(size: int) -> int:
    return sum(range(size))  # 隐式 for 循环代替显式 for 循环
def main():
    size = 10000
    for _ in range(size):
        sum = computeSum(size)
main()

```

### 6.3 减少内层`for`循环的计算

```
# 不推荐写法。代码耗时：12.8秒
import math
def main():
    size = 10000
    sqrt = math.sqrt
    for x in range(size):
        for y in range(size):
            z = sqrt(x) + sqrt(y)
main() 

```

上面的代码中`sqrt(x)`位于内侧`for`循环， 每次训练过程中都会重新计算一次，增加了时间开销。

```
# 推荐写法。代码耗时：7.0秒
import math
def main():
    size = 10000
    sqrt = math.sqrt
    for x in range(size):
        sqrt_x = sqrt(x)  # 减少内层 for 循环的计算
        for y in range(size):
            z = sqrt_x + sqrt(y)
main() 

```

## 使用 numba.jit

我们沿用上面介绍过的例子，在此基础上使用`numba.jit`。`numba`可以将 Python 函数 JIT 编译为机器码执行，大大提高代码运行速度。关于`numba`的更多信息见下面的主页：

http://numba.pydata.org/numba.pydata.org

```
# 推荐写法。代码耗时：0.62秒
import numba
@numba.jit
def computeSum(size: float) -> int:
    sum = 0
    for i in range(size):
        sum += i
    return sum
def main():
    size = 10000
    for _ in range(size):
        sum = computeSum(size)
main()

```

## **选择合适的数据结构**

Python 内置的数据结构如`str`, `tuple`, `list`, `set`, `dict`底层都是 C 实现的，速度非常快，自己实现新的数据结构想在性能上达到内置的速度几乎是不可能的。

`list`类似于 C++ 中的`std::vector`，是一种动态数组。其会预分配一定内存空间，当预分配的内存空间用完，又继续向其中添加元素时，会申请一块更大的内存空间，然后将原有的所有元素都复制过去，之后销毁之前的内存空间，再插入新元素。删除元素时操作类似，当已使用内存空间比预分配内存空间的一半还少时，会另外申请一块小内存，做一次元素复制，之后销毁原有大内存空间。

因此，如果有频繁的新增、删除操作，新增、删除的元素数量又很多时，list的效率不高。此时，应该考虑使用`collections.deque`。`collections.deque`是双端队列，同时具备栈和队列的特性，能够在两端进行 `O(1)`复杂度的插入和删除操作。

`list`的查找操作也非常耗时。当需要在`list`频繁查找某些元素，或频繁有序访问这些元素时，可以使用`bisect`维护`list`对象有序并在其中进行二分查找，提升查找的效率。

另外一个常见需求是查找极小值或极大值，此时可以使用`heapq`模块将`list`转化为一个堆，使得获取最小值的时间复杂度是`O(1)`。

下面的网页给出了常用的 Python 数据结构的各项操作的时间复杂度：
TimeComplexity - Python Wikiwiki.python.org

## **参考资料**

- https://zhuanlan.zhihu.com/p/143052860
    
- David Beazley & Brian K. Jones. Python Cookbook, Third edition. O'Reilly Media, ISBN: 9781449340377, 2013.
    
- 张颖 & 赖勇浩. 编写高质量代码：改善Python程序的91个建议. 机械工业出版社, ISBN: 9787111467045, 2014.
    

往期推荐 点击查看

[Python 实现循环最快的方式](https://mp.weixin.qq.com/s?__biz=Mzk0OTI1OTQ2MQ==&mid=2247511123&idx=2&sn=c1a5136874b1c933bc41d3e6e451b6c3&chksm=c359f1f9f42e78efdedb29ce69eee9414b11ed08b7d30028262bc695f27f11502a5597b477b2&scene=21#wechat_redirect)

[Python 使用和高性能技巧总结](https://mp.weixin.qq.com/s?__biz=Mzk0OTI1OTQ2MQ==&mid=2247510427&idx=2&sn=ed94f6db2452d28ee4139305a18e688f&chksm=c359f431f42e7d2784d4c35816a1e3593cb2e07231409a16db3e153d268fb4c5923c22028faa&scene=21#wechat_redirect)

[全网最棒总结！Python日志库 logging 使用指南](https://mp.weixin.qq.com/s?__biz=Mzk0OTI1OTQ2MQ==&mid=2247507043&idx=1&sn=320a21371be857c7c4c18b85d0822e40&chksm=c35981c9f42e08dffd484e02c044fb47e132b85a48ca7466c4155017d12198e2f516e95106b7&scene=21#wechat_redirect)

[Python基础｜一文讲透 python 协程](https://mp.weixin.qq.com/s?__biz=Mzk0OTI1OTQ2MQ==&mid=2247506998&idx=2&sn=ce123fa74a647672412ae2ba4e65703c&chksm=c359819cf42e088aaabc8c39e49c9d3aa1ee067e557c0acb6428fc7ea616bc13c34f1b3b77d5&scene=21#wechat_redirect)

[终于搞明白 Python 的 @装饰器 是干什么用的了！](https://mp.weixin.qq.com/s?__biz=Mzk0OTI1OTQ2MQ==&mid=2247506910&idx=2&sn=0159e0d5ebd7763d9bebfec866631eb0&chksm=c3598674f42e0f62315c8934f7c20c830ec603aa9dae3c87cb589826bc7285d72a8d0d591ef5&scene=21#wechat_redirect)

[34 个 Python 办公自动化工具库](https://mp.weixin.qq.com/s?__biz=Mzk0OTI1OTQ2MQ==&mid=2247506720&idx=2&sn=9ae57ce1af253f1d48e441c38a7d75ba&chksm=c359868af42e0f9c5de7937cf53d0740832740c12e2e3850e1f722be18c21f98ef2144fa6757&scene=21#wechat_redirect)

[数据科学家提高效率的 40 个 Python 技巧](https://mp.weixin.qq.com/s?__biz=Mzk0OTI1OTQ2MQ==&mid=2247506024&idx=1&sn=a311f30c7a7a96c50fdf6de48d3db463&chksm=c35985c2f42e0cd475eeea307e61028641654101f8c9575f9e6a759c571fc978a8ea3da67c5c&scene=21#wechat_redirect)

[精选 15 个顶级 Python 库](https://mp.weixin.qq.com/s?__biz=Mzk0OTI1OTQ2MQ==&mid=2247505824&idx=2&sn=e10b8206f7ad8ca87a67ce91c1690757&chksm=c3598a0af42e031c355b94b83b8b287c39759644a7f71ca09f3965c1d1c65f7410532b3d79e3&scene=21#wechat_redirect)

[24 个好用到爆的 Python 实用技巧！](https://mp.weixin.qq.com/s?__biz=Mzk0OTI1OTQ2MQ==&mid=2247505087&idx=1&sn=2c17139efd3e7103e088fe3103e90337&chksm=c3598915f42e0003b566dfd65865969b9ce663445587729790418c96231168dbbf6c4f3dd460&scene=21#wechat_redirect)

[68 个 Python 内置函数详解](https://mp.weixin.qq.com/s?__biz=Mzk0OTI1OTQ2MQ==&mid=2247503551&idx=2&sn=1ff63e062e893999cafda88cc4751ec4&chksm=c3599315f42e1a035be08c0ef25cc59be905cc213a306f1c891c62b14472c0701af48b728ba8&scene=21#wechat_redirect)

[过瘾！100道Python入门练习题](https://mp.weixin.qq.com/s?__biz=Mzk0OTI1OTQ2MQ==&mid=2247501855&idx=2&sn=c603f9a7e25517847f6ac3993659505b&chksm=c35995b5f42e1ca3c2c1613c26f7288bd220cfb435f32c24a26e94aea787bdc8563a83ac2ecf&scene=21#wechat_redirect)

长按👇关注\- 数据STUDIO -设为星标，干货速递

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

People who liked this content also liked

一个非常好用的 Python 魔法库

Python编程大全

不看的原因

- 内容质量低
- 不看此公众号

使用python绘制散点密度图（用颜色标识密度）

Chemocoder

不看的原因

- 内容质量低
- 不看此公众号

Apache 架构师总结的 30 条架构原则

架构文摘

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___cce8be664929435e8.bmp"/>

Scan to Follow