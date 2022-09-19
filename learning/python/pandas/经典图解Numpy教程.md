# 经典图解Numpy教程

尤而小屋 <a id="profileBt"></a><a id="js_name"></a>尤而小屋 *2022-03-07 12:30*

收录于话题

<a id="js_article_tag_name__2102097910560522242"></a>#numpy <a id="js_article_tag_num__2102097910560522242"></a>6 <a id="js_article_tag_tips__2102097910560522242"></a>个

<a id="js_article_tag_name__2226812146087165954"></a>#人工智能 <a id="js_article_tag_num__2226812146087165954"></a>25 <a id="js_article_tag_tips__2226812146087165954"></a>个

<a id="js_article_tag_name__1999223977696624640"></a>#机器学习 <a id="js_article_tag_num__1999223977696624640"></a>72 <a id="js_article_tag_tips__1999223977696624640"></a>个

<a id="js_article_tag_name__2019120411547860993"></a>#数据挖掘 <a id="js_article_tag_num__2019120411547860993"></a>37 <a id="js_article_tag_tips__2019120411547860993"></a>个

<a id="js_article_tag_name__2243139761421107203"></a>#矩阵运算 <a id="js_article_tag_num__2243139761421107203"></a>2 <a id="js_article_tag_tips__2243139761421107203"></a>个

> 公众号：尤而小屋
> 作者：Peter
> 编辑：Peter

大家好，我是Peter~

今天给大家介绍一篇用可视化的方式介绍 NumPy 的功能和使用示例的文章，可视化让过程更加具体化~

NumPy 软件包是 Python 生态系统中数据分析、机器学习和科学计算的主力军。它极大地简化了向量和矩阵的操作处理。Python 的一些主要软件包（如 scikit-learn、SciPy、pandas 和 tensorflow）都以 NumPy 作为其架构的基础部分。

除了能对数值数据进行切片（slice）和切块（dice）之外，使用 NumPy 还能为处理和调试上述库中的高级实例带来极大便利。
<img width="657" height="242" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_07f50b4a96f14bf78.jpg"/>

本文将介绍使用 NumPy 的一些主要方法，以及在将数据送入机器学习模型之前，它如何表示不同类型的数据（表格、图像、文本等）。

```
import numpy as np

```

## 创建数组

我们可以通过传递一个 python 列表并使用 np.array（）来创建 NumPy 数组（极大可能是多维数组）。在本例中，python 创建的数组如下图右所示：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

img

通常我们希望 NumPy 能初始化数组的值，为此 NumPy 提供了 ones()、zeros() 和 random.random() 等方法。我们只需传递希望 NumPy 生成的元素数量即可：

<img width="657" height="146" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_5de0fd880a3447e79.jpg"/>

img

一旦创建了数组，我们就可以尽情对它们进行操作。

## 数组运算

让我们创建两个 NumPy 数组来展示数组运算功能。我们将下图两个数组称为 data 和 ones：

<img width="657" height="109" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_f384431fed7b4620b.jpg"/>

img

将它们按位置相加（即每行对应相加），直接输入 data + ones 即可：

<img width="657" height="135" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_e111dfde6f46412ab.jpg"/>

img

当我开始学习这些工具时，我发现这样的抽象让我不必在循环中编写类似计算。此类抽象可以使我在更高层面上思考问题。

除了「加」，我们还可以进行如下操作：

<img width="657" height="79" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_396fc72833944ea5a.jpg"/>

img

通常情况下，我们希望数组和单个数字之间也可以进行运算操作（即向量和标量之间的运算）。比如说，我们的数组表示以英里为单位的距离，我们希望将其单位转换为千米。只需输入 data * 1.6 即可：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

img

看到 NumPy 是如何理解这个运算的了吗？这个概念叫做广播机制（broadcasting），它非常有用，以后会重点介绍广播机制！

## 索引

我们可以我们像对 python 列表进行切片一样，对 NumPy 数组进行任意的索引和切片：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

img

## 聚合

NumPy 还提供聚合功能：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

img

除了 min、max 和 sum 之外，你还可以使用 mean 得到平均值，使用 prod 得到所有元素的乘积，使用 std 得到标准差等等。

上述的例子都在一个维度上处理向量。NumPy 之美的关键在于，它能够将上述所有方法应用到任意数量的维度。

## 创建矩阵

我们可以传递下列形状的 python 列表，使 NumPy 创建一个矩阵来表示它：

```
np.array([[1,2],[3,4]])

```

我们也可以使用上面提到的方法（ones()、zeros() 和 random.random()），只要写入一个描述我们创建的矩阵维数的元组即可：

<img width="657" height="118" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_e650bb8519f34a109.jpg"/>

img

## 矩阵运算

如果两个矩阵大小相同，我们可以使用算术运算符（+-*/）对矩阵进行加和乘。NumPy 将它们视为 position-wise 运算：

