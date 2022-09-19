# 【Python】浅谈Python中的字符串格式化输出

<a id="profileBt"></a><a id="js_name"></a>机器学习初学者 *2022-05-20 12:00* *Posted on <a id="js_ip_wording"></a>浙江*

The following article is from Python爱好者集中营 Author 欣一

<a id="copyright_info"></a>[![](../../../_resources/0_812bfa4f1d53424bac19c5796ec0a00a.jpg)<br>**Python爱好者集中营** .<br>讲解Python的各方面知识，以及Python实战、面试等知识要点与经验分享](#)

今天小编就和大家来分享一下`Python`当中的字符串格式化输出，相信大多数人也对此有所耳闻，但是`f-string`的格式化输出还是很多不为人所数值的一些特征，因此本篇文章也是希望借此机会来向大家一一说明。

### 时间与日期的输出

`f-string`在形式上是以f或者F修饰符引领的字符串，（f'xxx'或者F'xxxx'），以大括号{}标明被替换的字段。首先，我们来看时间和日期的格式化输出，代码如下

```


import datetime
today = datetime.datetime.today()
print(f"{today:%Y-%m-%d}")



```

output

```
`2022-05-04
`
```

或者是

```
`print(f"{today:%Y}")
`
```

output

```
`2022
`
```

而关于`Python`的时间日期的格式化符号，其中`%Y`表示的是四位数的年份(000-9999)，而`%m`表示的则是月份(01-12),`%d`表示的则是月内中的一天(0-31)，除此之外，还有表示时间的`%H:%M:%S`格式，其中`%H`表示的是24小时制的小时数(0-23)，12小时制的则是`%I`，后面剩余的则是分钟和秒数，这里就不做过多的赘述。

### 打印浮点数更加多样化

打印浮点数的时候，形式也可以是多种多样的，我们可以指定小数点后面保留特定的位数

```


x, y = 10, 25
print(f"x={x:.3f}, y={y:.3f}")



```

output

```
`x=10.000, y=25.000
`
```

我们还可以给整数添加千分位的符号，代码如下

```
`number = 1234567890
print(f"{number: ,}")
`
```

output

```
` 1,234,567,890
`
```

另外还可以在数字的最前面添加上货币符号

```
`number = 254.3463
print(f"{f'${number:.3f}'}")
`
```

output

```
`$254.346
`
```

### 打印字符串时

当`f-string`和字符串结合时，可以与字符串的其他方法联用，代码如下

```
`name = 'ERIC'
print(f'My name is {name.lower()}')
`
```

output

```
`My name is eric
`
```

除了`lower()`方法之外，还有`upper()`方法、`capitalize()`方法、`replace()`方法、`split()`方法等等

```
`name = 'ERIC'
print(f'My name is {name.capitalize()}')
`
```

output

```
`My name is Eric
`
```

### 和`Lambda`函数的结合

最后`f-string`格式化输出也可以和`lambda`函数结合来使用，代码如下

```
`print(f"{(lambda x: x**2)(4)}")
`
```

output

```
`16
`
```

今天小编就和大家来分享一下`Python`当中的字符串格式化输出，相信大多数人也对此有所耳闻，但是`f-string`的格式化输出还是很多不为人所数值的一些特征，因此本篇文章也是希望借此机会来向大家一一说明。

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

用Go重写Node.js服务：项目性能提升5倍，内存减少40%

...

OSC开源社区

不看的原因

- 内容质量低
- 不看此公众号

如何拥有一个兼具CNN的速度、Transformer精度的模型？字节甩出TRT-ViT教你

...

极市平台

不看的原因

- 内容质量低
- 不看此公众号

时间序列数据分析与预测之Python工具汇总

...

AI蜗牛车

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___0b59fd766efe49588.bmp"/>

Scan to Follow