# Python 3.10，switch语法搞懂没？

<a id="profileBt"></a><a id="js_name"></a>深度学习初学者 *2022-04-13 11:40*

![Image](../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_b165293878c942869.jpg)

机器之心报道

> 不用再自己琢磨如何实现 switch 功能了。

对于从事数据科学和人工智能领域的人们来说，Python 是大家的首选编程语言。根据最近的一项调查，27% 的程序员开发职位要求掌握 Python 语言，今年年初这一数字还只是 18.5%。

Python 流行的原因在于其拥有非常直观的能力：这门语言拥有大量的库、足够高的生产效率，还相对易于学习。2020年 10 月，[Python 的 3.9 版正式](http://mp.weixin.qq.com/s?__biz=MzA3MzI4MjgzMw==&mid=2650799583&idx=2&sn=79f9c151c0f32faeb7ae9237bed98fb7&chksm=871a39a1b06db0b76bb738b72c26ab1e465ebd0463037792650d7792cbd6fe914de3fe7ed31a&scene=21#wechat_redirect)发布了，从字典更新 / 合并到添加新的字符串方法，再到 zoneinfo 库的引入，Python 3.9 添加了许多新特性.

[Python3.10 的第二个 alpha 版本](http://mp.weixin.qq.com/s?__biz=MzA3MzI4MjgzMw==&mid=2650803376&idx=2&sn=66c7c93ca52b571f0e7d31f4cfeae8e4&chksm=84e5c8ceb39241d80d79fc306a8104a23684e581e6b10c470f68edfb75dacbe29244cfeb2adf&scene=21#wechat_redirect)也已于2020年 11 月初发布，相比于不久前发布的 3.9 版本，新版本对类型注释扩展、zip、位计数、字典映射又有了新的改进。Python 3.10 beta 版发布了，新的 beta 版最大的亮点可能就是引入了 switch-case 语句。

**Python 3.10 beta 版新改进**

Switch 语句存在于很多编程语言中，但 Python 编程语言不支持 Switch 语句。早在 2016 年，PEP 3103 就被提出，建议 Python 支持 switch-case 语句。然而，在调查中发现很少人支持该特性，Python 开发人员放弃了它。

时间在推到 2020 年，Python 的创始人 Guido van Rossum，提交了显示 switch 语句的第一个文档，命名为 Structural Pattern Matching，见 PEP 634 。

如今，随着 Python 3.10 beta 版的发布，终于将 switch-case 语句纳入其中。

带圆括号的上下文管理器：现在支持在上下文管理器中跨多行使用括号进行延续。也可以在所包含组的末尾使用逗号。

```
with (
    CtxManager1() as example1,
    CtxManager2() as example2,
    CtxManager3() as example3,
):
    ...

```

错误消息 \- NameErrors：当打印由 interpreter 引发的 NameError 时，PyErr_Display() 将在引发异常的函数中提供相似变量名的建议：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

PEP 634 结构模式匹配：模式匹配允许用户在 match 后面跟随数个 case 语句。当在程序执行 match-case 时，有匹配的语句，程序就会进入相应的 case 语句来执行操作。

match-case 语法和操作：模式匹配的通用语法是：

```
match subject:
    case <pattern_1>:
        <action_1>
    case <pattern_2>:
        <action_2>
    case <pattern_3>:
        <action_3>
    case _:
        <action_wildcard>

```

match 语句接受一个表达式，并将其值与作为一个或多个 case 块给出的连续模式进行比较。match-case 示例如下：

```
http_code = "418"
match http_code:
    case "200":
        print("OK")
        do_something_good()
    case "404":
        print("Not Found")
        do_something_bad()
    case "418":
        print("I'm a teapot")
        make_coffee()
    case _:
        print("Code not found")

```

下图是 match-case 语句执行示意图。程序会检查多个 case 条件，并根据在变量 http_code 中找到的值执行不同的操作。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

同样的，你也可以使用一组 if-elif-else 语句来构建相同的逻辑：

```
http_code = "418"
if http_code == "418":
    print("OK")
    do_something_good()
elif http_code == "404":
    print("Not Found")
    do_something_bad()
elif http_code == "418"
    print("I'm a teapot")
    make_coffee()
else:
    print("Code not found")

```

然而，通过使用 match-case 语句，删除了 http\_code == 的重复执行，当测试许多不同的条件时使用 match-case，http\_code == 看起来更加清晰。

我们可以通过一个简单的例子来了解模式匹配：用 C、Java 或 JavaScript（以及许多其他语言）中的 switch 语句将对象（数据对象）与文本（模式）进行匹配。switch 语句通常用于将对象 / 表达式与包含文字的 case 语句进行比较。

虽然使用嵌套 if 语句的命令式指令系列可以用来完成类似于结构模式匹配的任务，但它不如声明式方法那么清晰。相反，声明性方法声明了匹配所需满足的条件，并且通过其显式模式更具可读性。虽然结构模式匹配可以以最简单的形式使用，将变量与 case 语句中的文本进行比较，但它对 Python 的真正价值在于它对对象类型和大小的处理。

match-case 可以说是此次 Python 3.10 beta 版本最大的亮点。对于这种表达式，有人喜欢，有人憎恶。在 Python 还不支持 switch-case 之前，大家可能都是使用字典进行相关操作。有人说 switch 除了读着方便，没什么优点；还有人说 Python 的闭包机制，dict 的 value 可以是带闭包的函数，这样就使得表达能力比 switch-case 更上一层楼；但总归 Python 3.10 beta 版本纳入了 switch-case，为开发者提供了另一种选择。

*参考链接：https://towardsdatascience.com/switch-case-statements-are-coming-to-python-d0caf7b2bfd3*

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

<a id="js_view_source"></a>Read more

People who liked this content also liked

分享10个超级实用事半功倍的Python自动化脚本

...

关于数据分析与可视化

不看的原因

- 内容质量低
- 不看此公众号

一文弄懂Python中的 if \_\_name\_\_==\_\_main\_\_

...

AI算法之道

不看的原因

- 内容质量低
- 不看此公众号

​有了这份小抄，你还学不会Python？再也不怕不记得Python的语法了

...

编程小小

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___8e270f3e139c459ca.bmp"/>

Scan to Follow