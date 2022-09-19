# 最近，又发现了 Pandas 中三个好用的函数

<a id="profileBt"></a><a id="js_name"></a>AI科技大本营 *2022-02-23 18:15*

The following article is from 小数志 Author luanhz

<a id="copyright_info"></a>[![](../../../_resources/0_8d153d6b21594dd5953de3ada04561ca.jpg)<br>**小数志** .<br>一个专注于数据科学的公众号！](#)

<img width="661" height="117" src="../../../_resources/640_wx_fmt_gif_wxfrom_5_wx_lazy__0519a57bfbd04cc1a.gif"/>

作者 | luanhz

来源 | 小数志

导读

近日，在github中查看一些他人提交的代码时，发现了Pandas中这三个函数，在特定场景中着实好用，遂成此文以作分享。

<img width="394" height="190" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__c4385f00735445e7a.png"/>

程序的基本结构大体包含三种，即顺序结构、分支结构和循环结构，其中循环结构应该是最能体现重复执行相同动作的代码控制语句，因此也是最必不可少的一种语法（当然，顺序和分支也都是必不可少的\- -!）。**虽然Pandas中提供了很多向量化操作，可以很大程度上避免暴力循环结构带来的效率低下，但也不得不承认仍有很多情况还是循环来的简洁实在。**

因此，为了在Pandas中更好的使用循环语句，本文重点介绍以下三个函数：

- **iteritems**
    
- **iterrows**
    
- **itertuples**
    

当然，这三个函数都是面向DataFrame这种数据结构的API，所以为了便于后文介绍三个函数，构造以下DataFrame实例：

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__edab98628348454a8.png)

01 iteritems

首先介绍iteritems。我们知道，Pandas中的DataFrame有很多特性，比如可以将其视作是一种嵌套的字典结构：外层字典的key为各个列名（column），相应的value为对应各列，而各列实际上即为内层字典，其中内层字典的key即为行索引，相应的value则为对应取值。所以，对于一个DataFrame，我们可以方便的使用类似字典那样，根据一个列名作为key来获取对应的value值，例如在上述DataFrame中：

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__78c832f2ee624da0b.png)

当然，这是Pandas中再基础不过的知识了，这里加以提及是为了引出DataFrame的下述API：即，类似于Python中字典的items()方法可以返回所有键值对那样，DataFrame也提供了items方法，返回结果相信也正是猜测的那样：

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__1ac23c4486a54c0aa.png)

当然，返回的结果是一个生成器（生成器是Python3中的一个重大优化，尤其适用于在数据量较大时提供memory-efficient的遍历）。我们可以将其强制转化为一个列表，并进而得到如下结果：

<img width="177" height="305" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__31a87521aa484f9da.png"/>

那么，DataFrame的items方法与这里要讲的iteritems方法有什么关系呢？在我初次看到这两个API时，直觉想法就是items显式的以列表形式返回各个item信息，而iteritems则以迭代器的形式返回各个item信息。但后来发现，实际上items()的返回值也是一个迭代器。进一步的，查看函数签名文档，发现二者其实就是一致的，甚至连iteritems文档中的example都用的items。

<img width="368" height="261" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__261dab2e0c1e44898.png"/>

iteritems的更多文档部分可自行查看

> 笔者猜测，可能是在早期items确实以列表形式返回，而后来优化升级为以迭代器形式返回了。不过在pandas文档中简单查阅，并未找到相关描述。

那么，说了这么多，iteritems到底有什么用呢？我个人总结为如下几个方面：

- 方便的以(columnName, Series)元组对的形式逐一遍历各行进行相应操作
    
- 以迭代器的形式返回，在DataFrame数据量较大时内存占用更为高效
    
- 另外，items是iteritems的同名函数，二者在功能上目前已无差别
    

02 iterrows

在前面介绍了iteritems的基础上，这里介绍iterrows就更加简单了。如果说iteritems是对各列进行遍历并以迭代器返回键值对，那么iterrows则是对各行进行遍历，并逐行返回（行索引，行）的信息。

首先来看函数的签名文档：

<img width="445" height="184" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__a962ee56e9ef4e6d9.png"/>

而后，仍以前述DataFrame为例，查看其返回结果：

<img width="138" height="291" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__cb0172b0218a43b1b.png"/>

这里仍然显式转化为list输出

结果不出所料：返回结果包含5个元组对，其中各元组的第一个值为相应的行索引，第二个值为对应行的Series格式。不过细看之下，其中有一个细节不容忽视：即各行对应Series的dtype均为object。在Pandas中，object往往是由于该行的数据类型存在多种类型而向上兼容为object。那么这里为何出现这样的结果呢？实际上，在iterrows的函数签名文档中给出了相应的解释：

<img width="416" height="307" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__18271c77ac1a4275a.png"/>

函数签名文档中的示例，由于两列的原始数据类型分别为int和float，所以经过iterrows遍历后，返回的各行Series中数据类型变为float64型，而在本文的示例DataFrame中，由于三列信息分别为int、float和object，所以最终返回的Series数据类型即为更通用的泛型：object。

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__dc440ddb400e4d698.png)

