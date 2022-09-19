# 厉害了，用Python绘制动态可视化图表，并保存成gif格式

<a id="profileBt"></a><a id="js_name"></a>AI科技大本营 *2022-02-21 18:43*

The following article is from 关于数据分析与可视化 Author 俊欣

<a id="copyright_info"></a>[![](../../../_resources/0_7059a4cc03174fd7a707ddd8b41da0b2.jpg)<br>**关于数据分析与可视化** .<br>本公众号定期分享数据分析与可视化干货文章，并有时结合热点话题进行深入讨论，希望您会喜欢，要是哪里写的不好，也渴望倾听您的想法和意见，感谢！❤️](#)

<img width="661" height="117" src="../../../_resources/640_wx_fmt_gif_wxfrom_5_wx_lazy__27bd5fa79e2d46768.gif"/>

作者 | 俊欣

来源 | 关于数据分析与可视化

最近有粉丝问道说“是不是可以将这些动态的可视化图表保存成gif图”，小编立马就回复了说后面会写一篇相关的文章来介绍如何进行保存gif格式的文件。那么我们就开始进入主题，来谈一下Python当中的gif模块。

<img width="661" height="70" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__600dcd65e00547edb.png"/>

### **安装相关的模块**

首先第一步的话我们需要安装相关的模块，通过pip命令来安装

```


pip install gif



```

另外由于`gif`模块之后会被当做是装饰器放在绘制可视化图表的函数上，主要我们依赖的还是Python当中绘制可视化图表的`matplotlib`、`plotly`、以及`altair`这些模块，因此我们还需要下面这几个库

```


pip install "gif[altair]"     
pip install "gif[matplotlib]"
pip install "gif[plotly]"



```

<img width="661" height="70" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__cdb71176c7a04f43a.png"/>

### **gif 和 matplotlib 的结合**

我们先来看gif和matplotlib模块的结合，我们先来看一个简单的例子，代码如下

```


import random
from matplotlib import pyplot as plt
import gif
x = [random.randint(0, 100) for _ in range(100)]
y = [random.randint(0, 100) for _ in range(100)]
gif.options.matplotlib["dpi"] = 300
@gif.frame
def plot(i):
    xi = x[i*10:(i+1)*10]
    yi = y[i*10:(i+1)*10]
    plt.scatter(xi, yi)
    plt.xlim((0, 100))
    plt.ylim((0, 100))
frames = []
for i in range(10):
    frame = plot(i)
    frames.append(frame)
gif.save(frames, 'example.gif', duration=3.5, unit="s", between="startend")



```

output

<img width="280" height="210" src="../../../_resources/640_wx_fmt_gif_wxfrom_5_wx_lazy__e202542c78464cf0b.gif"/>

代码的逻辑并不难理解，首先我们需要定义一个函数来绘制图表并且带上`gif`装饰器，接着我们需要一个空的列表，通过`for`循环将绘制出来的对象放到这个空列表当中然后保存成`gif`格式的文件即可。

<img width="661" height="70" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__ec1bda517e85405ea.png"/>

### **gif和plotly的结合**

除了和matplotlib的联用之外，gif和plotly之间也可以结合起来用，代码如下

```


import random
import plotly.graph_objects as go
import pandas as pd
import gif
df = pd.DataFrame({
    't': list(range(10)) * 10,
    'x': [random.randint(0, 100) for _ in range(100)],
    'y': [random.randint(0, 100) for _ in range(100)]
})
@gif.frame
def plot(i):
    d = df[df['t'] == i]
    fig = go.Figure()
    fig.add_trace(go.Scatter(
        x=d["x"],
        y=d["y"],
        mode="markers"
    ))
    fig.update_layout(width=500, height=300)
    return fig
frames = []
for i in range(10):
    frame = plot(i)
    frames.append(frame)
gif.save(frames, 'example_plotly.gif', duration=100)



```

output

<img width="309" height="185" src="../../../_resources/640_wx_fmt_gif_wxfrom_5_wx_lazy__670e8f641d8b4adba.gif"/>

整体的代码逻辑和上面的相似，这里也就不做具体的说明了

<img width="661" height="70" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__4945787722064925b.png"/>

### **matplotlib多子图动态可视化**

上面绘制出来的图表都是在单张图表当中进行的，那当然了我们还可以在多张子图中进行动态可视化的展示，代码如下

```


# 读取数据
df = pd.read_csv('weather_hourly_darksky.csv')
df = df.rename(columns={"time": "date"})
@gif.frame
def plot(df, date):
    df = df.loc[df.index[0]:pd.Timestamp(date)]
    fig, (ax1, ax2, ax3) = plt.subplots(3, figsize=(10, 6), dpi=100)
    ax1.plot(df.temperature, marker='o', linestyle='--', linewidth=1, markersize=3, color='g')
    maxi = round(df.temperature.max() + 3)
    ax1.set_xlim([START, END])
    ax1.set_ylim([0, maxi])
    ax1.set_ylabel('TEMPERATURE', color='green')
    ax2.plot(df.windSpeed, marker='o', linestyle='--', linewidth=1, markersize=3, color='b')
    maxi = round(df.windSpeed.max() + 3)
    ax2.set_xlim([START, END])
    ax2.set_ylim([0, maxi])
    ax2.set_ylabel('WIND', color='blue')
    ax3.plot(df.visibility, marker='o', linestyle='--', linewidth=1, markersize=3, color='r')
    maxi = round(df.visibility.max() + 3)
    ax3.set_xlim([START, END])
    ax3.set_ylim([0, maxi])
    ax3.set_ylabel('VISIBILITY', color='red')
frames = []
for date in pd.date_range(start=df.index[0], end=df.index[-1], freq='1M'):
    frame = plot(df, date)
    frames.append(frame)
gif.save(frames, "文件名称.gif", duration=0.5, unit='s')



```

output

<img width="289" height="173" src="../../../_resources/640_wx_fmt_gif_wxfrom_5_wx_lazy__0f3556efaf07467bb.gif"/>

<img width="661" height="70" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__847e5c94145d4c1f8.png"/>

### **动态气泡图**

最后我们用plotly模块来绘制一个动态的气泡图，代码如下

```


import gif
import plotly.graph_objects as go
import numpy as np
np.random.seed(1)
N = 100
x = np.random.rand(N)
y = np.random.rand(N)
colors = np.random.rand(N)
sz = np.random.rand(N) * 30
layout = go.Layout(
    xaxis={'range': [-2, 2]},
    yaxis={'range': [-2, 2]},
    margin=dict(l=10, r=10, t=10, b=10)
)
@gif.frame
def plot(i):
    fig = go.Figure(layout=layout)
    fig.add_trace(go.Scatter(
        x=x[:i],
        y=y[:i],
        mode="markers",
        marker=go.scatter.Marker(
            size=sz[:i],
            color=colors[:i],
            opacity=0.6,
            colorscale="Viridis"
        )
    ))
    fig.update_layout(width=500, height=300)
    return fig
frames = []
for i in range(100):
    frame = plot(i)
    frames.append(frame)
gif.save(frames, "bubble.gif")



```

以上就是今天给同学们分享的小技巧，大家赶紧操练起来吧~

<img width="37" height="28" src="../../../_resources/640_wx_fmt_gif_wxfrom_5_wx_lazy__c67a5ad481ae4cb79.gif"/>

**好消息！好消息！**

Python 福利来啦，还等啥呢！现已开启招聘通道，**后台+小编微信**，投递你的简历呦~

**招聘：Python后端开发工程师（西安）**

<img width="562" height="951" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_0809907e105946beb.jpg"/>

<img width="562" height="112" src="../../../_resources/640_wx_fmt_gif_wxfrom_5_wx_lazy__7c0b976a10b34c51b.gif"/>

![](../../../_resources/0_wx_fmt_png_895148141f6445e2a6e11724e24e3d00.png)

**AI科技大本营**

为AI领域从业者提供人工智能领域热点报道和海量重磅访谈；面向技术人员，提供AI技术领域前沿研究进展和技术成长路线；面向垂直企业，实现行业应用与技术创新的对接。全方位触及人工智能时代，连接AI技术的创造者和使用者。

<a id="js_profile_article"></a>1426篇原创内容

Official Account

往

期

回

顾

资讯

[冬奥会夺金背后的杀手锏，是他](http://mp.weixin.qq.com/s?__biz=Mzg4NDQwNTI0OQ==&mid=2247555474&idx=1&sn=6ed705215bb3625a048b489455518252&chksm=cfbafcfcf8cd75ea33af095a6e07bc25e0739ffaf85dcc2f4684d0eaf1dd014f3e2e22db8750&scene=21#wechat_redirect)

资讯

[首个深度强化学习AI，控制核聚变](http://mp.weixin.qq.com/s?__biz=Mzg4NDQwNTI0OQ==&mid=2247555474&idx=2&sn=7f291fc2f8328432af13302ae5d9e85d&chksm=cfbafcfcf8cd75ea68b313492972cef0d5cd2fc22b714ef9bc0b753cc66f8eb60d7e350c8814&scene=21#wechat_redirect)

技术

[在 Python 中妙用短路机制](http://mp.weixin.qq.com/s?__biz=Mzg4NDQwNTI0OQ==&mid=2247555530&idx=2&sn=7e1a77b65d62a9b5ad1f5bc31ced0564&chksm=cfbafca4f8cd75b25779f68fe8c18955f09a7149f50ea5e26df5522482da88c7bb0d02f81c42&scene=21#wechat_redirect)

资讯

[英特尔要对外开放 x86 授权？](http://mp.weixin.qq.com/s?__biz=Mzg4NDQwNTI0OQ==&mid=2247555530&idx=1&sn=b66c4a7e2a864d0e20aea61cb5562765&chksm=cfbafca4f8cd75b24a60e81fe60e4a5d9cf2b9c498308c89cb1db68b83802cc5876192dbda6a&scene=21#wechat_redirect)

<img width="30" height="28" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__1559618675c1486aa.png"/>

**分享**

<img width="30" height="27" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__c8ea0d822b0d4bae9.png"/>

**点收藏**

<img width="30" height="28" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__394dd2187fc44a24b.png"/>

**点点赞**

<img width="30" height="28" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__83a3a479dbf345ab9.png"/>

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

Python是不是被严重高估了？

编程学习部

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___48591b911e2249fc9.bmp"/>

Scan to Follow