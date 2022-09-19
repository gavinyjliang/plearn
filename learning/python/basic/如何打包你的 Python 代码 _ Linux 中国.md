# 如何打包你的 Python 代码 | Linux 中国

邀你一起加入 <a id="profileBt"></a><a id="js_name"></a>Linux *2021-11-21 07:30*

<img width="649" height="365" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_33718d771b984f599.jpg"/>

**导读：**使用 setuptools 来向用户交付 Python 代码。　 　 　 　 　 　 　 　 　 　 　 　 　

本文字数：3429，阅读时长大约：4分钟

https://linux.cn/article-13993-1.html
作者：Seth Kenlon
译者：geekpi

你花了几周的时间来完善你的代码。你已经对它进行了测试，并把它发送给一些亲近的开发者朋友以保证质量。你已经将所有的源代码发布在 你的个人 Git 服务器🔗 opensource.com 上，并且从一些勇敢的早期使用者收到了一些有用的错误报告。现在你已经准备好将你的 Python 代码提供给全世界。

就在这时你遇到一个问题。你不知道如何交付产品。

将代码交付给它的目标用户是一件大事。这是软件开发的一个完整的分支，是 CI/CD 中的 “D”，但很多人都忘记了，至少到最后才知道。我写过关于 Autotools🔗 opensource.com 和 Cmake🔗 opensource.com 的文章，但有些语言有自己的方法来帮助你将你的代码提供给用户。对于 Python 来说，向用户提供代码的一个常见方法是使用 `setuptools`。

<img width="32" height="32" src="../../../_resources/640_wx_fmt_svg_wxfrom_5_wx_lazy__954322ddbb564cf4a.svg"/>

安装 setuptools

安装和更新 `setuptools` 的最简单方法是使用 `pip`：

```


1.  `$ sudo python -m pip install --upgrade setuptools`
    


```

<img width="32" height="32" src="../../../_resources/640_wx_fmt_svg_wxfrom_5_wx_lazy__954322ddbb564cf4a.svg"/>

示例库

我创建了一个简单的 Python 库，名为 `myhellolib`，来作为需要打包的示例代码。这个库接受一个字符串，然后用大写字母打印出这个字符串。

它只有两行代码，但项目结构很重要，所以首先创建目录树：

```


1.  `$ mkdir  -p myhellolib.git/myhellolib`
    


```

为了确认这个项目是一个可导入的库（即 Python “模块”），在代码目录中创建一个空文件 `__init__.py`，同时创建一个包含代码的文件：

```


1.  `$ touch myhellolib.git/myhellolib/__init__.py`
    
2.  `$ touch myhellolib.git/myhellolib/myhellolib.py`
    


```

在 `myhellolib.py` 文件中，输入简单的 Python 代码：

```


1.  `def greeter(s):`
    
2.   `print(s.upper())`
    


```

这就是写好的库。

<img width="32" height="32" src="../../../_resources/640_wx_fmt_svg_wxfrom_5_wx_lazy__954322ddbb564cf4a.svg"/>

测试它

在打包之前，测试一下你的库。创建一个 `myhellolib.git/test.py` 文件并输入以下代码：

```


1.  `import myhellolib.myhellolib as hello`
    
2.  `hello.greeter("Hello Opensource.com.")`
    


```

运行该脚本：

```


1.  `$ cd myhellolib.git`
    
2.  `$ python ./test.py`
    
3.  `HELLO OPENSOURCE.COM`
    


```

它可以工作，所以现在你可以把它打包了。

<img width="32" height="32" src="../../../_resources/640_wx_fmt_svg_wxfrom_5_wx_lazy__954322ddbb564cf4a.svg"/>

Setuptools

要用 `setuptools` 打包一个项目，你必须创建一个 `.toml` 文件，将 `setuptools` 作为构建系统。将这段文字放在项目目录下的 `myhellolib.toml` 文件中。

```


1.  `[build-system]`
    
2.  `requires =  ["setuptools",  "wheel"]`
    
3.  `build-backend =  "setuptools.build_meta"`
    


```

接下来，创建一个名为 `setup.py` 的文件，包含项目的元数据：

```


1.  `from setuptools import setup`
    

3.  `setup(`
    
4.   `name='myhellolib',`
    
5.   `version='0.0.1',`
    
6.   `packages=['myhellolib'],`
    
7.   `install_requires=[`
    
8.   `'requests',`
    
9.   `'importlib; python_version == "3.8"',`
    
10.  `],`
    
11. `)`
    


```

不管你信不信，这就是 `setuptools` 需要的所有设置。你的项目已经可以进行打包。

<img width="32" height="32" src="../../../_resources/640_wx_fmt_svg_wxfrom_5_wx_lazy__954322ddbb564cf4a.svg"/>

