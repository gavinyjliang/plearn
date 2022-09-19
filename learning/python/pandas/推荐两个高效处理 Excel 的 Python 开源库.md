# 推荐两个高效处理 Excel 的 Python 开源库

<a id="profileBt"></a><a id="js_name"></a>数据分析与开发 *2021-04-21 12:05*

The following article is from Python开发精选 Author 南风草木香

<a id="copyright_info"></a>[![](../../../_resources/0_1fd0848e36c24e00b29dd48438565cfa.jpg)<br>**Python开发精选** .<br>分享 Python 技术文章、资源、课程、资讯。](#)

【导语】：openpyxl 和 formulas 是两个成熟的开源库，在Python中借助这两个库，处理Excel电子表格，可以实现自动访问、处理表格中数据的功能，省时高效，不易出错，是处理Excel表格的一种好办法。

### 简介

Excel在工作中很常见，许多公司的软件项目都会用到它。对于应用程序开发者而言，我们经常需要将Excel文件转换为应用程序。大多数情况下我们都把Excel作为数据的导出格式，有时也将其作为数据的输入格式。

虽然Excel并不是一个软件领域的专用语言，但一些软件领域的专家常常需要用其处理问题。这就导致会“说”Excel的软件更受用户的青睐。即使从长远来看，我们的目标是把Excel变成一个应用程序或者软件领域的一种语言(DSL)，但能自动处理Excel中的数据也是一个重要方向。

在以下情景中，自动处理Excel数据将非常有用：

例如在一个项目中，许多用户需要使用某些相同的Excel文件，而应用程序通过处理这些Excel文件，让用户使用起来更加方便。或者当一些用户想用Excel中的数据生成带有附加图表和formulas的报告时，应用程序可以生成一个电子表格，用来输出一些查询或计算结果，方便用户使用这些数据。

在本文中，我们将探索如何使用openpyxl库和其他工具，结合Python处理Excel电子表格。主要分为以下几点:

- 处理Excel文件，用不同的方法访问其中的数据;
    
- 使用formulas;
    
- 输出Excel文件。
    

注意:本文中我们处理的只是“现代”(2010)基于xml的Excel文件，文件扩展名为.xlsx。Openpyxl不支持之前的二进制Excel文件(.xls)，针对这类文件有其他的解决方案，在本文中不做讨论。本文中的所有示例代码均可在GitHub仓库中找到。

项目地址：

https://github.com/Strumenta/article-python-excel

### 安装

下面，我们设定一个类似unix的环境-Linux、OSX或者WSL。第一步，创建一个目录来存放我们的项目:

```
`mkdir python-excel
cd python-excel
`
```

如果想要把代码放到Github上，我们可以在Github上创建一个仓库，然后把它克隆到本地：

```
`git clone <repo-url> python-excel
cd python-excel
`
```

这将使我们能在GitHub为Python生成一个.gitignore文件，为我们生成一个README文件和一个license文件。在创建了项目根目录之后，我们可以使用pip安装来openpyxl。我们需要设置一个虚拟环境，这样就不会影响到系统的Python安装:

```
`python -m venv .venv
source .venv/bin/activate
`
```

激活环境后，我们应该可以在shell prompt中看到：

```
`(.venv) python-excel (main*) »
`
```

随后我们写一个requirements.txt文件,列出我们的依赖包：

```
`echo openpyxl > requirements.txt
`
```

即使当前只有一个依赖包，也要将它列在一个可以被机器解析的文件中，这样做会使其他开发人员更容易地使用我们的项目-即使在之后的版本中我们已经忘记了这个依赖包。之后用pip安装所需的依赖包:

```
`python -m pip install -r requirements.txt
`
```

### 简单使用

#### 1.加载一个Excel文件

在Openpyxl中，我们把Excel文件称为"workbook"，用openpyxl.workbook.Workbook类的实例来表示，打开一个Excel文件非常简单：

```
`wb = load_workbook(path)
`
```

<img width="657" height="493" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__d4903e959b1a4492b.png"/>

调用load_workbook的结果

Openpyxl打开文件后，一般可以同时进行读取和写入工作，除非我们给load\_workbook设置一个read\_only=True参数，表示只读取文件，当我们使用完一个Excel文件后，必须关闭它:

```
`wb.close()
`
```

不幸的是，Workbook不是一个“文件管理者”，所以不能用Python中的语句来自动关闭它。即使在一些exceptions的情况中，也必须得手动关闭文件：

```
`wb = load_workbook(path)
try:
    # Use wb...
finally:
    wb.close()
`
```

#### 2.处理一个Excel文件 - 通用案例

通常，Workbooks中可能有几个表，我们需要选择Excel文件中的一个表，访问其中的数据。随后，我们再学习如何处理多个表。现在，假设我们对active工作表中的数据比较感兴趣——当用户在他们的应用中打开文件就会看到的工作表:

```
`sheet = wb.active
`
```

这实际上是文档中最常用的表。我们可以用不同的方法访问表格中的数据。我们可以使用Pythonic生成器每次处理一行数据

(1)对行进行遍历:

```
`for row in sheet.rows():
    # Do something with the row
`
```

rows()产生的行,本身就是一个生成器，我们可以遍历它们:

```
`for row in sheet.rows():
    for cell in row:
        # Do something with the cell
`
```

还可以根据索引访问数据：

```
`for row in sheet.rows():
    header = row[2]
`
```

实际上，表格本身就是可以按行进行迭代的，所以我们可以忽略所有行:

```
`for row in sheet:
    pass
`
```

(2)使用cols方法对列进行遍历。

```
`for col in sheet.cols():
    # Use the column
`
```

遍历列与遍历行的操作基本相同:它们本身都是可迭代的，并且可以通过索引寻址。

(3)通过地址访问单元格。

如果我们需要某个单元格中的数据，那么并不需要遍历整个表格去找;可以使用excel样式的坐标来访问这个单元格：

```
`cell = sheet['C5']
`
```

在随后的章节中，将展示如何从一行，一列，或者一些单元格中获取一个生成器。

#### 3.处理单元格

在任何情况下，想要处理电子表格中的数据，就必须访问每个单元格。在Openpyxl中，单元格有一个值和许多仅用于编写的其他信息，比如样式信息。更方便地是，我们可以把单元格中的值作为Python对象(数字、日期、字符串等)，用Openpyxl将它们转换为Excel类型。因此，单元格内容就不一定要是字符串。例如，我们以数字的形式读取单元格的内容:

```
`tax_percentage = sheet['H16'].value
tax_amount = taxable_amount * tax_percentage
`
```

然而，我们并不能保证用户一定在单元格中输入了数字;如果它包含字符串"bug"，如果比较幸运的话，在运行上面的代码后，我们会得到一个运行错误:

```
`TypeError: can't multiply sequence by non-int of type 'float'
`
```

然而，在不那么幸运的情况下，例如，当taxable\_amount是一个整数时——因为我们在示例中处理的数据是钱，所以应该是整数——我们将得到一个重复taxable\_amount次的“bug”。这是因为Python把*操作符当成了字符串，整数就意味着“重复字符串n次”。这可能会导致进一步的类型错误，或者在Python无法放置如此大的字符串时出现内存错误。因此，我们应该验证程序的输入，包括Excel文件。在这个特殊的例子中，我们可以使用Python的isinstance函数来检查单元格中值的类型:

```
`if isinstance(cell.value, numbers.Number):
    # Use the value
`
```

我们还可以询问单元格它存储的数据类型是什么：

```
`if cell.data_type == TYPE_NUMERIC:
    # Use the numeric value
`
```

#### 4.单元格高级寻址

到目前为止，我们已经使用了访问单元格最简单、最直接的方法。然而，这并不是所有方法;让我们来看看更复杂的方案。

(1)除了active之外的其他表。

我们可以通过在workbook中通过名称来访问它们:

```
`sheet = wb['2020 Report']
`
```

然后我们就可以像之前看到的那样访问单元格了。

(2)单元格范围

我们不一定要一个一个的寻址单元格-还可以设定范围来访问单元格：

- sheet\['D'\]是指一整行（本例中是D这一行）
    
- sheet\[7\]是指一整列(本例中是第7列)
    
- sheet\['B:F'\]代表许多行
    
- sheet\['4:10'\] 代表许多列
    
- sheet\['C3:H5'\]是最通用的选择，代表任意范围的单元格
    

以上任何一种情况，结果都是一个按行迭代所有单元格(除非迭代的范围以列为标准，在这种情况下，单元格按列顺序进行迭代):

```
`for cell in sheet['B2:F10']:
    # B2, B3, ..., F1, F2, ..., F10
for cell in sheet['4:10']:
    # A4, B4, ..., A10, B10, ... 
`
```

<img width="657" height="329" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__6750f53d1c1d485a9.png"/>

sheet\['B2:F10'\]中的单元格

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

sheet\['4:10'\]中的单元格

#### 5.单元格迭代器

如果上述寻址方案解决不了问题，那我们可以考虑一些简单的方法iter\_rows和iter\_columns，它们分别按行和列返回单元格生成器。需要指出，这些方法都需要5个参数：

- min_row - 起始行的编号(1就是A,2就是B，以此类推)
    
- min_col - 起始列的编号
    
- max_row - 最后一行的编号
    
- max_col - 最后一列的编号
    
- values\_only - 生成器将只显示每个单元格的值，而不是整个单元格对象。所以，我们不需要用cell.value，而只要value。另一方面，我们不能访问单元格的其他属性，比如data\_type。例如，如果我们想按列在B2:F10的范围上进行迭代，可以这样写：
    

```
`for cell in sheet.iter_columns(min_row=2, min_col=2, max_row=6, max_col=10):
    # Use the cell
`
```

#### 6.编写一个Excel文件

要写一个Excel文件，我们只需在workbook上调用save方法：

```
`wb.save('someFile.xlsx')
`
```

知道如何保存一个workbook后，让我们看看如何修改它，这将会很有趣。我们可以修改文件中的workbook，也可以修改在Python中创建的workbook。

#### 7.修改单个单元格

我们可以用指定的方式来改变一个单元格中的值：

```
`cell.value = 42
`
```

这会自动更新单元格的数据类型以存储新的值。除了基本类型(整数、浮点数、字符串)之外，还包括datetime模块中的各种类，如果你安装了NumPy，那么NumPy数字类型也可以使用。不仅可以设置值和类型，我们还可以设置单元格的其他属性，特别是样式信息(字体、颜色等)，这对做一个好看的报告很有用。

Openpyxl的文档中有许多关于调整样式的详细信息，我们可以在这里查询:

https://openpyxl.readthedocs.io/en/stable/styles.html

#### 8.添加或移除表格

到目前为止，我们已经看到了如何处理一些对象，特别是workbooks和worksheets——就像处理字典一样，访问其中的细节:工作表、行、列、单个单元格、单元格范围。现在，我们将学习如何添加新信息，以及如何更改现有信息。我们先从表格开始。

使用 create_sheet方法来创建worksheet:

```
`new_sheet = wb.create_sheet()
`
```

这样就可以在workbook中的其他表格之后添加一个新表，我们可以给这个新表一个标题：

```
`new_sheet = wb.create_sheet(title = 'My new sheet')
`
```

如果我们想把这个表格放在其他位置，我们可以指定它的索引(从0开始):

```
`# The new sheet will be inserted as the third sheet
new_sheet = wb.create_sheet(index = 2)
`
```

要删除一个表格的话有两种方法。可以根据名字进行删除：

```
`del wb['My sheet']
`
```

我们可以使用in操作符来查看给出的表格名称是否在workbook中：

```
`name = 'My sheet'
if name in workbook:
    del workbook[name]
`
```

或者还能调用remove方法来删除表格：

```
`wb.remove(sheet)
`
```

#### 9.增加或移除行、列、单元格

看看下面这些例子。首先通过访问一个单元格，可以为创建行和列腾出空间：

```
`wb = Workbook()
# Initially, an empty worksheet has a single row and column, A and 1
self.assertEqual(wb.active.max_row, 1)
self.assertEqual(wb.active.max_column, 1)
# We set the value of the cell at C3; 
# openpyxl creates rows B, C and columns 2, 3 automatically
wb.active['C3'].value = 12
# Now the sheet has 3 rows and columns
self.assertEqual(wb.active.max_row, 3)
self.assertEqual(wb.active.max_column, 3)
wb.close()
`
```

此外，我们还可以用insert\_rows和insert\_cols方法在表格中添加行或列。当新创建一行或一列时，单元格会自动调整：

```
`wb = Workbook()
self.assertEqual(wb.active.max_row, 1)
wb.active['A1'].value = 11
# Insert 3 rows, starting at index 0 (i.e. row 1)
wb.active.insert_rows(0, 3)
self.assertEqual(wb.active.max_row, 4)
# Note how the cell, A1, has automatically moved by 3 rows to A4
self.assertEqual(wb.active['A4'].value, 11)
`
```

与此相对应，还可以使用delete\_rows和delete\_cols来删除行或列：

```
`# Delete 2 columns, starting from index 1, i.e. column B
sheet.delete_columns(1, 2)
`
```

#### 10.使用formulas

电子表格非常强大，因为它还支持用formulas来计算单元格中的值。当其他单元格发生变化时，通过计算取得值的单元格也会自动更新。让我们看看如何使用在Openpyxl中使用formulas吧。首先，如果我们只想读取一个Excel文件，我们可以完全忽略formulas。

此时，以“data only”模式打开它，这种模式将会隐藏formulas，所有单元格中的值都是固定的-也就是上次Excel文件计算后的结果:

```
`wb = load_workbook(filename, data_only=True)
`
```

只有在修改Excel文件时，才需要重新使用formulas。虽然openpyxl有一些对解析formulas的支持(例如，检查是否只调用了已知函数)，但openpyxl自身不能产生formulas。因此，如果我们想要使用formulas，就必须求助于第三方库。进入一个叫做“formulas”的库。让我们把它添加到requirements.txt文件中并安装它:

```
`$ cat requirements.txt
openpyxl
formulas
pip install -r requirements.txt
`
```

我们有两种方法来使用formulas库:

(1)像Excel那样，使用所有formulas，计算workbook中的值;

(2)将单独的formulas写入Python函数，这样就可以放入不同的参数，来使用这些formulas。

#### 11.计算所有formulas中的值：

第一个示例比较枯燥，因为功能与前面的data_only方法作用相似。事实上，在那种模式下，我们加载不了workbook，修改不了它，也不能重新使用其中的formulas。我们必须:

- 把修改后的workbook保存到一个文件中；
    
- 用formulas重新加载文件；
    
- 调用API使用formulas；
    
- 把计算后的值保存到文件中；
    
- 在data_only模式下用openpyxl打开文件，查看计算后的结果。
    

这简直是在浪费时间！

当然，formulas库的这个特性也有价值，因为它支持一些高级的用法:

- 同时在多个workbook中使用formulas
    

Excel中，可以在另一个文件中引用其他文件中的formulas。formulas可以在同一个集合中加载多个workbook，以解决这种跨文件引用问题，这是Excel中很少使用的特性。

- 将整个Excel工作簿编译为Python函数
    

我们可以将某些单元格定义为输入单元格，把剩下的定义为输出单元格，得到一个函数，在给定输入后使用formulas，并在计算后把值返回给输出单元格。然而为了保持本文的简洁性，就不做详述。

#### 12.把单独的formulas编译为Python函数：

让我们将重点放在单个formulas上，以便更好的使用openpyxl。操作如下：

```
`func = formulas.Parser().ast(value)[1].compile()
`
```

由于某些原因，ast方法返回一个由2个对象组成的元组，其中第二个对象builder是最有用的。尽管这是内部APl，但也应该被包装在一个更友好的用户界面中。无论如何，当我们对上面的代码求值时，得到的func将是一个带有许多参数的函数，这些参数与formulas的输入一样：

```
`func = formulas.Parser().ast('=A1+B1')[1].compile()
func(1, 2) == 3 # True
`
```

#### 13.处理formulas的依赖项

因此，我们可以将单个单元格的formulas编译成一个函数。但是，当formulas依赖于其他单元格本身的formulas时，会发生什么呢?formulas库这时候就失去了作用;我们必须计算所有的输入。让我们来看看怎么做。

首先，如何区分含formulas的单元格和含常规值的单元格?Openpyxl没有提供这样的区分方法，所以我们必须检查单元格的值是否以等号字符开头:

```
`def has_formula(cell: Cell)
   return isinstance(cell.value, str) and cell.value.startswith('=')
`
```

这样我们就知道了如何处理不包含formulas的单元格了：

```
`def compute_cell_value(cell: Cell):
   if not has_formula(cell):
       return cell.value
`
```

现在需要处理的是含有formulas的单元格：

```
`func = formulas.Parser().ast(cell.value)[1].compile()
args = []
# TODO: compute function arguments
return func(*args)
`
```

我们将formulas编译成一个Python函数，然后调用它。因为输入是对单元格的值，所以我们递归地调用compute\_cell\_value来获取它们的值:

```
`sheet = cell.parent
for key in func.inputs.keys():
   args.append(compute_cell_value(sheet[key]))
`
```

我们利用这样一个关系:每个单元格都与包含它的工作表有关。我们还使用formulas保留的信息，这样就可以检查函数的输入——包含单元格的字典。注意，它不支持跨表或跨文件的使用。

#### 14.基于单元格范围使用formulas

到目前为止，compute\_cell\_value函数使用基于其他单元格的formulas，成功地计算了单元格的值。然而，对于那些不依赖于单个单元格，而是依赖于许多单元格的formulas，又该如何计算呢?在这种情况下，函数的输入是一个范围表达式，例如=SUM(A1:21)中的A1:Z1。我们给compute\_cell\_ value传入以下信息:

```
`sheet[key]
`
```

当key是单个单元格的地址时，我们将得到一个单元格对象;但是当key是许多单元格时，我们将得到一个元组。compute\_cell\_value不知道如何处理这样的输入，所以我们必须修改它，来应对这种情况:

```
`if isinstance(input, Tuple):
   return tuple(map(compute_cell_value, input))
`
```

函数的完整版本如下：

```
`def compute_cell_value(input: Union[Cell, Tuple]):
   if isinstance(input, Tuple):
       return tuple(map(compute_cell_value, input))
   if not has_formula(input):
       return input.value
   func = formulas.Parser().ast(input.value)[1].compile()
   args = []
   sheet = input.parent
   for key in func.inputs.keys():
       args.append(compute_cell_value(sheet[key]))
   return func(*args)
`
```

#### 15.添加新的formula函数

formulas支持许多内置的Excel函数，但不包括所有函数。当然，它也不支持VBA中的自定义函数。但是，我们可以添加一些新的Python函数，这样就可以在formulas中调用这些函数：

```
`def is_number(number):
   ... # This is actually defined in formulas, but strangely not exposed as the Excel function
FUNCTIONS = formulas.get_functions()
FUNCTIONS['ISNUMBER'] = is_number
`
```

函数的输入值就是Python中的值，比如字符串、数字、日期等，而不是cell类中的值。此外，与普通Python函数相比，我们需要防止XIError，它表示计算中的错误，例如#DIV/0!或#REF! (当我们在输入formulas中犯了一些错误时，通常会在Excel中看到这些):

```
`def is_number(number):
    if isinstance(number, XlError):
        return False
    ...
`
```

### 结论

通过使用openpyxl和formulas这两个成熟的开源库，我们可以更高效地用Python处理Excel。对于那些经常使用Excel的用户来说，能够处理复杂的Excel文件是一个非常有用的功能。

在本文中，我们学习了如何读写带有formulas的Excel文件。你还可以在样式，图表，合并单元格中学到其他相关的知识。

\- EOF -

推荐阅读  点击标题可跳转

<ins>1、[再见 VBA！神器工具统一 Excel 和 Python](http://mp.weixin.qq.com/s?__biz=MzA5ODM5MDU3MA==&mid=2650870520&idx=1&sn=db366c322f12d588732b6344b3b0d829&chksm=8b67f5bdbc107cab1bc2bb07974d326afed47487cf59101c7ecd2333520d3ff3b5eca685866f&scene=21#wechat_redirect)</ins>

<ins>2、[手把手教你用Python进行时间序列分解和预测](http://mp.weixin.qq.com/s?__biz=MzA5ODM5MDU3MA==&mid=2650870027&idx=1&sn=889ce408fc84ea42f95c1a75f06368ef&chksm=8b67f64ebc107f5820b86420770b20836b5429b200fd21bdeab414834eeca9c7913dee26a025&scene=21#wechat_redirect)</ins>

<ins>3、[向 Excel 说再见，神级编辑器统一表格与 Python](http://mp.weixin.qq.com/s?__biz=MzA5ODM5MDU3MA==&mid=2650865342&idx=1&sn=4cedcf5f3ee0c5ccd4c27130f7ef5d5a&chksm=8b6619fbbc1190edde7308b6612cc0e367f096012356578ad8ce6d0a3bbc93fb2045d5c2668c&scene=21#wechat_redirect)</ins>

看完本文有收获？请转发分享给更多人

**推荐关注「数据分析与开发」，提升数据技能**

![](../../../_resources/0_wx_fmt_png_4c425febeb0240e792c72dd2cfac46fd.png)

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

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___d39daf7c88c747cd9.bmp"/>

Scan to Follow