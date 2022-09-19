# 四行代码秒解微积分！Python这个模块神了！

<a id="profileBt"></a><a id="js_name"></a>快学Python *2022-05-05 23:56* *Posted on <a id="js_ip_wording"></a>北京*

The following article is from Python实用宝典 Author Ckend

<a id="copyright_info"></a>[![](../../../_resources/0_17c1fc7f4db647d4a2bfa1915867f677.jpg)<br>**Python实用宝典** .<br>如此Python，怎能不爱](#)

<img width="677" height="136" src="../../../_resources/640_wx_fmt_gif_wxfrom_5_wx_lazy__2e3fd3b217994552a.gif"/>

人生苦短，快学Python！

之前我们分享过很多[有用有趣的Python库](https://mp.weixin.qq.com/mp/appmsgalbum?__biz=MzI2MDY2NzQwMQ==&action=getalbum&album_id=1882957746111627266#wechat_redirect)，点击蓝字可以查看，今天继续介绍一个：

SymPy 是一个Python库，专注于符号数学，它的目标是成为一个全功能的计算机代数系统，同时保持代码简洁、易于理解和扩展。

举一个简单的例子，比如说展开二次方程：

```
from sympy import *
x = Symbol('x')
y = Symbol('y')
d = ((x+y)**2).expand()
print(d)
# 结果：x**2 + 2*x*y + y**2
```

你可以随便输入表达式，即便是十次方，它都能轻易的展开，非常方便：

```
from sympy import *
x = Symbol('x')
y = Symbol('y')
d = ((x+y)**10).expand()
print(d)
# 结果：x**10 + 10*x**9*y + 45*x**8*y**2 + 120*x**7*y**3 + 210*x**6*y**4 + 252*x**5*y**5 + 210*x**4*y**6 + 120*x**3*y**7 + 45*x**2*y**8 + 10*x*y**9 + y**10
```

下面就来讲讲这个模块的具体使用方法和例子。

***1.准备***

**请选择以下任一种方式输入命令安装依赖**：

1\. Windows 环境 打开 Cmd (开始-运行-CMD)。
2\. MacOS 环境 打开 Terminal (command+空格输入Terminal)。
3\. 如果你用的是 VSCode编辑器 或 Pycharm，可以直接使用界面下方的Terminal.

```
pip install Sympy
```

***2.基本使用***

**简化表达式(化简)**

sympy支持三种化简方式，分别是普通化简、三角化简、指数化简。

普通化简 simplify( )：

```
from sympy import *
x = Symbol('x')
d = simplify((x**3 + x**2 - x - 1)/(x**2 + 2*x + 1))
print(d)
# 结果：x - 1
```

三角化简 trigsimp( )：

```
from sympy import *
x = Symbol('x')
d = trigsimp(sin(x)/cos(x))
print(d)
# 结果：tan(x)
```

指数化简 powsimp( )：

```
from sympy import *
x = Symbol('x')
a = Symbol('a')
b = Symbol('b')
d = powsimp(x**a*x**b)
print(d)
# 结果：x**(a + b)
```

**解方程 solve()**

第一个参数为要解的方程，要求右端等于0，第二个参数为要解的未知数。

如一元一次方程：

```
from sympy import *
x = Symbol('x')
d = solve(x * 3 - 6, x)
print(d)
# 结果：[2]
```

二元一次方程：

```
from sympy import *
x = Symbol('x')
y = Symbol('y')
d = solve([2 * x - y - 3, 3 * x + y - 7],[x, y])
print(d)
# 结果：{x: 2, y: 1}
```

**求极限 limit()**

dir=’+’表示求解右极限，dir=’-‘表示求解左极限：

```
from sympy import *
x = Symbol('x')
d = limit(1/x,x,oo,dir='+')
print(d)
# 结果：0
d = limit(1/x,x,oo,dir='-')
print(d)
# 结果：0
```

**求积分 integrate( )**

先试试求解不定积分：

```
from sympy import *
x = Symbol('x')
d = integrate(sin(x),x)
print(d)
# 结果：-cos(x)
```

再试试定积分：

```
from sympy import *
x = Symbol('x')
d = integrate(sin(x),(x,0,pi/2))
print(d)
# 结果：1
```

**求导 diff()**

使用 diff 函数可以对方程进行求导：

```
from sympy import *
x = Symbol('x')
d = diff(x**3,x)
print(d)
# 结果：3*x**2
d = diff(x**3,x,2)
print(d)
# 结果：6*x
```

**解微分方程 dsolve( )**

以 y′=2xy 为例:

```
from sympy import *
x = Symbol('x')
f = Function('f')
d = dsolve(diff(f(x),x) - 2*f(x)*x,f(x))
print(d)
# 结果：Eq(f(x), C1*exp(x**2))
```

***3.实战一下***

今天群里有同学问了这个问题，“大佬们，我想问问，如果这个积分用Python应该怎么写呢，谢谢大家”：

<img width="375" height="312" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_f7534ab4f29f4a0c9.jpg"/>

```
# Python 实用宝典
from sympy import *
x = Symbol('x')
y = Symbol('y')
d = integrate(x-y, (y, 0, 1))
print(d)
# 结果：x - 1/2
```

为了计算这个结果，integrate的第一个参数是公式，第二个参数是积分变量及积分范围下标和上标。

运行后得到的结果便是 x - 1/2 与预期一致。

如果大家也有求解微积分、复杂方程的需要，可以试试sympy，它几乎是完美的存在。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

鸿蒙HarmonyOS应用开发从入门到精通：华为OpenHarmony首席架构师力荐的HarmonyOS开发宝典！

这本书应该是市面上最厚的“鸿蒙”专业书籍之一了，非常具有竞争力。这本书有个特色，就是实战例子特别多，全书共有75个实战例子、4个综合案例。各章节由架构、原理、使用示范、实战例子组成，不愧是本指导读者从入门到精通的好书。

推荐阅读    点击标题可跳转

[🔥7个实用的Python自动化代码，别再重复造轮子了！](http://mp.weixin.qq.com/s?__biz=MzI2MDY2NzQwMQ==&mid=2247495894&idx=1&sn=d34c7e48ae5fd0fce093f3cc766a390f&chksm=ea6491b0dd1318a640fba4eef121ddac0e1206565534e0d5faf42c2af4028f993b15b0801a78&scene=21#wechat_redirect)

[10个有趣的Python高级脚本，建议收藏！](http://mp.weixin.qq.com/s?__biz=MzI2MDY2NzQwMQ==&mid=2247495887&idx=2&sn=69ccd0c9b108057a368bf811ddc75ea9&chksm=ea6491a9dd1318bf50bb985da36a1de5a5acea3c008a141497bd43e950d91fd76478e96cbcf3&scene=21#wechat_redirect)

[40000字!全网最强Matplotlib实操指南!](http://mp.weixin.qq.com/s?__biz=MzI2MDY2NzQwMQ==&mid=2247495876&idx=2&sn=4cc73b8e18b3dc289b23a90498402566&chksm=ea6491a2dd1318b4133b1856d56b058d7a66428e2902d0ec5a35107e78c8b2326a1360085dbd&scene=21#wechat_redirect)

[Python竟然能把“长的”变成“短的”？](http://mp.weixin.qq.com/s?__biz=MzI2MDY2NzQwMQ==&mid=2247495843&idx=1&sn=5ca29085fcad627b89480c0741687434&chksm=ea6491c5dd1318d3b0171ebad01e3ed08986c909e63f34351cdb0bafbcd517710679eb9623a4&scene=21#wechat_redirect)[](http://mp.weixin.qq.com/s?__biz=MzI2MDY2NzQwMQ==&mid=2247495894&idx=1&sn=d34c7e48ae5fd0fce093f3cc766a390f&chksm=ea6491b0dd1318a640fba4eef121ddac0e1206565534e0d5faf42c2af4028f993b15b0801a78&scene=21#wechat_redirect)

[🔥60 种数据图表，制作工具和使用场景](https://mp.weixin.qq.com/s?__biz=MzU5Nzg5ODQ3NQ==&mid=2247521644&idx=2&sn=9b5d0b8aa4f1540f60cd1a1e1606bf5c&scene=21#wechat_redirect)

[BI做表sop](https://mp.weixin.qq.com/s?__biz=MzU5Nzg5ODQ3NQ==&mid=2247521644&idx=3&sn=8a7bd422233cb1d9b211c1529a5f88e9&scene=21#wechat_redirect)

<img width="677" height="278" src="../../../_resources/640_wx_fmt_gif_wxfrom_5_wx_lazy__c5cd682b327241b79.gif"/>

<img width="30" height="30" src="../../../_resources/640_wx_fmt_gif_wxfrom_5_wx_lazy__a90caf32553b417a9.gif"/>

点击这里，阅读更多

Python

文章！

<a id="js_view_source"></a>Read more

People who liked this content also liked

像Python一样玩C/C++

...

光城

不看的原因

- 内容质量低
- 不看此公众号

Python 全自动解密解码神器 — Ciphey

...

Python实用宝典

不看的原因

- 内容质量低
- 不看此公众号

7\. 前端工程化打包篇：webpack 是如何动态将 js/css 资源路径注入 html 中的

...

互联网大厂面试

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___6169253abab14f8fb.bmp"/>

Scan to Follow