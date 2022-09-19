# 5分钟搞定几百张表格转换，Python办公自动化就是这么6！

<a id="profileBt"></a><a id="js_name"></a>python数据分析之禅 *2022-04-29 12:44* *Posted on <a id="js_ip_wording"></a>北京*

The following article is from Python数据科学家之路 Author 老狼Max

<a id="copyright_info"></a>[![](../../../_resources/0_8e3460a73dca4bdb824fb19bb2acd4df.jpg)<br>**Python数据科学家之路** .<br>瀚海天蓝，万物互联。这个时代终究还是来了！ 淹没在海量的数据中是否让你感到窒息？ 没关系，请和我一起用Python作为指挥棒，舞动数据，挖掘深埋在数据里面的宝藏！ Python数据科学家之路，分享我对数据的思考，记录修炼之路的点滴与进步](#)

最近在参加学习开源社区Datawhale组织的"21天精通Pandas学习"，其中有个练习题做起来很有意思，练习题本身很简单，我在这里稍微引申一下让大家体会一下Pandas处理数据功能的灵活和强大。

题目如下：提供2020-04-12到2020-12-31日的美国新冠病毒（COVID-19）各个州的统计数据，原始数据是每一天为一份csv文件，每份csv文件包含了各个州的报告感染人数，死亡人数等数据，如下：

1.  原始数据：以日期命名的csv文件， 部分截图<img width="632" height="727" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__51df3ae716534ab69.png"/>
    
2.  每份文件包含数据， 以其中一份为例：<img width="632" height="260" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__86d61e3f74de4c059.png"/>
    

每份文件的格式都相同，包含了所有州当天的数据统计情况。现在要求从**「每份」**文件中提取纽约州的'Confirmed','Deaths','Recovered','Active'四项数据数据，并重新组成一个新的DataFrame，以每份文件的文件名，即日期为索引，四项数据中每一项为一列。其实如果就几份文件，直接新建一个新的表格，在 CTRL + C在 CTRL + V拉到了。但是这里有将近300份表格，不上办公自动化要死人的。用python，一个循环语句全部搞定！

```
import pandas as pd
import numpy as np
import os
path = './covid_19_daily_reports_us/'
print(os.listdir(path)[:3]) # 打印前五份文件查看
print('There are {} reports need processed'.format(len(os.listdir(path))))
>>>
['04-12-2020.csv', '04-13-2020.csv', '04-14-2020.csv']
There are 264 reports need processed

```

下面为代码的主体部分：

```
state = 'New York'
need_col = ['Confirmed','Deaths','Recovered','Active']
def file_process(f):
    f_index = [f.split('.')[0]] # 注意index必须要是一个列表的形式
    f_df = pd.read_csv(path + f)
    new_df = f_df[f_df['Province_State'] == state][need_col]
    new_df.index = f_index
    
    return new_df
# 使用一个文件调试，看看是否能够正确的提取信息
file_process('05-01-2020.csv')

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

上面的图片为程序最后一句测试的结果,可以看到正确输出需要的内容。上面的代码，主要部分就是一个函数，用来从每份文件中提取文件名作为新的DataFrame的索引，然后扣取需要的内容。在这里注意我们将需要提取的州（New York)和内容（'Confirmed','Deaths','Recovered','Active'）定义在函数体的外部，这样做是防止将来有人更改需求的是时候可以不用重写函数，只更改外部的变量即可。例如现在老特跑过来跟你说我想看看佛罗里达州的情况，不要死亡人数，只看看确诊人数，这时只要在外面重新定义state和need_col即可。这样做也体现了代码中函数特征：可以高度复用，但同时和程序的其他部分保持低耦合的关系。有了这个函数，下面加一个循环搞定所有文件：

```
# 读取所有样本
df1 = file_process('04-12-2020.csv')
for file in os.listdir(path)[1:]:
    df1 = pd.concat([df1, file_process(file)])
df1

```

<img width="657" height="587" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__9c64495869f04c7b9.png"/>

OK，搞定！下面我们在引申一下，需求更改：现在的原始数据是按照日期来统计，现在要求按照州来组织每份文件。即一份文件为一个州，包含所有天的'Confirmed','Deaths','Recovered','Active'数据，同时以州名为文件名。上面我们已经完成了纽约州的统计，稍微一思考，也就是把这个程序在每个州上运行一遍即可：

1.  获取所有州的州名形成一个列表
    
2.  遍历这个列表，将每个州取出重复之前的动作，即遍历所有文件，取出需要的内容，之后在将取出的内容写入csv文件，以州名命名文件名即可
    

```
for state in df['Province_State']:
    df1 = file_process('04-12-2020.csv')
    for file in os.listdir(path)[1:]:
        df1 = pd.concat([df1, file_process(file)])
        df1.to_csv('./state_data/' + state + '.csv')

```

也就是多一个循环的事！OK，运行完之后，我们可以看到在定义的文件夹下程序写好的文件：

<img width="657" height="492" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__486d759fa15e4e0a8.png"/>

随便打开一份查看：![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__2f4da4c2fd90472c9.png)

OK，没有问题，搞定！这就完了吗？没有，本期我是想给大家介绍一个非常好用的工具——进度条，tqdm! 大家有没有发现程序在跑循环的时候，有时候因为数据量比较大，等待时间会比较长。有时候会有一种程序挂掉的错觉。这时候如果有个进度条告诉你循环程序跑到哪里了，还剩多少就nice了！tqdm这个工具就完美的解决了这个问题。使用也超级简单，在可迭代对象前面加上即可！

```
# 加上可视化进度条
from tqdm import tqdm # 先导入tqdm
for state in tqdm(df['Province_State']):  # 将可迭代对象封装到tqdm即可
    df1 = file_process('04-12-2020.csv')
    for file in os.listdir(path)[1:]:
        df1 = pd.concat([df1, file_process(file)])
        df1.to_csv('./state_data/' + state + '.csv')

```

这里没有运行的效果的截屏展示，网上找了一张图，感受一下！<img width="657" height="423" src="../../../_resources/640_wx_fmt_gif_wxfrom_5_wx_lazy__f573cdcc61764c3aa.gif"/>

效果完美！这也是为什么要学习Python的原因，因为有完善丰富的社区，很多人开发了很多好用的工具，避免了重复造轮子。

People who liked this content also liked

像Python一样玩C/C++

...

光城

不看的原因

- 内容质量低
- 不看此公众号

Python 全自动解密解码神器 — Ciphey

...

Python实用宝典

不看的原因

- 内容质量低
- 不看此公众号

四行代码秒解微积分！Python这个模块神了！

...

快学Python

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___b4f6fe9e556a471d8.bmp"/>

Scan to Follow