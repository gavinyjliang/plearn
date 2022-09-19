# python连接MySQL数据库之pandas数据整理

<a id="copyright_logo"></a>Original 昊哥 <a id="profileBt"></a><a id="js_name"></a>昊哥说测试 *2022-03-03 16:09*

本文主要介绍，python连接MySQL服务器的一个库pymysql。我们在进行python链接MySQL数据库之前，需要先安装一下pymysql。如果你是使用pycharm操作的话，直接在preferences中直接点击安装即可。或者直接是pip命令在线安装。

pymysql  模块安装:

进入cmd终端输入：pip install pymysql  命令安装。

pandas 模块安装：

今日cmd 终端输入：pip install pandas  命令安装。

    我们用python 来进行数据库的操作，那么第一步就应该连接数据库，这里我用的是pymysql模块的 connect 方法来连接数据库。

- 使用 connect 创建连接对象。
    

<img width="677" height="88" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__2cc439ab78d54d49a.png"/>

注意：charset="utf8",编码不要写成“utf-8”

- connect.cursor 创建游标对象，sql 语句的执行基本都在游标上进行。
    

<img width="677" height="287" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__0f51ce6c1cf34dc19.png"/>

- 定义一个要执行的sql语句。
    

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

- cursor.execute() 方法执行sql语句。
    

<img width="677" height="353" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__d09efd09fbb94e6e9.png"/>

- 使用pandas的 DataFrame 方法对查询出来的数据进行数据清洗处理。创建一个xlsx文件，把清洗过的数据写入xlsx文件中，最后导出到D盘。
    

<img width="677" height="554" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__08ecd87ed480454f8.png"/>

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

- 调用close 方法关闭游标 cursor 和数据库连接。
    

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__022dbfbec6824df29.png)

    最后把脚本编译成exe执行程序，直接打开就能进行使用。具体如何去编译脚本我在前面的文章有讲到，不清楚的可以去看一下《Pycharm项目工程编译exe方法》这篇文章。

People who liked this content also liked

MySQL使用军规，你不知道的MySQL优化都在这里了

mamba架构算法

不看的原因

- 内容质量低
- 不看此公众号

MySQL的内部组件结构

程序JAVA圈

不看的原因

- 内容质量低
- 不看此公众号

mysql的归档日志：bin-log

程序JAVA圈

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___804c0f42b19245a79.bmp"/>

Scan to Follow