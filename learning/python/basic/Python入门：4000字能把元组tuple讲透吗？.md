# Python入门：4000字能把元组tuple讲透吗？

<a id="copyright_logo"></a>Original Peter <a id="profileBt"></a><a id="js_name"></a>尤而小屋 *2021-08-21 15:29*

收录于话题

<a id="js_article_tag_name__1999223975448477698"></a>#python <a id="js_article_tag_num__1999223975448477698"></a>57 <a id="js_article_tag_tips__1999223975448477698"></a>个

<a id="js_article_tag_name__2001714341183553538"></a>#Python入门 <a id="js_article_tag_num__2001714341183553538"></a>16 <a id="js_article_tag_tips__2001714341183553538"></a>个

<a id="js_article_tag_name__1999223975666581509"></a>#数据分析 <a id="js_article_tag_num__1999223975666581509"></a>104 <a id="js_article_tag_tips__1999223975666581509"></a>个

<a id="js_article_tag_name__1999223977696624640"></a>#机器学习 <a id="js_article_tag_num__1999223977696624640"></a>72 <a id="js_article_tag_tips__1999223977696624640"></a>个

![](../../../_resources/0_wx_fmt_png_58a9dbfaf0d742f4b85ffa90529525b0.png)

**尤而小屋**

尤而小屋，一个温馨且有爱的小屋🏡 小屋主人，一手代码谋求生存，一手掌勺享受生活，欢迎你的光临~

<a id="js_profile_article"></a>216篇原创内容

Official Account

> 公众号：尤而小屋
> 作者：Peter
> 编辑：Peter

大家好，我是Peter

在前面介绍的python数据类型：列表list，我们发现list是可以进行修改的。但是有时候，我们需要创建一系列不可修改的元素，此时Python中另一种有序的数据类型-元组tuple就可以满足这种需求。

<img width="657" height="439" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_1f08b2feed4841b5a.jpg"/>

本篇文章在jupyter notebook中的整体布局：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 一、如何写文章

最近有朋友问过我：**Peter，你是如何写一篇公众号的文章**？

今天列了个提纲，主要是从4个方面来展开，以后我会专门写一篇文章来回答这个问题：

- 写作前
    
- 写作中
    
- 写作后
    
- 发布文章
    

> 时间挤挤都是有的；有时候熬夜，周末都在写！
> 
> 互联网，卷！

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 二、元组创建

- 元组在Python中使用圆括号`()`括起来的，列表使用方括号`[]`括起来的
    
- 元组里面的元素是通过**逗号**来隔开的
    
- 元组中的元素可以是任意的python数据类型
    
- 元组是序列，和列表一样，但是元组中的元素是不能直接更改的
    

### 2.1创建空元组

```
`a = ()
`
```

```
`a
`
```

下面的结果表示创建了一个空的元组：

```
()

```

```
`type(a)
`
```

```
tuple

```

### 2.2单个元素

```
`b = (3,)  # 数值型
b
`
```

```
(3,)

```

```
`type(b)  # 查看数据类型为：元组
`
```

```
tuple

```

```
`c = ("python",)  # 字符型
c
`
```

```
('python',)

```

```
`type(c)
`
```

```
tuple

```

```
`d = (["python","java",3],)  # 只有一个元素，并且是列表
d
`
```

```
(['python', 'java', 3],)

```

```
`type(d)
`
```

```
tuple

```

需要注意的是：当元组中只有一个元素的时候，后面一定要带上逗号，否则python不会认为是元组的：

```
`e = (3)  # 没有带上逗号，系统默认是数值型
e
`
```

```
3

```

```
`type(e)  
`
```

```
int

```

```
`f = (["python","java"])  # 只有一个元素列表
f
`
```

```
['python', 'java']

```

```
`type(f)   # 系统默认为：列表
`
```

```
list

```

上面的多个例子都表明：如果创建只有一个元素（任意python类型）的元组，最后一定得带上括号

### 2.3多个元素

通过多个元素组成的元组，元素可以是不同的数据类型

```
`t1 = (1,2,3)   # 全部是数值型 
t1
`
```

```
(1, 2, 3)

```

```
`type(t1)
`
```

```
tuple

```

```
`t2 = (1,"pyton","java")  # 数值型+字符串
t2
`
```

```
(1, 'pyton', 'java')

```

```
`type(t2)
`
```

```
tuple

```

```
`t3 = (1,[1,2,3],"python")    # 数值型+列表+字符串
t3
`
```

```
(1, [1, 2, 3], 'python')

```

```
`type(t3)
`
```

```
tuple

```

看看下面这个神奇的例子：当我们给变量t4赋值的时候，后面有3个值；

通过运行结果可以看到，Python把它们当成了一个整体，放到了一个元组中

```
`t4 = 100,"python","hello"
t4
`
```

```
(100, 'python', 'hello')

```

```
`type(t4)  # 数据类型是元组
`
```

