# Pandas学习笔记(一)

<a id="copyright_logo"></a>Original <a id="js_author_name"></a>付学威 <a id="profileBt"></a><a id="js_name"></a>大气化学python笔记 *2021-10-21 23:21*

收录于话题

<a id="js_article_tag_name__2070277671715930115"></a>#python <a id="js_article_tag_num__2070277671715930115"></a>29 <a id="js_article_tag_tips__2070277671715930115"></a>个

<a id="js_article_tag_name__2082367141265080322"></a>#数据分析 <a id="js_article_tag_num__2082367141265080322"></a>14 <a id="js_article_tag_tips__2082367141265080322"></a>个

<a id="js_article_tag_name__2073532319717457922"></a>#pandas <a id="js_article_tag_num__2073532319717457922"></a>17 <a id="js_article_tag_tips__2073532319717457922"></a>个

<a id="js_article_tag_name__2101124600976703488"></a>#pandas学习笔记 <a id="js_article_tag_num__2101124600976703488"></a>6 <a id="js_article_tag_tips__2101124600976703488"></a>个

<img width="613" height="204" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__097aea228aff4646a.png"/>

**Python数据分析系列之**

**Panda学习笔记(一)**

——安装导入Pandas库，读取与保存数据文件。

Panda是数据分析三剑客之一，是Python的核心数据分析库，它提供了快速、灵活、明确的数据结构，能够简单、直观、快速地处理各种类型的数据：

1.  与SQL或Excel表类似的数据；
    
2.  有序或无序(非固定频率)的时间序列数据；
    
3.  带行列标签的矩阵数据；
    
4.  任意其他形式的观测、统计数据集；
    

Pandas提供了两个主要数据结构，一维的Series和二维的DataFrame，可以处理多个领域的大多数典型数据案例。Pandas是基于numpy开发的，可以与python其他的第三方库完美集成，是python中用来处理数据最理想的工具。

本文将介绍pandas库的安装，及如何读取与保存数据文件。

<img width="36" height="36" src="../../../_resources/640_wx_fmt_gif_wxfrom_5_wx_lazy__3a59b72a5ff545d69.gif"/>

**01**

**Pandas库安装与导入**

