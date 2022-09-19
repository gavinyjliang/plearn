# Python 自动化办公之 Excel 对比工具

<a id="profileBt"></a><a id="js_name"></a>AI科技大本营 *2022-03-30 18:30*

The following article is from 萝卜大杂烩 Author 周萝卜

<a id="copyright_info"></a>[![](../../../_resources/0_df6e1e604298426e811933505429739e.jpg)<br>**萝卜大杂烩** .<br>Do it by yourself！爱好Python，测试，NLP，数据分析，小程序，k8s等技术，期待与你一同成长](#)

<img width="657" height="116" src="../../../_resources/640_wx_fmt_gif_wxfrom_5_wx_lazy__7344de38ce034e7f9.gif"/>

作者 | 周萝卜

来源丨萝卜大杂烩

今天我们继续分享真实的自动化办公案例，希望各位 Python 爱好者能够从中得到些许启发，在自己的工作生活中更多的应用 Python，使得工作事半功倍！

### 需求

由于工作当中经常需要对比前后两个 Excel 文件，文件内容比较多，人工肉眼对比太费劲，还容易出错，搞个 Python 小工具，会不会事半功倍

<img width="641" height="187" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__587cebd68e4f4f9fb.png"/>

运行脚本，可以把前后两个 Excel 文件当中不同的内容数据展现出来，不同 sheet 页签表示不同的数据处理结果

### 需求解析

不需要解析，直接干

### 代码实现

我们先导入两份测试数据，进行 old 和 new 的处理，注意数据中 account number 是唯一索引

```


old = pd.read_excel('sample-address-1.xlsx', 'Sheet1', na_values=['NA'])
new = pd.read_excel('sample-address-2.xlsx', 'Sheet1', na_values=['NA'])
old['version'] = "old"
new['version'] = "new"



```

<img width="641" height="276" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__8fedae6a21034fb08.png"/>

对于我们这个小工具，主要考虑三种变化类型

- 哪些是新增的 account
    
- 哪些是被删除的 account
    
- 哪些是被修改的 account
    

对于新增和删除的 account，我们可以直接用两份数据相减即可

```


old_accts_all = set(old['account number'])
new_accts_all = set(new['account number'])
dropped_accts = old_accts_all - new_accts_all
added_accts = new_accts_all - old_accts_all



```

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__639df7c30a95481fb.png)

接下来我们再将所有的数据拼接到一起，并使用 drop_duplicates 来保留被修改的数据

```


all_data = pd.concat([old,new],ignore_index=True)
changes = all_data.drop_duplicates(subset=["account number",
                                           "name", "street",
                                           "city","state",
                                           "postal code"], keep='last')



```

<img width="641" height="287" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__18cd50f4a5894306a.png"/>

接下来，我们需要找出哪些 account 有重复的条目，重复的 account 表明更改了我们需要标记的字段中的值。我们可以使用重复函数来获取所有这些 account 的列表，并仅过滤掉那些重复的 account

```


dupe_accts = changes[changes['account number'].duplicated() == True]['account number'].tolist()
dupes = changes[changes["account number"].isin(dupe_accts)]dupe_accts = changes[changes['account number'].duplicated() == True]['account number'].tolist()dupes = changes[changes["account number"].isin(dupe_accts)]



```

<img width="641" height="203" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__d07005648bc847eab.png"/>

现在我们将旧数据和新数据进行拆分，删除不必要的版本列并将 account 设置为索引

```


change_new = dupes[(dupes["version"] == "new")]
change_old = dupes[(dupes["version"] == "old")]
change_new = change_new.drop(['version'], axis=1)
change_old = change_old.drop(['version'], axis=1)
change_new.set_index('account number', inplace=True)
change_old.set_index('account number', inplace=True)
df_all_changes = pd.concat([change_old, change_new],
                            axis='columns',
                            keys=['old', 'new'],
                            join='outer')
df_all_changes



```

<img width="641" height="279" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__f215d829e8404c03b.png"/>

接下来我们定义一个函数来展示从一列到另一列的变化

```


def report_diff(x):
    return x[0] if x[0] == x[1] else '{} ---> {}'.format(*x)def report_diff(x):    return x[0] if x[0] == x[1] else '{} ---> {}'.format(*x)



```

现在使用 swaplevel 函数来获取彼此相邻的旧列和新列

<img width="481" height="135" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__4763092050284bcf9.png"/>

最后我们使用 groupby 然后应用我们自定义 report_diff 函数将两个相应的列相互比较

```


df_changed = df_all_changes.groupby(level=0, axis=1).apply(lambda frame: frame.apply(report_diff, axis=1))
df_changed = df_changed.reset_index()df_changed = df_all_changes.groupby(level=0, axis=1).apply(lambda frame: frame.apply(report_diff, axis=1))df_changed = df_changed.reset_index()



```

<img width="641" height="130" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__bed0251601b44cc6a.png"/>

接下来我们需要找出被删除和新增的数据

```


df_removed = changes[changes["account number"].isin(dropped_accts)]
df_added = changes[changes["account number"].isin(added_accts)]df_removed = changes[changes["account number"].isin(dropped_accts)]df_added = changes[changes["account number"].isin(added_accts)]



```

我们可以使用单独的选项卡将所有内容输出到 Excel 文件，对应于更改、添加和删除