```
tuple

```

元组中的元素还可以是元组类型

```
`t5 = (1,2,(3,4,5),"python")  # 其中(3,4,5)部分就是元组
t5
`
```

```
(1, 2, (3, 4, 5), 'python')

```

```
`type(t5)
`
```

```
tuple

```

### 2.4通过tuple函数创建

在使用tuple创建的过程中，要将元素先用小括号括起来

```
`tuple1 = tuple((1,3,5,7))  # 两层括号
tuple1
`
```

```
(1, 3, 5, 7)

```

```
`type(tuple1)
`
```

```
tuple

```

如果只使用一层，则会报错；tuple方法只接受一个参数：

```
`tuple(1,3,5,7)    
`
```

```
---------------------------------------------------------------------------
TypeError                                 Traceback (most recent call last)
<ipython-input-26-b16145832f5a> in <module>
----> 1 tuple(1,3,5,7)
TypeError: tuple expected at most 1 arguments, got 4

```

```
`tuple2 = tuple((1,"c",5,"python"))
tuple2
`
```

```
`tuple3 = tuple(([1,2,3],3,"python",7))
tuple3
`
```

### 2.5zip函数创建

zip是Python中的一个高阶函数，后面会专门介绍zip的使用；我们也可以通过它来创建元组

```
`name = ["小明","小红","小周"]
age = [20,28,19]
zip(name,age)  # 生成一个zip对象
`
```

```
<zip at 0x107d34320>

```

```
`list(zip(name,age)) # 1、对象转成列表，列表中的元素是一个个的元组
`
```

```
[('小明', 20), ('小红', 28), ('小周', 19)]

```

```
`tuple(zip(name,age))   # 2、转成元组中嵌套元组
`
```

```
(('小明', 20), ('小红', 28), ('小周', 19))

```

```
`dict(zip(name,age))  # 3、还可以转成字典的形式（后续介绍字典，也是Python的一种数据类型）
`
```

```
{'小明': 20, '小红': 28, '小周': 19}

```

## 三、元组基本操作

### 3.1求长度

```
`t6 = (0,1,2,3,4,5,6,7,8)   # 纯数值型
t6
`
```

```
(0, 1, 2, 3, 4, 5, 6, 7, 8)

```

```
`t7 = ("python","java","c")   # 字符类型 
`
```

```
`type(t7)
`
```

```
tuple

```

```
`len(t6)
`
```

```
9

```

```
`len(t7)
`
```

```
3

```

### 3.2重复元组元素

```
`t7 * 3  # 复制3倍
`
```

```
('python', 'java', 'c', 'python', 'java', 'c', 'python', 'java', 'c')

```

### 3.3多个元组相加

```
`t6 + t7
`
```

```
(0, 1, 2, 3, 4, 5, 6, 7, 8, 'python', 'java', 'c')

```

### 3.4查看最值

是通过元组中元素的ASCII码来区分大小的

```
`max(t6)
`
```

```
8

```

```
`min(t7)
`
```

```
'c'

```

```
`t5
`
```

```
(1, 2, (3, 4, 5), 'python')

```

此时需要注意的是：使用max或者min的时候，元组中元素的数据类型必须一致，否则无法进行比较，则会报错：

```
`max(t5)
`
```

```
---------------------------------------------------------------------------
TypeError                                 Traceback (most recent call last)
<ipython-input-41-f6f6f0f7436f> in <module>
----> 1 max(t5)
TypeError: '>' not supported between instances of 'tuple' and 'int'

```

### 3.5成员判断in

```
`1 in t6
`
```

```
True

```

```
`10 in t6
`
```

```
False

```

### 3.6遍历元组

```
`for i in t6:  # 遍历元组中的元素进行打印
    print(i)
`
```

```
0
1
2
3
4
5
6
7
8

```

## 四、修改元组？

### 4.1不能直接修改

在开头我们已经提到过：元组中的元素不能直接进行修改的

```
`t7
`
```

```
('python', 'java', 'c')

```

```
`t7[1]
`
```

```
'java'

```

当我们想把其中的java改成JavaScript的时候，就会出现报错，因为元组本身就是不能修改的：

```
`t7[1] = "JavaScript"     
`
```

```
---------------------------------------------------------------------------
TypeError                                 Traceback (most recent call last)
<ipython-input-47-7885d09b5b02> in <module>
----> 1 t7[1] = "JavaScript"
TypeError: 'tuple' object does not support item assignment

```

### 4.2转成列表进行修改

因为列表是可以进行修改的，我们可以先将元组转成列表，然后修改对应列表中的元素，最后再转回去

```
`t7
`
```

```
('python', 'java', 'c')

```

```
`t7_1 = list(t7)  # 1、转成列表
t7_1
`
```

```
['python', 'java', 'c']

```

```
`t7_1[1] = "javascript" # 2、修改元素
t7_1
`
```

