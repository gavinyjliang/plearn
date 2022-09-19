# 【Python】50个Pandas的奇淫技巧:一网打尽各种索引 iloc,loc,ix,iat,at…

<a id="profileBt"></a><a id="js_name"></a>机器学习初学者 *2022-05-24 12:11* *Posted on <a id="js_ip_wording"></a>浙江*

The following article is from 小伍哥聊风控 Author 小伍哥

<a id="copyright_info"></a>[![](../../../_resources/0_d75927867bf34f288f44f7084295374a.jpg)<br>**小伍哥聊风控** .<br>互联网风控策略与算法，内容风控，复杂网络挖掘，图神经网络，黑产挖掘等知识分享](#)

数据处理，也是风控非常重要的一个环节，甚至说是模型成败的关键环节。因此，娴熟简洁的数据处理技巧，是提高建模效率和建模质量的必要能力。这里开个专题，总结下Pandas的使用方法，方便大家，也方便自己查阅。

这个专题叫做：【**50个Pandas的奇淫技巧】，**今天这个算是第一讲，后续慢慢更新。

# **一、Pandas索引概述**

很多人在使用Pandas处理数据时，总会迷失在data\[\]、iloc()、loc()、ix()中，似乎记得，又似乎不记得，每到用时都需要百度，不知所以然的解决了问题，下次继续百度，记忆点基本上非常混乱。总结本文，希望能解决这个问题，通过一个简单的案例彻底搞明白这几种索引方法到底有什么区别。

日常使用中，推荐使用loc和iloc进行索引，loc是指location的意思，iloc中的 i 是指integer，这两个方法容易混淆，可以使用特殊方式来加强记忆。

**iloc：****基于位置，用****行号、列号****进行索引，**i 可以看着 int，因此 iloc **只能用整数** 来索引，例如data.iloc\[0:2,:\]

**loc ：****基于标签，用****行名、列名****进行索引，**数据的index经常为整数，因此 loc 的使用范围要远高于iloc，loc可以使用整数切片、名称(index,columns)索引、也可以切片和名称混合使用。例如：data.loc\[0:5:,'row1':'row2'\]

我们简单构造一个数据集，在下面的案例中需要用到。

```
`import pandas as pd``import numpy  as np``data = pd.DataFrame(np.arange(25).reshape(5, 5), `` index = ['row1', 'row2','row3','row4','row5'], `` columns=['col1', 'col2','col3','col4','col5'])``data `` col1  col2  col3  col4  col5``row1     0     1     2     3     4``row2     5     6     7     8     9``row3    10    11    12    13    14``row4    15    16    17    18    19``row5    20    21    22    23    24`
```

创建的表格数据如下：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

# **二、****直接****用列名索引**

**取一列：**data\['col1'\] ，即取得第一列，得到的是一个Series对象。

**取多列：**data\[\['col1','col2'\]\] ，即取得第一列、第二列，得到的是一个DataFrame对象。

**注 意：**用data\['row1'\] 、data\[0\]、data\[:,0\]、data\[0,:\]、data\[:,'col1':'col2'\] 统统都会报错的，这类命令只能用来**按列名取一列或多列**。

```
`data['col1']``row1     0``row2     5``row3    10``row4    15``row5    20`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

```
`data[['col1','col2']] `` col1  col2``row1     0     1``row2     5     6``row3    10    11``row4    15    16``row5    20    21``#下面的命令直接应用都会报错，但是用loc 和 iloc 就不会报错``data['row1']``data[0]``data[:,0]``data[0,:]``data[:,'col1':'col2']` `#TypeError: '(slice(None, None, None), 0)' is an invalid ke`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

# **三、****直接****用行号索引**

**data\[0:2\]** 代表取得第0行和第1行，**不包含最后一个。**

**注 意：**只取一行的话，要用data\[0:1\]，不能用data\[0\]，data\[0:2,\]也会报错

```
`data[0:2]` `col1  col2  col3  col4  col5``row1     0     1     2     3     4``row2     5     6     7     8     9`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

# **四、iloc按行号、列号索引**

官方：https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.iloc.html

## 1、行索引

**1）取一行 ：**data.iloc\[0\] 、data.iloc\[0,:\]

**2）取多行 ：**data.iloc\[\[0,2\]\] 、data.iloc\[\[0,2\],:\]

**3）取连续多行 ：**data.iloc\[0:2\] 、data.iloc\[0:2,:\]

## 2、列索引

**4）取一列 ：**data.iloc\[:,0\]

**5）取多列 ：**data.iloc\[:,\[0,2\]\]、data.iloc\[:,\[0,2\]\]

**6）取连续多列** ：data.iloc\[:,0:2\]

**注 意：**

取行的时候可以不提列，也可以用 "**，：**" 来指全列

取列的时候必须用"**：，**"来指定全行。

可以使用一个数字来代表一个，可以使用一个列表\[a,b\]代表多个，也可以使用a:b代表连续多个。

```
`data.iloc[0]``col1    0``col2    1``col3    2``col4    3``col5    4`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

```
`data.iloc[:,2:4]` `col3  col4``row1     2     3``row2     7     8``row3    12    13``row4    17    18``row5    22    23`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

```
`data.iloc[:,[2,4]]` `col3  col5``row1     2     4``row2     7     9``row3    12    14``row4    17    19``row5    22    24`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

# **五、loc按行名、列名索引**

官方网址：https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.loc.html

## **1、行索引**

**取一行：**data.**loc**\['row1'\] 、data.**loc**\['row1',:\]

**取多行：**data.**loc**\[\['row1','row2'\]\] 、data.**loc**\[\['row1','row2'\],:\]

**取连续多行：**data.**loc**\['row1':'row2'\] 、data.**loc**\['row1':'row2',:\]

## **2、列索引**

**取一列：**data.**loc**\[:,'col1'\]

**取多列：**data.**loc**\[:,\['row1','row2'\]\]

**取连续多列：**data.**loc**\[:,'row1':'row2'\]

**注 意：**

取行的时候可以不提列，也可以用"**，：**"来指全列。

取列的时候必须用"**：，**"来指定全行。

可以使用一个数字来代表一个，可以使用一个list \['a','b'\]代表多个，也可以使用'a':'b'代表连续多个。

```
`data.loc[:,'col1':'col3'] `` col1  col2  col3``row1     0     1     2``row2     5     6     7``row3    10    11    12``row4    15    16    17``row5    20    21    22`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

```
`data.loc[:,['col1','col3']]` `col1  col3``row1     0     2``row2     5     7``row3    10    12``row4    15    17``row5    20    22`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

```
`#当索引为整数时，可以用整数进行索引``data = pd.DataFrame(np.arange(25).reshape(5, 5), `` columns=['col1', 'col2','col3','col4', 'col5'])` `col1  col2  col3  col4  col5``0     0     1     2     3     4``1     5     6     7     8     9``2    10    11    12    13    14``3    15    16    17    18    19``4    20    21    22    23    24``data.loc[0:3,'col1':'col3'] `` col1  col2  col3``0     0     1     2``1     5     6     7``2    10    11    12``3    15    16    17`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

# **六、使用iat和at**

iat 和 at 只能取单个元素，iat 使用行、列索引，at 使用行、列名，但是其功能被 iloc 和 loc 包含，因此不推荐。

```
`data.iat[1,2]` `7`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

```
`data.at['row4','col4']` `18`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

# **七、最后总结（重点！！！！）**

正常情况下，推荐使用 iloc 和 loc。最核心的点记住，取行可以不提列，取列必须提行，可以用一个数字，一个list，或者一个区间来取行列。ix新版的已经弃用了，所以可以不用太关注。

**···  END  ···**

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

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)


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

实战：使用 PyTorch 和 OpenCV 实现实时目标检测系统

...

小白学视觉

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___484fb801b7414ea5b.bmp"/>

Scan to Follow