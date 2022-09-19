# Python入门：3000字深入剖析变量和赋值

<a id="copyright_logo"></a>Original <a id="js_author_name"></a>尤而小屋 <a id="profileBt"></a><a id="js_name"></a>尤而小屋 *2021-10-04 09:40*

收录于话题

<a id="js_article_tag_name__1999223975448477698"></a>#python <a id="js_article_tag_num__1999223975448477698"></a>57 <a id="js_article_tag_tips__1999223975448477698"></a>个

<a id="js_article_tag_name__2001714341183553538"></a>#Python入门 <a id="js_article_tag_num__2001714341183553538"></a>16 <a id="js_article_tag_tips__2001714341183553538"></a>个

> 公众号：尤而小屋
> 作者：Peter
> 编辑：Peter

大家好，我是Peter~

今天给大家带来的是一篇关于**Python变量与赋值**的文章。

其实Python中的赋值语句我们在之前的学习过程已经接触了很多，比如a=1，就是将数值1赋值给变量a。

在正式介绍赋值语句之前，我们先了解下Python中的变量问题。

![Image](../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_d042edeb8df94984b.jpg)

## 环境

有读者反映建议Peter写下文章的环境，这就来了：

- 系统：MacOS
    
- 工具：jupyter notebook
    
- Python版本：3.7.5
    
- 文档编辑器：Typora
    

## 变量

### 深刻理解变量的内存地址

变量可以说是一个存放数据的容器。Python中在定义变量的时候，不需要声明变量。当我们首次为变量赋值的时候，会自动创建变量并指定类型。

> 变量本身是没有类型的，只是对象（赋值的数据）有类型

```
`a = 66
a
`
```

```
66

```

```
`b = 66
b
`
```

```
66

```

```
`type(a)  # 查看数值类型为整型int
`
```

```
int

```

```
`type(b)  # 字符串类型
`
```

```
int

```

我们定义了两个变量a和b，它们都是数字66。虽然名称不同，但是在计算机中它们却代表的是同一个元素，看看他们的内存地址。

就好比：猪八戒（数值66）这个人，我们可以称之为“二师兄”（放在变量a），也可以称之为“天蓬元帅”（放在变量b），但是本质上他们都是指猪八戒，只是换了个别名，**本质相同**

```
`id(a)  # a和b的内存地址相同
`
```

```
4387310752

```

```
`id(b)
`
```

```
4387310752

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

我们再定义一个变量的赋值看看：

```
`a = 77
a
`
```

```
77

```

```
`id(a)
`
```

```
4387311104

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

我们看到，之前我们把数值66赋值给变量a，内存地址是4430785696，然后我们又把数值77赋值为变量a，内存地址变成了4430786048。

为什么?其实，这两数值66和77本质上在计算机就是两个对象，只不过它们刚好有个相同的名字而已。好比说，西游记有个情节：**真假美猴王**

真美猴王（数值66）和假美猴王（数值77）虽然都被称为猴子（标签a），但是他们实际上是两只不同的猴子呀（分配了不同的地址）~假的最后不还是被如来给收服啦！

### 创建变量

通过赋值语句来实现变量的创建

```
`x = 99  # 数值型
language = "python"  # 字符串型
number = [1,3,5,7,9]  # 列表型
print(x)
print(language)
print(number)
`
```

```
99
python
[1, 3, 5, 7, 9]

```

### 变量名称命名规则

python中变量的命名可以使用短名称，比如上面的x、y、z、a、b等，也可以使用具有一定描述作用的名称，比如age、name、sex，其他人看到就可以知道变量的含义。通常Python中的变量命名规则：

- 变量名必须以字母或下划线字符开头，不能以数字开头
    
- 变量名只能包含字母数字字符和下划线（A-z、0-9 和 _）
    
- 变量名称区分大小写（name、Name 和 NAME 就是三个不同的变量）
    
- 变量名不能和Python中的关键字冲突（相同），否则无效
    

