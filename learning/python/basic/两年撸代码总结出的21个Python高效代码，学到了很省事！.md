# 两年撸代码总结出的21个Python高效代码，学到了很省事！

<a id="profileBt"></a><a id="js_name"></a>Python编程大全 *2022-01-24 17:30*

点击下方名片关注和星标『**Python编程大全**』！

![](../../../_resources/0_wx_fmt_png_0084f482de0c4d5e8e209edb96816e6a.png)

**Python编程大全**

分享Python技术文章，实用案例，热点资讯。 你想了解的Python的那些事都在这里...... 当你的才华还撑不起你的野心的时候，那就安静下来学习吧！

<a id="js_profile_article"></a>6篇原创内容

Official Account

python编程中的一些小技巧，运用的恰当，会让你的程序事半功倍~

下面这些代码看似平平无奇，但学到了可以帮助大家解决在日常的python编程中遇到的大多数问题，快来Look Look！

1、检查文件是否存在

在程序运行中，会遇到保存一些图片、文字的情况，这个时候就需要利用程序来判断某个文件或者文件夹是否存在。

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__00b65682ed1c43c69.png)

2、简易计算器制作

下图的程序中，不需要if-else的操作，即可制作一个简易的计算器。

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__73bb49a60b1443eaa.png)

3、同时获取索引和数值

在进行数值的迭代时，可以利用enumerate的内置函数来获取可迭代对象数值的同时，得到数值的索引，并利用索引对数值进行操作。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

4、一次性进行多个数值的输入

对于数值的输入问题，是很多笔试题目中经常遇到的问题，一次性输入多个参数值 ，可以节省时间和代码量，为后面的程序编写节省时间。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

5、格式化字符串

对于Python的输入，逻辑和输出，这三个部分在编写代码时都需要某种格式，Python提供了多种格式化字符串的方法，以便获得更好和易于阅读的输出。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

> 字符串类型格式化采用format()方法：
> 调用format()方法后会返回一个新的字符串，参数从0 开始编号。
> format()方法可以非常方便地连接不同类型的变量或内容，如果需要输出大括号，采用：
> {{表示{，}}表示}

format()的优点有：

- 1 .格式化时不用关心数据类型的问题，format()会自动转换，而在%方法中，%s用来格式化字符串类型，%d用来格式化整型;
    
- 2\. 单个参数可以多次输出，参数顺序可以不同
    
- 3\. 填充方式灵活，对齐方式强大
    

6、对象内存占用量

通过下图的程序，可以进行对象的内存占用量查询；

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

7、检查是否有重复元素

对于检查列表中是否有重复的元素，可以通过将列表转换为set来快速检查。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

8、检查列表、字符串是否有相同的元素

不同的字符串，可以有相同的字母组成，同样，列表也可以有相同的元素组成，通过下述的程序，可以判断不同字符串或者是列表是否有相同的元素。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

9、字符串列表的排序

当大家需要对一个字符串列表进行排序时，可以利用下图中的程序进行排序。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

10、利用if和else对列表进行处理

利用if和else的操作，可以基于某些条件过滤数据，如下图所示。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

11、对列表元素进行操作

通过Python语言的内联for循环的方式，实现对于列表中的所有元素的操作。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

12、合并两个列表

对于两个列表的合并，可以通过花式的列表合并来将两个列表组合成一个新的列表。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

13、合并字典

当处理json数据或者是数据库中的内容时，会用到字典的合并，有时候还会遇到具有相同键值的字典，可以通过下图程序中的两种方法进行解决。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

**列表、字典、集合这三个可变数据类型新手需要注意区分它们几个的特点和用法，比如list 的各个元素可以改变，set 不能嵌套，不能存可以改变的数据类型，set 和 dict 的唯一区别仅在于没有存储对应的 value；**

14、将两个列表转换为字典

将两个列表转换为字典，常见的情况是一个列表作为键，另一个列表作为值来构造字典。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

15、对字典列表进行排序

当有字典组成的列表时，可以按照字典的键值对列表进行排序。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

16、列表元素频率统计

对于列表等可迭代对象中的元素进行频次的统计，也是一项非常常见的问题。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

17、两个数值交换

Python中的交换，不仅仅可以直接通过a,b = b,a的方式进行数值的交换，而且还可以进行列表等可迭代对象的交换。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

18、错误捕捉

在Python语言中，提供了使用try，except和finally块处理异常报错的方法。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

19、链式函数调用

通过一行程序，可以调用多个不同的函数，进行计算。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

20、计算程序执行的时间

对于程序计算时间的计算，可以帮助大家对于程序或者算法的性能有更好的了解。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

21、其他函数

1）abs(number)，返回数字的绝对值

2）cmath.sqrt(number)，返回平方根，也可以应用于负数

3）float(object)，把字符串和数字转换为浮点数

4）input(prompt)，获取用户输入

5）int(object)，把字符串和数字转换为整数

6）math.ceil(number)，返回数的上入整数，返回值的类型为浮点数

7）math.floor(number)，返回数的下舍整数，返回值的类型为浮点数

