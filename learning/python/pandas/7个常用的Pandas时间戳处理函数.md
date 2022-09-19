# 7个常用的Pandas时间戳处理函数

<a id="profileBt"></a><a id="js_name"></a>数据STUDIO *2022-05-11 08:35* *Posted on <a id="js_ip_wording"></a>四川*

![Image](../../../_resources/640_wx_fmt_gif_wxfrom_5_wx_lazy__8dcd3f4c36c1404c8.gif)

<a id="js_appmsg_search_keywords_head"></a>![](../../../_resources/0_wx_fmt_png_2c10e768ef1b49eda61206f97f540da4.png)<a id="appmsg_search_keywords_nickname"></a>数据STUDIO<a id="appmsg_search_keywords_title"></a>推荐搜索<a id="appmsg_search_keywords_tips"></a>关键词列表：<a id="appmsg_search_keywords_keywords_list"></a>PythonPandasSQL数据分析

<img width="677" height="103" src="../../../_resources/640_wx_fmt_gif_wxfrom_5_wx_lazy__fce06f97583041428.gif"/>

在零售、经济和金融等行业，数据总是由于货币和销售而不断变化，生成的所有数据都高度依赖于时间。 如果这些数据没有时间戳或标记，实际上很难管理所有收集的数据。Python 程序允许我们使用 NumPy timedelta64 和 datetime64 来操作和检索时间序列数据。 sklern库中也提供时间序列功能，但 pandas 为我们提供了更多且好用的函数。

Pandas 库中有四个与时间相关的概念

- **日期时间**：日期时间表示特定日期和时间及其各自的时区。它在 pandas 中的数据类型是 datetime64\[ns\] 或 datetime64\[ns, tz\]。
    
- **时间增量**：时间增量表示时间差异，它们可以是不同的单位。示例："天、小时、减号"等。换句话说，它们是日期时间的子类。
    
- **时间跨度**：时间跨度被称为固定周期内的相关频率。时间跨度的数据类型是 period\[freq\]。
    
- **日期偏移**：日期偏移有助于从当前日期计算选定日期，日期偏移量在 pandas 中没有特定的数据类型。
    

