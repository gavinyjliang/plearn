# 一图胜千言，超形象图解NumPy教程！

<a id="profileBt"></a><a id="js_name"></a>小数志 *2022-04-29 12:00* *Posted on <a id="js_ip_wording"></a>浙江*

The following article is from pythonic生物人 Author pythonic生物人

<a id="copyright_info"></a>[![](../../../_resources/0_a3d16460945d4f3a8eeaa0faa66b9376.png)<br>**pythonic生物人** .<br>分享数据科学干货（涉及Python/R/统计等）](#)

- NumPy是Python中诸多数据科学库的重要基础，例如，pandas，OpenCV，TensorFlow等，学习NumPy对其它NumPy依赖数据科学库意义重大👇。<img width="632" height="567" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__eb453df3be0c40bc8.png"/>
    
- **本文翻译一篇不错的NumPy基础图解教程**，从`一维数组(向量)`，`二维数组(矩阵)`和`多维数组(3维及更高维数组)`介绍NumPy。
    
- 原文：NumPy Illustrated: The Visual Guide to NumPy，文章灵感部分来自👉*[Jay Alammar的NumPy图解文章](https://mp.weixin.qq.com/s?__biz=MzUwOTg0MjczNw==&mid=2247485245&idx=1&sn=8c2d69bb54d0bc52e589365e075d6ff8&chksm=f90d4363ce7aca751c382d72294165550b5b6f948ed4f2e31f335b51191dc35ac5a5da92d5fc&scene=21#wechat_redirect)*
    

* * *

# 0、NumPy数组 vs Python列表

- Numpy数组中插入、移除元素没Python列表高效；
    
- Numpy数组可`直接做四则运算`、Python列表则需借助列表推倒式等；![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)
    
- Numpy数组更紧凑,`高维`时尤为明显；
    
- Numpy数组`向量化后运算`速度比Python列表更快；
    
- Numpy数组通常是`同质化`的，仅仅当数组中元素类型一致时处理速度快。
    

* * *

# 1、一维数组 (向量)

## 1.1 数组创建

- 通过Python列表创建![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)⚠️必须确保列表中元素类型一致，否则，进一步处理报错。![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)⚠️NumPy数组区别于Python列表，不能在数组末尾直接扩展元素。
    
- 通过np.zeros或np.empty等创建 可以通过np.zeros、np.ones、np.empty、np.full等创建数组。![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)
    
- 通过np.zeros\_like或np.empty\_like等创建 以上都有对应的_like函数，可快速创建a数组长度一致数组， ![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)
    
- 通过np.arange和np.linspace创建 np.arange类似于Python range![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)⚠️np.arange对元素数据类型敏感，特别是float类别，以下方式可供参考![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)
    
- 创建随机数组 已弃用方法，![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)新方法， ![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)
    

* * *

## 1.2 数组索引

1.1中介绍了多种构建数组的方法，1.2中介绍如何愉快的从数组中取元素。

- 常见索引方法![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)
    
- 布尔运算符索引 ![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)⚠️不能用三元运算符，如3<=a<=5。
    
- np.where、np.clip+布尔运算符索引![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)
    

* * *

## 1.3 数组操作

计算速度是NumPy的亮点之一，其数组运算操作速度接近C++，把数组当作整体来运算，避免Python的循环，可以解决一部分Python慢的问题，这一节介绍NumPy的数组操作。

- 加减乘除、取整等基础操作 类似python加减乘除、取余数，![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)向上、向下、四舍五入![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)最大、最小、均值等，![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)
    
- 三角函数![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)
    
- 标量计算![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)
    
- 支持更多数学方法![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)
    
- 排序 只有部分Python列表排序操作，![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)
    

* * *

## 1.4 数组元素查找

无Python list的index函数，通常有三种方法，

- where，但不够pythonic、需压遍历数组；
    
- next，较快，需要Numba；
    
- searchsorted，适合已排序数组。![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)
    

* * *

## 1.5 浮点数比较

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

* * *

# 2 二维数组 (矩阵)

## 2.1 二维数组创建

同一维数组，⚠️使用\[\[\]\],![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)随机数组构建![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

* * *

## 2.2 二维数组索引

比Python嵌套列表更方便![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

* * *

## 2.3 二维数组操作

NumPy支持跨行、跨列操作，此时NumPy引入了axis的概念，axis=1，axis=0👇，![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

- 行列or行or列求和 ![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)
    
- +、-、*、/、//、**和@  +、-、*、/、//、**类似一维数组，@为二维数组独有，二维数组整体计算，![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)二维数组与单个元素、一维数组计算，![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)@计算非对称线性代数外积，![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)
    
- 行/列向量计算![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)
    
- 二维数组变形 ![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)一维数组、二维数组之间互转，![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)
    
- hstack、vstack、column_stack拼接数组![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)
    
- hsplit、vsplit拆分二维数组![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)
    
- tile、repeat复制数组![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)
    
- delete二维数组删除 ![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)
    
- insert二维数组插入 ![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)
    
- append二维数组末尾操作![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)
    
- pad二维数组边界操作![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)
    

* * *

## 2.4 二维数组网格化

针对网格化，C、传统Python及MATLAB都有解决办法，![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)NumPy的广播机制让网格化更高效，![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)此外，网格还可以用于数组索引，![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

* * *

## 2.5 二维数组统计

支持min/max, argmin/argmax, mean/median/percentile, std/var等函数，前面2.3节介绍过部分。![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)argmin/argmax返回最大、最小值下标，![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)all和any适用特定维度， ![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

* * *

## 2.6 二维数组排序

注意指定axis，![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

- argsort按某一列排序![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)
    
- lexsort对所有列进行排序 ⚠️总是按行执行，从下到上，![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)
    
- sort结合order![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)
    

* * *

# 3 **多维数组 (3维及更高维数组)**

下面主要以3维数组为例。

## 3.1 **多维数组创建**

通过reshape 1维数组、嵌套Python list创建多维数组，索引(z,y,x)中，z是平面方向，(y,x)在z平面上坐标，![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

* * *

## 3.2 **多维数组创建**hstack，vstack

适用3维数组，多了一个dstack，

⚠️但是，这些函数支持的索引顺序是(y,x,z)，![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)需要借助concatenate改变多维数组布局，![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)广播机制也适用多维数组，einsum(Einstein summation)函数在多维数组中可避免过多Python循环，让代码更简洁，![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

相关阅读：

- [写在1024：一名数据分析师的修炼之路](http://mp.weixin.qq.com/s?__biz=MzU0OTk5MDg3OQ==&mid=2247485017&idx=1&sn=1c4b44929e0df8e67d5f2460cf2fa37c&chksm=fba63ee9ccd1b7ffe08868d924d25e8bf32bfbe08536bfaa1904fcf049c48d487b4b15bfcda6&scene=21#wechat_redirect)
    
- [数据科学系列：sklearn库主要模块功能简介](http://mp.weixin.qq.com/s?__biz=MzU0OTk5MDg3OQ==&mid=2247484865&idx=1&sn=fcd8cfbf6adb777e693177ad1c169a39&chksm=fba63d71ccd1b4678adac1d357ed8ee70740676e8e637b43cb71eef7492863d8b154c03d43c5&scene=21#wechat_redirect)
    
- [数据科学系列：seaborn入门详细教程](http://mp.weixin.qq.com/s?__biz=MzU0OTk5MDg3OQ==&mid=2247484633&idx=1&sn=bd24e3a089587674a2a6991066ff41c1&chksm=fba63c69ccd1b57f4f48ea2c82b1c79e7fdd7bfaf7c11d65764ff793bdf5ec421e7430354326&scene=21#wechat_redirect)
    
- [数据科学系列：pandas入门详细教程](http://mp.weixin.qq.com/s?__biz=MzU0OTk5MDg3OQ==&mid=2247484410&idx=1&sn=b9654d33bc46795a3c296a6e5e0c6735&chksm=fba63b4accd1b25cb1c31527b3e9dad70917092b19c8f6ffe57404fee642b3e6a3577c8eb727&scene=21#wechat_redirect)
    
- [数据科学系列：matplotlib入门详细教程](http://mp.weixin.qq.com/s?__biz=MzU0OTk5MDg3OQ==&mid=2247484405&idx=1&sn=0e3ec6923c630965d0fbdf633341d0f5&chksm=fba63b45ccd1b253282f253fac55827f723439ca067472772e346343780343f8270a4c1a5298&scene=21#wechat_redirect)
    
- [数据科学系列：numpy入门详细教程](http://mp.weixin.qq.com/s?__biz=MzU0OTk5MDg3OQ==&mid=2247484400&idx=1&sn=f317d6c11ebda49eb3e197fa3f5a5379&chksm=fba63b40ccd1b256afe19cca78f013a65271e1f138d0cae45278e3c45c4fd50042ff9471c5a7&scene=21#wechat_redirect)
    

People who liked this content also liked

HBase写入全流程剖析

...

Flink实战剖析

不看的原因

- 内容质量低
- 不看此公众号

python免杀---进化史

...

广软NSDA安全团队

不看的原因

- 内容质量低
- 不看此公众号

C++那些事之json解析

...

光城

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___65314a741fae454ca.bmp"/>

Scan to Follow