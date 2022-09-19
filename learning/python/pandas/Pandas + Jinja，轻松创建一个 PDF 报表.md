# Pandas + Jinja，轻松创建一个 PDF 报表

<a id="profileBt"></a><a id="js_name"></a>Python数据之道 *2022-02-26 11:01*

The following article is from 萝卜大杂烩 Author 周萝卜

<a id="copyright_info"></a>[![](../../../_resources/0_34a39ed03bcb4cb8abdb25d64c90fa67.jpg)<br>**萝卜大杂烩** .<br>Do it by yourself！爱好Python，测试，NLP，数据分析，小程序，k8s等技术，期待与你一同成长](#)

![](../../../_resources/0_wx_fmt_png_e13865c6783c4c01b9dd6ebee8abc28c.png)

**Python数据之道**

点击领取《Python知识手册》高清电子版，回复数字 “600” 获取。「Python数据之道」秉承“让数据更有价值”的理念​，聚焦于 Python 数据分析、数据可视化、AI、机器学习、深度学习等领域。

<a id="js_profile_article"></a>235篇原创内容

Official Account

> 来源：萝卜大杂烩

我们都知道，Pandas 擅长处理大量数据并以多种文本和视觉表示形式对其进行总结，它支持将结构输出到 CSV、Excel、HTML、json 等。但是如果我们想将多条数据合并到一个文档中，就有些复杂了。例如，如果要将两个 DataFrames 放在一张 Excel 工作表上，则需要使用 Excel 库手动构建输出。虽然可行，但并不简单。本文将介绍一种将多条信息组合成 HTML 模板，然后使用 Jinja 模板和 WeasyPrint 将其转换为独立 PDF 文档的方法，一起来看看吧~

## 总体流程

如报告文章所示，使用 Pandas 将数据输出到 Excel 文件中的多个工作表或从 pandas DataFrames 创建多个 Excel 文件都非常方便。但是，如果我们想将多条信息组合到一个文件中，那么直接从 Pandas 中完成的简单方法却并不多，下面我们来探索一条可行的简单方法

在本文中，我将使用以下流程来创建多页 PDF 文档

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

这种方法的好处是我们可以将自己的工具替换到此工作流程中。不喜欢用 Jinja？那么可以插入 mako 或其他任何模板工具

## 工具选择

首先，我们使用 HTML 作为模板语言，因为它可能是生成结构化数据并允许设置相对丰富的格式的最简单方法

其次，选择 Jinja 是因为我有使用 Django/Flask 的经验，上手比较容易

这个工具链中最困难的部分是弄清楚如何将 HTML 呈现为 PDF。我觉得目前还没有非常好的解决方案，我这里选择了 WeasyPrint，大家也可以尝试一下其他的工具

## 数据处理

导入模块，读取销售信息

```
`from __future__ import print_function
import pandas as pd
import numpy as np
df = pd.read_excel("sales-funnel.xlsx")
df.head()
`
```

Output:

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

将数据进行透视表汇总处理

```
`sales_report = pd.pivot_table(df, index=["Manager", "Rep", "Product"], values=["Price", "Quantity"],
                           aggfunc=[np.sum, np.mean], fill_value=0)
sales_report.head()
`
```

Output:

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 模板

Jinja 模板非常强大，支持许多高级功能，例如沙盒执行和自动转义等等

Jinja 的另一个不错的功能是它包含多个内置过滤器，这将允许我们以在 Pandas 中难以做到的方式格式化我们的一些数据

为了在我们的应用程序中使用 Jinja，我们需要做 3 件事：

- 创建模板
    
- 将变量添加到模板上下文中
    
- 将模板渲染成 HTML
    

我们先创建一个简单的模板 myreport.html

```
`<!DOCTYPE html>
<html>
<head lang="en">
    <meta charset="UTF-8">
    <title>{{ title }}</title>
</head>
<body>
    <h2>Sales Funnel Report - National</h2>
     {{ national_pivot_table }}
</body>
</html>
`
```

此代码的两个关键部分是 {{ title }} 和 {{ national\_pivot\_table }}。它们本质上是我们在渲染文档时将提供的变量的占位符

要填充这些变量，我们需要创建一个 Jinja 环境并获取我们的模板：

```
`from jinja2 import Environment, FileSystemLoader
env = Environment(loader=FileSystemLoader('.'))
template = env.get_template("myreport.html")
`
```

在上面的示例中，我们假设模板位于当前目录中

另一个关键组件是 env 的创建，这个变量是我们将内容传递给模板的方式。我们创建一个名为 template_var 的字典，其中包含我们要传递给模板的所有变量

变量的名称与我们的模板匹配

```
`template_vars = {"title" : "Sales Funnel Report - National",
                 "national_pivot_table": sales_report.to_html()}
`
```

最后一步是使用输出中包含的变量来呈现 HTML，这将创建一个字符串，我们最终将传递给我们的 PDF 创建引擎

```
`html_out = template.render(template_vars)
`
```

## 生成 PDF

PDF 创建部分也相对简单，我们需要做一些导入并将一个字符串传递给 PDF 生成器

```
`from weasyprint import HTML
HTML(string=html_out).write_pdf("report.pdf")
`
```

此命令会创建一个如下所示的 PDF 报告：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

虽然报告生成了，但是看起来很难看啊，我们来优化下，添加 CSS

这里使用 blue print 的 typography.css 作为我们的 style.css 的基础，它有以下几个优点：

- 它比较小且易于理解
    
- 它可以在 PDF 引擎中工作而不会引发错误和警告
    
- 它包括看起来相当不错的基本表格格式
    

```
`HTML(string=html_out).write_pdf(args.outfile.name, stylesheets=["style.css"])
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

可以看到，仅仅添加一行代码，产生的效果却大大不同

## 更复杂的模板

为了生成更有用的报告，我们将结合上面显示的汇总统计数据，并将报告拆分为每个经理包含一个单独的 PDF 页面

让我们从更新的模板（myreport.html）开始：

```
`<!DOCTYPE html>
<html>
<head lang="en">
    <meta charset="UTF-8">
    <title>{{ title }} </title>
</head>
<body>
<div class="container">
    <h2>Sales Funnel Report - National</h2>
     {{ national_pivot_table }}
    {% include "summary.html" %}
</div>
<div class="container">
    {% for manager in Manager_Detail %}
        <p style="page-break-before: always" ></p>
        <h2>Sales Funnel Report - {{manager.0}}</h2>
        {{manager.1}}
        {% include "summary.html" %}
    {% endfor %}
</div>
</body>
</html>
`
```

我们注意到的第一件事是有一个包含语句，它提到了另一个文件。包含允许我们引入一段 HTML 并在代码的不同部分重复使用它。在这种情况下，摘要包含一些我们希望在每个报告中包含的简单的国家级统计数据，以便管理人员可以将他们的绩效与全国平均水平进行比较。

以下是 summary.html 的样子：

```
`<h3>National Summary: CPUs</h3>
    <ul>
        <li>Average Quantity: {{CPU.0|round(1)}}</li>
        <li>Average Price: {{CPU.1|round(1)}}</li>
    </ul>
<h3>National Summary: Software</h3>
    <ul>
        <li>Average Quantity: {{Software.0|round(1)}}</li>
        <li>Average Price: {{Software.1|round(1)}}</li>
    </ul>
`
```

在此代码段中，看到我们可以访问一些其他变量：CPU 和 Software 。其中每一个都是一个 python 列表，其中包括 CPU 和软件销售的平均数量和价格

还注意到我们使用管道`|`将每个值四舍五入到小数点后 1 位。这是使用 Jinja 过滤器的一个具体示例

还有一个 `for` 循环允许我们在报告中显示每个经理的详细信息。Jinja 的模板语言只包含一个非常小的代码子集，它会改变控制流

## 附加统计信息

下面编写供模板调用的函数和代码

一个简单的汇总函数

```
`def get_summary_stats(df,product):
    """
    For certain products we want National Summary level information on the reports
    Return a list of the average quantity and price
    """
    results = []
    results.append(df[df["Product"]==product]["Quantity"].mean())
    results.append(df[df["Product"]==product]["Price"].mean())
    return results
`
```

创建经理详细信息

```
`manager_df = []
for manager in sales_report.index.get_level_values(0).unique():
    manager_df.append([manager, sales_report.xs(manager, level=0).to_html()])
`
```

最后，使用以下变量调用模板

```
`template_vars = {"title" : "National Sales Funnel Report",
                 "CPU" : get_summary_stats(df, "CPU"),
                 "Software": get_summary_stats(df, "Software"),
                 "national_pivot_table": sales_report.to_html(),
                 "Manager_Detail": manager_df}
# Render our file and create the PDF using our css style file
html_out = template.render(template_vars)
HTML(string=html_out).write_pdf("report.pdf",stylesheets=["style.css"])
`
```

这样我们的 pdf 报表就完成了，整体效果如下

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

完整代码：

```
`from __future__ import print_function
import pandas as pd
import numpy as np
import argparse
from jinja2 import Environment, FileSystemLoader
from weasyprint import HTML
def create_pivot(df, infile, index_list=["Manager", "Rep", "Product"], value_list=["Price", "Quantity"]):
    """
    Create a pivot table from a raw DataFrame and return it as a DataFrame
    """
    table = pd.pivot_table(df, index=index_list, values=value_list,
                           aggfunc=[np.sum, np.mean], fill_value=0)
    return table
def get_summary_stats(df,product):
    """
    For certain products we want National Summary level information on the reports
    Return a list of the average quantity and price
    """
    results = []
    results.append(df[df["Product"]==product]["Quantity"].mean())
    results.append(df[df["Product"]==product]["Price"].mean())
    return results
if __name__ == "__main__":
    parser = argparse.ArgumentParser(description='Generate PDF report')
    parser.add_argument('infile', type=argparse.FileType('r'),
    help="report source file in Excel")
    parser.add_argument('outfile', type=argparse.FileType('w'),
    help="output file in PDF")
    args = parser.parse_args()
    df = pd.read_excel(args.infile.name)
    sales_report = create_pivot(df, args.infile.name)
    manager_df = []
    for manager in sales_report.index.get_level_values(0).unique():
        manager_df.append([manager, sales_report.xs(manager, level=0).to_html()])
    env = Environment(loader=FileSystemLoader('.'))
    template = env.get_template("myreport.html")
    template_vars = {"title" : "National Sales Funnel Report",
                     "CPU" : get_summary_stats(df, "CPU"),
                     "Software": get_summary_stats(df, "Software"),
                     "national_pivot_table": sales_report.to_html(),
                     "Manager_Detail": manager_df}
    html_out = template.render(template_vars)
    HTML(string=html_out).write_pdf(args.outfile.name,stylesheets=["style.css"])
`
```

**\-\-\-\-\-\-\-\- End --------**

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

#### 精选资料

回复关键词，获取对应的资料：

| 关键词 | 资料名称 |
| --- | --- |
| **600** | [《Python知识手册》](https://mp.weixin.qq.com/mp/appmsgalbum?__biz=MzI2NjY5NzI0NA==&action=getalbum&album_id=1370549534602133504#wechat_redirect) |
| **md** | [《Markdown速查表》](https://mp.weixin.qq.com/s?__biz=MzI2NjY5NzI0NA==&mid=2247495631&idx=1&sn=720c931b1f5cdb82ceccd9b34b182a95&scene=21#wechat_redirect) |
| **time** | [《Python时间使用指南》](https://mp.weixin.qq.com/s?__biz=MzI2NjY5NzI0NA==&mid=2247492370&idx=1&sn=1dc6b3edef0fcb241d07757fb9e2ae03&scene=21#wechat_redirect) |
| **str** | [《Python字符串速查表》](https://mp.weixin.qq.com/s?__biz=MzI2NjY5NzI0NA==&mid=2247496716&idx=1&sn=8ec7a6b373059fa49f04433990581aa6&scene=21#wechat_redirect) |
| **pip** | [《Python：Pip速查表》](https://mp.weixin.qq.com/s?__biz=MzI2NjY5NzI0NA==&mid=2247500405&idx=1&sn=c5a760279babd1075c6153858af84af8&scene=21#wechat_redirect) |
| **style** | [《Pandas表格样式配置指南》](https://mp.weixin.qq.com/s?__biz=MzI2NjY5NzI0NA==&mid=2247501026&idx=1&sn=378292e5435b7ef5eede36192812da3b&scene=21#wechat_redirect) |
| **mat** | [《Matplotlib入门100个案例》](https://mp.weixin.qq.com/s?__biz=MzI2NjY5NzI0NA==&mid=2247493274&idx=1&sn=323662b49f3b8e0d619d44315537a76a&scene=21#wechat_redirect) |
| **px** | [《Plotly Express可视化指南》](https://mp.weixin.qq.com/s?__biz=MzI2NjY5NzI0NA==&mid=2247501349&idx=1&sn=04502491758816e83d43525e911cce56&scene=21#wechat_redirect) |

#### 精选内容

**数据科学：** [VS Code 中 Python配置使用指南](https://mp.weixin.qq.com/s?__biz=MzI2NjY5NzI0NA==&mid=2247495918&idx=1&sn=6b06eadc1604b693f48a127e7b5986a4&scene=21#wechat_redirect) | [财经工具 Tushare](https://mp.weixin.qq.com/s?__biz=MzI2NjY5NzI0NA==&mid=2247497358&idx=1&sn=e9574b69ca3c05142edd534e438e855e&scene=21#wechat_redirect) | [Matplotlib 最有价值的 50 个图表](https://mp.weixin.qq.com/s?__biz=MzI2NjY5NzI0NA==&mid=2247485238&idx=1&sn=f2d6b9136ff94697bdce9c9f48397e78&scene=21#wechat_redirect)

**书籍阅读：** [如何阅读一本书](https://mp.weixin.qq.com/s?__biz=MzU3OTA1MTUwOA==&mid=2247484765&idx=1&sn=0d3a39a00797b8710af2ea175bb20566&scene=21#wechat_redirect) | [巴菲特之道](https://mp.weixin.qq.com/s?__biz=MzU3OTA1MTUwOA==&mid=2247483880&idx=1&sn=a28b5ef681e81a54b5281242e7e2ad2f&scene=21#wechat_redirect) | [价值](https://mp.weixin.qq.com/s?__biz=MzU3OTA1MTUwOA==&mid=2247484963&idx=1&sn=341e105747bb1642e3bc89b74d69e98e&scene=21#wechat_redirect) | [原则](https://mp.weixin.qq.com/s?__biz=MzU3OTA1MTUwOA==&mid=2247485540&idx=1&sn=ce0d2dc7a87a5695d388e8ff7d4405b9&scene=21#wechat_redirect) | [投资最重要的事](https://mp.weixin.qq.com/s?__biz=MzU3OTA1MTUwOA==&mid=2247485096&idx=1&sn=a73862a6b3fc166916d0c71b8d16bfac&scene=21#wechat_redirect) | [戴维斯王朝](https://mp.weixin.qq.com/s?__biz=MzU3OTA1MTUwOA==&mid=2247485191&idx=1&sn=a8f45215977c2f1e1776fa396b105fbf&scene=21#wechat_redirect) | [客户的游艇在哪里](https://mp.weixin.qq.com/s?__biz=MzU3OTA1MTUwOA==&mid=2247485799&idx=1&sn=6197a65ac44c5d8001f61dd75d8419c8&scene=21#wechat_redirect) | [刻意练习](https://mp.weixin.qq.com/s?__biz=MzU3OTA1MTUwOA==&mid=2247485688&idx=1&sn=ae808f8999cc40dcab5c551be673b56b&scene=21#wechat_redirect) | [林肯传](https://mp.weixin.qq.com/s?__biz=MzU3OTA1MTUwOA==&mid=2247485857&idx=1&sn=d2149626b4b3d2fd5a8fab31acff2002&scene=21#wechat_redirect) | [金字塔原理](https://mp.weixin.qq.com/s?__biz=MzU3OTA1MTUwOA==&mid=2247484887&idx=1&sn=e8404120ca4c7800214d8bcf3971eb3d&scene=21#wechat_redirect)

**投资小结：** [2021Q4](https://mp.weixin.qq.com/s?__biz=MzU3OTA1MTUwOA==&mid=2247487417&idx=1&sn=a45d93aec5a3b91413f8a9d6b6a8decb&scene=21#wechat_redirect) | [2021Q3](https://mp.weixin.qq.com/s?__biz=MzU3OTA1MTUwOA==&mid=2247487065&idx=1&sn=8766ac15616c871c0ba23cada09aa47b&scene=21#wechat_redirect) | [2021Q2](https://mp.weixin.qq.com/s?__biz=MzU3OTA1MTUwOA==&mid=2247486390&idx=1&sn=91c3cad42676f67d96e94f9b2f205d9e&scene=21#wechat_redirect) | [2021Q1](https://mp.weixin.qq.com/s?__biz=MzU3OTA1MTUwOA==&mid=2247485738&idx=1&sn=5a5636423731174cde98902af2e47d7f&scene=21#wechat_redirect) | [2020Q4](https://mp.weixin.qq.com/s?__biz=MzU3OTA1MTUwOA==&mid=2247485416&idx=1&sn=0cf825834c4f86a81ba87abf300416a7&scene=21#wechat_redirect)

#### 精选视频

**可视化：** [Plotly Express](https://mp.weixin.qq.com/s?__biz=MzI2NjY5NzI0NA==&mid=2247501395&idx=2&sn=9306078c98e6a2efc6e478a5921f5276&scene=21#wechat_redirect)

**财经：** [Plotly在投资领域的应用](https://mp.weixin.qq.com/s?__biz=MzI2NjY5NzI0NA==&mid=2247496637&idx=1&sn=91910ea5f9034fc6685d28ef0f00a70c&scene=21#wechat_redirect) | [绘制K线图表](https://mp.weixin.qq.com/s?__biz=MzI2NjY5NzI0NA==&mid=2247497147&idx=3&sn=9e788955d2268e1ba3f9f8c85ec4ece2&scene=21#wechat_redirect)

**排序算法：** [汇总](https://mp.weixin.qq.com/s?__biz=MzI2NjY5NzI0NA==&mid=2247505089&idx=1&sn=71021ec9da57933b41b64174f3d904c2&scene=21#wechat_redirect) | [冒泡排序](https://mp.weixin.qq.com/s?__biz=MzI2NjY5NzI0NA==&mid=2247504041&idx=2&sn=8dc4e755344edda2fdbb1f4922fa013b&scene=21#wechat_redirect) | [选择排序](https://mp.weixin.qq.com/s?__biz=MzI2NjY5NzI0NA==&mid=2247504041&idx=3&sn=b9aeb552253f6c5e8efe635bbcaead1b&scene=21#wechat_redirect) | [快速排序](https://mp.weixin.qq.com/s?__biz=MzI2NjY5NzI0NA==&mid=2247504041&idx=4&sn=22a3d67b3cec0269060502c13fabae35&scene=21#wechat_redirect) | [归并排序](https://mp.weixin.qq.com/s?__biz=MzI2NjY5NzI0NA==&mid=2247504041&idx=5&sn=90feeb44a998a57dbdabb9e1823bb051&scene=21#wechat_redirect) | [堆排序](https://mp.weixin.qq.com/s?__biz=MzI2NjY5NzI0NA==&mid=2247504041&idx=6&sn=5e8421ee83aa301b37f7f25d7ba6388f&scene=21#wechat_redirect) | [插入排序](https://mp.weixin.qq.com/s?__biz=MzI2NjY5NzI0NA==&mid=2247504041&idx=7&sn=1889203106bce18adc50ff151399e6fc&scene=21#wechat_redirect) | [希尔排序](https://mp.weixin.qq.com/s?__biz=MzI2NjY5NzI0NA==&mid=2247504041&idx=8&sn=d1eccecfbb6f37e34b2eaf3d8908248b&scene=21#wechat_redirect) | [计数排序](https://mp.weixin.qq.com/s?__biz=MzI2NjY5NzI0NA==&mid=2247504165&idx=3&sn=0d79c516472fcfa73ebe97fdf1c13004&scene=21#wechat_redirect) | [桶排序](https://mp.weixin.qq.com/s?__biz=MzI2NjY5NzI0NA==&mid=2247504461&idx=3&sn=78afcc98eef1bc38060b701af6ac3822&scene=21#wechat_redirect) | [基数排序](https://mp.weixin.qq.com/s?__biz=MzI2NjY5NzI0NA==&mid=2247505087&idx=5&sn=52b1af66f7a48de7ab7d61e769b81f82&scene=21#wechat_redirect)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

People who liked this content also liked

有了这篇 Docker 网络原理，彻底爱了~

高效运维

不看的原因

- 内容质量低
- 不看此公众号

C函数指针别再停留在语法，得上升到软件设计~

嵌入式资讯精选

不看的原因

- 内容质量低
- 不看此公众号

详解 seaborn，快速实现统计数据可视化

大邓和他的Python

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___d8ccd5018a2e46bba.bmp"/>

Scan to Follow