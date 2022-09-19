# 5 分钟掌握 openpyxl 操作：Python 轻松处理 Excel

<a id="profileBt"></a><a id="js_name"></a>Python程序员 *2021-07-09 08:30*

<img width="677" height="381" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__dcc52fb066ce49199.png"/>

来源：python中文社区

各种数据需要导入Excel？多个Excel要合并？目前，Python处理Excel文件有很多库，openpyxl算是其中功能和性能做的比较好的一个。接下来我将为大家介绍各种Excel操作。

**打开Excel文件**

新建一个Excel文件

```
`    >>> from openpyxl import Workbook
    >>> wb = Workbook()
`
```

打开现有Excel文件

```
`    >>> from openpyxl import load_workbook
    >>> wb2 = load_workbook('test.xlsx')
`
```

打开大文件时，根据需求使用只读或只写模式减少内存消耗。

```
`wb = load_workbook(filename='large_file.xlsx', read_only=True)
wb = Workbook(write_only=True)
`
```

**获取、创建工作表**

获取当前活动工作表：

```
`    >>> ws = wb.active
`
```

创建新的工作表：

```
`    >>> ws1 = wb.create_sheet("Mysheet") # insert at the end (default)
    # or
    >>> ws2 = wb.create_sheet("Mysheet", 0) # insert at first position
    # or
    >>> ws3 = wb.create_sheet("Mysheet", -1) # insert at the penultimate position
`
```

使用工作表名字获取工作表：

```
`    >>> ws3 = wb["New Title"]
`
```

获取所有的工作表名称：

```
`    >>> print(wb.sheetnames)
    ['Sheet2', 'New Title', 'Sheet1']
使用for循环遍历所有的工作表：
    >>> for sheet in wb:
    ...     print(sheet.title)
`
```

**保存**

保存到流中在网络中使用：

```
`    >>> from tempfile import NamedTemporaryFile
    >>> from openpyxl import Workbook
    >>> wb = Workbook()
    >>> with NamedTemporaryFile() as tmp:
            wb.save(tmp.name)
            tmp.seek(0)
            stream = tmp.read()
保存到文件：
    >>> wb = Workbook()
    >>> wb.save('balances.xlsx')
保存为模板：
    >>> wb = load_workbook('document.xlsx')
    >>> wb.template = True
    >>> wb.save('document_template.xltx')
`
```

**单元格**

单元格位置作为工作表的键直接读取：

```
`    >>> c = ws['A4']
`
```

为单元格赋值：

```
`    >>> ws['A4'] = 4
    >>> c.value = 'hello, world'
`
```

多个单元格可以使用切片访问单元格区域：

```
`    >>> cell_range = ws['A1':'C2']
`
```

使用数值格式：

```
`    >>> # set date using a Python datetime
    >>> ws['A1'] = datetime.datetime(2010, 7, 21)
    >>>
    >>> ws['A1'].number_format
    'yyyy-mm-dd h:mm:ss'
`
```

使用公式：

```
`    >>> # add a simple formula
    >>> ws["A1"] = "=SUM(1, 1)"
`
```

合并单元格时，除左上角单元格外，所有单元格都将从工作表中删除：

```
`    >>> ws.merge_cells('A2:D2')
    >>> ws.unmerge_cells('A2:D2')
    >>>
    >>> # or equivalently
    >>> ws.merge_cells(start_row=2, start_column=1, end_row=4, end_column=4)
    >>> ws.unmerge_cells(start_row=2, start_column=1, end_row=4, end_column=4) 
`
```

**行、列**

可以单独指定行、列、或者行列的范围：

```
`    >>> colC = ws['C']
    >>> col_range = ws['C:D']
    >>> row10 = ws[10]
    >>> row_range = ws[5:10]
`
```

可以使用`Worksheet.iter_rows()`方法遍历行：

```
`    >>> for row in ws.iter_rows(min_row=1, max_col=3, max_row=2):
    ...    for cell in row:
    ...        print(cell)
    <Cell Sheet1.A1>
    <Cell Sheet1.B1>
    <Cell Sheet1.C1>
    <Cell Sheet1.A2>
    <Cell Sheet1.B2>
    <Cell Sheet1.C2>
`
```

同样的`Worksheet.iter_cols()`方法将遍历列：

