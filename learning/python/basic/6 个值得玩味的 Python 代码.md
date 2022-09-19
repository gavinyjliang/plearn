# 6 个值得玩味的 Python 代码

<a id="profileBt"></a><a id="js_name"></a>Python编程时光 *2022-01-04 09:02*

The following article is from Python七号 Author somenzz

<a id="copyright_info"></a>[![](../../../_resources/0_5addcbb601394f5480837db621e06ac4.jpg)<br>**Python七号** .<br>一个玩鲁班七号的程序员，带你玩转 Python。专注于分享 Python 编程和实战，关注后回复【2048】，获取我珍藏的学习资源，交个朋友，一起用 Python 超神。](#)

先选取了 6 个自己认为值得玩味的 python 代码，希望对正在学习 python 的你有所帮助。

**1、类有两个方法，一个是 \_\_new\_\_,一个是 \_\_init\_\_,有什么区别，哪个会先执行呢？**

```
class test(object):
    def __init__(self):
        print("test -> __init__")
    def __new__(cls):
        print("test ->__new__")
        return super().__new__(cls)
a = test()

```

运行结果如下：

```
test ->__new__
test -> __init__

```

再来看另一个例子

```
class test2(object):
    def __init__(self):
        print("test2 -> __init__")
    def __new__(cls):
        print("test2 ->__new__")
        return object()
b = test2()

```

运行结果如下：

```
test2 ->__new__

```

这里给出官方的解释：\_\_init\_\_ 作用是类实例进行初始化，第一个参数为 self，代表对象本身，可以没有返回值。\_\_new\_\_ 则是返回一个新的类的实例，第一个参数是 cls 代表该类本身，必须有返回值。很明显，类先实例化才能产能对象，显然是 \_\_new\_\_ 先执行，然后再 \_\_init\_\_，实际上，只要 \_\_new\_\_ 返回的是类本身的实例，它会自动调用 \_\_init\_\_ 进行初始化。但是有例外，如果 \_\_new\_\_ 返回的是其他类的实例，则它不会调用当前类的 \_\_init\_\_。下面我们分别输出下对象 a 和对象 b 的类型：

```
print( type(a))
#<class '__main__.test'>
print( type(b))
#<class 'object'>

```

可以看出，a 是 test 类的一个对象，而 b 就是 object 的对象。

参考文档：

```
https://docs.python.org/3/reference/datamodel.html?highlight=__new__#object.__new__

```

**2、map 函数返回的对象**

map（）函数第一个参数是 fun，第二个参数是一般是 list，第三个参数可以写 list，也可以不写，作用就是对列表中 list 的每个元素顺序调用函数 fun 。

```
>>> b=map(lambda x:x*x,[1,2,3])
>>> [i for i in b]
[1, 4, 9]
>>> [i for i in b]
[]
>>>

```

有没有发现，第二次输出 b 中的元素时，发现变成空了。原因是 map() 函数返回的是一个迭代器，并用对返回结果使用了 yield，这样做的目的在于节省内存。
举个例子：

```
#encoding:UTF-8  
def yield_test(n):  
    for i in range(n):  
        yield call(i)  
    #做一些其它的事情      
def call(i):  
    return i*2  
#使用for循环  
x = yield_test(5)
print([i for i in x])
print([i for i in x])

```

执行结果为：

```
 [0, 2, 4, 6, 8]
 []

```

这里如果不用 yield，那么在列表中的元素非常大时，将会全部装入内存，这是非常浪费内存的，同时也会降低效率。

**3、正则表达式中 compile 是否多此一举？**

比如现在有个需求，对于文本

中国

，用正则匹配出标签里面的“中国”，其中 class 的类名是不确定的。有两种方法，代码如下：

```
>>> import re
>>> text = '<div class="nam">中国</div>'
>>> #方法一
...
>>> re.findall('<div class=".*">(.*)</div>',text)
['中国']
>>> #方法二
...
>>> regex='<div class=".*">(.*)</div>'
>>> pattern = re.compile(regex)
>>> re.findall(pattern,text)
['中国']
>>>

```

这里为什么要用 compile 多写两行代码呢？原因是 compile 将正则表达式编译成一个对象，加快速度，并重复使用。

**4、\[\[1,2\],\[3,4\],\[5,6\]\]一行代码展开该列表，得出\[1,2,3,4,5,6\]**

```
>>> [j for i in [[1,2],[3,4],[5,6]] for j in i]
[1, 2, 3, 4, 5, 6]
>>>

```

**5、一行代码将字符串 "->" 插入到 "abcdefg"中每个字符的中间**

```
>>> "->".join("abcdef")
'a->b->c->d->e->f'
>>>

```

这里也建议多使用 os.path.join() 来拼接操作系统的文件路径。

**6、zip 函数**

zip() 函数在运算时，会以一个或多个序列（可迭代对象）做为参数，返回一个元组的列表。同时将这些序列中并排的元素配对。zip() 参数可以接受任何类型的序列，同时也可以有两个以上的参数;当传入参数的长度不同时，zip 能自动以最短序列长度为准进行截取，获得元组。

```
>>> a=[1,2]
>>> b=(3,4)
>>> zip(a,b)
<zip object at 0x000001A20201AA08>
>>> for i in zip(a,b):
...     print(i)
...
(1, 3)
(2, 4)
>>> a="ab"
>>> b="xyz"
>>> for i in zip(a,b):
...     print(i)
...
('a', 'x')
('b', 'y')
>>>

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

[<img width="677" height="130" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__e08e4cae1e3546e8a.png"/>](http://mp.weixin.qq.com/s?__biz=MzIzMzMzOTI3Nw==&mid=2247505072&idx=1&sn=3605fcc95b6c0c7b0b9ec5dd15480231&chksm=e885b452dff23d44649944d41a190d55955a9f8ea6987e1ed73363c10bf3711528f4f8524e8a&scene=21#wechat_redirect)

[<img width="677" height="121" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__fa7a35aaccb34b32b.png"/>](http://mp.weixin.qq.com/s?__biz=MzIzMzMzOTI3Nw==&mid=2247505084&idx=1&sn=d3bc3a37cda2759c1a078ce9811fdec2&chksm=e885b45edff23d48744d42d9f6658cdddd2164af7042544815851a851dc710c70a1527a9b686&scene=21#wechat_redirect)

[<img width="677" height="122" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__428acd645f184abda.png"/>](http://mp.weixin.qq.com/s?__biz=MzIzMzMzOTI3Nw==&mid=2247505065&idx=1&sn=a8b98840693b8d57bc5422da6d68a25d&chksm=e885b44bdff23d5d93cd686803d091d4b280c6f8a33afcf9e38c3f92746a1c222d1b40a8ad9a&scene=21#wechat_redirect)

People who liked this content also liked

低代码平台边界探索：多技术栈支持及高低代码混合开发

InfoQ

不看的原因

- 内容质量低
- 不看此公众号

20 个实例玩转 Java 8 Stream，写的太好了！

快学Java

不看的原因

- 内容质量低
- 不看此公众号

老板：你来弄一个团队代码规范！？

程序员黑叔

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___a768832a0b0346b08.bmp"/>

Scan to Follow