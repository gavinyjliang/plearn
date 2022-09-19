# 强烈推荐！8个让 pandas 更高效的 option 设置

<a id="profileBt"></a><a id="js_name"></a>数据分析与开发 *2021-07-01 12:10*

The following article is from Python数据科学 Author 东哥起飞

<a id="copyright_info"></a>[![](../../../_resources/0_ba06fc71884949e29651645c5194642c.jpg)<br>**Python数据科学** .<br>以Python为核心语言，专攻于「数据科学」领域，文章涵盖数据分析，数据挖掘，机器学习等干货内容，分享大量数据挖掘实战项目分析和讲解，以及海量的学习资源。](#)

通过`pandas`的使用，我们经常要交互式地展示表格（`dataframe`）、分析表格。而表格的格式就显得尤为重要了，因为大部分时候如果我们直接展示表格，格式并不是很友好。

其实呢，这些痛点都可以通过`pandas`的`option`来解决。短短几行代码，只要提前配置好，一次设置好，全局生效，perfect！

```
# 使用方法
import pandas as pd
pd.set_option()
pd.get_option()
# 使用属性，例如展示的最大行数
pd.option.display.max_rows

```

文章整理了8个常用的配置选项，供大家参考。记住这8个option代码，下次直接粘贴进去，效率可以提高很多，爽歪歪。

- 显示更多行
    
- 显示更多列
    
- 改变列宽
    
- 设置float列的精度
    
- 数字格式化显示
    
- 更改绘图方法
    
- 配置info()的输出
    
- 打印出当前设置并重置所有选项
    

## 1\. 显示更多行

默认情况下，`pandas` 是不超出屏幕的显示范围的，如果表的行数很多，它会截断中间的行只显示一部分。我们可以通过设置`display.max_rows`来控制显示的最大行数，比如我想设置显示200行。

```
pd.set_option('display.max_rows', 200)
# pd.options.display.max_rows = 200

```

如果行数超过了`display.max_rows`，那么`display.min_rows`将确定显示的部分有多少行。因为`display.min_rows`的默认行数为5，,下面例子只显示前5行和最后5行，中间的所有行省略。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

同理，也可根据自己的习惯显示可显示的行数，比如10, 20..

```
pd.set_option('display.min_rows', 10)
# pd.options.display.min_rows = 10

```

还可以直接重置。

```
# 重置
pd.reset_option('display.max_rows')

```

## 2\. 显示更多列

行可以设置，同样的列也可以设置，`display.max_columns`控制着可显示的列数，默认值为20。

```
pd.get_option('display.max_columns') 
# pd.options.display.max_columns
20

```

<img width="677" height="107" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__7746b47a624d4085a.png"/>

## 3\. 改变列宽

`pandas`对列中显示的字符数有一些限制，默认值为50字符。所以，有的值字符过长就会显示省略号。如果想全部显示，可以设置`display.max_colwidth`，比如设置成500。

```
pd.set_option ('display.max_colwidth',500)
# pd.options.display.max_colwidth = 500

```

<img width="677" height="132" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__3ca7586a8f1a435d9.png"/>

## 4\. 设置float列的精度

对于float浮点型数据，`pandas`默认情况下只显示小数点后6位。我们可以通过预先设置`display.precision`让其只显示2位，避免后面重复操作。

```
pd.set_option( 'display.precision',2)
# pd.options.display.precision = 2

```

<img width="677" height="287" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__67c1a8fa970e42d1b.png"/>

这个设置不影响底层数据，它只影响浮动列的显示。

## 5\. 数字格式化显示

`pandas`中有一个选项`display.float_formatoption`可以用来格式化任何浮点列。这个仅适用于浮点列，对于其他数据类型，必须将它们转换为浮点数才可以。

### 用逗号格式化大值数字

例如 1200000 这样的大数字看起来很不方便，所以我们用逗号进行分隔。

```
pd.set_option('display.float_format','{:,}'.format)

```

<img width="677" height="264" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__b84b73a4e7914e018.png"/>

### 设置数字精度

和上面`display.precision`有点类似，假如我们只关心小数点后的2位数字，我们可以这样设置格式化：

```
pd.set_option('display.float_format',  '{:,.2f}'.format)

```

<img width="677" height="278" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__4ccec31c25f64d329.png"/>

### 百分号格式化

如果我们要显示一个百分比的列，可以这样设置。

```
pd.set_option('display.float_format', '{:.2f}%'.format)

```

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__ad92082e3ad74eb6b.png)

或者其它币种的符号等均可，只需要在大括号`{}`前后添加即可。

## 6\. 更改绘图方法

默认情况下，`pandas`使用`matplotlib`作为绘图后端。从 0.25 版本开始，`pandas`提供了使用不同后端选择，比如`plotly`，`bokeh`等第三方库，但前提是你需要先安装起来。

