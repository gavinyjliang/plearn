# Python Delorean 优秀的时间格式智能转换工具

<a id="copyright_logo"></a>Original <a id="js_author_name"></a>Ckend <a id="profileBt"></a><a id="js_name"></a>Python实用宝典 *2022-03-07 20:20*

收录于话题

<a id="js_article_tag_name__1537415731615678465"></a>#python <a id="js_article_tag_num__1537415731615678465"></a>45 <a id="js_article_tag_tips__1537415731615678465"></a>个

<a id="js_article_tag_name__2079136972664520705"></a>#时间工具 <a id="js_article_tag_num__2079136972664520705"></a>2 <a id="js_article_tag_tips__2079136972664520705"></a>个

<a id="js_article_tag_name__2079136971976654849"></a>#datetime <a id="js_article_tag_num__2079136971976654849"></a>2 <a id="js_article_tag_tips__2079136971976654849"></a>个

DeLorean是一个Python的第三方模块，基于 pytz 和 dateutil 开发，用于处理Python中日期时间的格式转换。

由于时间转换是一个足够微妙的问题，DeLorean希望为移位、操作和生成日期时间提供一种更干净、更省事的解决方案。比如，实例化字符串形式的时间对象，Delorean只需要 parse 指定字符串，不需要声明其格式就可以进行转换。

至于 Delorean 这个模块名称的由来，Delorean 是电影《回到未来》里的那辆极为炫酷的鸥翼汽车，采用这部电影里的非常具有代表性的汽车的名字作为库名，作者估计也是想表达使用这个库能让你在时空里任意遨游，没有掣肘。

这个模块最让我感到智能的是，它能自动识别字符串的时间格式，转换为 Delorean 对象，而且这个 Delorean 对象和 Datetime 对象是相通的：

```
from delorean import parse
parse("2011/01/01 00:00:00 -0700")
# Delorean(datetime=datetime.datetime(2011, 1, 1, 0, 0), timezone=pytz.FixedOffset(-420))
parse("2018-05-06")
# Delorean(datetime=datetime.datetime(2018, 6, 5, 0, 0), timezone='UTC')
```

下面就来介绍一下它的基本使用方法。

***1.准备***