8）math.sqrt(number)，返回平方根不适用于负数

9）pow(x,y\[.z\]),返回X的y次幂（有z则对z取模）

10）repr(object)，返回值的字符串标示形式

11）round(number\[.ndigits\])，根据给定的精度对数字进行四舍五入

12）str(object),把值转换为字符串

这些代码和函数还是非常实用的，大家在日常的编程中多多使用，多多练习，尤其是对菜鸟来说，多练习一下对功力提升大有裨益！读百遍，看千遍，不如自己动手敲一遍。

*关注： Python编程大全 ，加星标精彩内容不迷路*

```


![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

如果喜欢我的文章，那么

“在看”和转发是对我最大的支持！

```


![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

 （戳下面蓝字阅读）

- [Python 的十大特性](http://mp.weixin.qq.com/s?__biz=Mzg5NzcxMzA0NA==&mid=2247483803&idx=1&sn=7c7a6b9bdd5d4506d49d583dc7b9a52e&chksm=c06cd760f71b5e7690e1ee8092c59a24609c71fdaee13789edfac2e1f29c9cbb46c9d16b6d35&scene=21#wechat_redirect)
    
- [30 个 Python 代码实现的常用功能](http://mp.weixin.qq.com/s?__biz=Mzg5NzcxMzA0NA==&mid=2247484085&idx=1&sn=b00838903213cb1dc72d3bc01e27b113&chksm=c06cd44ef71b5d582710d50abf1f4cb5b28962b5696a555f544ec8ea5510b541adacf58ed31b&scene=21#wechat_redirect)
    
- [终于把所有的Python库，都整理出来啦！](http://mp.weixin.qq.com/s?__biz=Mzg5NzcxMzA0NA==&mid=2247484001&idx=1&sn=9be186c81f09819c519f9e5a3cdf8666&chksm=c06cd49af71b5d8cf87a165fa0e615538a60fd487b2587fcb4ee6f3f8131f09634940f1f0b41&scene=21#wechat_redirect)
    
- [Python 之父：别等了，Python 4.0 可能不会来了](http://mp.weixin.qq.com/s?__biz=Mzg5NzcxMzA0NA==&mid=2247483802&idx=1&sn=3a20576308fbde9db169c2470de42848&chksm=c06cd761f71b5e77f8856cc1e4aae8c11c375fada50a5b6dca7566175391147b7f12f6910596&scene=21#wechat_redirect)
    
- [Python中使用构造方法创建对象](http://mp.weixin.qq.com/s?__biz=Mzg5NzcxMzA0NA==&mid=2247483871&idx=1&sn=85cacff5a50fd77aa71be4fa8544fd1f&chksm=c06cd724f71b5e321e503fa1d69ce53f53afe0bd2c3b84ba52e98f79504bdea479c7ce9bc4a5&scene=21#wechat_redirect)
    
- [一些著名的软件都用什么语言编写？](http://mp.weixin.qq.com/s?__biz=Mzg5NzcxMzA0NA==&mid=2247483934&idx=1&sn=1a67d7e67521bd719dde09d9e02bc068&chksm=c06cd4e5f71b5df3857ede19be1627c5e3176be13224cd03ab9d30868c86040a1f066aa45978&scene=21#wechat_redirect)
    
- [Python超车，C#错失年度编程语言](http://mp.weixin.qq.com/s?__biz=Mzg5NzcxMzA0NA==&mid=2247483887&idx=1&sn=c9c39d8a239f405017ba8eac1665fc9b&chksm=c06cd714f71b5e0233062e2a1722dd95a49cf3f4da9bec3b96d0bbe07e50a1b8096b2dd3f077&scene=21#wechat_redirect)
    
- [Python再获年度编程语言，微软或成最大赢家](http://mp.weixin.qq.com/s?__biz=Mzg5NzcxMzA0NA==&mid=2247483886&idx=1&sn=b5fe0bde24ae244dfc90ef6b68e096cb&chksm=c06cd715f71b5e03f7109a13cc306f060e8837bb84aa4e8f4c92aed042508affe4443cfe02d9&scene=21#wechat_redirect)
    
- [Python 应该怎么学？](http://mp.weixin.qq.com/s?__biz=Mzg5NzcxMzA0NA==&mid=2247484170&idx=1&sn=4b3cc59a6c12b5f0c6de3778d44e94a1&chksm=c06cd5f1f71b5ce76e2845a08526d03df025b92f3e25d5933f881add7cabb32b218926ea4ae9&scene=21#wechat_redirect)
    

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

Python编程大全

进群学习交流加 : mm1552923

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)觉得不错，请点个在看呀














```


```

```


![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

分享














```

People who liked this content also liked

可能是最强的Python可视化神器，建议一试！

Python编程大全

不看的原因

- 内容质量低
- 不看此公众号

Fortran基础编程（入门简介篇）

易木木响叮当

不看的原因

- 内容质量低
- 不看此公众号

15 个让新手爱不释手的 Python 高级库

Python编程大全

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___e5b251c9de95425a8.bmp"/>

Scan to Follow