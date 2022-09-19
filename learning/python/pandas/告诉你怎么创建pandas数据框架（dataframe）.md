# 告诉你怎么创建pandas数据框架（dataframe）

<a id="copyright_logo"></a>Original <a id="js_author_name"></a>fanjy <a id="profileBt"></a><a id="js_name"></a>完美Excel *2022-03-23 05:06*

收录于话题

<a id="js_article_tag_name__2102551132639166464"></a>#python <a id="js_article_tag_num__2102551132639166464"></a>71 <a id="js_article_tag_tips__2102551132639166464"></a>个

<a id="js_article_tag_name__2108213647335325698"></a>#pandas <a id="js_article_tag_num__2108213647335325698"></a>45 <a id="js_article_tag_tips__2108213647335325698"></a>个

<a id="js_article_tag_name__2015004433893392388"></a>#excel <a id="js_article_tag_num__2015004433893392388"></a>51 <a id="js_article_tag_tips__2015004433893392388"></a>个

学习Excel技术，关注微信公众号：

*excelperfect*

**标签：**Python与Excel,pandas

<img width="677" height="908" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_48a6aa1d2fcf4c68a.jpg"/>

通过前面的一系列文章的学习，我们已经学习了使用pandas将数据加载到Python中的多种不同方法，例如.read_csv()或.read_excel()。这些方法就像Excel中的“打开文件”，但我们通常也需要“创建新文件”。下面，我们就来学习如何创建一个空的数据框架（例如，像一个空白的Excel工作表）。

**基本语法**

在pandas中创建数据框架有很多方法，这里将介绍一些最常用和最直观的方法。所有这些方法实际上都是从相同的语法pd.DataFrame()开始的。下面是该方法的几个重要参数：

- **data****：**确切地说，这是你想要放到数据框架中的数据。
    
- **index****：**命名索引。
    
- **columns****：**命名列。
    

这里的参数data可以接受多种不同的形式：int、string、boolean、list、tuple、dictionary，等。

**创建一个****n****×****m****大小的数据框架**

让我们创建一个10行5列的数据框架，填充的值都为1。这里我们指定data=1，且有10行（索引）和5列。

![Image](../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_e6eb066d2bdb4d4da.jpg)

图1

**从列表中创建数据框架**

从列表创建数据框架，开始可能会让人困惑，但一旦你掌握了窍门，它就会慢慢变得直观。让我们看看下面的例子。有两个列表，然后创建一个这两个列表的列表\[a，b\]。注意输出的结果。

![Image](../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_f22ab041854f4fdb9.jpg)

图2

现在，让我们从列表\[a，b\]中创建一个数据框架。它实际上只是将上述结构放入一个数据框架中。因为我们没有指定index和columns参数，默认情况下它们被设置为从0开始的整数值。记住，Python是基于0的索引。

![Image](../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_73f683d0f2ef46cf9.jpg)

图3

如果你查看\[a，b\]和新的数据框架，以上内容实际上非常直观。然而，如果你打算创建两列，第一列包含a中的值，第二列包含b中的值，该怎么办？你仍然可以使用列表，但这一次必须将其zip()。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

图4

好的，但是zip对象到底是什么？它实际上是一个迭代器，只是一个对象，你可以通过它进行迭代（循环）。一般来说，如果你想查看迭代器中的内容，只需执行一个循环，然后像下面这样打印出迭代器中的元素。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

图5

还记得列表\[a，b\]的样子吗？现在，如果从该迭代器创建一个数据框架，那么将获得两列数据：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

图6

**从字典创建数据框架**

最让人喜欢的创建数据框架的方法是从字典中创建，因为其可读性最好。当我们向dataframe()提供字典时，键将自动成为列名。让我们从构建列表字典开始。

![Image](../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_3e6fa6e0392c4ed5a.jpg)

图7

于是，我们在这个字典里有两个条目，第一个条目名称是“a”，第二个条目名称是“b”。让我们从上面的字典创建一个数据框架。

![Image](../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_e2dc4d1ce9b84cb59.jpg)

图8

上述方法等同于下面的方法，但更具可读性。

![Image](../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_32c821870f4c48a9a.jpg)

图9

**小结**

记住，数据框架是相当灵活的，一旦创建它，你就可以调整其大小以满足需要。我们可以自由地将行或列插入数据框架，反之亦然（使用我们之前的10 x 5数据框架示例）。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

图10

这可能是显而易见的，但这里仍然想指出，一旦我们创建了一个数据框架，更具体地说，一个pd.dataframe()对象，我们就可以访问pandas提供的所有精彩的方法。例如，我们可以按降序对数据框架行进行排序：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

图11

注：本文学习整理自pythoninoffice.com。

Ad

## 深入理解Python特性

作者：\[德\]达恩·巴德尔（Dan Bader）

**当当**

**欢迎在下面留言，完善本文内容，让更多的人学到更完美的知识。**

欢迎到知识星球：***完美******Excel******社群***，进行技术交流和提问，获取更多电子资料，并通过社群加入专门的微信讨论群，更方便交流。

<img width="677" height="367" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_5b08700be87d4d8bb.jpg"/>

People who liked this content also liked

保存输入的值：Worksheet_Change事件应用示例

完美Excel

不看的原因

- 内容质量低
- 不看此公众号

有了这篇 Docker 网络原理，彻底爱了~

高效运维

不看的原因

- 内容质量低
- 不看此公众号

Excel VBA解读系列视频讲解持续更新中......

完美Excel

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___53204c3748f447e88.bmp"/>

Scan to Follow