时间序列分析至关重要，因为它们可以帮助我们了解随着时间的推移影响趋势或系统模式的因素。在数据可视化的帮助下，分析并做出后续决策。前面我们也介绍过几种使用pandas处理时间序列文章，可以戳👇：  
[时间序列 | pandas时间序列基础](http://mp.weixin.qq.com/s?__biz=Mzk0OTI1OTQ2MQ==&mid=2247493668&idx=1&sn=6cdd56ead0a9c84c82b286c09dcc2538&chksm=c359b58ef42e3c9896f01ee6bddef80b5d58d465ed9c8f32ff91d3ef57734ea9f87315b4cc09&scene=21#wechat_redirect)
[时间序列 | 字符串和日期的相互转换](http://mp.weixin.qq.com/s?__biz=Mzk0OTI1OTQ2MQ==&mid=2247493670&idx=1&sn=f2980e9b42f9eb46501e6727ad4d970b&chksm=c359b58cf42e3c9ae6fda2d131b3a6fcb5013380bcc3644e99c69cd1a3de1f75b506d3de2984&scene=21#wechat_redirect)
[时间序列 | 重采样及频率转换](http://mp.weixin.qq.com/s?__biz=Mzk0OTI1OTQ2MQ==&mid=2247493672&idx=1&sn=e037e5248b88a796cf3a1d7ba5f109a3&chksm=c359b582f42e3c942db51a3aaff5571bc36f9b2d817590e355f146653fe7b028892457a32b5f&scene=21#wechat_redirect)
[时间序列 | 时期(Period)及其算术运算](http://mp.weixin.qq.com/s?__biz=Mzk0OTI1OTQ2MQ==&mid=2247493671&idx=1&sn=25d49d8f9f88b184d9b4b48c2510d26c&chksm=c359b58df42e3c9b63f51a363d7e4dd2ec759faf67b259f56e70712a188d6a63d10186cae835&scene=21#wechat_redirect)

现在我们接续看几个使用这些函数的例子。

## 1、查找特定日期的某一天的名称

```
import pandas as pd 
day = pd.Timestamp('2021/1/5') 
day.day_name()

```

```
'Tuesday'

```

上面的程序是显示特定日期的名称。第一步是导入 panda 的并使用 Timestamp 和 day\_name 函数。"Timestamp"功能用于输入日期，"day\_name"功能用于显示指定日期的名称。

## 2、执行算术计算

```
import pandas as pd 
 
day = pd.Timestamp('2021/1/5') 
day1 = day + pd.Timedelta("3 day") 
day1.day_name() 
 
day2 = day1 + pd.offsets.BDay() 
day2.day_name()

```

```
'Monday'

```

在第一个代码中，显示三天后日期名称。"Timedelta"功能允许输入任何天单位（天、小时、分钟、秒）的时差。

在第二个代码中，使用"offsets.BDay()"函数来显示下一个工作日。换句话说，这意味着在星期五之后，下一个工作日是星期一。

## 3、使用时区信息来操作转换日期时间

获取时区的信息

```
import pandas as pd 
import numpy as np 
from datetime import datetime 
 
dat_ran = dat_ran.tz_localize("UTC") 
dat_ran

```

```
DatetimeIndex(
['2021-01-01 00:00:00+00:00', '2021-01-01 00:01:00+00:00',
 '2021-01-01 00:02:00+00:00', '2021-01-01 00:03:00+00:00',
 '2021-01-01 00:04:00+00:00', '2021-01-01 00:05:00+00:00',
 '2021-01-01 00:06:00+00:00', '2021-01-01 00:07:00+00:00',
 '2021-01-01 00:08:00+00:00', '2021-01-01 00:09:00+00:00',
 ...
 '2021-01-04 23:51:00+00:00', '2021-01-04 23:52:00+00:00',
 '2021-01-04 23:53:00+00:00', '2021-01-04 23:54:00+00:00',
 '2021-01-04 23:55:00+00:00', '2021-01-04 23:56:00+00:00',
 '2021-01-04 23:57:00+00:00', '2021-01-04 23:58:00+00:00',
 '2021-01-04 23:59:00+00:00', '2021-01-05 00:00:00+00:00'],             
dtype='datetime64[ns, UTC]',
length=5761, freq='T')

```

**转换为美国时区**

```
dat_ran.tz_convert("US/Pacific")

```

```
DatetimeIndex(
['2020-12-31 16:00:00-08:00', '2020-12-31 16:01:00-08:00',
 '2020-12-31 16:02:00-08:00', '2020-12-31 16:03:00-08:00',
 '2020-12-31 16:04:00-08:00', '2020-12-31 16:05:00-08:00',
 '2020-12-31 16:06:00-08:00', '2020-12-31 16:07:00-08:00',
 '2020-12-31 16:08:00-08:00', '2020-12-31 16:09:00-08:00',
 ...
 '2021-01-04 15:51:00-08:00', '2021-01-04 15:52:00-08:00',
 '2021-01-04 15:53:00-08:00', '2021-01-04 15:54:00-08:00',
 '2021-01-04 15:55:00-08:00', '2021-01-04 15:56:00-08:00',
 '2021-01-04 15:57:00-08:00', '2021-01-04 15:58:00-08:00',
 '2021-01-04 15:59:00-08:00', '2021-01-04 16:00:00-08:00'],
dtype='datetime64[ns, US/Pacific]', 
length=5761, freq='T')

```

代码的目标是更改日期的时区。首先需要找到当前时区。这是"tz\_localize()"函数完成的。我们现在知道当前时区是"UTC"。使用"tz\_convert()"函数，转换为美国/太平洋时区。

## 4、使用日期时间戳

```
import pandas as pd 
import numpy as np 
from datetime import datetime 
dat_ran = pd.date_range(start = '1/1/2021', end = '1/5/2021', freq = 'Min') 
print(type(dat_ran[110]))

```

```
<class 'pandas._libs.tslibs.timestamps.
Timestamp'>

```

## 5、创建日期系列

```
import pandas as pd 
import numpy as np 
from datetime import datetime 
dat_ran = pd.date_range(start = '1/1/2021', end = '1/5/2021', freq = 'Min') 
print(dat_ran)

```

```
DatetimeIndex(
['2021-01-01 00:00:00', '2021-01-01 00:01:00',
 '2021-01-01 00:02:00', '2021-01-01 00:03:00',
 '2021-01-01 00:04:00', '2021-01-01 00:05:00',
 '2021-01-01 00:06:00', '2021-01-01 00:07:00',
 '2021-01-01 00:08:00', '2021-01-01 00:09:00',
 ...
 '2021-01-04 23:51:00', '2021-01-04 23:52:00',
 '2021-01-04 23:53:00', '2021-01-04 23:54:00',
 '2021-01-04 23:55:00', '2021-01-04 23:56:00',
 '2021-01-04 23:57:00', '2021-01-04 23:58:00',
 '2021-01-04 23:59:00', '2021-01-05 00:00:00'],
dtype='datetime64[ns]', 
length=5761, freq='T')

```

上面的代码生成了一个日期系列的范围。使用"date_range"函数，输入开始和结束日期，可以获得该范围内的日期。

## 6、操作日期序列

```
import pandas as pd 
from datetime import datetime 
import numpy as np 
 
dat_ran = pd.date_range(start ='1/1/2019', end ='1/08/2019',freq ='Min') 
df = pd.DataFrame(dat_ran, columns =['date']) 
df['data'] = np.random.randint(0, 100, size =(len(dat_ran))) 
print(df.head(5))

```

```
 date  data
0 2019-01-01 00:00:00    68
1 2019-01-01 00:01:00    77
2 2019-01-01 00:02:00    78
3 2019-01-01 00:03:00    64
4 2019-01-01 00:04:00    42
```

在上面的代码中，使用"DataFrame"函数将字符串类型转换为dataframe。最后`"np.random.randint()"`函数是随机生成一些假定的数据。

## 7、使用时间戳数据对数据进行切片

```
import pandas as pd 
from datetime import datetime 
import numpy as np 
dat_ran = pd.date_range(start ='1/1/2019', end ='1/08/2019', freq ='Min') 
 
df = pd.DataFrame(dat_ran, columns =['date']) 
df['data'] = np.random.randint(0, 100, size =(len(dat_ran))) 
string_data = [str(x) for x in dat_ran] 
 
print(string_data[1:5])

```

```
['2019-01-01 00:01:00', 
'2019-01-01 00:02:00', 
'2019-01-01 00:03:00', 
'2019-01-01 00:04:00']

```

上面代码是是第6条的的延续。在创建dataframe并将其映射到随机数后，对列表进行切片。

最后总结，本文通过示例演示了时间序列和日期函数的所有基础知识。建议参考本文中的内容并尝试pandas中的其他日期函数进行更深入的学习，因为这些函数在我们实际工作中非常的重要。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

##### 🏴‍☠️宝藏级🏴‍☠️ 原创公众号『**数据STUDIO**』内容超级硬核。公众号以Python为核心语言，垂直于数据科学领域，包括可戳👉[**Python**](https://mp.weixin.qq.com/mp/appmsgalbum?__biz=Mzk0OTI1OTQ2MQ==&action=getalbum&album_id=1974978822768771072&scene=173&from_msgid=2247519294&from_itemidx=1&count=3&nolastread=1#wechat_redirect)**｜**[**MySQL**](https://mp.weixin.qq.com/mp/appmsgalbum?__biz=Mzk0OTI1OTQ2MQ==&action=getalbum&album_id=2023684574089658370&scene=173&from_msgid=2247519619&from_itemidx=2&count=3&nolastread=1#wechat_redirect)**｜**[**数据分析**](https://mp.weixin.qq.com/mp/appmsgalbum?__biz=Mzk0OTI1OTQ2MQ==&action=getalbum&album_id=1974978820940054530&scene=173&from_msgid=2247518366&from_itemidx=1&count=3&nolastread=1#wechat_redirect)**｜**[**数据可视化**](https://mp.weixin.qq.com/mp/appmsgalbum?__biz=Mzk0OTI1OTQ2MQ==&action=getalbum&album_id=1974991176839544834&scene=173&from_msgid=2247519244&from_itemidx=1&count=3&nolastread=1#wechat_redirect)**｜**[**机器学习与数据挖掘**](https://mp.weixin.qq.com/mp/appmsgalbum?__biz=Mzk0OTI1OTQ2MQ==&action=getalbum&album_id=1963494160565354497&scene=173&from_msgid=2247512171&from_itemidx=1&count=3&nolastread=1#wechat_redirect)**｜**[**爬虫**](https://mp.weixin.qq.com/mp/appmsgalbum?__biz=Mzk0OTI1OTQ2MQ==&action=getalbum&album_id=2318258648965644288&scene=173&from_msgid=2247518366&from_itemidx=1&count=3&nolastread=1#wechat_redirect) 等，从入门到进阶！

长按👇关注\- 数据STUDIO -设为星标，干货速递

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

<a id="js_view_source"></a>Read more

People who liked this content also liked

感谢ggblanket开发者，使得ggplot2绘图变得更简单

...

R语言与SPSS学习笔记

不看的原因

- 内容质量低
- 不看此公众号

5K+，Pandas读存CSV文件

...

尤而小屋

不看的原因

- 内容质量低
- 不看此公众号

R语言绘图—趣味散点图

...

R语言与医学生

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___806592fadc0e450b8.bmp"/>

Scan to Follow