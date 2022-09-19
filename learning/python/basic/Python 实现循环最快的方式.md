# Python 实现循环最快的方式

<a id="profileBt"></a><a id="js_name"></a>数据STUDIO *2022-01-16 08:35*

收录于话题

<a id="js_article_tag_name__1974978820940054530"></a>#数据分析 <a id="js_article_tag_num__1974978820940054530"></a>53 <a id="js_article_tag_tips__1974978820940054530"></a>个

<a id="js_article_tag_name__1974978822768771072"></a>#python <a id="js_article_tag_num__1974978822768771072"></a>45 <a id="js_article_tag_tips__1974978822768771072"></a>个

大家好，我是云朵君！
**关注和星标『数据STUDIO』**，和云朵君一起学习数据分析与挖掘！

![](../../../_resources/0_wx_fmt_png_63f9e344527e406386fede81223d37df.png)

**数据STUDIO**

点击领取《Python学习手册》，后台回复「福利」获取。『数据STUDIO』专注于数据科学原创文章分享，内容以 Python 为核心语言，涵盖机器学习、数据分析、可视化、MySQL等领域干货知识总结及实战项目。

<a id="js_profile_article"></a>173篇原创内容

Official Account

👆点击关注｜设为星标｜干货速递👆

* * *

众所周知，Python 不是一种执行效率较高的语言。此外在任何语言中，循环都是一种非常消耗时间的操作。假如任意一种简单的单步操作耗费的时间为 1 个单位，将此操作重复执行上万次，最终耗费的时间也将增长上万倍。

`while` 和 `for` 是 Python 中常用的两种实现循环的关键字，它们的运行效率实际上是有差距的。比如下面的测试代码：

```
import timeit
def while_loop(n=100_000_000):
    i = 0
    s = 0
    while i < n:
        s += i
        i += 1
    return s
def for_loop(n=100_000_000):
    s = 0
    for i in range(n):
        s += i
    return s
def main():
    print('while loop\t\t', timeit.timeit(while_loop, number=1))
    print('for loop\t\t', timeit.timeit(for_loop, number=1))
if __name__ == '__main__':
    main()
# => while loop               4.718853999860585
# => for loop                 3.211570399813354

```

这是一个简单的求和操作，计算从 1 到 n 之间所有自然数的总和。可以看到 `for` 循环相比 `while` 要快 1.5 秒。

其中的差距主要在于两者的机制不同。

