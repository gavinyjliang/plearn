# 好习惯！pandas 8 个常用的 option 设置

<a id="copyright_logo"></a>Original 东哥起飞 <a id="profileBt"></a><a id="js_name"></a>Python数据科学 *2021-06-20 13:45*

<a id="js_article-tag-card__left"></a>收录于合集 #pandas骚操作 <a id="js_article-tag-card__right"></a>25个

大家好，我是东哥。

本篇是pandas100个骚操作系列的第 17 篇：8 个常用的 option 设置

系列内容，请看👉「[pandas100个骚操作](https://mp.weixin.qq.com/mp/appmsgalbum?__biz=MzUzODYwMDAzNA==&action=getalbum&album_id=1699019347278561282&scene=173&from_msgid=2247515707&from_itemidx=4&count=3&nolastread=1#wechat_redirect)」话题，订阅后文章更新可第一时间推送至订阅号。

* * *

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

东哥整理了8个常用的配置选项，供大家参考。记住这8个option代码，下次直接粘贴进去，效率可以提高很多，爽歪歪。

- 显示更多行
    
- 显示更多列
    
- 改变列宽
    
- 设置float列的精度
    
- 数字格式化显示
    
- 更改绘图方法
    
- 配置info()的输出
    
- 打印出当前设置并重置所有选项
    

## 1\. 显示更多行

默认情况下，`pandas` 是不超出屏幕的显示范围的，如果表的行数很多，它会截断中间的行只显示一部分。我们可以通过设置`display.max_rows`来控制显示的最大行数，比如我想设置显示200行。

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

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 3\. 改变列宽

`pandas`对列中显示的字符数有一些限制，默认值为50字符。所以，有的值字符过长就会显示省略号。如果想全部显示，可以设置`display.max_colwidth`，比如设置成500。

```
pd.set_option ('display.max_colwidth',500)
# pd.options.display.max_colwidth = 500

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 4\. 设置float列的精度

对于float浮点型数据，`pandas`默认情况下只显示小数点后6位。我们可以通过预先设置`display.precision`让其只显示2位，避免后面重复操作。

```
pd.set_option( 'display.precision',2)
# pd.options.display.precision = 2

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

这个设置不影响底层数据，它只影响浮动列的显示。

## 5\. 数字格式化显示

`pandas`中有一个选项`display.float_formatoption`可以用来格式化任何浮点列。这个仅适用于浮点列，对于其他数据类型，必须将它们转换为浮点数才可以。

### 用逗号格式化大值数字

例如 1200000 这样的大数字看起来很不方便，所以我们用逗号进行分隔。

```
pd.set_option('display.float_format','{:,}'.format)

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 设置数字精度

和上面`display.precision`有点类似，假如我们只关心小数点后的2位数字，我们可以这样设置格式化：

```
pd.set_option('display.float_format',  '{:,.2f}'.format)

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 百分号格式化

如果我们要显示一个百分比的列，可以这样设置。

```
pd.set_option('display.float_format', '{:.2f}%'.format)

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

或者其它币种的符号等均可，只需要在大括号`{}`前后添加即可。

## 6\. 更改绘图方法

默认情况下，`pandas`使用`matplotlib`作为绘图后端。从 0.25 版本开始，`pandas`提供了使用不同后端选择，比如`plotly`，`bokeh`等第三方库，但前提是你需要先安装起来。

这个东哥之前也分享过设置后端可视化方法的内容：[再见，可视化！你好，pandas！](https://mp.weixin.qq.com/s?__biz=MzUzODYwMDAzNA==&mid=2247515707&idx=4&sn=3b504bcea18b0e6162f81de32444a031&chksm=fad7c936cda040206c60cc49e0dba1b5329a54aa945cb25dff1f73609ab64d46baf39d4bcb70&token=420025874&lang=zh_CN&scene=21#wechat_redirect)

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

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

还可以打印特定的选项，例如，行显示。

```
# 具体的搜索
pd.describe_option('rows')

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

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

**原创不易，欢迎点赞、留言、分享，支持我继续写下去。**

参考：

\[1\] https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.set_option.html

\[2\] https://towardsdatascience.com/8-commonly-used-pandas-display-options-you-should-know-a832365efa95

* * *

我是东哥，最近正在原创👉「[pandas100个骚操作](https://mp.weixin.qq.com/mp/appmsgalbum?__biz=MzUzODYwMDAzNA==&action=getalbum&album_id=1699019347278561282#wechat_redirect)」系列话题，欢迎订阅。订阅后，文章更新可第一时间推送至订阅号，每篇都不错过。

![](../../../_resources/0_wx_fmt_png_6237644247724e7bb3514f653b06fce7.png)

**Python数据科学**

以Python为核心语言，专攻于「数据科学」领域，文章涵盖数据分析，数据挖掘，机器学习等干货内容，分享大量数据挖掘实战项目分析和讲解，以及海量的学习资源。

<a id="js_profile_article"></a>186篇原创内容

Official Account

**推荐阅读**

[机器学习领域必知必会的12种概率分布（附Python代码实现）](http://mp.weixin.qq.com/s?__biz=MzUzODYwMDAzNA==&mid=2247536413&idx=2&sn=9dfd7e430ceedc423b589cbdfff1d7fe&chksm=fad73810cda0b1061624676d16b0467b308b8cee23916758a67f80c3d659cf7b6da17649db0c&scene=21#wechat_redirect)

[深度学习最常用的10个激活函数！](http://mp.weixin.qq.com/s?__biz=MzUzODYwMDAzNA==&mid=2247535988&idx=2&sn=1db1748865d0d2ae1a7d53fff863a927&chksm=fad73a79cda0b36f90c18ed216fd3eca5dd4dac678e23472c3644b5bed7e474c719fbee53ca4&scene=21#wechat_redirect)

[知乎热议！一个博士生接受怎样的训练是完整的科研训练？](http://mp.weixin.qq.com/s?__biz=MzUzODYwMDAzNA==&mid=2247535927&idx=1&sn=ceb8d7e937ccd71664ee1e679d58ac69&chksm=fad73a3acda0b32cc42762d083db4b35e037d9897036f93952ae581e6e9591ae2c60c3aa4da3&scene=21#wechat_redirect)

收录于合集 #<a id="js_album_keep_read_title"></a>pandas骚操作

 <a id="js_album_keep_read_size"></a>25个

<a id="js_album_prev"></a>上一篇 <a id="js_album_keep_read_pre_title"></a>pandas100个骚操作：concat 5 个常用技巧！ <a id="js_album_next"></a>下一篇 <a id="js_album_keep_read_next_title"></a>好习惯！pandas 8 个常用的 index 设置

People who liked this content also liked

超强图解pandas，建议收藏(文末赠书)

...

Python数据科学

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___503c7655fcfd4973a.bmp"/>

Scan to Follow