下面我们看看Python中常见的赋值方法

## 赋值语句

### 常规赋值

赋值：将Python的某个数据对象贴在某个变量上，好像给这个对象贴上了一个标签。Python 使用等号=作为赋值运算符，具体格式为：

```
`name = value
# 变量 = 某个值
`
```

```
`list1 = ["python","java"]  # 列表赋值给变量b
list1
`
```

```
['python', 'java']

```

```
`list2 = [1,2,["python","html"],(1,4,7)]  # 嵌套列表
list2
`
```

```
[1, 2, ['python', 'html'], (1, 4, 7)]

```

```
`age = 28  # 数值
age
`
```

```
28

```

```
`information = "xiaoming is a boy"  # 字符串
information
`
```

```
'xiaoming is a boy'

```

```
`# 定义一个变量dic，字典类型
dic = {"name":"xiaoming","age":20,"sex":"fale"}
dic
`
```

```
{'name': 'xiaoming', 'age': 20, 'sex': 'fale'}

```

### 多变量赋值

同时赋值3个变量

```
`m, n, o= 22, "xiaoming","男"  # 同时赋值3个变量
`
```

```
`m
`
```

```
22

```

```
`n
`
```

```
'xiaoming'

```

```
`o
`
```

```
'男'

```

上面的例子表示22赋值给m，字符串对象"xiaoming"赋值给n，"男"赋值给变量o

```
`name, age = ("Peter",20)  # 通过元组形式赋值
`
```

```
`name
`
```

```
'Peter'

```

```
`age
`
```

```
20

```

上面的例子通过Python元组的形式进行了链式赋值

### 链式赋值

```
`x1 = y1 = 33
`
```

```
`x1
`
```

```
33

```

```
`y1
`
```

```
33

```

在上面的例子中我们通过链式赋值同时定义了两个变量x1和y1。其实它们在内存中就是同一个对象，通过id查看内存地址：

```
`id(x1)
`
```

```
4387309696

```

```
`id(y1)
`
```

```
4387309696

```

其实就是同一个python对象贴上了不同的标签而已，但是本质相同

### 变量互换

```
`k, j = 9, 5
`
```

上面的变量赋值等价于：

```
`k=9
j=5
`
```

```
`print("k =",k)
print("j =",j)
`
```

```
k = 9
j = 5

```

```
`print("id(k): ", id(k))
print("id(j): ", id(j))
`
```

```
id(k):  4387308928
id(j):  4387308800

```

下面我们交换kj两个变量的值：

```
`k, j = j, k  # 变量值的交换
`
```

上面语句的含义表示为：将变量j的值（已经赋值了5）再赋值给变量k；将变量的值（已经赋值了9）再赋值给变量j；

```
`print("k =",k)
print("j =",j)
`
```

```
k = 5
j = 9

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

```
`print("id(k): ", id(k))
print("id(j): ", id(j))
`
```

```
id(k):  4387308800
id(j):  4387308928

```

通过对比交换前后两个变量的内存地址，我们发现：内存地址交换了，也就是变量已经发生了交换

在其他编程语言中实现变量值的交换的操作是（假设已经定义了两个变量a和b）：

```
`temp = a  # a赋值中间变量temp
a = b     # b的值赋给变量a
b = temp  # temp的值赋给变量b
`
```

## 变量的相等和相同

首先，必须声明的是Python中变量的相等和相同，是不同的两个概念，举例子说明

```
`number1 = 88
number2 = 88
`
```

```
`id(number1)
`
```

```
4387311456

```

```
`id(number2)
`
```

```
4387311456

```

判断两个变量是否相等：**使用==**

```
`number1 == number2
`
```

```
True

```

判断两个变量是否相同：**使用is**

```
`number1 is number2
`
```

```
True

```

结果都是True，说明number1和number2两个变量就是同一个对象

```
`list1 = "hello python" 
list2 = "hello python"
`
```

```
`list1 == list2  # 相等
`
```

```
True