<img width="657" height="119" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_848dd5e3e80c46518.jpg"/>

img

我们也可以对不同大小的两个矩阵执行此类算术运算，但前提是某一个维度为 1（如矩阵只有一列或一行），在这种情况下，NumPy 使用广播规则执行算术运算：

## 点乘

算术运算和矩阵运算的一个关键区别是矩阵乘法使用点乘。NumPy 为每个矩阵赋予 dot() 方法，我们可以用它与其他矩阵执行点乘操作：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

img

我在上图的右下角添加了矩阵维数，来强调这两个矩阵的临近边必须有相同的维数。你可以把上述运算视为：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

img

## 矩阵索引

当我们处理矩阵时，索引和切片操作变得更加有用：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

img

## 矩阵聚合

我们可以像聚合向量一样聚合矩阵：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

img

我们不仅可以聚合矩阵中的所有值，还可以使用 axis 参数执行跨行或跨列聚合：

<img width="657" height="117" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_26ba2dd76c6c4342a.jpg"/>

img

## 转置和重塑

处理矩阵时的一个常见需求是旋转矩阵。当需要对两个矩阵执行点乘运算并对齐它们共享的维度时，通常需要进行转置。NumPy 数组有一个方便的方法 T 来求得矩阵转置：

<img width="657" height="264" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_968548cfbf0e49998.jpg"/>

img

在更高级的实例中，你可能需要变换特定矩阵的维度。在机器学习应用中，经常会这样：某个模型对输入形状的要求与你的数据集不同。在这些情况下，NumPy 的 reshape() 方法就可以发挥作用了。只需将矩阵所需的新维度赋值给它即可。可以为维度赋值-1，NumPy 可以根据你的矩阵推断出正确的维度：

<img width="657" height="243" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_7bd24216fd95493d9.jpg"/>

img

## 再多维度

NumPy 可以在任意维度实现上述提到的所有内容。其中心数据结构被叫作 ndarray（N 维数组）不是没道理的。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

img

在很多情况下，处理一个新的维度只需在 NumPy 函数的参数中添加一个逗号：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

img

以下是 NumPy 可实现的有用功能的实例演示。

## 公式

实现可用于矩阵和向量的数学公式是 NumPy 的关键用例。这就是 NumPy 是 python 社区宠儿的原因。例如均方差公式，它是监督机器学习模型处理回归问题的核心：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

img

在 NumPy 中实现该公式很容易：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

img

这样做的好处在于，NumPy 并不关心 predictions 和 labels 包含一个值还是一千个值（只要它们大小相同）。我们可以通过一个示例依次执行上面代码行中的四个操作：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

img

预测和标签向量都包含三个值，也就是说 n 的值为 3。减法后，得到的值如下：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

img

然后将向量平方得到：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

img

现在对这些值求和：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

img

得到的结果即为该预测的误差值和模型质量评分。

## 数据表示

考虑所有需要处理和构建模型所需的数据类型（电子表格、图像、音频等），其中很多都适合在 n 维数组中表示：

表格和电子表格

电子表格或值表是二维矩阵。电子表格中的每个工作表都可以是它自己的变量。python 中最流行的抽象是 pandas 数据帧，它实际上使用了 NumPy 并在其之上构建。

<img width="657" height="266" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_fd19e5ac39254badb.jpg"/>

img

## **音频和时间序列**

音频文件是样本的一维数组。每个样本都是一个数字，代表音频信号的一小部分。CD 质量的音频每秒包含 44,100 个样本，每个样本是-65535 到 65536 之间的整数。这意味着如果你有一个 10 秒的 CD 质量 WAVE 文件，你可以将它加载到长度为 10 * 44,100 = 441,000 的 NumPy 数组中。如果想要提取音频的第一秒，只需将文件加载到 audio 的 NumPy 数组中，然后获取 audio\[:44100\]。

以下是一段音频文件：

<img width="657" height="178" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_fee8b781803a4dfc9.jpg"/>

img

时间序列数据也是如此（如股票价格随时间变化）。

## **图像**

图像是尺寸（高度 x 宽度）的像素矩阵。

如果图像是黑白（即灰度）的，则每个像素都可以用单个数字表示（通常在 0（黑色）和 255（白色）之间）。想要裁剪图像左上角 10 x 10 的像素吗？在 NumPy 写入

即可。

下图是一个图像文件的片段：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

img

如果图像是彩色的，则每个像素由三个数字表示——红色、绿色和蓝色。在这种情况下，我们需要一个三维数组（因为每个单元格只能包含一个数字）。因此彩色图像由尺寸为（高 x 宽 x3）的 ndarray 表示：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

img

## **语言**

如果我们处理文本，情况就不同了。文本的数字表示需要一个构建词汇表的步骤（模型知道的唯一字清单）和嵌入步骤。让我们看看用数字表示以下文字的步骤：

