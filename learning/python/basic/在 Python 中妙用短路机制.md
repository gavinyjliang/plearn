# 在 Python 中妙用短路机制

<a id="profileBt"></a><a id="js_name"></a>AI科技大本营 *2022-02-18 18:00*

The following article is from Python大数据分析 Author 费弗里

<a id="copyright_info"></a>[![](../../../_resources/0_68abebb8dee84cc39eaae7fce6009015.jpg)<br>**Python大数据分析** .<br>分享python编程、可视化设计、大数据分析、机器学习等技术以及数据分析案例，包括但不限于pandas、numpy、spark、matplotlib、sklearn、tensorflow、keras、tableau等](#)

<img width="661" height="117" src="../../../_resources/640_wx_fmt_gif_wxfrom_5_wx_lazy__d725678664a649829.gif"/>

作者 | 费弗里

来源 | Python大数据分析

本期我们即将学习的是：`Python中短路机制的妙用`。

不同于物理学中的**「短路」**（Short circuit）那般危险，`Python`中的短路机制非常有用，跟很多其他编程语言中的短路机制作用类似，一句话概括就是一段条件判断表达式在从左到右按顺序执行的过程中，提前确定了表达式的True/False结果，从而终止右边剩余的运算。

让我们通过几个简单的例子总结`Python`中可用的几种短路机制：

- **X or Y**
    

`X or Y`是最常用的短路机制，我们都知道只要`X`或`Y`中至少有一个为`True`时，整段判断表达式就为`True`，譬如下面的例子中，本来`1 / 0`会触发`ZeroDivisionError: division by zero`错误，但因为`or`左边的部分已经逻辑判断为`True`，`Python`的短路机制就会停止后续的执行，直接返回`or`左边的结果：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

而当`or`左边部分逻辑判断为`False`时，则会返回右边部分的结果：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

- **X and Y**
    

类似`X or Y`的机制，`X and Y`会在`X`逻辑判断为`False`时提前终止后续的运算，只返回`X`部分的结果：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

- ****any()****
    

`Python`中的`any()`函数用于接受序列形式的多个等待逻辑判断的部分，并在序列中至少有一个部分逻辑判断为`True`时返回`True`。

而只要`any()`按顺序遇到第一个逻辑判断为`True`的结果，也会触发短路，正如下面的例子中只花费3秒就完成了判断过程，因为循环到`1`时触发了短路：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

- **all()**
    

`Python`中的`all()`函数类似`any()`，会在传入序列中每个部分逻辑判断均为`True`时返回`True`，其也会在按顺序遇到第一个`False`时终止后续运算：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

- **「比较运算符」**
    

`Python`中用于数值大小比较的各个运算符也具有短路机制，从左到右，一旦执行到判断结果为`False`的部分都会终止运算：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

- **「实际使用示例」**
    

当我们的代码中涉及到条件判断，且参与条件判断的值具有一定的**「运算成本」**时，就可以灵活运用短路机制来提升运行效率，譬如我们需要根据用户id信息向多个接口查询其权限，全部满足时将其标记为“超级权限”，就可以利用到短路机制。

这里我们随意写几个具有时间成本的函数作为接口示意：

```
def api1(id_):
    
    time.sleep(1)
    
    return id_ in ['admin1', 'admin2']
def api2(id_):
    
    time.sleep(1)
    
    return id_ in ['admin1', 'admin2', 'su1', 'su2']
def api3(id_):
    
    time.sleep(1)
    
    return id_ not in ['ban1', 'ban2', 'ban3']

```

利用短路机制在用户第一次没有满足条件时就终止后续判断，写法简洁：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

* * *

本期分享结束，大家学起来~👋

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

往

期

回

顾

资讯

[冬奥会夺金背后的杀手锏，是他](http://mp.weixin.qq.com/s?__biz=Mzg4NDQwNTI0OQ==&mid=2247555474&idx=1&sn=6ed705215bb3625a048b489455518252&chksm=cfbafcfcf8cd75ea33af095a6e07bc25e0739ffaf85dcc2f4684d0eaf1dd014f3e2e22db8750&scene=21#wechat_redirect)

资讯

[首个深度强化学习AI，控制核聚变](http://mp.weixin.qq.com/s?__biz=Mzg4NDQwNTI0OQ==&mid=2247555474&idx=2&sn=7f291fc2f8328432af13302ae5d9e85d&chksm=cfbafcfcf8cd75ea68b313492972cef0d5cd2fc22b714ef9bc0b753cc66f8eb60d7e350c8814&scene=21#wechat_redirect)

技术

[真香，邮件分类准确识别垃圾邮件](http://mp.weixin.qq.com/s?__biz=Mzg4NDQwNTI0OQ==&mid=2247555434&idx=1&sn=dc36edd92235a0dbde9c6e55a5f01b2d&chksm=cfbafc04f8cd7512d6b46aa2fec8ba543a4769b1915e2600c2322cc93225731ebc9d549a00fa&scene=21#wechat_redirect)

技术

[程序员元宵节的正确打开方式](http://mp.weixin.qq.com/s?__biz=Mzg4NDQwNTI0OQ==&mid=2247555352&idx=1&sn=99b583ae487835dcf1cb9575dfe342c5&chksm=cfbafc76f8cd75604c0b575f33bd51df3bffeac46693fffaa3f0ba361efbf547bbffaceac2b4&scene=21#wechat_redirect)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

**分享**

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

**点收藏**

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

**点点赞**

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

**点在看**

People who liked this content also liked

C4D Formula 实用函数整理 +克拉尼图案工程

动态玄学 RnD Lab

不看的原因

- 内容质量低
- 不看此公众号

西门子S7-200 Smart Modbus通信介绍与实例编程

爱上PLC

不看的原因

- 内容质量低
- 不看此公众号

SpringCloud 整合 Dubbo 3.X 使用新特性 应用级服务发现

狮子领域 程序圈

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___772f5746ec9b47d09.bmp"/>

Scan to Follow