设置很简单，只要安装好三方库后，同样只需要一行。

```
import pandas as pd
import numpy as np
pd.set_option('plotting.backend', 'altair')
data = pd.Series(np.random.randn(100).cumsum())
data.plot()

```

## 7\. 配置info()的输出

`pandas`中我们经常要使用`info()`来快速查看`DataFrame`的数据情况。但是，`info`这个方法对要分析的最大列数是有默认限制的，并且如果数据集中有`null`，那么在大数据集计数统计时会非常慢。

`pandas`提供了两种选择：

- `display.max_info_columns`: 设置要分析的最大列数，默认为100。
    
- `display.max_info_rows`: 设置计数null时的阈值，默认为1690785。
    

比如，在分析有 150 个特征的数据集时，我们可以设置`display.max_info_columns`为涵盖所有列的值，比如将其设置为 200：

```
pd.set_option('display.max_info_columns', 200)

```

在分析大型数据集时，df.info()由于要计算所有`null`，导致速度很慢。因此我们可以简单地设置`display.max_info_rows`为一个小的值来避免计数，例如只在行数不超过5时才计数`null`：

```
pd.set_option('display.max_info_rows', 5)

```

## 8\. 打印出当前设置并重置所有选项

`pd.describe_option()`将打印出设置的描述及其当前值。

```
pd.describe_option()

```

<img width="677" height="250" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__db21885bdec442009.png"/>

还可以打印特定的选项，例如，行显示。

```
# 具体的搜索
pd.describe_option('rows')

```

<img width="677" height="297" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__537aee1102bc441ab.png"/>

最后，我们还可以直接全部重置。

```
pd.reset_option('all')

```

**以上就是8个常用`set_option`的使用，下面进行了汇总，方便大家粘贴使用。**

```
pd.set_option('display.max_rows',xxx) # 最大行数
pd.set_option('display.min_rows',xxx) # 最小显示行数
pd.set_option('display.max_columns',xxx) # 最大显示列数
pd.set_option ('display.max_colwidth',xxx) #最大列字符数
pd.set_option( 'display.precision',2) # 浮点型精度
pd.set_option('display.float_format','{:,}'.format) #逗号分隔数字
pd.set_option('display.float_format',  '{:,.2f}'.format) #设置浮点精度
pd.set_option('display.float_format', '{:.2f}%'.format) #百分号格式化
pd.set_option('plotting.backend', 'altair') # 更改后端绘图方式
pd.set_option('display.max_info_columns', 200) # info输出最大列数
pd.set_option('display.max_info_rows', 5) # info计数null时的阈值
pd.describe_option() #展示所有设置和描述
pd.reset_option('all') #重置所有设置选项
```

参考：

\[1\] https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.set_option.html

\[2\] https://towardsdatascience.com/8-commonly-used-pandas-display-options-you-should-know-a832365efa95

\- EOF -

推荐阅读  点击标题可跳转

<ins>1、[聊聊 Pandas 的前世今生](http://mp.weixin.qq.com/s?__biz=MzA5ODM5MDU3MA==&mid=2650872980&idx=1&sn=e89ee0bb457c8dbd87def3effc96c0ed&chksm=8b67fbd1bc1072c7f20eec3d676893f34070635c4495a704a037d074be3feff3e5891b12b2f7&scene=21#wechat_redirect)</ins>

<ins>2、[一文搞定 Pandas 聚合统计](http://mp.weixin.qq.com/s?__biz=MzA5ODM5MDU3MA==&mid=2650872985&idx=2&sn=3db879fa714a9039b0944c5aa880d2aa&chksm=8b67fbdcbc1072ca226cd5cb13df652ba706bba6184292c43d72383567690f4c19bb9499f10c&scene=21#wechat_redirect)</ins>

<ins>3、[太秀了！用 Pandas 秒秒钟搞定 24 张 Excel 报表，还做了波投放分析！](http://mp.weixin.qq.com/s?__biz=MzA5ODM5MDU3MA==&mid=2650873096&idx=2&sn=38a98bbde5aa0fb0049cfe9775e24770&chksm=8b67fa4dbc10735bbbefce9fdd7a413048fb934e63b83424e10c778cdf929df06a8eeabf96d6&scene=21#wechat_redirect)</ins>

看完本文有收获？请转发分享给更多人

**推荐关注「数据分析与开发」，提升数据技能**

![](../../../_resources/0_wx_fmt_png_8f9581c8ddeb445e92d63ff2d51a384c.png)

**数据分析与开发**

「数据分析与开发」分享数据分析与开发相关技术文章、教程、工具

<a id="js_profile_article"></a>55篇原创内容

Official Account

点赞和在看就是最大的支持❤️

People who liked this content also liked

我学习 Python 的三个神级网站

数据分析与开发

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___05815387fe704673a.bmp"/>

Scan to Follow