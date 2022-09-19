# 【Python】Matplotlib 可视化进阶之动画

<a id="profileBt"></a><a id="js_name"></a>机器学习初学者 *2022-05-31 12:00* *Posted on <a id="js_ip_wording"></a>浙江*

The following article is from 数据STUDIO Author 云朵君

<a id="copyright_info"></a>[![](../../../_resources/0_6ce94b11079d4b29a4565c14a70c6eb3.jpg)<br>**数据STUDIO** .<br>点击领取《Python学习手册》，后台回复「福利」获取。『数据STUDIO』专注于数据科学原创文章分享，内容以 Python 为核心语言，涵盖机器学习、数据分析、可视化、MySQL等领域干货知识总结及实战项目。](#)

##### 使用matplotlib可以很容易地创建动画框架。我们从一个非常简单的动画开始。

## matplotlib 动画

我们想制作一个动画，其中正弦和余弦函数在屏幕上逐步绘制。首先需要告诉matplotlib我们想要制作一个动画，然后必须指定想要在每一帧绘制什么。一个常见的错误是重新绘制每一帧的所有内容，这会使整个过程非常缓慢。相反地，只能更新必要的内容，因为我们知道许多内容不会随着帧的变化而改变。对于折线图，我们将使用`set_data`方法更新绘图，剩下的工作由matplotlib完成。

注意随着动画移动的终点标记。原因是我们在末尾指定了一个标记(`markevery=[-1]`)，这样每次我们设置新数据时，标记就会自动更新并随着动画移动。参见下图。

<img width="677" height="194" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__2bca1bdbb50942d79.png"/>

```
import numpy as np
import matplotlib.pyplot as plt
import matplotlib.animation as animation
fig = plt.figure(figsize=(7, 2))
ax = plt.subplot()
X = np.linspace(-np.pi, np.pi, 256, endpoint=True)
C, S = np.cos(X), np.sin(X)
(line1,) = ax.plot(X, C, marker="o", markevery=[-1], 
                   markeredgecolor="white")
(line2,) = ax.plot(X, S, marker="o", markevery=[-1], 
                   markeredgecolor="white")
def update(frame):
    line1.set_data(X[:frame], C[:frame])
    line2.set_data(X[:frame], S[:frame])
plt.tight_layout()
ani = animation.FuncAnimation(fig, update, interval=10)

```

如果我们现在想要保存这个动画，matplotlib可以创建一个mp4文件，但是选项非常少。一个更好的解决方案是使用外部库，如FFMpeg，它可以在大多数系统上使用。安装完成后，我们可以使用专用的FFMpegWriter，如下图所示:

```
writer = animation.FFMpegWriter(fps=30)
anim = animation.FuncAnimation(fig, update, 
                               interval=10,
                               frames=len(X))
anim.save("sine-cosine.mp4", writer=writer, dpi=100)

```

注意，当我们保存mp4动画时，动画不会立即开始，因为实际上有一个与影片创建相对应的延迟。对于正弦和余弦，延迟相当短，可以忽略。但对于长且复杂的动画，这种延迟会变得非常重要，因此有必要跟踪其进展。因此我们使用`tqdm`库添加一些信息。

```
from tqdm.autonotebook import tqdm
bar = tqdm(total=len(X)) 
anim.save("../data/sine-cosine.mp4", 
          writer=writer, dpi=300,
          progress_callback = lambda i, n: bar.update(1)) 
bar.close()

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

> \[Errno 2\] No such file or directory: 'ffmpeg'
> 
> 如果你在 macOS 上，只需通过 homebrew 安装它：`brew install ffmpeg`

## 人口出生率

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

```
x = data['指标'].values
rate= data['人口出生率(‰)']
y = rate.values
xvals = np.linspace(2002,2021,1000)
yinterp = np.interp(xvals,x,y)
(line1,) = ax.plot(xvals, yinterp, marker="o", 
                   markevery=[-1], markeredgecolor="white")
text = ax.text(0.01, 0.95,'text', ha="left", va="top", 
               transform=ax.transAxes, size=25)
ax.set_xticks(x)
def update(frame):
    line1.set_data(xvals[:frame], yinterp[:frame])
    text.set_text("%d 年人口出生率(‰) " % int(xvals[frame]))
    return line1, text

```

## 男女人口总数

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

```
# 设置画布
fig = plt.figure(figsize=(10, 5))
ax = plt.subplot()
# 数据准备
X = data['指标']
male, female =data['男性人口(万人)'], data['女性人口(万人)']
# 绘制折线图
(line1,) = ax.plot(X, male, marker="o", 
                   markevery=[-1], markeredgecolor="white")
(line2,) = ax.plot(X, female, marker="o", 
                   markevery=[-1], markeredgecolor="white")
# 设置图形注释
text = ax.text(0.01, 0.75,'text', 
               ha="left", va="top", 
               transform=ax.transAxes,size=20)
text2 = ax.text(X[0],male[0], '', ha="left", va="top")
text3 = ax.text(X[0],female[0], '', ha="left", va="top")
# 设置坐标轴刻度标签
ax.set_xticks(X)
ax.set_yticks([])
# 设置坐标轴线格式
ax.spines["top"].set_visible(False)
ax.spines["left"].set_visible(False)
ax.spines["right"].set_visible(False)
# 定义更新函数
def update(frame):
    line1.set_data(X[:frame+1], male[:frame+1])
    line2.set_data(X[:frame+1], female[:frame+1])
    text.set_text("%d 人口(万人)" % X[frame])
    text2.set_position((X[frame], male[frame]))
    text2.set_text(f'男性: {male[frame]}')
    text3.set_position((X[frame], female[frame]))
    text3.set_text(f'女性: {female[frame]}')
    return line1,line2, text
# 定义输出
plt.tight_layout()
writer = animation.FFMpegWriter(fps=5)
# 执行动画
anim = animation.FuncAnimation(fig, update, interval=500, frames=len(X))
# 存储动画
# 设置进度条
bar = tqdm(total=len(X))
anim.save(
    "num_people2.mp4",
    writer=writer,
    dpi=300,
    progress_callback=lambda i, n: bar.update(1),
)
# 关闭进度条
bar.close()

```

## 雨滴

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

```
# 设置雨滴绘图更新函数
def rain_update(frame):
    global R, scatter
  # 数据获取
    R["color"][:, 3] = np.maximum(0, R["color"][:, 3] - 1 / len(R))
    R["size"] += 1 / len(R)
    i = frame % len(R)
    R["position"][i] = np.random.uniform(0, 1, 2)
    R["size"][i] = 0
    R["color"][i, 3] = 1
    # 散点形状设置
    scatter.set_edgecolors(R["color"])
    scatter.set_sizes(1000 * R["size"].ravel())
    scatter.set_offsets(R["position"])
    return (scatter,)
# 绘制画布
fig = plt.figure(figsize=(6, 8), facecolor="white", dpi=300)
ax = fig.add_axes([0, 0, 1, 1], frameon=False)  # , aspect=1)
# 绘制初始化散点图
scatter = ax.scatter([], [], s=[], 
                     linewidth=0.5, edgecolors=[], 
                     facecolors="None",cmap='rainbow')
# 设置雨滴数量
n = 250
# 为雨滴设置参数值
R = np.zeros(
    n, dtype=[("position", float, (2,)), 
              ("size", float, (1,)),
              ("color", float, (4,))])
R["position"] = np.random.uniform(0, 1, (n, 2))
R["size"] = np.linspace(0, 1.5, n).reshape(n, 1)
R["color"][:, 3] = np.linspace(0, 1, n)
# 设置坐标轴格式
ax.set_xlim(0, 1), ax.set_xticks([])
ax.set_ylim(0, 1), ax.set_yticks([])
# 保存同上

```

## 流体

最后一个更加复杂的流体

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

**Matplotlib绘制动图成品赏析**

数据STUDIO

，赞 14

### 参考资料

\[1\]

Scientific Visualisation-Python & Matplotlib

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

```


![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

```


```


```


```


```


往期精彩回顾

- [适合初学者入门人工智能的路线及资料下载](http://mp.weixin.qq.com/s?__biz=MzIwODI2NDkxNQ==&mid=2247484737&idx=1&sn=27c52b4bc4ca98d3ab817344b84226cc&chksm=97048efda07307eb78d4f4ec0039a386a658404156b051af0cb715fafa8d2ae66cbe49343bf3&scene=21#wechat_redirect)
    
- [(图文+视频)机器学习入门系列下载](https://mp.weixin.qq.com/mp/appmsgalbum?__biz=MzIwODI2NDkxNQ==&action=getalbum&album_id=2259163844755406853#wechat_redirect)
    
- [中国大学慕课《机器学习》（黄海广主讲）](http://mp.weixin.qq.com/s?__biz=MzIwODI2NDkxNQ==&mid=2247502323&idx=1&sn=598d7231681ce2f316503201dd615c86&chksm=9707424fa070cb59f861a47f9eb4218cc5d3b835be49e93e67bb086dcc4c3b555a3546ebe9c5&scene=21#wechat_redirect)
    
- [机器学习及深度学习笔记等资料打印](http://mp.weixin.qq.com/s?__biz=MzIwODI2NDkxNQ==&mid=2247488304&idx=1&sn=581944f63eab1822ca53b9a4eeedad79&chksm=9704988ca073119a38a534adbedd51ca0b5705cdd6a104fed74b265bb092485e97c91bb5b347&scene=21#wechat_redirect)
    
- [《统计学习方法》的代码复现专辑](https://mp.weixin.qq.com/mp/appmsgalbum?action=getalbum&album_id=1337257945842778113&__biz=MzIwODI2NDkxNQ==#wechat_redirect)
    
- 机器学习交流qq群955171419，加入微信群请扫码
    




```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)


```


```


```


```






```

People who liked this content also liked

如何优雅地学习Pandas

...

大邓和他的Python

不看的原因

- 内容质量低
- 不看此公众号

深度学习基础教程（1）

...

古月居

不看的原因

- 内容质量低
- 不看此公众号

PYTHON条件生存森林模型CONDITIONAL SURVIVAL FOREST分类预测客户流失交叉验证可视化|数据分享

...

拓端数据部落

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___60b91c948ba44604a.bmp"/>

Scan to Follow