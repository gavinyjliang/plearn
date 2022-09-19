# 五种 Pandas 图表美化样式汇总

<a id="profileBt"></a><a id="js_name"></a>数据分析与开发 *2021-07-06 12:10*

The following article is from Python大数据分析 Author 朱卫军

<a id="copyright_info"></a>[![](../../../_resources/0_14a278f8e57f48999447de6051acd813.jpg)<br>**Python大数据分析** .<br>分享python编程、可视化设计、大数据分析、机器学习等技术以及数据分析案例，包括但不限于pandas、numpy、spark、matplotlib、sklearn、tensorflow、keras、tableau等](#)

Pandas是一种高效的数据处理库，它以dataframe和series为基本数据类型，呈现出类似excel的二维数据。

在Jupyter中，会美化Pandas的输出。不同于IDE展示的文本形式，Jupyter可以通过CSS修改表格的样式。

我们在做excel表格的时候，常常会对重要数据进行highlight，或者用不同颜色表示数据的大小。这在Pandas中也是可以实现的，而且非常简洁。![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__505f3543078c4566b.png)

Pandas提供了`DataFrame.style`属性，它会返回`Styler`对象，用以数据样式的美化。<img width="675" height="312" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__f180ed6f9cdd4ee39.png"/>

一般的，我们需要将样式函数作为参数传递到下面方法中，就可以实现图表美化。

- Styler.applymap: 作用于元素
    
- Styler.apply:作用于行、列或整个表
    

下面通过一些例子，具体展示常用的美化形式。

## 一、高亮显示

为便于展示，数据示例是用的**2021世界人口数量前十国家数据**。

```
import pandas as pd
data = pd.read_excel(r"E:\\jupyter_notebook\\2021世界人口数据.xlsx")
data

```

<img width="622" height="316" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__960af57b7e9643ffb.png"/>

我们先看下该表的信息:

```
data.info()

```

<img width="327" height="278" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__cf71ea88a67b47ceb.png"/>

除了前两列，其他列都为数字类型。

现在对指定列的最大值进行高亮处理：

```
def highlight_max(s):
    '''
    对列最大值高亮（黄色）处理
    '''
    is_max = s == s.max()
    return ['background-color: yellow' if v else '' for v in is_max]
data.style.apply(highlight_max,subset=['2021人口', '2020人口', '面积','单位面积人口','人口增幅','世界占比'])

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

如果不想对元素背景高亮处理，也可以直接更改指定元素颜色，从而达到突出重点的目的。

标记**单位面积人口列**大于200的元素：

```
def color_red(s):
    is_max = s > 200
    return ['color : red' if v else '' for v in is_max]
data.style.apply(color_red,subset=['单位面积人口'])

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 二、数据条显示

Excel条件格式里，有一个数据条显示方式，用以可视化表达数据大小。

Pandas Style方法中也有数据条的表达形式，用`df.style.bar`来实现。

还是用前面人口数据的例子，我们来看下如何操作数据条。

```
import pandas as pd
data = pd.read_excel(r"E:\\jupyter_notebook\\2021世界人口数据.xlsx")
# 数据条显示指定列数据大小
data.style.bar(subset=['2021人口', '2020人口'], color='#FFA500')

```

<img width="675" height="285" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__808f205e0aa54a33b.png"/>

## 三、色阶显示

色阶也就是热力图，它和数据条一样，都用来表达数据大小。

Pandas Style中色阶的使用也很简单，用`df.style.background_gradient`实现。

```
import seaborn as sns
# 使用seaborn获取颜色
cm = sns.light_palette("green", as_cmap=True)
# 色阶实现
data.style.background_gradient(cmap=cm,subset=['2021人口', '2020人口', '面积','单位面积人口','人口增幅','世界占比'])

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

可以通过选择最大最小颜色比例，调节色阶范围。

调节前：

```
import seaborn as sns
# 色阶实现,这里使用内置色阶类型，不调节颜色范围
data.style.background_gradient(cmap='viridis',high=0.2,low=0.1,subset=['2021人口', '2020人口', '面积','单位面积人口','人口增幅','世界占比'])

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

调节后：

```
import seaborn as sns
# 色阶实现,这里使用内置色阶类型，调节颜色范围
data.style.background_gradient(cmap='viridis',high=0.5,low=0.3,subset=['2021人口', '2020人口', '面积','单位面积人口','人口增幅','世界占比'])

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 四、百分比显示

有些数字需要百分比显示才能准确表达，比如说人口数据里的人口增幅、世界占比。

Pandas可以数据框中显示百分比，通过`Styler.format`来实现。

```
data.style.format("{:.2%}",subset=['人口增幅','世界占比'])

```

<img width="630" height="321" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__77aa7227cb2b46f0b.png"/>

## 五、标记缺失值

数据集中可能会存在缺失值，如果想突出显示缺失值，该怎么操作？

这里有好几种常用的方法，一是用`-`符号替代，二是高亮显示

先创建一个带缺失值的表，还是用人口数据。

```
import pandas as pd
import numpy as np
data = pd.read_excel(r"E:\\jupyter_notebook\\2021世界人口数据.xlsx")
data.iloc[1, 4] = np.nan
data.iloc[3, 1] = np.nan
data.iloc[6, 6] = np.nan
data

```

<img width="616" height="320" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__e29db64ca4414670a.png"/>

上面数据中有三个缺失值，我们用`-`符号替代缺失值：

```
data.style.format(None, na_rep="-")

```

<img width="656" height="317" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__090f1c8f383340e8b.png"/>

再试试对缺失值高亮显示：

```
data.style.highlight_null(null_color='red')

```

<img width="656" height="318" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__7f3d1fa30a784007a.png"/>

## 附：将样式输出到excel

Pandas中的数据美化样式不仅可以展示在notebook中，还可以输出到excel。

这里使用to_excel方法，并用openpyxl作为内核

```
import pandas as pd
import numpy as np
data = pd.read_excel(r"E:\\jupyter_notebook\\2021世界人口数据.xlsx")
data.style.background_gradient(cmap='viridis',subset=['2021人口', '2020人口', '面积','单位面积人口','人口增幅','世界占比']).\
                              to_excel('style.xlsx', engine='openpyxl')

```

<img width="677" height="214" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__a09a819f07024fb9b.png"/>

> ❝
> 
> 本文参考Pandas官方文档Styling章节

\- EOF -

推荐阅读  点击标题可跳转

<ins>1、[强烈推荐！8个让 pandas 更高效的 option 设置](http://mp.weixin.qq.com/s?__biz=MzA5ODM5MDU3MA==&mid=2650873166&idx=2&sn=dd2213a66b260180c2c491d6fa69e100&chksm=8b67fa0bbc10731d5b6ec2a7c98b1a18a81e696d77f0ba5633137ce8c0b8df42a0a4706f3162&scene=21#wechat_redirect)</ins>

<ins>2、[聊聊 Pandas 的前世今生](http://mp.weixin.qq.com/s?__biz=MzA5ODM5MDU3MA==&mid=2650872980&idx=1&sn=e89ee0bb457c8dbd87def3effc96c0ed&chksm=8b67fbd1bc1072c7f20eec3d676893f34070635c4495a704a037d074be3feff3e5891b12b2f7&scene=21#wechat_redirect)</ins>

<ins>3、[太秀了！用 Pandas 秒秒钟搞定 24 张 Excel 报表，还做了波投放分析！](http://mp.weixin.qq.com/s?__biz=MzA5ODM5MDU3MA==&mid=2650873096&idx=2&sn=38a98bbde5aa0fb0049cfe9775e24770&chksm=8b67fa4dbc10735bbbefce9fdd7a413048fb934e63b83424e10c778cdf929df06a8eeabf96d6&scene=21#wechat_redirect)</ins>

看完本文有收获？请转发分享给更多人

**推荐关注「数据分析与开发」，提升数据技能**

![](../../../_resources/0_wx_fmt_png_8397cb50d7ad42aabe19752cbea1b275.png)

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

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___ceb8056bf3f141b8b.bmp"/>

Scan to Follow