```


output_columns = ["account number", "name", "street", "city", "state", "postal code"]
writer = pd.ExcelWriter("my-diff.xlsx")
df_changed.to_excel(writer,"changed", index=False, columns=output_columns)
df_removed.to_excel(writer,"removed",index=False, columns=output_columns)
df_added.to_excel(writer,"added",index=False, columns=output_columns)
writer.save()



```

最后，我们就得到了最开始的效果图片展示的一个新的 Excel 文件

当然上面的代码对于毫无编程的人来说还是有一点点复杂，我们还是做成 GUI 小程序吧，这次我们使用 Tkinter 来编写 GUI 程序

我们首先导入 Tkinter 库并进行初始化

```


import tkinter
from tkinter import *
from tkinter import Label, Button, Entry, messagebox
from tkinter import filedialog
from deal import deal_excel
window = tkinter.Tk()
path_file1 = StringVar()
path_file2 = StringVar()
path_path = StringVar()
window.geometry('380x150')



```

这里我们定义了三个 String 类型的变量，用来保存文件地址和文件夹路径

然后我们进行简单的页面排版，只需要用到 Label，Entry 和 Button 就够了

```


label1 = Label(window, text="文件1：").grid(column=0, row=0)
txt1 = Entry(window, width="30", textvariable=path_file1).grid(column=1, row=0)
button1 = Button(window, text="文件选择1", command=selectFile1).grid(column=2, row=0)
label2 = Label(window, text="文件2：").grid(column=0, row=1)
txt2 = Entry(window, width="30", textvariable=path_file2).grid(column=1, row=1)
button2 = Button(window, text="文件选择2", command=selectFile2).grid(row=1, column=2)
label3 = Label(window, text="新文件路径：").grid(column=0, row=2)
txt3 = Entry(window, width="30", textvariable=path_path)
txt3.grid(column=1, row=2)
button3 = Button(window, text="新文件路径", command=selectPath).grid(row=2, column=2)
button4 = Button(window, text="开始处理", command=save_path).grid(row=3, column=1)



```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

用于获取文件和文件夹的函数

```


def selectFile1():
    path_ = filedialog.askopenfilename()
    path_file1.set(path_)



```

用于保存新生成文件和提示消息的函数

```


def save_path():
    path = txt3.get()
    deal_excel(path)
    res = "对比处理完成！"
    messagebox.showinfo('萝卜大杂烩', res)



```

这样，一个简单的 Excel 对比工具就完成啦

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

好了，这样我们就完成了一个简易的 GUI 拆分 PDF 文件的工具喽~

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![](../../../_resources/0_wx_fmt_png_00bdfccd2d864ab3bb5a20532553bad2.png)

**AI科技大本营**

为AI领域从业者提供人工智能领域热点报道和海量重磅访谈；面向技术人员，提供AI技术领域前沿研究进展和技术成长路线；面向垂直企业，实现行业应用与技术创新的对接。全方位触及人工智能时代，连接AI技术的创造者和使用者。

<a id="js_profile_article"></a>1426篇原创内容

Official Account

往

期

回

顾

技术

[用Python写了个使命召唤外挂](http://mp.weixin.qq.com/s?__biz=Mzg4NDQwNTI0OQ==&mid=2247558139&idx=3&sn=6a7a845120c437339a0b0ef8527cdf67&chksm=cfbb0695f8cc8f83e6d30f350d6522d176357cbba88380b65eeddd5b45074de49edfc47c2abe&scene=21#wechat_redirect)

资讯

[俄罗斯 Android 系统受限](http://mp.weixin.qq.com/s?__biz=Mzg4NDQwNTI0OQ==&mid=2247558139&idx=1&sn=8c5fc4653c64a1ccca96151be8bb7d7c&chksm=cfbb0695f8cc8f83034416566192e81f47f7ee20ee1234c8c0a1db943e94342941b1a341d19b&scene=21#wechat_redirect)

技术

[面向小白超全Python可视化教程](http://mp.weixin.qq.com/s?__biz=Mzg4NDQwNTI0OQ==&mid=2247558059&idx=1&sn=b1ac8efe1aa460d4e49c2f7f4db85bab&chksm=cfbb06c5f8cc8fd3652c9195de5b78d0bb5012c9f27c4da247a8902e73bbd57d8fb4e4f92249&scene=21#wechat_redirect)

技术

[一行Python代码能干嘛？来看！](http://mp.weixin.qq.com/s?__biz=Mzg4NDQwNTI0OQ==&mid=2247557004&idx=3&sn=df96e9d68ac312b52d0eeab8851b5a5e&chksm=cfbafae2f8cd73f4f8c84909dc2b914312e63b3aa59801446b0089d7a6c97fb0760d66d382d0&scene=21#wechat_redirect)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

**分享**

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

**点收藏**

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

**点点赞**

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

**点在看**

People who liked this content also liked

Fortran基础编程（入门简介篇）

易木木响叮当

不看的原因

- 内容质量低
- 不看此公众号

python也能轻松实现界面编程

简易编程网

不看的原因

- 内容质量低
- 不看此公众号

【Docker学习】1、使用 Linux（CentOS7）搭建 Docker 基础环境

花海里

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___6b43be1063774e69b.bmp"/>

Scan to Follow