开始之前，你要确保Python和pip已经成功安装在电脑上，如果没有，可以访问这篇文章：[超详细Python安装指南](http://mp.weixin.qq.com/s?__biz=MzI3MzM0ODU4Mg==&mid=2247485004&idx=1&sn=6f89120cf926e71c7eb4788744ff625f&chksm=eb25e4c5dc526dd31f216f56b963179a0bc301a5654644ef98f436aa4740caa6f2774046296f&scene=21#wechat_redirect) 进行安装。

**(可选1) **如果你用Python的目的是数据分析，可以直接安装Anaconda：[Python数据分析与挖掘好帮手—Anaconda](http://mp.weixin.qq.com/s?__biz=MzI3MzM0ODU4Mg==&mid=2247486014&idx=1&sn=4519422fbd83b5feffcbb21552226bc3&chksm=eb25e8b7dc5261a1aef2fa400ca7bcaa8c06394ea1f9a5860ab02bcf95d4664f41903b12bbd8&scene=21#wechat_redirect)，它内置了Python和pip.

**(可选2) **此外，推荐大家用VSCode编辑器，它有许多的优点：[Python 编程的最好搭档—VSCode 详细指南](http://mp.weixin.qq.com/s?__biz=MzI3MzM0ODU4Mg==&mid=2247485849&idx=1&sn=ec098cf67a55bd1d61d4513397434c94&chksm=eb25eb10dc52620682db716d206c18b00bd53c01743729a9dea381e1791566a04a06f1fabca5&scene=21#wechat_redirect)。

**请选择以下任一种方式输入命令安装依赖**：
1\. Windows 环境 打开 Cmd (开始-运行-CMD)。
2\. MacOS 环境 打开 Terminal (command+空格输入Terminal)。
3\. 如果你用的是 VSCode编辑器 或 Pycharm，可以直接使用界面下方的Terminal.

```
pip install Delorean
```

***2.Delorean 基础使用***

轻松获取当前时间：

```
from delorean import Delorean
d = Delorean()
print(d)
# Delorean(datetime=datetime.datetime(2021, 10, 6, 9, 5, 57, 611589), timezone='UTC')
```

将datetime格式的时间转化为Delorean：

```
import datetime
from delorean import Delorean
d = Delorean()
print(d)
d = Delorean(datetime=datetime.datetime(2018, 5, 10, 8, 52, 23, 560811), timezone='UTC')
# 这里默认的是UTC时间
print(d)
# Delorean(datetime=datetime.datetime(2021, 10, 6, 9, 5, 57, 611589), timezone='UTC')
# Delorean(datetime=datetime.datetime(2018, 5, 10, 8, 52, 23, 560811), timezone='UTC')
```

转换为国内时区：

```
import datetime
from delorean import Delorean
d = Delorean(datetime=datetime.datetime(2018, 5, 10, 8, 52, 23, 560811), timezone='UTC')
d = d.shift("Asia/Shanghai")
print(d)
# Delorean(datetime=datetime.datetime(2018, 5, 10, 16, 52, 23, 560811), timezone='Asia/Shanghai')
```

输出为 datetime、date 也不在话下:

```
import datetime
from delorean import Delorean
d = Delorean(datetime=datetime.datetime(2018, 5, 10, 8, 52, 23, 560811), timezone='UTC')
d = d.shift("Asia/Shanghai")
print(d.datetime)
print(d.date)
# 2018-05-10 16:52:23.560811+08:00
# 2018-05-10
```

查看时间戳及UTC时间：

```
import datetime
from delorean import Delorean
d = Delorean(datetime=datetime.datetime(2018, 5, 10, 8, 52, 23, 560811), timezone='UTC')
d = d.shift("Asia/Shanghai")
print(d.epoch)
print(d.naive)
# 1525942343.560811
# 2018-05-10 08:52:23.560811
```

用unix时间戳初始化Delorean：

```
from delorean import epoch
d = epoch(1357971038.102223).shift("Asia/Shanghai")
print(d)
# Delorean(datetime=datetime.datetime(2013, 1, 12, 14, 10, 38, 102223), timezone='Asia/Shanghai')
```

Delorean支持timedelta的时间加减法。Delorean可以使用timedelta进行加减，得到一个Delorean对象：

```
import datetime
from delorean import Delorean
d = Delorean(datetime=datetime.datetime(2018, 5, 10, 8, 52, 23, 560811), timezone='UTC')
d = d.shift("Asia/Shanghai")
print(d)
d2 = d + datetime.timedelta(hours=2)
print(d2)
d3 = d - datetime.timedelta(hours=3)
print(d3)
# Delorean(datetime=datetime.datetime(2018, 5, 10, 16, 52, 23, 560811), timezone='Asia/Shanghai')
# Delorean(datetime=datetime.datetime(2018, 5, 10, 18, 52, 23, 560811), timezone='Asia/Shanghai')
# Delorean(datetime=datetime.datetime(2018, 5, 10, 13, 52, 23, 560811), timezone='Asia/Shanghai')
```

***3\. Delorean 高级使用***

通常情况下我们不关心有多少微妙或者多少秒，因此Delorean提供了非常方便的过滤方式：

```
from delorean import Delorean
d = Delorean()
print(d)
# Delorean(datetime=datetime.datetime(2019, 3, 14, 4, 0, 50, 597357), timezone='UTC')
d.truncate('second')
# Delorean(datetime=datetime.datetime(2019, 3, 14, 4, 0, 50), timezone='UTC')
d.truncate('hour')
# Delorean(datetime=datetime.datetime(2019, 3, 14, 4, 0), timezone='UTC')
d.truncate('month')
# Delorean(datetime=datetime.datetime(2019, 3, 1, 0, 0), timezone='UTC')
d.truncate('year')
# Delorean(datetime=datetime.datetime(2019, 1, 1, 0, 0), timezone='UTC')
```

另外，datetime格式的字符串处理的时候转换需要标明各种各样的格式，在Delorean里，我们不需要那么麻烦，直接**parse**就可以了：

```
from delorean import parse
parse("2011/01/01 00:00:00 -0700")
# Delorean(datetime=datetime.datetime(2011, 1, 1, 0, 0), timezone=pytz.FixedOffset(-420))
parse("2018-05-06")
# Delorean(datetime=datetime.datetime(2018, 6, 5, 0, 0), timezone='UTC')
```

我们的文章到此就结束啦，如果你喜欢今天的Python 实战教程，请持续关注Python实用宝典。

有任何问题，可以在公众号后台回复：**加群**，回答相应**红字验证信息**，进入互助群询问。

原创不易，希望你能在下面点个赞和在看支持我继续创作，谢谢！

**点击下方阅读原文可获得更好的阅读体验**

Python实用宝典 (pythondict.com)
不只是一个宝典
欢迎关注公众号：Python实用宝典

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

<a id="js_view_source"></a>Read more

People who liked this content also liked

Vue组件库设计 | Vue3组���在线交互解释器

前端下午茶

不看的原因

- 内容质量低
- 不看此公众号

在Python中使用Greppo构建的地理空间仪表板

大邓和他的Python

不看的原因

- 内容质量低
- 不看此公众号

RISC-V又一开源SoC-zqh_riscv

OpenFPGA

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___4f9c759a87164d2d8.bmp"/>

Scan to Follow