```

```
`list1 is list2  # 不相同
`
```

```
False

```

上面的结果表明：list1和list2是相等，但是不相同。

```
`id(list1)
`
```

```
4444494000

```

```
`id(list2)
`
```

```
4444495024

```

通过查看二者的内存地址发现：它们的地址真的不同，所以肯定是**不相同**的两个对象。

我们再看最后一个情况：

```
`number3 = 1000
number4 = 1000
`
```

```
`number3 == number4  # 相等
`
```

```
True

```

```
`number3 is number4  # 居然不相等啦！
`
```

```
False

```

我们查看下二者的内存地址，发现它们的地址真的不同，所以肯定不是同一个对象啦~

```
`id(number3)
`
```

```
4444408880

```

```
`id(number4)
`
```

```
4444409104

```

这到底是为什么呢？以后揭晓~

<img width="17" height="36" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__373b4c00766747f0b.png"/>

推荐阅读

<img width="17" height="36" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__e1b6a6ebd6b14e14b.png"/>

[简洁且实用Python一行代码，40个案例讲解](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247496698&idx=1&sn=734208035e246021b1da187af9daae89&chksm=cf12e320f8656a36985eccf47eaeb732a96b0ca6c1163ad488aa0a1c97567b782f668f5534e9&scene=21#wechat_redirect)

[6000+字：详解python集合set，建议珍藏！！](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247495288&idx=1&sn=9102892bf1c5bdf315f6218e10624ba4&chksm=cf12fea2f86577b48a90a292e477d32f553d0f708737cff4ff28ab638867711de0cd08481e28&scene=21#wechat_redirect)

[如何学习Python数据分析：Peter帮你整理了一份资料！](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247494631&idx=1&sn=ab88d518c041ad93059fadaee528df2b&chksm=cf12fb3df865722bd84b05422d3515dd95edcd4c9b51f5d0ab080ef6c6873449b604950d0cbf&scene=21#wechat_redirect)

[Python入门：4000字能把元组tuple讲透吗？](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247493601&idx=1&sn=fcec4fc0f53192cd58ca9a668e3936e0&chksm=cf12f73bf8657e2d927c2df5d12dd4c4fe0c9ea7142c80796067ee6adee9d902f25a336dd37f&scene=21#wechat_redirect)

[Python入门：55个案例讲透列表的索引和切片！](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247493373&idx=1&sn=2875106912d287a8b6b3526d2106b3d7&chksm=cf12f627f8657f312b84f886f5396be6a6550fd2ba3978c07fe32471628f9c5fbb23e628ebac&scene=21#wechat_redirect)

> 尤而小屋，一个温馨的小屋。小屋主人，一手代码谋求生存，一手掌勺享受生活，欢迎你的光临

![](../../../_resources/0_wx_fmt_png_2b2f08134f674d9a8e575aa05556c4b6.png)

**尤而小屋**

尤而小屋，一个温馨且有爱的小屋🏡 小屋主人，一手代码谋求生存，一手掌勺享受生活，欢迎你的光临~

<a id="js_profile_article"></a>216篇原创内容

Official Account

收录于话题 #<a id="js_album_keep_read_title"></a>python

 <a id="js_album_keep_read_size"></a>57个

<a id="js_album_prev"></a>上一篇 <a id="js_album_keep_read_pre_title"></a>6大模块拿下Python时间序列 <a id="js_album_next"></a>下一篇 <a id="js_album_keep_read_next_title"></a>简洁且实用Python一行代码，40个案例讲解

People who liked this content also liked

一个非常好用的 Python 魔法库

Python编程大全

不看的原因

- 内容质量低
- 不看此公众号

Python单元测试模块doctest的具体使用

软件测试unittest

不看的原因

- 内容质量低
- 不看此公众号

离线安装python依赖库

永乐的运维笔记

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___0567bade9aea4663b.bmp"/>

Scan to Follow