之前的的推文 [python环境及常用IDE安装教程](http://mp.weixin.qq.com/s?__biz=MzkwMDI5MDAzOQ==&mid=2247483675&idx=1&sn=d3e92c3578327e949b360aa92c7db411&chksm=c0470583f7308c9560a0d27fdf857bbdb177b17da6d29697b9f1e1b79d3bd762d5cdc46d341b&scene=21#wechat_redirect) 介绍了如何配置python环境，包括Anaconda和IDE的安装。安装好Anaconda环境之后，其会自带pandas库，无需再手动安装。

手动安装也很简单，使用pip安装即可。在系统搜索框中输入cmd，打开‘命令提示符’窗口，输入以下命令，等待系统自动安装配置即可：

```
pip install pandas
```

除了pip工具安装之外，另一种手动安装方法是在pycharm开发环境中安装。运行pycharm—file—setting——project demo—project interpreter——选择想要安装pandas的python解释器，点击添加(+)按钮进入搜索界面。

<img width="677" height="492" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_d2b08983dc5a4daab.jpg"/>

搜索界面输入pandas进行搜索，单击install Package按钮即可安装pandas库。

安装完成后，在cmd或pycharm中输入import pandas，运行后无报错，说明pandas已经成功安装。打印输出pandas.\_\_version\_\_可查看pandas的版本。

```
`import pandas as pd  #导入pandas模块``print('pandas版本为：',pd.__version__) # 查看pandas版本`
```

![Image](../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_0c54e1900a5240908.jpg)

<img width="36" height="36" src="../../../_resources/640_wx_fmt_gif_wxfrom_5_wx_lazy__3a59b72a5ff545d69.gif"/>

**02**

**读取数据**

|     |     |     |
| --- | --- | --- |
| **数据类型** | **说明** | **读取办法** |
| csv、txt | 默认逗号分隔 | pd.read_csv() |
| table | 默认\\t分隔 | pd.read_table() |
| excel | xls或xlsx | pd.read_excel() |
| sql | 关系数据库表 | pd.read_sql() |
| json | json文件 | pd.read_json() |
| html | html文件 | pd.read_html() |

**2.1**

**读取csv、txt数据**

```
`# 读取数据，sep指定分隔符号(按文件中分隔符类型来确定)，encoding指定编码类型``import pandas as pd``data1 = pd.read_csv('example1.csv', sep=',', encoding='gbk')``data2 = pd.read_csv('example2.txt', sep='\t', encoding='gbk')``print('查看前5行数据:\n',data1.head()) # 默认是5行，指定行数写小括号里``print('**'*20)``data1.head(5).to_csv('example1前五行数据.csv') # 将data1前五行数据保存到csv文件``data1.head(5).to_excel('example1前五行数据.xlsx')# 将data1前五行数据保存到excel文件``# 运行完程序后，在目录中可查看新生成的csv和xlsx文件。``print('查看前3行数据:\n',data2.head(3))``print('**'*20)``print('查看数据组成(行数,列数):\n',data1.shape)``print('**'*20)``print('查看列表名称:\n',data1.columns)``print('**'*20)``print('查看索引名称:\n',data1.index)``print('**'*20)``print('查看数据类型:\n',data1.dtypes)``print('**'*20)`
```

输出结果如下：

<img width="677" height="619" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_4ec4765dbe9546eda.jpg"/>

**2.2**

**读取excel数据**

```
`import pandas as pd            #导入pandas模块``#解决数据输出时列名不对齐的问题``pd.set_option('display.unicode.east_asian_width', True)``# 解决数据输出时行列显示不全的问题``pd.set_option('display.max_rows',1000)``df=pd.read_excel('example3.xlsx')  #读取Excel文件``# 查看数据结构，数据类型，列名、索引名等，和2.1小节中代码相���``#显示前5行数据``print(df.head()) `` #显示后5行数据``print(df.tail())`
```

读入的前5行和后5行数据输出如下：

<img width="677" height="293" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_4f7d413cfe4142e6b.jpg"/>

<img width="36" height="36" src="../../../_resources/640_wx_fmt_gif_wxfrom_5_wx_lazy__3a59b72a5ff545d69.gif"/>

总结

本文主要介绍了pandas库的安装、读取与保存数据等基础操作，所用到的命令和函数总结如下：

```
`# cmd命令行输入以下指令，安装pandas``pip install pandas``# 从csv、txt、xlsx、dat文件中读取数据``pd.read_csv(filename)``pd.read_excel(filename)``pd.read_table(filename)``# 数据保存``df.to_csv(filename)``df.to_excel(filename)`
```

1

**END**

1

![](../../../_resources/0_wx_fmt_png_a02085a399a34bf28d9920aaf193bbae.png)

**大气化学python笔记**

记录与分享python学习心得。请多多支持！

<a id="js_profile_article"></a>32篇原创内容

Official Account

<img width="677" height="381" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_11da45e76a5d44cd9.jpg"/>

<img width="54" height="54" src="../../../_resources/640_wx_fmt_gif_wxfrom_5_wx_lazy__69b113c5310f4714a.gif"/>

**球分享**

<img width="50" height="50" src="../../../_resources/640_wx_fmt_gif_wxfrom_5_wx_lazy__69b113c5310f4714a.gif"/>

**球点赞**

<img width="50" height="50" src="../../../_resources/640_wx_fmt_gif_wxfrom_5_wx_lazy__69b113c5310f4714a.gif"/>

**球在看**

People who liked this content also liked

Webpack原理与实践(一)：webpack究竟解决了什么问题？

前端一码平川

不看的原因

- 内容质量低
- 不看此公众号

【经验分享】linux 下使用 Makefile 快速构建单工程教程

极智视界

不看的原因

- 内容质量低
- 不看此公众号

我用了半年的时间，把python学到了能出书的程度

一起进步一起挣钱

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___99cfa218c2fa409c9.bmp"/>

Scan to Follow