模型需要先查看大量文本，再用数字表示这位诗人的话语。我们可以让它处理一个小数据集，并用它来构建一个词汇表（71,290 个单词）：

<img width="657" height="578" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_34cfe0a90aa34b948.jpg"/>

img

这个句子可以被分成一个 token 数组（基于通用规则的单词或单词的一部分）：

<img width="657" height="40" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_399d99ce67f546e29.jpg"/>

img

然后我们用词汇表中的 ID 替换每个单词：

<img width="657" height="33" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_7b6c90df4bbd4c519.jpg"/>

img

这些 ID 仍然没有为模型提供太多信息价值。因此，在将这一组单词输入到模型之前，我们需要用嵌入替换 token/单词（在本例中为 50 维 word2vec 嵌入）：

<img width="657" height="123" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_a97fcbaa9b0c4f238.jpg"/>

img

可以看到，该 NumPy 数组的维度为 \[embedding\_dimension x sequence\_length\]。出于性能原因，深度学习模型倾向于保留批大小的第一维（因为如果并行训练多个示例，模型训练速度会加快）。在这种情况下，reshape() 变得非常有用。如像 BERT 这样的模型期望的输入形式是：\[batch\_size，sequence\_length，embedding_size\]。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

img

现在这是 numeric volume 形式，模型可以处理并执行相应操作。其他行虽然留空，但是它们会被填充其他示例以供模型训练（或预测）。

*感谢原文：https://jalammar.github.io/visual-numpy/*；翻译：机器之心

<img width="17" height="36" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__351fbe7057c446be9.png"/>

推荐阅读

<img width="17" height="36" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__3e9e033876d74dd89.png"/>

[机器学习神器Scikit-Learn入门.PPT](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247504337&idx=1&sn=638ef41e5ef71ce6bad0fe0667ade85c&chksm=cf12c10bf865481d05feae03978bbfddac4ea278222ff354909889e3939dc81db2f57bc46619&scene=21#wechat_redirect)

[机器学习矩阵运算必学库Numpy首秀！](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247504322&idx=1&sn=2abaf2d5e0119303bdd0b5f5904f2a12&chksm=cf12c118f865480e204f24c3b1b59c8ac3706c54160928db967ca52a884c2eab9a9e6487d380&scene=21#wechat_redirect)

[Pandas高级函数transform使用指南](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247503540&idx=1&sn=0ff61e2da67fe2e324791ebae5e5b993&chksm=cf12de6ef8655778e1fe5498966f1e960a03f03365c9d68c58c334c56dc6c967fd58ee70e6d2&scene=21#wechat_redirect)

[纯国产可视化库Pyecharts首秀！](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247499203&idx=1&sn=b19044259e59df024e3e9882a1a1aaa0&chksm=cf12ed19f865640fa1851aee4890397b4407814541c74361ee7072781f82c944f3eef4bd25aa&scene=21#wechat_redirect)

[桑基图或许可以告诉你打工人的故事！](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247493084&idx=1&sn=ff56e5d6bd63ad993b0602ae3e1d2a83&chksm=cf12f506f8657c108416176561ee84ab0f3dda547fe53ed27fc590aff7af2f9f51a09c93fe84&scene=21#wechat_redirect)

[种草Peter的7大写作利器](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247504145&idx=1&sn=b78fbc16d66250cb603f24c94e27d8e0&chksm=cf12c1cbf86548dd497b7ac21395ad6e74d7a37c4c4e71bfc69ecdbe4397b162c06af57ad02c&scene=21#wechat_redirect)

> 尤而小屋，一个温馨的小屋。小屋主人，一手代码谋求生存，一手掌勺享受生活，欢迎你的光临

![](../../../_resources/0_wx_fmt_png_08208d42f71d4bfcb69bf5efc77724ac.png)

**尤而小屋**

尤而小屋，一个温馨且有爱的小屋🏡 小屋主人，一手代码谋求生存，一手掌勺享受生活，欢迎你的光临~

<a id="js_profile_article"></a>216篇原创内容

Official Account

收录于话题 #<a id="js_album_keep_read_title"></a>机器学习

 <a id="js_album_keep_read_size"></a>72个

<a id="js_album_prev"></a>上一篇 <a id="js_album_keep_read_pre_title"></a>全方位解析：7大回归分析模型 <a id="js_album_next"></a>下一篇 <a id="js_album_keep_read_next_title"></a>机器学习、气象数据处理、科研资讯，免费代码和科研鹊桥一网打尽！！！

People who liked this content also liked

【Docker学习】1、使用 Linux（CentOS7）搭建 Docker 基础环境

花海里

不看的原因

- 内容质量低
- 不看此公众号

Python是不是被严重高估了？

编程学习部

不看的原因

- 内容质量低
- 不看此公众号

extractvalue函数报错注入

here404

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___4e2ce802277c4bba9.bmp"/>

Scan to Follow