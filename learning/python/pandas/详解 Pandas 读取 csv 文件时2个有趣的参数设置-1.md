# 详解 Pandas 读取 csv 文件时2个有趣的参数设置

<a id="profileBt"></a><a id="js_name"></a>AI科技大本营 *2022-05-07 18:00* *Posted on <a id="js_ip_wording"></a>北京*

The following article is from 小数志 Author luanhz

<a id="copyright_info"></a>[![](../../../_resources/0_b48c2b6a5f8342b29a326d12821adf21.jpg)<br>**小数志** .<br>一个专注于数据科学的公众号！](#)

<img width="661" height="117" src="../../../_resources/640_wx_fmt_gif_wxfrom_5_wx_lazy__188cff20e6b543b19.gif"/>

作者 | luanhz

来源丨小数志

导读

Pandas可能是广大Python数据分析师最为常用的库了，其提供了从数据读取、数据预处理到数据分析以及数据可视化的全流程操作。其中，在数据读取阶段，应用pd.read_csv读取csv文件是常用的文件存储格式之一。今天，本文就来分享关于pandas读取csv文件时2个非常有趣且有用的参数。

打开jupyter lab，键入pd.read_csv?并运行即可查看该API的常用参数注解，主要如下：

<img width="661" height="233" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__d33f6036e9044af18.png"/>

其中大部分参数相信大家都应该已经非常熟悉，本文来介绍2个参数的不一样用法。

给定一个模拟的csv文件，其中主要数据如下：

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__9b174ff912294a9db.png)

可以看到，这个csv文件主要有3列，列标题分别为year、month和day，但特殊之处在于其分隔符不是常规的comma，而是一个冒号。另外也显而易见的是这三列拼凑起来是一个正常的年月日的日期格式。所以今天本文就来分享如何通过这两个参数来实现巧妙的加载和自动解析。

01 sep设置None触发自动解析

既然是csv文件（Comma-Separated Values），所以read_csv的默认sep是","，然而对于那些不是","分隔符的文件，该默认参数下显然是不能正确解析的。此时，当然可以简单的通过传入正确的分隔符作为sep参数来实现正确加载，但如果文件的分隔符是未知的呢？实际上，我们可以无需传入分隔符，而交由解析器自动解析。

查看pd.read_csv中关于sep参数的介绍，可以看到如下说明：

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__48e03b3300914395b.png)

其中，值得注意的有两点：

- sep默认为","，如果传入None，则C引擎由于不能自动检测和解析分隔符，所以Python引擎将会自动应用于解析和检测（当然，C引擎的解析速度要更快一些，所以实际上这两种解析引擎是各有利弊）
    
- 如果sep传入参数超过1个字符，则其将会被视作正则表达式。实际上这也是一个强大的功能，但应用场景不如前者实用
    

基于上述对sep参数的理解，为了正确加载和解析前述的示例文件，只需将传入sep=None即可：

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__79a2f4227577470b8.png)

02 parse_dates实现日期多列拼接

在完成csv文件正确解析的基础上，下面通过parse_dates参数实现日期列的拼接。首先仍然是查看API文档中关于该参数的注解：

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__bdf095badcde4f6fa.png)

其中，可以看出parse_dates参数默认为False，同时支持4种自定义格式的参数的传递，包括：

- 传入bool值，若传入True值，则将尝试解析索引列
    
- 传入列表，并将列表中的每一列尝试解析为日期格式；
    
- 传入嵌套列表，并尝试将每个子列表中的所有列拼接后解析为日期格式；
    
- 出啊如字典，其中key为解析后的新列名，value为原文件中的待解析的列索引的列表，例如示例中{'foo': \[1, 3\]}即是用于将原文件中的1和3列拼接解析，并重命名为foo
    

基于上述理解，完成前面的特殊csv文件中三列拼接解析为日期的需求就非常容易，即将0/1/2列拼接解析就可以了。实现如下：

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__207a7e3b668647ca8.png)

不得不说，pandas提供的这些函数的参数可真够丰富的了！

```


```


<img width="661" height="132" src="../../../_resources/640_wx_fmt_gif_wxfrom_5_wx_lazy__14294d5ad62647c1a.gif"/>



```

往

期

回

顾

技术

[利用Pandas进行分类数据编码的十种方式](http://mp.weixin.qq.com/s?__biz=Mzg4NDQwNTI0OQ==&mid=2247559441&idx=2&sn=4c5fed0415d83397fe026e6bf0251014&chksm=cfbb0c7ff8cc85696a67367b8dfce6f855157b1cd89579119e74e3b1d1651a6b74a2e13d1ae0&scene=21#wechat_redirect)

资讯

[程序员化身“侦探”识破AI律所骗局](http://mp.weixin.qq.com/s?__biz=Mzg4NDQwNTI0OQ==&mid=2247559602&idx=1&sn=0385fa77b6822c878d80b47a37183787&chksm=cfbb0cdcf8cc85ca6fdd88045d4756ba17234e2181d49f2bf8e42cce2dbf986c4c9c719cafb4&scene=21#wechat_redirect)

技术

[Python实现三维姿态估计遮挡预测](http://mp.weixin.qq.com/s?__biz=Mzg4NDQwNTI0OQ==&mid=2247559563&idx=1&sn=5626867386530e1967632ac62908f6a3&chksm=cfbb0ce5f8cc85f3a5c7046a0e653d4cb45bba1349da07c55464713eddb456f915437b08591c&scene=21#wechat_redirect)

技术

[10个有趣的Python高级脚本！](http://mp.weixin.qq.com/s?__biz=Mzg4NDQwNTI0OQ==&mid=2247559235&idx=3&sn=d160e0ea521b785bcc19b5e6544318b8&chksm=cfbb0d2df8cc843b3c74cdb8cc4fe2bc641104243a8af5471c1730a591a7eb120b50e93e9267&scene=21#wechat_redirect)

![Image](https://mmbiz.qpic.cn/mmbiz_png/VeJKXItpwPX2FZVxJbbER98riaKxfbdicAoaWibibhVHicMhT0lJR8XvXs6U1fqAnmhH2q1ooImrAgSIVfNicjNLMzZQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

分享

![Image](https://mmbiz.qpic.cn/mmbiz_png/VeJKXItpwPX2FZVxJbbER98riaKxfbdicAL8bkIiaLIqnn0OY6W10TRgtSMWLMOxicm3q2ibbJ8Y9ybJBsiaqeTqBpqQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

点收藏

![Image](https://mmbiz.qpic.cn/mmbiz_png/VeJKXItpwPX2FZVxJbbER98riaKxfbdicACtBqqOgf8qDicxXJYAynmgBjgqaFYZcH5PwYjibIceYLvXC66FAfxTOQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

点点赞

![Image](https://mmbiz.qpic.cn/mmbiz_png/VeJKXItpwPX2FZVxJbbER98riaKxfbdicA2N4IEaJic9hzA5VlkZ7zyD8Eibe5gRX5iae5ZH6bRf72qDz6v1nnBEpxw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

点在看












```

People who liked this content also liked

牛！一个名叫“简单作图”的R包，瞧瞧它到底有多简单？！

...

R语言和统计

不看的原因

- 内容质量低
- 不看此公众号

浅谈 Python 中的字符串格式化输出

...

AI科技大本营

不看的原因

- 内容质量低
- 不看此公众号

3000帧动画图解MySQL为什么需要binlog、redo log和undo log

...

CoderW

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___72f7f5fd9989449ba.bmp"/>

Scan to Follow