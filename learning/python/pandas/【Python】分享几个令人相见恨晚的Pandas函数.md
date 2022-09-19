# 【Python】分享几个令人相见恨晚的Pandas函数

<a id="profileBt"></a><a id="js_name"></a>机器学习初学者 *2022-05-18 13:30* *Posted on <a id="js_ip_wording"></a>浙江*

The following article is from 关于数据分析与可视化 Author 俊欣

<a id="copyright_info"></a>[![](../../../_resources/0_b326a8c19f1747c087c89c7e00940c2b.jpg)<br>**关于数据分析与可视化** .<br>本公众号定期分享数据分析与可视化干货文章，并有时结合热点话题进行深入讨论，希望您会喜欢，要是哪里写的不好，也渴望倾听您的想法和意见，感谢！❤️](#)

又是新的一周，今天小编给大家来分享几个好用到爆的`Pandas`函数，或许不那么为人所知，但是相信会给大家在数据分析与挖掘的过程中起到不小的帮助。

### 创建数据集

首先我们先来创建一个数据集，代码如下

```


import numpy as np
import pandas as pd
df = pd.DataFrame({
   "date": pd.date_range(start="2021-11-20", periods=100, freq="D"),
   "class": ["A","B","C","D"] * 25,
   "amount": np.random.randint(10, 100, size=100)
})
df.head()



```

output

<img width="288" height="182" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__c31e117ac5d948258.png"/>

### To_period

当我们在处理日期数据时，有时候需要提取出月份的数据，有时候我们需要的是季度的数据，这里就可以通过`to_period()`方法来实现了，代码如下

```


df["year"] = df["date"].dt.to_period("Y")
df["month"] = df["date"].dt.to_period("M")
df["day"] = df["date"].dt.to_period("D")
df["quarter"] = df["date"].dt.to_period("Q")
df.head()



```

output

<img width="617" height="238" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__7d7b8fd7193d4d8ab.png"/>

在此基础之上，我们可以进一步对数据进行分析，例如

```
`df["month"].value_counts()
`
```

output

<img width="617" height="150" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__1c039d717c5a4f1b9.png"/>

我们想要筛选出“2021-12”该时段的数据，代码如下

```
`df[df['month'] == "2021-12"].head()
`
```

output

<img width="617" height="226" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__9f2f69f0e6a745b4b.png"/>

### 生成假数据

我们在建模、训练模型的时候，需要用到大量的数据集，然鹅很多时候我们会遇到数据量不够的情况，小编之前写过一篇相关的教程，使用`Python`中的`faker`模块或者通过一些深度学习的模型来生成假数据

[【原创好文】当机器学习遇到数据量不够时，这几个Python技巧为你化解难题](http://mp.weixin.qq.com/s?__biz=Mzk0NzI3ODMyMA==&mid=2247499116&idx=1&sn=ac8422fe7c58361d32750ba7c1868fae&chksm=c37be5f3f40c6ce54dbfe22a2e7e58e4c4eaa5fdb3d7e8b95804f7880eaa9244e557487001ec&scene=21#wechat_redirect)

`pandas`模块中也有一些相关的方法来帮助我们解决数据量不够的问题，代码如下

```
`pd.util.testing.makeDataFrame()
`
```

output

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

默认生成的假数据是30行4列的，当然我们也可以指定生成数据的行数和列数，代码如下

```
`pd.util.testing.makeCustomDataframe(nrows=1000, ncols=5)
`
```

output

<img width="617" height="304" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__80153cde404d4e909.png"/>

要是我们希望创建的数据集当中存在的缺失值，调用的则是`makeMissingDataframe()`方法

```
`pd.util.testing.makeMissingDataframe()
`
```

output

<img width="617" height="386" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__6c318db9c7bf4e2ea.png"/>

要是我们希望创建的数据集包含了整型、浮点型以及时间日期等其他类型的数据，调用的是`makeMixedDataFrame()`方法

```
`pd.util.testing.makeMixedDataFrame()
`
```

output

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 将数据集导出至压缩包中

众多周知，我们可以轻松地将数据集导出至csv文件、json格式的文件等等，但是有时候我们想要节省存储的资源，例如在文件的传送过程当中，想将其导出至压缩包当中，代码如下

```


df = pd.util.testing.makeCustomDataframe(nrows=1000, ncols=5)
df.shape



```

output

```
`(1000, 5)
`
```

我们先将其存储成`csv`格式的文件，看一下文件的大小，结果大概是占到了45KB的存储，代码如下

```


import os
os.path.getsize("sample.csv")/1024



```

output

```
`44
`
```

要是最后导出至压缩包当中呢，我们看一下文件的大小有多少？代码如下

```
`df.to_csv('sample.csv.gz', compression='gzip')
os.path.getsize('sample.csv.gz')/1024
`
```

output

```
`14
`
```

结果只占到了13KB的空间大小，大概是前者的三分之一吧，当然`pandas`还能够直接读取压缩包变成`DataFrame`数据集，代码如下

```


df = pd.read_csv('sample.csv.gz', compression='gzip', index_col=0)
df.head()



```

output

<img width="617" height="299" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__1427283127bc4c0eb.png"/>

### 一行代码让`Pandas`提速

很多时候我们想要通过`pandas`中的`apply()`方法将自定义函数或者是一些内部自带的函数应用到`DataFrame`每一行的数据当中，如果行数非常多的话，处理起来会非常地耗时间，这里使用的是`swifter`可以自动使`apply()`方法的运行速度达到最快，并且只需要一行代码即可，例如

```
`import swifter
 
df.swifter.apply(lambda x: x.max() - x.mean())
`
```

当然使用前，我们需要先前下载该模块，使用`pip`命令

```
`pip install swifter
`
```

```


![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

```


```


```


```


```


往期精彩回顾

- [适合初学者入门人工智能的路线及资料下载](http://mp.weixin.qq.com/s?__biz=MzIwODI2NDkxNQ==&mid=2247484737&idx=1&sn=27c52b4bc4ca98d3ab817344b84226cc&chksm=97048efda07307eb78d4f4ec0039a386a658404156b051af0cb715fafa8d2ae66cbe49343bf3&scene=21#wechat_redirect)
    
- [(图文+视频)机器学习入门系列下载](https://mp.weixin.qq.com/mp/appmsgalbum?__biz=MzIwODI2NDkxNQ==&action=getalbum&album_id=2259163844755406853#wechat_redirect)
    
- [中国大学慕课《机器学习》（黄海广主讲）](http://mp.weixin.qq.com/s?__biz=MzIwODI2NDkxNQ==&mid=2247502323&idx=1&sn=598d7231681ce2f316503201dd615c86&chksm=9707424fa070cb59f861a47f9eb4218cc5d3b835be49e93e67bb086dcc4c3b555a3546ebe9c5&scene=21#wechat_redirect)
    
- [机器学习及深度学习笔记等资料打印](http://mp.weixin.qq.com/s?__biz=MzIwODI2NDkxNQ==&mid=2247488304&idx=1&sn=581944f63eab1822ca53b9a4eeedad79&chksm=9704988ca073119a38a534adbedd51ca0b5705cdd6a104fed74b265bb092485e97c91bb5b347&scene=21#wechat_redirect)
    
- [《统计学习方法》的代码复现专辑](https://mp.weixin.qq.com/mp/appmsgalbum?action=getalbum&album_id=1337257945842778113&__biz=MzIwODI2NDkxNQ==#wechat_redirect)
    
- 机器学习交流qq群955171419，加入微信群请扫码：
    




```

![Image](https://mmbiz.qpic.cn/mmbiz_png/87HjJEl4c1vUSMHW0MHPHNgg7as7ia2XDIXvIBfBEFXkOr3uibbibPNUWvUxplwUv9ZdicAkOd7d2zocUa0VPQSzYw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)


```


```


```


```






```

People who liked this content also liked

如何拥有一个兼具CNN的速度、Transformer精度的模型？字节甩出TRT-ViT教你

...

极市平台

不看的原因

- 内容质量低
- 不看此公众号

Python 强大的模式匹配工具—Pampy

...

简说Python

不看的原因

- 内容质量低
- 不看此公众号

时间序列数据分析与预测之Python工具汇总

...

AI蜗牛车

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___d3bf576d4bf944948.bmp"/>

Scan to Follow