# 简单好用！Pandas 读取异常数据结构 Excel！

<a id="profileBt"></a><a id="js_name"></a>早起Python *2021-11-29 10:24*

The following article is from 萝卜大杂烩 Author 周萝卜

<a id="copyright_info"></a>[![](../../../_resources/0_f5f525e8d0184da7bee79666854da4ae.jpg)<br>**萝卜大杂烩** .<br>Do it by yourself！爱好Python，测试，NLP，数据分析，小程序，k8s等技术，期待与你一同成长](#)

通常情况下，我们使用 Pandas 来读取 Excel 数据，可以很方便的把数据转化为 DataFrame 类型。但是现实情况往往很骨干，当我们遇到结构不是特别良好的 Excel 的时候，常规的 Pandas 读取操作就不怎么好用了，今天我们就来看两个读取非常规结构 Excel 数据的例子

<img width="661" height="353" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__55a945f7c0aa475da.png"/>

本文使用的测试 Excel 内容如下

<img width="661" height="275" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__a0bdb12a8fad40efa.png"/>

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

文末可以获取到该文件

### 指定列读取

一般情况下，我们使用 read_excel 函数读取 Excel 数据时，都是默认从第 A 列开始读取的，但是对于某些 Excel 数据，往往不是从第 A 列就有数据的，此时我们需要参数 usecols 来进行规避处理

比如上面的 Excel 数据，如果我们直接使用 read\_excel(src\_file) 读取，会得到如下结果

<img width="661" height="255" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__cb0495924fba4825b.png"/>

我们得到了很多未命名的列以及很多我们根本不需要的列数据

此时我们可以通过 **usecols** 来指定读取哪些列数据

```
from pathlib import Path
src_file = Path.cwd() /  'shipping_tables.xlsx'
df = pd.read_excel(src_file, header=1, usecols='B:F')

```

<img width="661" height="407" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__94eda3f8608a40d2b.png"/>

可以看到生成的 DataFrame 中只包含我们需要的数据，特意排除了 notes 列和 date 字段

usecols 可以接受一个 Excel 列的范围，例如 B:F 并仅读取这些列，header 参数需要一个定义标题列的整数，它的索引从0开始，所以我们传入 1，也就是 Excel 中的第 2 行

我们也可以将列定义为数字列表

```
df = pd.read_excel(src_file, header=1, usecols=[1,2,3,4,5])

```

也可以通过列名称来选择所需的列数据

```
df = pd.read_excel(
    src_file,
    header=1,
    usecols=['item_type', 'order id', 'order date', 'state', 'priority'])

```

> 这种做法在列的顺序改变但是列的名称不变的时候非常有用

最后，usecols 还可以接受一个可调用的函数

```
def column_check(x):
    if 'unnamed' in x.lower():
        return False
    if 'priority' in x.lower():
        return False
    if 'order' in x.lower():
        return True
    return True
df = pd.read_excel(src_file, header=1, usecols=column_check)

```

该函数将按名称解析每一列，并且必须为每一列返回 True 或 False

当然也可以使用 lambda 表达式

```
cols_to_use = ['item_type', 'order id', 'order date', 'state', 'priority']
df = pd.read_excel(src_file,
                   header=1,
                   usecols=lambda x: x.lower() in cols_to_use)

```

### 范围和表格

在某些情况下，Excel 中的数据可能会更加不确定，在我们的 Excel 数据中，我们有一个想要读取的名为 ship_cost 的表，这该怎么获取呢

<img width="661" height="654" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__3d1e7fcac6f144cb8.png"/>

在这种情况下，我们可以直接使用 openpyxl 来解析 Excel 文件并将数据转换为 pandas DataFrame

以下是使用 openpyxl（安装后）读取 Excel 文件的方法：

```
from openpyxl import load_workbook
import pandas as pd
from pathlib import Path
src_file = src_file = Path.cwd() / 'shipping_tables.xlsx'
wb = load_workbook(filename = src_file)

```

查看所有的 sheet 页，获取某个 sheet 页，获取 Excel 范围数据

```
wb.sheetnames
sheet = wb['shipping_rates']
lookup_table = sheet.tables['ship_cost']
lookup_table.ref

```

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__5f7f5afc21124cb9b.png)

现在我们以及知道要加载的数据范围了， 接下来就是将该范围转换为 Pandas DataFrame

```
# 获取数据范围
data = sheet[lookup_table.ref]
rows_list = []
# 循环获取数据
for row in data:
    cols = []
    for col in row:
        cols.append(col.value)
    rows_list.append(cols)
df = pd.DataFrame(data=rows_list[1:], index=None, columns=rows_list[0])

```

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__09e4960deff946d98.png)

这样我们就获取到了干净的表数据了

好了，今天的两个小知识点就分享到这里了，我们下次再见！

**点击下载「pandas进阶修炼300题」👇**

[<img width="677" height="288" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__f95b308ef93f405e9.png"/>](http://mp.weixin.qq.com/s?__biz=Mzg5OTU3NjczMQ==&mid=2247519402&idx=1&sn=535dddd429bf383a1eda9f0316507ec4&chksm=c053ea5ef72463488c4fdd99282d9d4d3922c0ad9fe034865f351d4ec19b375672b3e41350ec&scene=21#wechat_redirect)

* * *

<img width="677" height="615" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__d32d2a95858f4796b.png"/>

People who liked this content also liked

MySQL通过bin log恢复数据｜手撕MySQL｜对线面试官

程序员白泽

不看的原因

- 内容质量低
- 不看此公众号

python连接MySQL数据库之pandas数据整理

昊哥说测试

不看的原因

- 内容质量低
- 不看此公众号

VBA合并多个csv文件

VBA爱好者

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___fb3c0d20892d40519.bmp"/>

Scan to Follow