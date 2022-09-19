# 【Python】Python小技巧：​args 和 kwargs 的乐趣

<a id="profileBt"></a><a id="js_name"></a>机器学习初学者 *2022-05-26 11:34* *Posted on <a id="js_ip_wording"></a>浙江*

The following article is from 程序员zhenguo Author 小伟

<a id="copyright_info"></a>[![](../../../_resources/0_fa47f7b71d6e4ddb8f77304cad337856.jpg)<br>**程序员zhenguo** .<br>程序员zhenguo（原名：Python与算法社区）关注并回复 宝书 获取我沉淀的两本Python与算法宝书。正在推送100个Python、爬虫、数据分析和机器学习、深度学习项目，敬请关注。](#)

这篇文章是伟兄给我的稿子，总结实用、到位。另外，欢迎访问并关注他的博客：

https://jl-zhenlaixiaowei.blog.csdn.net/

我曾经和一个聪明的 Pythonista 结对编程，每次他输入带有可选或关键字参数的函数定义时,他都会惊呼“argh!”和“kwargh！”。

要不然我们相处的很好，我猜想这就是学术界编程最终对人所带来的影响吧。

现在**args**和 **kwargs**参数仍然是 Python 中非常有用的特性，而且理解它们的威力将使您成为更有效的开发人员。

**那么“args”和“kwargs”参数用来做什么呢？**

它们允许一个函数接受可选参数，因此你能够在你的模块和类里创建弹性APIs。

**示例代码如下：**

```


In [2]: def foo(required, *args, **kwargs):
   ...:     print(required)
   ...:     if args:
   ...:         print(args)
   ...:     if kwargs:
   ...:         print(kwargs)



```

上面的函数需要至少一个叫做“必须的”参数，但是它也能接受额外的位置参数和关键字参数。

**如果我们调用带有附加参数的函数，参数将会收集额外的位置参数作为一个元组，因为这个参数的名字有一个*（单星号）前缀。**

同样地，kwargs将收集额外的关键字参数作为一个字典，因为这个参数名字有**（双星号）前缀。

**如果没有附加参数被传递给函数。args 和 kwargs 可以为空。**

当我们调用带有参数的不同组合的函数时，你会看到在args和kwargs内部参数。

Python如何收集它们，根据它们是否为位置参数或者关键字参数。代码如下：

```


In [3]: foo()
---------------------------------------------------------------------------
TypeError                                 Traceback (most recent call last)
<ipython-input-3-c19b6d9633cf> in <module>
----> 1 foo()
TypeError: foo() missing 1 required positional argument: 'required'



```

```


In [4]: foo('hello')
hello



```

```


In [5]: foo('hello', 1, 2, 3)
hello
(1, 2, 3)



```

```


In [6]: foo('hello', 1, 2, 3, key1='value', key2=999)
hello
(1, 2, 3)
{'key1': 'value', 'key2': 999}



```

我要明确一下，调用args和kwargs参数是简单的命名惯例。

像之前的例子里，如果称他们\*parms和\*\*argv也可以。实际上语法分别是单星号（*）或者双星号（**）。

然而，我还是推荐你还是坚持可接受的命名惯例以避免混淆。（而且每隔一段时间还有机会喊“argh!”和“kwargh!”）。

**\## 转发可选或者关键字参数**

有可能从一个函数到另一个函数传递可选或者关键字参数。

当你调用要转发参数的函数时，你可以通过使用解包参数操作符*和**。在你传递之前这也给你一个机会修改参数。

示例如下：

```


In [8]: def foo(x, *args, **kwargs):
   ...:     kwarg['name'] = 'Alice'
   ...:     new_args = args + ('extra', )
   ...:     bar(x, *new_args, **kwargs)



```

这种技术对于子类化和编写包装函数很有用。

例如，您可以使用它来扩展父类的行为，而不必在子类中复制其构造函数的完整签名。

如果您使用的 API 可能会在您的控制之外发生变化，这会非常方便，示例代码如下：

```


In [9]: class Car:
   ...:     def __init__(self, color, mileage):
   ...:         self.color = color
   ...:         self.mileage = mileage
   ...: 
In [10]: class AlwaysBlueCar(Car):
    ...:     def __init__(self, *args, **kwargs):
    ...:         super().__init__(*args, **kwargs)
    ...:         self.color = 'blue'
In [12]: AlwaysBlueCar('green', 48392).color
Out[12]: 'blue'



```

**AlwaysBlueCar 构造函数只是将所有参数传递给它的父类，然后覆盖一个内部属性。**

这意味着如果父类构造函数发生更改，AlwaysBlueCar 很有可能仍会按预期运行。

这里的缺点是 AlwaysBlueCar 构造函数现在有一个相当无用的签名——如果不查找父类，我们不知道它需要什么参数。

通常，您不会将这种技术用于您自己的类层次结构。更有可能的情况是您想要修改或覆盖某些您无法控制的外部类中的行为。

但这总是危险的领域，所以最好小心（否则你可能很快就会有另一个理由尖叫“argh!”）。

**这种技术可能有用的另一种情况是编写包装函数，例如装饰器。在那里，您通常还希望接受要传递给包装函数的任意参数。**

而且，如果我们可以在不必复制和粘贴原始函数签名的情况下做到这一点，那可能更易于维护：

```


import functools
def trace(f):
    ...:     @functools.wraps(f)
    ...:     def decorated_function(*args, **kwargs):
    ...:         print(f, args, kwargs)
    ...:         result = f(*args, **kwargs)
    ...:         print(result)
    ...:     return decorated_function
    
In [11]: @trace
    ...: def greet(greeting, name):
    ...:     return '{}, {}!'.format(greeting, name)



```

```


In [14]: greet('Hello', 'Bob')
<function greet at 0x7fefa69db700> ('Hello', 'Bob') {}
Hello, Bob!



```

使用像这样的技术，有时很难在使代码足够明确的想法和遵守不要重复自己（DRY）原则的想法之间取得平衡。

这可能永远是一个艰难的选择。如果你能从同事那里得到第二个意见，我鼓励你尝试一下。

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
    
- 机器学习交流qq群955171419，加入微信群请扫码：
    




```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)


```


```


```


```






```

People who liked this content also liked

【Docker】命令使用大全

...

小白学视觉

不看的原因

- 内容质量低
- 不看此公众号

用Python去除图片背景：​Rembg库

...

Python大数据分析

不看的原因

- 内容质量低
- 不看此公众号

C++库文件和头文件编写教程

...

小白学视觉

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___a76ca22ca7904fcfb.bmp"/>

Scan to Follow