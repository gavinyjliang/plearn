# Python提取并整理txt文档中目标数据

<a id="copyright_logo"></a>Original <a id="js_author_name"></a>付学威 <a id="profileBt"></a><a id="js_name"></a>大气化学python笔记 *2021-11-16 12:07*

收录于话题

<a id="js_article_tag_name__2137135026105286658"></a>#数据处理 <a id="js_article_tag_num__2137135026105286658"></a>2 <a id="js_article_tag_tips__2137135026105286658"></a>个

<a id="js_article_tag_name__2070277671715930115"></a>#python <a id="js_article_tag_num__2070277671715930115"></a>29 <a id="js_article_tag_tips__2070277671715930115"></a>个

<a id="js_article_tag_name__2111129921791000576"></a>#数据分析案例 <a id="js_article_tag_num__2111129921791000576"></a>13 <a id="js_article_tag_tips__2111129921791000576"></a>个

<a id="js_article_tag_name__2073532319717457922"></a>#pandas <a id="js_article_tag_num__2073532319717457922"></a>17 <a id="js_article_tag_tips__2073532319717457922"></a>个

**Python数据处理系列**

**Python数据处理案例(六)**

**大家好，今天给大家分享的案例是我在科研学习中遇到的一个数据处理问题。我需要分析的文件是isrpia2模型输出的txt文件，该文档中包含了我所需要的目标数据，用于计算气溶胶的酸度和液态水含量。模型每运行一次，就会有一个相应的txt文件，多次运行后，txt文档可能多达几十个，手动整理起来将会耗费大量的时间和精力。为了提高工作效率，我写了一个python程序，自动化地完成该过程。****总的来说，需要让python做的事情，是将两千多行的txt文档中所需要的数据提取出来，整理成我想要的数据格式。**

**目录**

1.  txt文档组成及目标数据
    
2.  整理后的目标数据
    
3.  代码实现
    

<img width="36" height="36" src="../../../_resources/640_wx_fmt_gif_wxfrom_5_wx_lazy__10c03e33c91b408db.gif"/>

**01**

**txt文档组成及目标数据**

txt文档中共分为26部分，每部分以 *** \[ INPUT \] ***为起始行，以====为结束行。我的目标数据为如下红框中所标注的第二列数据。可以发现目标数据的位置存在着固定的规律，即在GAS: 所在行的下三行和LIQUID AEROSOL: 所在行的下一行和后15行。目标数据有着固定的规律，便可以使用python编程来自动地提取整个txt文档中的目标数据。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

**02**

**目标数据及格式**

最终将每个txt文档中目标数据整理汇总成如下excel表格，第一列为目标数据的名称，后面的各列分别为txt文档中各个部分的目标数据：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

**03**

**代码实现**

代码的整体思路，是逐行遍历txt文档的内容，当遍历到 "GAS:" 所在行时，便取其下三行数据中第一个数据，即为第一部分目标数据，当遍历到 "LIQUID AEROSOL: " 所在行时，便取其下一行和其后第15行中的另一部分目标数据。这里展示了提取单个txt文档中数据的代码，当需提取多个isrpia2模型输出的txt文件时，只需要将文件名和目标数据的保存名称依次更改，运行程序即可自动地提取整理好每个txt文档中的目标数据。

```
`import pandas as pd
NH3_GAS = []
HNO3_GAS = []
HCL_GAS = []
WATER = []
PH =[]
with open("DEMO1.txt", "r") as f:
    lines = f.readlines()
    for index,item in enumerate(lines): # 对txt每行内容进行索引赋值
        if 'GAS:' in item:
            NH3_GAS_temp = lines[index+1].strip('\n').split(' ') # 提取出目标行中的目标数据
            NH3_GAS_temp = [x for x in NH3_GAS_temp if x !='']
            NH3_GAS.append(NH3_GAS_temp[2])
            HNO3_GAS_temp = lines[index + 2].strip('\n').split(' ')
            HNO3_GAS_temp = [x for x in HNO3_GAS_temp if x != '']
            HNO3_GAS.append(HNO3_GAS_temp[2])
            HCL_GAS_temp = lines[index + 3].strip('\n').split(' ')
            HCL_GAS_temp = [x for x in HCL_GAS_temp if x != '']
            HCL_GAS.append(HCL_GAS_temp[2])
        if 'LIQUID AEROSOL:' in item:
            WATER_temp = lines[index+1].strip('\n').split(' ')
            WATER_temp = [x for x in WATER_temp if x != '']
            WATER.append(WATER_temp[2])
            PH_temp = lines[index+15].strip('\n').split(' ')
            PH_temp = [x for x in PH_temp if x !='']
            PH.append(PH_temp[2])
data = pd.DataFrame({'NH3_GAS':NH3_GAS,'HNO3_GAS':HNO3_GAS,'HCL_GAS':HCL_GAS,'WATER':WATER,'PH':PH},dtype='float32')
data.T.to_excel('run1.xlsx')
print(data.T)
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

总结

本文介绍了python提取txt文本中的目标数据，主要使用的方法是通过enumerate()函数对txt文本各行进行索引赋值，然后根据目标数据分布规律，根据行索引进行定位并提取数据，最后运用pandas库保存目标数据，导出为excel格式。希望本文能对读者今后使用python处理txt文档提供思路和借鉴，需要本文用于演示的txt文档的，可在公众号对话框，点击**与我联系**，备注来意即可。

1

**END**

1

![](../../../_resources/0_wx_fmt_png_e41ec6e74e91459da0269d4316c12053.png)

**大气化学python笔记**

记录与分享python学习心得。请多多支持！

<a id="js_profile_article"></a>32篇原创内容

Official Account

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

**球分享**

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

**球点赞**

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

**球在看**

收录于话题 #<a id="js_album_keep_read_title"></a>数据分析案例

 <a id="js_album_keep_read_size"></a>13个

<a id="js_album_prev"></a>上一篇 <a id="js_album_keep_read_pre_title"></a>Python分析广州近五年4万+条空气质量时间序列数据 <a id="js_album_next"></a>下一篇 <a id="js_album_keep_read_next_title"></a>python数据处理案例(五)

People who liked this content also liked

Go语言学习笔记05 -- 复合数据类型之数组与切片

奶昔的学习笔记

不看的原因

- 内容质量低
- 不看此公众号

9个好用的python技巧分享

AI算法之道

不看的原因

- 内容质量低
- 不看此公众号

Python基础(下)

啊颜的学习场所

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___e9320b8227fd49c8b.bmp"/>

Scan to Follow