示例DataFrame的各列信息

那么，如果想要保留DataFrame中各列的原始数据类型时，该如何处理呢？这就需要下面的itertuples。

03 itertuples

在介绍itertuples之前，需要首先科普一下Python中预置的一种数据结构，namedtuple：

<img width="441" height="317" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__848e9c8e26064ff3a.png"/>

实际上，namedtuple是一个继承自tuple的子类，区别在于namedtuple除了可以使用索引来访问各元素取值外，还支持以各位置的'name'来访问元素（类似于C语言中的结构体类型），或者说namedtuple可以很方便的无缝转换为dict。

以此为基础，为了弥补iterrows中可能无法保留各行Series原始数据类型的问题，itertuples以namedtuple的形式返回各行，并也以迭代器的形式返回，以便于高效遍历。仍然来看函数签名文档：

<img width="458" height="262" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__4bd411a4f26a4b109.png"/>

而后，再看上述DataFrame调用itertuples后的返回结果：

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__1f41fead8a7e4fffa.png)

其中，返回值包含5个namedtuple，这里每个namedtuple都被命名为Pandas，这可以通过itertuples中的name参数加以修改；另外，注意到在每个namedtuple都包含了4个元素，除了A、B、C三个列取值外，还以index的形式返回了行索引信息，这可以通过itertuples中的index参数设置保留或舍弃。

> 由于行索引作为namedtuple中可选的一部分信息，所以与iteritems和iterrows不同，这里的返回值不再以元组队的形式显示行索引信息。

04 小结

以上就是本文分享的Pandas中三个好用的函数，其使用方法大体相同，并均以迭代器的形式返回遍历结果，这对数据量较大时是尤为友好和内存高效的设计。对于具体功能而言：

- iteritems是面向列的迭代设计，items函数的功能目前与其相同；
    
- iterrows和itertuples都是面向行的迭代设计，其中iterrows以元组对的形式返回，但返回的各行Series可能无法保留原始数据结构类型；而itertuples则以namedtuple形式返回各行信息，行索引不再单独显示而是作为namedtuple中的一项，并可通过itertuples参数加以设置是否保留。
    

<img width="661" height="132" src="../../../_resources/640_wx_fmt_gif_wxfrom_5_wx_lazy__962aacc0ecb644819.gif"/>

![](../../../_resources/0_wx_fmt_png_1078b188d8804e3fb062ac519d47218c.png)

**AI科技大本营**

为AI领域从业者提供人工智能领域热点报道和海量重磅访谈；面向技术人员，提供AI技术领域前沿研究进展和技术成长路线；面向垂直企业，实现行业应用与技术创新的对接。全方位触及人工智能时代，连接AI技术的创造者和使用者。

<a id="js_profile_article"></a>1426篇原创内容

Official Account

往

期

回

顾

资讯

[冬奥会夺金背后的杀手锏，是他](http://mp.weixin.qq.com/s?__biz=Mzg4NDQwNTI0OQ==&mid=2247555474&idx=1&sn=6ed705215bb3625a048b489455518252&chksm=cfbafcfcf8cd75ea33af095a6e07bc25e0739ffaf85dcc2f4684d0eaf1dd014f3e2e22db8750&scene=21#wechat_redirect)

技术

[用python绘制动态可视化图表](http://mp.weixin.qq.com/s?__biz=Mzg4NDQwNTI0OQ==&mid=2247555634&idx=1&sn=81c05c21690df3f538ff55200e37571e&chksm=cfbaff5cf8cd764ae15bf38133a3cd0fc086d4f633c2bcf8809ffeccf80bcaee035854150d7c&scene=21#wechat_redirect)

技术

[在 Python 中妙用短路机制](http://mp.weixin.qq.com/s?__biz=Mzg4NDQwNTI0OQ==&mid=2247555530&idx=2&sn=7e1a77b65d62a9b5ad1f5bc31ced0564&chksm=cfbafca4f8cd75b25779f68fe8c18955f09a7149f50ea5e26df5522482da88c7bb0d02f81c42&scene=21#wechat_redirect)

资讯

[M2芯片终于要来了？全线换新](http://mp.weixin.qq.com/s?__biz=Mzg4NDQwNTI0OQ==&mid=2247555747&idx=1&sn=d78476bcb289c18a1e1a2e731b1af777&chksm=cfbaffcdf8cd76db4ce19ce4fea2844ad08092be0af481bc2558a2c447e696c70265b454ab9f&scene=21#wechat_redirect)

<img width="30" height="28" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__33849df9dad1451bb.png"/>

**分享**

<img width="30" height="27" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__94a8e4dae2874b379.png"/>

**点收藏**

<img width="30" height="28" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__f0744d9793924070b.png"/>

**点点赞**

<img width="30" height="28" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__5482bd40c07742268.png"/>

**点在看**

People who liked this content also liked

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

深入理解Lustre分布式文件系统之Lustre架构

存储内核技术交流

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___7580a6c07bed41228.bmp"/>

Scan to Follow