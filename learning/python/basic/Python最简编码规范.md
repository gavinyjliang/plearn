# Python最简编码规范

<a id="profileBt"></a><a id="js_name"></a>SQL数据库开发 *2022-05-07 08:10* *Posted on <a id="js_ip_wording"></a>广东*

**点击关注上方“SQL数据库开发”，**

**设为“置顶或星标****”，第一时间送达干货**

**0、前言**

本文是阅读《Python Coding Rule》之后总结的最为精华及简单的编码规范，根据每个人不同喜好有些地方会有不同的选择，我只是做了对自己来说最简单易行的选择，仅供大家参考。

![Image](../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_10abcefcb94f40698.jpg)

**1、重要原则**

a.保持风格的一致性很重要，但最重要的是：知道何时不一致
b.打破一条既定规则的两个好理由：
c.当应用规则会导致代码可读性下降(可读性赛高)
d.为了和周围代码保持一致而打破规则(历史遗留)

**2、最简规范**

a.只使用空格缩进
b.使用UTF-8编码
c.每行只写一条语句
d.使用行末反斜杠折叠长行，限制每行最大79字符
e.导入包：每行唯一、从大到小、绝对路径
f.类内方法空1行分隔，类外空2行分隔
g.运算符除 * 外，两边空1格分隔，函数参数=周围不用空格
h.除类名使用驼峰法以外，其他模块、函数、方法、变量均使用全小写+下划线
i.1个前导下划线表示半公开，2个前导下划线表示私有，与保留字区分使用单个后置下划线
j.开发时使用中文注释，发布时再写英文文档

**3、详细规范**

a.全文通用
b.只使用空格缩进，4个空格表示1个缩进层次
c.每行长度限制在79字符内，使用行末反斜杠折叠长行
d.使用UTF-8编码
e.每行只写一条语句

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

**4、代码命名**

一行只import一个包，Imports的顺序为：标准库、相关主包、特定应用，每组导入之间放置1行空行，所有导入使用包的绝对路径。

分割顶层函数和类的定义使用2行空行，分割类内方法定义使用1行空行，class行与第一个方法定义之间要有1行空行。

整体使用英文书写方式来使用空格，即仅在逗号、分号后面添加1个空格，其他任何符号如圆括号、方括号、花括号等都不用空格把符号与字符分开，写在一起表示一个整体;运算符除 * 号以外，其他符号两边都各用1个空格分隔;函数参数=号周围不用空格。

模块名：不含下划线、简短、全小写;

类名、异常名：首字母大写单词串的驼峰法;

函数名、全局变量名、方法名、实例变量：全小写，加下划线增加可读性;

一个前导下划线仅用于不想被导入的全局变量(还有内部函数和类)前加一个下划线)、不打算作为类的公共接口的内部方法和实例变量;

两个前导下划线以表示类私有的名字，只用来避免与类(为可以子类化所设计)中的属性发生名字冲突。

私有属性必须有两个前导下划线，无后置下划线;

非公有属性必须有一个前导下划线，无后置下划线。

公共属性没有前导和后置下划线，除非它们与保留字冲突，此情况下，单个后置下划线比前置或混乱的拼写要好，例如:class_优于klass。

**5、编写技巧**

与None之类的单值比较，永远用:'is'或'is not'来做：if x is not None

在模块和包内定义基异常类(base exception class)

使用字符串方法(methods)代替字符串模块。

在检查前缀或后缀时避免对字符串进行切片，用startswith()和endswith()代替，如：No: if foo\[:3\] == 'bar':Yes: if foo.startswith('bar'):

只用isinstance()进行对象类型的比较，如：No: if type(obj) is type(1):Yes: if isinstance(obj, int)

判断True或False不要用 ==，如：No: if greeting == True:Yes: if greeting:

**6、注释**

开发时，注释全部用中文来写，当要发布脚本工具时，再写英文文档。

注释应该是是完整的句子(短语也可)，首字母大写;如果注释很短，省略末尾句号;注释块由一个or多个完整句子构成的段落组成，则每个句子使用句子结尾;句末句号后使用两个空格。

注释块每行以#和一个空格开始，并且跟随注释的代码具有相同的缩进层次，注释块上下方有一空行包围。

谨慎使用行内注释，至少使用两个空格与语句分开。

使用 pydoc; epydoc; Doxgen 等文档化工具，为所有公共模块、函数、类和方法边写文档字符串，文档字符串对非公开的方法不是必要的，但你应该有一个描述这个方法做什么的注释，这个注释应该在"def"这行后。

多行文档字符串结尾的""" 应该单独成行。

**版本注记：**定义一个变量\_\_version\_\_ = "$Revision: 1.4 $"

Stay hungry. Stay foolish.

文章链接：https://www.cnblogs.com/Chayeen/p/8884776.html

```


![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg== "音符")

最后给大家分享我写的SQL两件套：《SQL基础知识第二版》和《SQL高级知识第二版》的PDF电子版。里面有各个语法的解释、大量的实例讲解和批注等等，非常通俗易懂，方便大家跟着一起来实操。

有需要的读者可以下载学习，在下面的公众号「数据前线」(非本号)后台回复关键字：SQL，就行

数据前线

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

——End——

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




```

People who liked this content also liked

基于jsoneditor二次封装一个可实时预览的json编辑器组件(react版)

...

React

不看的原因

- 内容质量低
- 不看此公众号

.net 项目使用 JSON Schema

...

DotNetCore实战

不看的原因

- 内容质量低
- 不看此公众号

学 SQL 必须了解的10个高级概念

...

杰哥的IT之旅

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___9932102635544d61b.bmp"/>

Scan to Follow