在每次循环中，`while` 实际上比 `for` 多执行了两步操作：边界检查和变量 `i` 的自增。即每进行一次循环，while 都会做一次边界检查 (`while i < n`）和自增计算（`i +=1`）。这两步操作都是显式的纯 Python 代码。

`for` 循环不需要执行边界检查和自增操作，没有增加显式的 Python 代码（纯 Python 代码效率低于底层的 C 代码）。当循环的次数足够多，就出现了明显的效率差距。

可以再增加两个函数，在 `for` 循环中加上不必要的边界检查和自增计算：

```
import timeit
def while_loop(n=100_000_000):
    i = 0
    s = 0
    while i < n:
        s += i
        i += 1
    return s
def for_loop(n=100_000_000):
    s = 0
    for i in range(n):
        s += i
    return s
def for_loop_with_inc(n=100_000_000):
    s = 0
    for i in range(n):
        s += i
        i += 1
    return s
def for_loop_with_test(n=100_000_000):
    s = 0
    for i in range(n):
        if i < n:
            pass
        s += i
    return s
def main():
    print('while loop\t\t', timeit.timeit(while_loop, number=1))
    print('for loop\t\t', timeit.timeit(for_loop, number=1))
    print('for loop with increment\t\t',
          timeit.timeit(for_loop_with_inc, number=1))
    print('for loop with test\t\t', timeit.timeit(for_loop_with_test, number=1))
if __name__ == '__main__':
    main()
# => while loop               4.718853999860585
# => for loop                 3.211570399813354
# => for loop with increment  4.602369500091299
# => for loop with test       4.18337869993411

```

可以看出，增加的边界检查和自增操作确实大大影响了 `for` 循环的执行效率。

前面提到过，Python 底层的解释器和内置函数是用 C 语言实现的。而 C 语言的执行效率远大于 Python。

对于上面的求等差数列之和的操作，借助于 Python 内置的 `sum` 函数，可以获得远大于 `for` 或 `while` 循环的执行效率。

```
import timeit
def while_loop(n=100_000_000):
    i = 0
    s = 0
    while i < n:
        s += i
        i += 1
    return s
def for_loop(n=100_000_000):
    s = 0
    for i in range(n):
        s += i
    return s
def sum_range(n=100_000_000):
    return sum(range(n))
def main():
    print('while loop\t\t', timeit.timeit(while_loop, number=1))
    print('for loop\t\t', timeit.timeit(for_loop, number=1))
    print('sum range\t\t', timeit.timeit(sum_range, number=1))
if __name__ == '__main__':
    main()
# => while loop               4.718853999860585
# => for loop                 3.211570399813354
# => sum range                0.8658821999561042

```

可以看到，使用内置函数 `sum` 替代循环之后，代码的执行效率实现了成倍的增长。

内置函数 `sum` 的累加操作实际上也是一种循环，但它由 C 语言实现，而 `for` 循环中的求和操作是由纯 Python 代码 `s += i` 实现的。C > Python。

再拓展一下思维。小时候都听说过童年高斯巧妙地计算 1 到 100 之和的故事。1…100 之和等于 (1 + 100) * 50。这个计算方法同样可以应用到上面的求和操作中。

```
import timeit
def while_loop(n=100_000_000):
    i = 0
    s = 0
    while i < n:
        s += i
        i += 1
    return s
def for_loop(n=100_000_000):
    s = 0
    for i in range(n):
        s += i
    return s
def sum_range(n=100_000_000):
    return sum(range(n))
def math_sum(n=100_000_000):
    return (n * (n - 1)) // 2
def main():
    print('while loop\t\t', timeit.timeit(while_loop, number=1))
    print('for loop\t\t', timeit.timeit(for_loop, number=1))
    print('sum range\t\t', timeit.timeit(sum_range, number=1))
    print('math sum\t\t', timeit.timeit(math_sum, number=1))
if __name__ == '__main__':
    main()
# => while loop               4.718853999860585
# => for loop                 3.211570399813354
# => sum range                0.8658821999561042
# => math sum                 2.400018274784088e-06

```

最终 math sum 的执行时间约为 `2.4e-6`，缩短了上百万倍。这里的思路就是，既然循环的效率低，一段代码要重复执行上亿次。

索性直接不要循环，通过数学公式，把上亿次的循环操作变成只有一步操作。效率自然得到了空前的加强。

最后的结论（有点谜语人）：

**实现循环的最快方式**——**就是不用循环**

对于 Python 而言，则尽可能地使用内置函数，将循环中的纯 Python 代码降到最低。

### 参考资料

\[1\]

The Fastest Way to Loop in Python - mCoding: *https://youtu.be/Qgevy75co8c*

> 作者：StarryLand
> 链接：
> https://www.starky.ltd/2021/11/23/the-fastest-way-to-loop-in-python

长按👇关注\- 数据STUDIO -设为星标，干货速递

<img width="677" height="227" src="../../../_resources/640_wx_fmt_gif_wxfrom_5_wx_lazy__42623e6f4ba84689a.gif"/>

<img width="40" height="47" src="../../../_resources/640_wx_fmt_gif_wxfrom_5_wx_lazy__c287fd57020a46ceb.gif"/>

点个在看你最好看

People who liked this content also liked

使用python绘制散点密度图（用颜色标识密度）

Chemocoder

不看的原因

- 内容质量低
- 不看此公众号

函数调用时底层发生了什么？

码农的荒岛求生

不看的原因

- 内容质量低
- 不看此公众号

学Python 新手做到这7点，提升编程能力真不难！

Python编程大全

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___b35277b4f71e4645b.bmp"/>

Scan to Follow