打包 Python

要创建你的 Python 包，你需要一个构建器。一个常见的工具是 `build`，你可以用 `pip` 安装它：

```


1.  `$ python -m pip install build --user`
    


```

构建你的项目：

```


1.  `$ python -m build`
    


```

过了一会儿，构建完成了，在你的项目文件夹中出现了一个新的目录，叫做 `dist`。这个文件夹包含一个 `.tar.gz` 和一个 `.whl` 文件。

这是你的第一个 Python 包! 下面是包的内容：

```


1.  `$ tar  --list  --file dist/myhellolib-0.0.1.tar.gz`
    
2.  `myhellolib-0.0.1/`
    
3.  `myhellolib-0.0.1/PKG-INFO`
    
4.  `myhellolib-0.0.1/myhellolib/`
    
5.  `myhellolib-0.0.1/myhellolib/__init__.py`
    
6.  `myhellolib-0.0.1/myhellolib/myhellolib.py`
    
7.  `myhellolib-0.0.1/myhellolib.egg-info/`
    
8.  `myhellolib-0.0.1/myhellolib.egg-info/PKG-INFO`
    
9.  `myhellolib-0.0.1/myhellolib.egg-info/SOURCES.txt`
    
10. `myhellolib-0.0.1/myhellolib.egg-info/dependency_links.txt`
    
11. `myhellolib-0.0.1/myhellolib.egg-info/requires.txt`
    
12. `myhellolib-0.0.1/myhellolib.egg-info/top_level.txt`
    
13. `myhellolib-0.0.1/setup.cfg`
    
14. `myhellolib-0.0.1/setup.py`
    

16. `$ unzip -l dist/myhellolib-0.0.1-py3-none-any.whl`
    
17. `Archive: dist/myhellolib-0.0.1-py3-none-any.whl`
    
18. `Name`
    
19. `----`
    
20. `myhellolib/__init__.py`
    
21. `myhellolib/myhellolib.py`
    
22. `myhellolib-0.0.1.dist-info/METADATA`
    
23. `myhellolib-0.0.1.dist-info/WHEEL`
    
24. `myhellolib-0.0.1.dist-info/top_level.txt`
    
25. `myhellolib-0.0.1.dist-info/RECORD`
    
26. `-------`
    
27. `6 files`
    


```

<img width="32" height="32" src="../../../_resources/640_wx_fmt_svg_wxfrom_5_wx_lazy__954322ddbb564cf4a.svg"/>

让它可用

现在你知道了打包你的 Python 包是多么容易，你可以使用 Git 钩子、GitLab Web 钩子、Jenkins 或类似的自动化工具来自动完成这个过程。你甚至可以把你的项目上传到 PyPi，这个流行的 Python 模块仓库。一旦它在 PyPi 上，用户就可以用 `pip` 来安装它，就像你在这篇文章中安装 `setuptools` 和 `build` 一样！

当你坐下来开发一个应用或库时，打包并不是你首先想到的事情，但它是编程的一个重要方面。Python 开发者在程序员如何向世界提供他们的工作方面花了很多心思，没有比 `setuptools` 更容易的了。试用它，使用它，并继续用 Python 编码！

* * *

via: https://opensource.com/article/21/11/packaging-python-setuptools

作者：Seth Kenlon 选题：lujun9972 译者：geekpi 校对：wxy

本文由 LCTT 原创编译，Linux中国 荣誉推出

<img width="24" height="24" src="../../../_resources/640_wx_fmt_svg_wxfrom_5_wx_lazy__5cf5149f2b914b749.svg"/>

欢迎遵照 CC-BY-NC-SA 协议规定转载，

如需转载，请在文章下留言 “转载：公众号名称”，

我们将为您添加白名单，授权“转载文章时可以修改”。

[<img width="677" height="113" src="../../../_resources/640_wx_fmt_gif_wxfrom_5_wx_lazy__4a2554e5cd7549b9b.gif"/>](https://mp.weixin.qq.com/s?__biz=MjM5NjQ4MjYwMQ==&mid=2664637017&idx=1&sn=418656faabf31e5a8ba301425bea0db0&scene=21#wechat_redirect)

<a id="js_view_source"></a>Read more

People who liked this content also liked

低代码平台边界探索：多技术栈支持及高低代码混合开发

InfoQ

不看的原因

- 内容质量低
- 不看此公众号

Java 18 给开发者带来哪些加速与新功能

21CTO

不看的原因

- 内容质量低
- 不看此公众号

20 个实例玩转 Java 8 Stream，写的太好了！

快学Java

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___0310a1297e2f47529.bmp"/>

Scan to Follow