```
`    >>> for col in ws.iter_cols(min_row=1, max_col=3, max_row=2):
    ...     for cell in col:
    ...         print(cell)
    <Cell Sheet1.A1>
    <Cell Sheet1.A2>
    <Cell Sheet1.B1>
    <Cell Sheet1.B2>
    <Cell Sheet1.C1>
    <Cell Sheet1.C2>
`
```

遍历文件的所有行或列，可以使用`Worksheet.rows`属性：

```
`    >>> ws = wb.active
    >>> ws['C9'] = 'hello world'
    >>> tuple(ws.rows)
    ((<Cell Sheet.A1>, <Cell Sheet.B1>, <Cell Sheet.C1>),
    (<Cell Sheet.A2>, <Cell Sheet.B2>, <Cell Sheet.C2>),
    (<Cell Sheet.A3>, <Cell Sheet.B3>, <Cell Sheet.C3>),
    (<Cell Sheet.A4>, <Cell Sheet.B4>, <Cell Sheet.C4>),
    (<Cell Sheet.A5>, <Cell Sheet.B5>, <Cell Sheet.C5>),
    (<Cell Sheet.A6>, <Cell Sheet.B6>, <Cell Sheet.C6>),
    (<Cell Sheet.A7>, <Cell Sheet.B7>, <Cell Sheet.C7>),
    (<Cell Sheet.A8>, <Cell Sheet.B8>, <Cell Sheet.C8>),
    (<Cell Sheet.A9>, <Cell Sheet.B9>, <Cell Sheet.C9>))
`
```

或`Worksheet.columns`属性：

```
`    >>> tuple(ws.columns)
    ((<Cell Sheet.A1>,
    <Cell Sheet.A2>,
    <Cell Sheet.A3>,
    <Cell Sheet.A4>,
    <Cell Sheet.A5>,
    <Cell Sheet.A6>,
    ...
    <Cell Sheet.B7>,
    <Cell Sheet.B8>,
    <Cell Sheet.B9>),
    (<Cell Sheet.C1>,
    <Cell Sheet.C2>,
    <Cell Sheet.C3>,
    <Cell Sheet.C4>,
    <Cell Sheet.C5>,
    <Cell Sheet.C6>,
    <Cell Sheet.C7>,
    <Cell Sheet.C8>,
    <Cell Sheet.C9>))
`
```

使用`Worksheet.append()`或者迭代使用`Worksheet.cell()`新增一行数据：

```
`    >>> for row in range(1, 40):
    ...     ws1.append(range(600))
    >>> for row in range(10, 20):
    ...     for col in range(27, 54):
    ...         _ = ws3.cell(column=col, row=row, value="{0}".format(get_column_letter(col)))
`
```

插入操作比较麻烦。可以使用`Worksheet.insert_rows()`插入一行或几行：

```
`     >>> from openpyxl.utils import get_column_letter
     >>> ws.insert_rows(7) 
     >>> row7 = ws[7]
     >>> for col in range(27, 54):
    ...         _ = ws3.cell(column=col, row=7, value="{0}".format(get_column_letter(col)))
`
```

`Worksheet.insert_cols()`操作类似。`Worksheet.delete_rows()`和`Worksheet.delete_cols()`用来批量删除行和列。

**只读取值**

使用`Worksheet.values`属性遍历工作表中的所有行，但只返回单元格值：

```
`    for row in ws.values:
       for value in row:
         print(value)
`
```

`Worksheet.iter_rows()`和`Worksheet.iter_cols()`可以设置`values_only`参数来仅返回单元格的值：

```
`    >>> for row in ws.iter_rows(min_row=1, max_col=3, max_row=2, values_only=True):
    ...   print(row)
    (None, None, None)
    (None, None, None)`
```

作者：Sinchard，主攻Python库文档翻译，开发代码片段，源码分析

Blog：zhihu.com/people/aiApple

* * *

**微软于年初推出了自己的Python教程，我们将其汉化提供给大家，欢迎大家收藏关注哦~（已经汉化完成的20集，我们日更1集，未完成部分我们尽快更新）**

沃依得学堂

，赞 104

People who liked this content also liked

深扒吴勇幕后的人物关系！新疆的毒教材，文化出版行业的黑幕多到你数不清

...

Python程序员

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___b5267102628b4ffbb.bmp"/>

Scan to Follow