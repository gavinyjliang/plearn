# 如何只用一行 Python 代码制作一个GUI(图形界面)？

<a id="profileBt"></a><a id="js_name"></a>AI科技大本营 *2022-05-20 18:00* *Posted on <a id="js_ip_wording"></a>北京*

The following article is from 法纳斯特 Author 小F

<a id="copyright_info"></a>[![](../../../_resources/0_73428230799248cbb7a9c0226af36274.jpg)<br>**法纳斯特** .<br>分享学习Python爬虫、数据分析、数据挖掘的点滴。](#)

<img width="661" height="117" src="../../../_resources/640_wx_fmt_gif_wxfrom_5_wx_lazy__3a914bff611a4e149.gif"/>

作者 | 小F

来源 | 法纳斯特

GUI(图形用户界面)，顾名思义就是用图形的方式，来显示计算机操作的界面，更加方便且直观。

一个好看又好用的GUI，可以大大提高大家的使用体验，提高效率。

比如你想开发一个计算器，如果只是一个程序输入，输出窗口的话，是没有用户体验的。

所以开发一个图形化的小窗口，就变得很有必要。

今天，就给大家介绍如何只用一行Python代码制作一个GUI。

主要使用Python的PySimpleGUI库来完成这个工作。

```


# 安装PySimpleGUI
pip install PySimpleGUI -i https://mirror.baidu.com/pypi/simple



```

详细的接口文档地址如下。

*https://pysimplegui.readthedocs.io/en/latest/call%20reference/*

▍1、选择文件夹

首先导入PySimpleGUI库，并且用缩写sg来表示。

```


import PySimpleGUI as sg
# 窗口显示文本框和浏览按钮, 以便选择一个文件夹
dir_path = sg.popup_get_folder("Select Folder")
if not dir_path:
    sg.popup("Cancel", "No folder selected")
    raise SystemExit("Cancelling: no folder selected")
else:
    sg.popup("The folder you chose was", dir_path)



```

通过使用PySimpleGUI的popup\_get\_folder()方法，一行代码就能实现选择文件夹的操作。

示例如下。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

点击Browse按钮，选择文件夹，文本框就会显示出文件夹的绝对路径。

点击OK按钮，显示最终选择的路径信息，再次点击OK按钮，结束窗口。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

如果没有选择文件夹，而是直接点击OK按钮，会直接提示没有选取文件夹。

▍2、选择文件

选择文件操作和上面选择文件夹的有点相似。

不同的是，选择文件可以设置multiple\_files(是否为多个文件)和file\_types(文件类型)参数。

```


# 窗口显示文本框和浏览按钮, 以便选择文件
fname = sg.popup_get_file("Choose Excel file", multiple_files=True, file_types=(("Excel Files", "*.xls*"),),)
if not fname:
    sg.popup("Cancel", "No filename supplied")
    raise SystemExit("Cancelling: no filename supplied")
else:
    sg.popup("The filename you chose was", fname)



```

示例如下。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

选择了多个Excel文件，最终结果返回了所有文件的路径地址。

▍3、选择日期

使用popup\_get\_date()方法，显示一个日历窗口。

```


# 显示一个日历窗口, 通过用户的选择, 返回一个元组(月, 日, 年)
date = sg.popup_get_date()
if not date:
    sg.popup("Cancel", "No date picked")
    raise SystemExit("Cancelling: no date picked")
else:
    sg.popup("The date you chose was", date)



```

示例如下。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

选择好日期后，点击OK按钮，即可返回日期元组结果。

▍4、输入文本

使用popup\_get\_text()方法，显示一个文本输入框。

```


# 显示文本输入框, 输入文本信息, 返回输入的文本, 如果取消则返回None
text = sg.popup_get_text("Please enter a text:")
if not text:
    sg.popup("Cancel", "No text was entered")
    raise SystemExit("Cancelling: no text entered")
else:
    sg.popup("You have entered", text)



```

键入信息，示例如下。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

点击OK按钮，返回输入的文本信息。

如果没有输入，直接点击OK按钮，会提示没有文本输入。

▍5、弹窗无按钮

```


# 显示一个弹窗, 但没有任何按钮
sg.popup_no_buttons("You cannot click any buttons")


```

结果如下。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

▍6、弹窗无标题

```


# 显示一个没有标题栏的弹窗
sg.popup_no_titlebar("A very simple popup")



```

结果如下。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

▍7、弹窗只有OK按钮

```


# 显示弹窗且只有OK按钮
sg.popup_ok("You can only click on 'OK'")



```

结果如下。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

▍8、弹窗只有Error按钮(红色)

```


# 显示弹窗且只有error按钮, 按钮带颜色
sg.popup_error("Something went wrong")



```

结果如下。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

▍9、显示通知窗口

```


# 显示一个“通知窗口”, 通常在屏幕的右下角, 窗口会慢慢淡入淡出
sg.popup_notify("Task done!")



```

结果如下， Task done提示信息淡入淡出。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

▍10、弹窗选择

```


# 显示弹窗以及是和否按钮, 选择判断
answer = sg.popup_yes_no("Do you like this video?")
sg.popup("You have selected", answer)



```

结果如下。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

▍11、自定义弹窗

上面那些弹窗都是库自带的，如果想自定义创建，可以参考下面的方法。

```


# 自定义创建弹窗, 一行代码完成
choice, _ = sg.Window(
    "Continue?",
    [[sg.T("Do you want to subscribe to this channel?")], [sg.Yes(s=10), sg.No(s=10), sg.Button('Maybe', s=10)]],
    disable_close=True,
).read(close=True)
sg.popup("Your choice was", choice)



```

结果如下。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

▍12、实战

最后来个综合实战案例，将某个文件夹下所有的Excel文件中的**sheet表**，一一保存为**单独的Excel文件**。

代码如下，需要安装xlwings库，其中pathlib库是内置的。

```


from pathlib import Path
import PySimpleGUI as sg
import xlwings as xw
# 选择输入文件夹
INPUT_DIR = sg.popup_get_folder("Select an input folder")
if not INPUT_DIR:
    sg.popup("Cancel", "No folder selected")
    raise SystemExit("Cancelling: no folder selected")
else:
    INPUT_DIR = Path(INPUT_DIR)
# 选择输出文件夹
OUTPUT_DIR = sg.popup_get_folder("Select an output folder")
if not OUTPUT_DIR:
    sg.popup("Cancel", "No folder selected")
    raise SystemExit("Cancelling: no folder selected")
else:
    OUTPUT_DIR = Path(OUTPUT_DIR)
# 获取输入文件夹中所有xls格式文件的路径列表
files = list(INPUT_DIR.rglob("*.xls*"))
with xw.App(visible=False) as app:
    for index, file in enumerate(files):
        # 显示进度
        sg.one_line_progress_meter("Current Progress", index + 1, len(files))
        wb = app.books.open(file)
        # 提取sheet表为单独的Excel表格
        for sheet in wb.sheets:
            wb_new = app.books.add()
            sheet.copy(after=wb_new.sheets[0])
            wb_new.sheets[0].delete()
            wb_new.save(OUTPUT_DIR / f"{file.stem}_{sheet.name}.xlsx")
            wb_new.close()
sg.popup_ok("Task done!")



```

首先选择输入文件夹和输出文件夹的地址。

然后通过pathlib库对输入文件夹进行遍历，查找出所有xls格式文件的路径地址。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

点击OK按钮后，就会开始表格转换，操作如下。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

使用了one\_line\_progress_meter()方法显示程序处理的进度。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

20表示有20次循环，原始Excel文件总计有20个，需要处理20次，其他的都在上图中标示出来咯。

好了，今天的分享就到这里了，有兴趣的小伙伴可以自行去学习。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

**往期回顾**

[分享一个火遍全网的Python框架！](http://mp.weixin.qq.com/s?__biz=Mzg4NDQwNTI0OQ==&mid=2247560092&idx=1&sn=aa6ee28caf965170e0800be44023370b&chksm=cfbb0ef2f8cc87e454aa1e21164ef4c30c7f5b9f9cfdb50f30e3483f30a8c56eefbb35e95bef&scene=21#wechat_redirect)

[“由于一段Python代码，我的号被封了”](http://mp.weixin.qq.com/s?__biz=Mzg4NDQwNTI0OQ==&mid=2247560150&idx=1&sn=e45b944ff2ec6b932eae5c298a56ef86&chksm=cfbb0eb8f8cc87aec4c0a308484d9563e3573cf9b0c9ba50f7e1d9dc24937ba8e1aaf8b37f2f&scene=21#wechat_redirect)

[云上风景虽好，但不要盲目跟风！](http://mp.weixin.qq.com/s?__biz=Mzg4NDQwNTI0OQ==&mid=2247559943&idx=2&sn=054537cae62bd26286c028acb3ae7782&chksm=cfbb0e69f8cc877f89b5c87ed6b79c7c143c34a96a20e3ecaa245344380ed18abc4a18c04123&scene=21#wechat_redirect)

[Python中最简单易用的并行加速技巧](http://mp.weixin.qq.com/s?__biz=Mzg4NDQwNTI0OQ==&mid=2247560055&idx=1&sn=e870ba3fa23f66969ceb9b3f431057b2&chksm=cfbb0e19f8cc870fea11a6aa4eaf74d207b70c5f4ae0bb42e85c171d990b8dd183cb574b391a&scene=21#wechat_redirect)

```


![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

分享

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

点收藏

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

点点赞

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

点在看












```

<a id="js_view_source"></a>Read more

People who liked this content also liked

用 Python 制作可视化 GUI 界面，一键实现自动分类管理文件！

...

AI科技大本营

不看的原因

- 内容质量低
- 不看此公众号

​有了这份小抄，你还学不会Python？再也不怕不记得Python的语法了

...

编程小小

不看的原因

- 内容质量低
- 不看此公众号

一文弄懂Python中的 if \_\_name\_\_==\_\_main\_\_

...

AI算法之道

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___de2ac70f02d9480a9.bmp"/>

Scan to Follow