```
['python', 'javascript', 'c']

```

```
`t7_2 = tuple(t7_1)  # 3、再转成元组
t7_2
`
```

```
('python', 'javascript', 'c')

```

## 五、索引和切片

元组和列表一样，都是python中一种有序的数据类型，也是存在使用和切片的概念

### 5.1使用索引

使用索引号来访问元组元素

```
`t6.index(0)  # 元素0的索引号
`
```

```
0

```

```
`t6.index(6)  # 元素6的索引号
`
```

```
6

```

```
`t6[8]  # 正索引号
`
```

```
8

```

```
`t6[-4]  # 负索引号
`
```

```
5

```

```
`t7.index("java")   # 查看元素的使用号
`
```

```
1

```

```
`t7[1]   # 通过索引号查看元素
`
```

```
'java'

```

### 5.2使用切片

元组中切片使用规则和列表是完全一模一样的，可以参考列表的文章来进行学习。

下面仅仅是展示在元组中使用切片后的案例学习：

```
`t6[:7]
`
```

```
(0, 1, 2, 3, 4, 5, 6)

```

```
`t6[1:8:2]
`
```

```
(1, 3, 5, 7)

```

```
`t6[:8:2]   # 从头开始，步长为2
`
```

```
(0, 2, 4, 6)

```

从最后一个元素（索引号-1），到索引号-8（不包含），步长为-2

```
`t6[-1:-8:-2]   
`
```

```
(8, 6, 4, 2)

```

```
`t6[-2:-7:-2]
`
```

```
(7, 5, 3)

```

```
`t6[1:8:3]
`
```

```
(1, 4, 7)

```

## 六、元组和列表比较

### 6.1相同点

1.  都是Python中的有序数据类型
    
2.  都存在很多相同的操作方法：求长度、最值、成员判断、索引和切片等
    

### 6.2不同点

1.  列表可直接修改，元组不行；我们可以将元组转成列表之后，再间接地进行修改元素
    
2.  元组比列表快。如果我们定义了一个变量，需要不断地去遍历它，使用元组会更快
    

<img width="17" height="36" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__56149e0f1f5e456ea.png"/>

推荐阅读

<img width="17" height="36" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__f6678fd77ba94715b.png"/>

[Python入门：55个案例讲透列表的索引和切片！](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247493373&idx=1&sn=2875106912d287a8b6b3526d2106b3d7&chksm=cf12f627f8657f312b84f886f5396be6a6550fd2ba3978c07fe32471628f9c5fbb23e628ebac&scene=21#wechat_redirect)

[七夕节：介绍3个超实用的Pandas宝藏函数](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247493247&idx=1&sn=60731f01e57abe5d9c9d9f0a8521029e&chksm=cf12f6a5f8657fb3b11dc634389f139d3c310ca88576131a04a4183c3932657ae7c4ec57d375&scene=21#wechat_redirect)

[Python入门-列表初相识](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247493104&idx=1&sn=9eeee899fbdfd4376b875f2f2bf79e62&chksm=cf12f52af8657c3c6eaa4cd0a0a1e8f9dcd2252f5af309eec93d925b5458f43c69e721dd1244&scene=21#wechat_redirect)

[数据分析师=7大主题，24份资料](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247493099&idx=1&sn=b5c1885a38e1de34a79fa5ff581bfaa7&chksm=cf12f531f8657c278f814c8363ca3551616ada416d8edf317dae05b618fda4cc7bdbad3585ae&scene=21#wechat_redirect)

[55个案例：吃透Python字符串格式化](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247493094&idx=1&sn=e62f809024edad67a0bdfefa70405c5e&chksm=cf12f53cf8657c2a0eef022f32ea263b050d2e58ee3f170bcd1ac033c87d6c3f930c78ca95bd&scene=21#wechat_redirect)

> 尤而小屋，一个温馨的小屋。小屋主人，一手代码谋求生存，一手掌勺享受生活，欢迎你的光临

![](../../../_resources/0_wx_fmt_png_58a9dbfaf0d742f4b85ffa90529525b0.png)

**尤而小屋**

尤而小屋，一个温馨且有爱的小屋🏡 小屋主人，一手代码谋求生存，一手掌勺享受生活，欢迎你的光临~

<a id="js_profile_article"></a>216篇原创内容

Official Account

收录于话题 #<a id="js_album_keep_read_title"></a>python

 <a id="js_album_keep_read_size"></a>57个

<a id="js_album_prev"></a>上一篇 <a id="js_album_keep_read_pre_title"></a>14种方式，34个案例：对比SQL，学习Pandas操作 <a id="js_album_next"></a>下一篇 <a id="js_album_keep_read_next_title"></a>七夕节：介绍3个超实用的Pandas宝藏函数

People who liked this content also liked

Python关联规则挖掘情侣、基友、渣男和狗

尤而小屋

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___231319cac8a243e28.bmp"/>

Scan to Follow