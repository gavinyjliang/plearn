# 10个必会的 PyCharm 技巧

刘善国 <a id="profileBt"></a><a id="js_name"></a>SQL数据库开发 *2022-04-29 08:30* *Posted on <a id="js_ip_wording"></a>广东*

![](../../../_resources/0_wx_fmt_png_dddc20023a9c4ca6be0dc317c93c039d.png)

**SQL数据库开发**

专注数据相关领域，主要分享MySQL，数据分析，Python，Linux ，大数据等相关技术内容，关注回复「1024」获取资源大礼包。

<a id="js_profile_article"></a>426篇原创内容

Official Account

来源：麻瓜编程（easypython）

**#**** 0\. PyCharm 常用快捷键**

<img width="677" height="369" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__a108a8b87c674964b.jpg"/>

<img width="677" height="369" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__4eb9730af1104c6ba.jpg"/>

**#**** 1. 查看使用库源码**

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

PyCharm 主程序员在 Stackoverflow 上答道

经常听人说，多看源码。源码不仅能帮我们搞清楚运行机制，还能学习优秀的库或者框架的最佳实践。

调用库时，你可以在你好奇的几乎任何地方点击 Command+B，就可以很方便的跳转到源码里的类，方法，函数，变量的定义。

操作如下：

****#**** 2\. 让你的代码 PEP8****

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

写 Python 代码时，你会严格遵守 pep8 规范么？还是要遵守的，不然代码传到 github 或者知乎上被人怼就不好了。但是如果靠肉眼去检查和注意的话，太累，靠 PyCharm 来做这事就好，Command+Option+L，一键 pep8 走起。

********#**** 3\. 新手不再愁安装库********

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

如果你是新手，可能会为了安装库而感到烦恼，在 PyCharm 里面可以使用你熟悉的图形化界面来安装库，就不用陷在一堆命令行里了。

操作如下：

********#**** 4\. 查找文件、类、方法********

当我们需要在项目中寻找一个文件名的时候，输入 Command + Shift + O，然后输入你想查找的文件名就可以了。如果你不记得全名了，只需要输入首字母，Pycharm 就会提示你。比如我想查找一个叫 test_errors.py 的文件，那么只需要输入 tee 就可以找到。又或者查询 test\_errors\_1.py 那么只需要输入 tee1 就可以查找到。

******# 5\. 快速选择代码块******

你会怎么快速注释一段 Python 代码块？不会是一行一行的加#吧……

在需要选择某个函数的时候，只需要把光标放在最前面，然后点击 Command + Option + Shift + \[ ，就可以选择对当前代码块, 使用 Command + / 注释。

**# 6\. 快速插入常用代码**

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

有时候需要输入很长的代码，比如 if \_\_name\_\_ == '\_\_main\_\_': ，这时候手动输入不如直接 Command + J ，就可以直接插入常用代码了。

**# 7\. 运行/调试代码**

运行代码、调试代码应该是大多数人最常用的快捷键吧。

Mac：

Control + r：直接运行当前代码

Control + d：以Debug(调试)模式运行代码

Windows/Linux：

Shift + F10：直接运行当前代码

Shift + F9：以Debug(调试)模式运行代码

**# 8\. 缩进你的代码块**

在写前端页面的时候，经常要更改一大段代码的缩进，这时候可以先用 Shift + 上下键 来选择你要缩进的代码块，然后使用 Tab 就能缩进啦。

**# 9\. 展开/收缩代码**

当项目写到一定规模的时候，难免方法/函数会很多，这个时候我们可以使用Command + Shift + -符号 来收缩代码，这个主要是为了方便查看。

**# 10\. 展示多个页面**

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

当你的公司不愿意为你配置2个显示屏时，你依然可以使用 PyCharm 在一个屏幕里查看多个文件。鼠标放到当前导航处的文件名，然后右键 Split Vertically 或者 Split Horizontally 就可以啦。

操作如下

```



```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg== "音符")

最后给大家分享我写的SQL两件套：**《SQL基础知识第二版》**和**《SQL高级知识第二版》**的PDF电子版。里面有各个语法的解释、大量的实例讲解和批注等等，非常通俗易懂，方便大家跟着一起来实操。

有需要的读者可以下载学习，在下面的公众号「**数据前线**」(非本号)后台回复关键字：**SQL**，就行

**数据前线**

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

**——End——**

#### 

```


后台回复关键字：1024，获取一份精心整理的技术干货

后台回复关键字：进群，带你进入高手如云的交流群。

```


```


推荐阅读

- [一千行 MySQL 学习笔记](http://mp.weixin.qq.com/s?__biz=MzA3MTg4NjY4Mw==&mid=2457326778&idx=2&sn=955e62a5136fc2b31c31bde258084982&chksm=88a5c68ebfd24f986416e16c41afd82fb88fa5c486812f93e8f9f4489ec6c5f0d29ea5277448&scene=21#wechat_redirect)
    
- [SQL自定义排序](http://mp.weixin.qq.com/s?__biz=MzA3MTg4NjY4Mw==&mid=2457326649&idx=2&sn=f84c02633c15cbac9527ec85818b8266&chksm=88a5c60dbfd24f1b2d4df7f3be8486def1f3a63f978f30bc1691d5fad58dbb05678aa1f43869&scene=21#wechat_redirect)
    
- [SQL 性能优化梳理](http://mp.weixin.qq.com/s?__biz=MzA3MTg4NjY4Mw==&mid=2457326564&idx=2&sn=1564e56ff3d2f8fbf000146d774a870b&chksm=88a5c1d0bfd248c6f19bb59f238f17ef712c9f1dcaa248d90e515c239c82548009f326fd4ada&scene=21#wechat_redirect)
    
- [一行SQL代码能做什么？](http://mp.weixin.qq.com/s?__biz=MzA3MTg4NjY4Mw==&mid=2457326590&idx=1&sn=edff5602c22c607663dd8e09db13b3d6&chksm=88a5c1cabfd248dc268cfd7c268181b8ea594d45bd1891fc365934fc5a9cc4c60313842288a9&scene=21#wechat_redirect)
    
- [安利几款我常用的数据软件](http://mp.weixin.qq.com/s?__biz=MzA3MTg4NjY4Mw==&mid=2457326486&idx=1&sn=d44e8bf6286230996c26ac377196362d&chksm=88a5c1a2bfd248b4820a1e490827519a139047cabae2b0f4329ee6c33ec4cc6071bccd2bae88&scene=21#wechat_redirect)![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg== "分享点赞在看.gif")
    


```


```


```

People who liked this content also liked

MySQL8.0 InnoDB并行查询特性

...

yangyidba

不看的原因

- 内容质量低
- 不看此公众号

Python之父狠话未兑现？CPython 3.11仅提速25%

...

OSC开源社区

不看的原因

- 内容质量低
- 不看此公众号

Python中最简单易用的并行加速技巧

...

Python大数据分析

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___63f196e00b3d47a79.bmp"/>

Scan to Follow