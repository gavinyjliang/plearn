# Matplotlib 可视化之路径效果高级应用

<a id="copyright_logo"></a>Original 云朵君 <a id="profileBt"></a><a id="js_name"></a>数据STUDIO *2022-05-05 08:35* *Posted on <a id="js_ip_wording"></a>四川*

收录于合集

<a id="js_article_tag_name__1974991176839544834"></a>#可视化 <a id="js_article_tag_num__1974991176839544834"></a>41 <a id="js_article_tag_tips__1974991176839544834"></a>个

<a id="js_article_tag_name__2154302528702726148"></a>#matplotlib <a id="js_article_tag_num__2154302528702726148"></a>13 <a id="js_article_tag_tips__2154302528702726148"></a>个

![Image](../../../_resources/640_wx_fmt_gif_wxfrom_5_wx_lazy__eba65a94472e45f78.gif)

<a id="js_appmsg_search_keywords_head"></a>![](../../../_resources/0_wx_fmt_png_944f9e6b681140e288c5d0530bbef92d.png)<a id="appmsg_search_keywords_nickname"></a>数据STUDIO<a id="appmsg_search_keywords_title"></a>推荐搜索<a id="appmsg_search_keywords_tips"></a>关键词列表：<a id="appmsg_search_keywords_keywords_list"></a>Python数据可视化数据挖掘SQL

<img width="677" height="103" src="../../../_resources/640_wx_fmt_gif_wxfrom_5_wx_lazy__b7b593bb569b4799a.gif"/>

##### 今天云朵君将和大家一起学习，如何通过定义路径效果，来改变图表元素的艺术效果，该方法有别于一般绘图函数关键字（例如`facecolor和edgecolor`等），通过Matplotlib patheffects 模块灵活配置图表元素的艺术效果。

<img width="507" height="109" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__08edc783a3c54b7f9.png"/>

Matplotlib 绘制

## 路径效果

### 什么是路径？

路径表示一系列可能断开的、可能已关闭的线和曲线段。

路径指的是matplotlib.path里面所实现的功能，最简单的路径就是比如一条任意的曲线都可以看成是路径。比如我要绘制一个心形，就需要通过路径去完成。

### 定义Artist路径效果

定义Artist在画布上遵循的路径效果。

```
matplotlib.patheffects

```

Matplotlib patheffects 模块提供了将多个绘制阶段应用于任何可通过 path.Path .

可以应用路径效果的对象包括 `patches.Patch` ， `lines.Line2D` ， `collections.Collection` 甚至 `text.Text` 。每个对象的路径效果可以通过 `Artist.set_path_effects` 方法，该方法需要 `AbstractPathEffect` 实例。

最简单的路径效应是 Normal 效果，简单地画出没有任何效果的图表：

```
import matplotlib.pyplot as plt
import matplotlib.patheffects as path_effects
plt.figure(figsize=(10, 3))
text = plt.text(0.5, 0.5, '欢迎关注\n@公众号：数据STUDIO '
                          '\n和云朵君一起学习数据分析与挖掘！',
                ha='center', va='center', size=20)
text.set_path_effects([path_effects.Normal()])
plt.show()

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 添加阴影

更有趣的路径效果是投影，可以应用于任何基于路径的艺术家。类 SimplePatchShadow 和 SimpleLineShadow 通过在原始艺术家下面绘制填充的面片或线面片来精确地执行此操作。

```
text = plt.text(0.5, 0.5, '+关注 @公众号：数据STUDIO',size=20,
                path_effects=[path_effects.withSimplePatchShadow()])
plt.plot([0, 3, 2, 5], linewidth=5, color='blue',
         path_effects=[path_effects.SimpleLineShadow(),
                       path_effects.Normal()])
plt.show()

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

> 注意在这个例子中设置路径效果的两种方法。第一个使用 `with*` 类包含所需的功能，并自动跟随“Normal”效果，而“Normal”效果显式定义要绘制的两个路径效果。

### 使Artist脱颖而出

让Artist在视觉上脱颖而出的一个好方法是在实际Artist的下方用粗体颜色画出一个轮廓。这个 Stroke 路径效应使得这项任务相对简单：

```
text.set_path_effects([path_effects.Stroke(linewidth=3, foreground='black'),
                       path_effects.Normal()])

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 更灵活的配置

我们注意到，`Stroke、SimplePatchShadow和SimpleLineShadow`的关键字不是通常的Artist关键字（例如`facecolor和edgecolor`等）。这是因为使用这些路径效果，我们操作了 `matplotlib` 的较低层。实际上，接受的关键字是用于 `matplotlib.backend_bases.GraphicsContextBase`实例的关键字，它们为易于创建新的后端而设计，而不是用于其用户界面。

有一个通用的PathPatchEffect路径效果，它创建一个具有原始路径的PathPatch类。此效果的关键字与PathPatch相同：

```
import matplotlib.pyplot as plt
import matplotlib.patheffects as path_effects
plt.figure(figsize=(12, 3),dpi=150)
t = plt.text(0.02, 0.5, '@公众号：数据STUDIO', fontsize=50, weight=1000, va='center')
t.set_path_effects([path_effects.PathPatchEffect(offset=(4, -4), hatch='xxxx',
                                                  facecolor='gray'),
                    path_effects.PathPatchEffect(edgecolor='white', linewidth=1.1,
                                                 facecolor='black')])
plt.show()

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 应用实例

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 绘图准备

```
import numpy as np
import matplotlib.pyplot as plt
import matplotlib.patheffects as path_effects
fig = plt.figure(figsize=(8, 8))
ax = plt.axes()

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 绘制散点图

```

# 样例数据准备
n = 250
np.random.seed(1)
X, Y = np.zeros(2 * n), np.zeros(2 * n)
S, C = np.zeros(2 * n), np.zeros(2 * n)
X[:n] = np.random.normal(1.60, 0.1, n)
Y[:n] = np.random.normal(50, 10, n)
S[:n] = np.random.uniform(25, 50, n)
C[:n] = 0
X[n:] = np.random.normal(1.75, 0.1, n)
Y[n:] = np.random.normal(75, 10, n)
S[n:] = np.random.uniform(25, 50, n)
C[n:] = 1
cmap = plt.get_cmap("RdYlBu")
# 绘制散点图
scatter = plt.scatter(X, Y, c=C, s=S, cmap=cmap, edgecolor="None", alpha=0.25)

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 配置轴标签与标题

```
# 设置轴标签和标题
ax.set_xlabel("身高 (m)", weight="medium")
ax.set_ylabel("体重 (kg)", weight="medium")
ax.set_title(
    """根据性别和年龄\n身高&体重的分布(伪数据)""")

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 绘制散点样式

```
# 绘制散点的黑边和白底
scatter = plt.scatter(X, Y, s=S, edgecolor="black", linewidth=0.75, zorder=-20)
scatter = plt.scatter(X, Y, s=S, edgecolor="None", facecolor="white", zorder=-10)

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 绘制投影分布图

```
plt.scatter(X, [19] * len(X), marker="|", color=cmap(C), linewidth=0.5, alpha=0.25)
plt.scatter([1.3] * len(X), Y, marker="_", color=cmap(C), linewidth=0.5, alpha=0.25)

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 配置图例

上下滑动查看更多源码

```
# 设置年龄图例
handles, labels = scatter.legend_elements(
    num=3, prop="sizes", alpha=1,
    markeredgewidth=0.5,
    markeredgecolor="black",
    markerfacecolor="None",
)
legend = plt.legend(
    handles, labels, title=" 年龄",
    loc=(0.6, 0.05),
    handletextpad=0.1, labelspacing=0.25,
    facecolor="None", edgecolor="None")
    
legend.get_children()[0].align = "left"
legend.get_title().set_fontweight("medium")
ax.add_artist(legend)
handles, labels = scatter.legend_elements(
        markeredgewidth=0.0,
        markeredgecolor="black")
labels = ["女性", "男性"]
legend = plt.legend(
    handles, labels, title=" 性别",
    loc=(0.75, 0.05),
    handletextpad=0.1, labelspacing=0.25,
    facecolor="None", edgecolor="None")
    
legend.get_children()[0].align = "left"
legend.get_title().set_fontweight("medium")
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 设置路径效果

```
ax.set_title(
    """根据性别和年龄\n身高&体重的分布(伪数据)""",
    path_effects=[
        path_effects.withSimplePatchShadow()
        ])
legend.get_title().set_path_effects(
    [path_effects.Stroke(
         linewidth=2,
         foreground='orange'),
         path_effects.Normal()
    ])

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 参考资料

\[1\]

Scientific Visualisation-Python&Matplotlib

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

##### 🏴‍☠️宝藏级🏴‍☠️ 原创公众号『**数据STUDIO**』内容超级硬核。公众号以Python为核心语言，垂直于数据科学领域，包括可戳👉[**Python**](https://mp.weixin.qq.com/mp/appmsgalbum?__biz=Mzk0OTI1OTQ2MQ==&action=getalbum&album_id=1974978822768771072&scene=173&from_msgid=2247519294&from_itemidx=1&count=3&nolastread=1#wechat_redirect)**｜**[**MySQL**](https://mp.weixin.qq.com/mp/appmsgalbum?__biz=Mzk0OTI1OTQ2MQ==&action=getalbum&album_id=2023684574089658370&scene=173&from_msgid=2247519619&from_itemidx=2&count=3&nolastread=1#wechat_redirect)**｜**[**数据分析**](https://mp.weixin.qq.com/mp/appmsgalbum?__biz=Mzk0OTI1OTQ2MQ==&action=getalbum&album_id=1974978820940054530&scene=173&from_msgid=2247518366&from_itemidx=1&count=3&nolastread=1#wechat_redirect)**｜**[**数据可视化**](https://mp.weixin.qq.com/mp/appmsgalbum?__biz=Mzk0OTI1OTQ2MQ==&action=getalbum&album_id=1974991176839544834&scene=173&from_msgid=2247519244&from_itemidx=1&count=3&nolastread=1#wechat_redirect)**｜**[**机器学习与数据挖掘**](https://mp.weixin.qq.com/mp/appmsgalbum?__biz=Mzk0OTI1OTQ2MQ==&action=getalbum&album_id=1963494160565354497&scene=173&from_msgid=2247512171&from_itemidx=1&count=3&nolastread=1#wechat_redirect)**｜**[**爬虫**](https://mp.weixin.qq.com/mp/appmsgalbum?__biz=Mzk0OTI1OTQ2MQ==&action=getalbum&album_id=2318258648965644288&scene=173&from_msgid=2247518366&from_itemidx=1&count=3&nolastread=1#wechat_redirect) 等，从入门到进阶！

长按👇关注\- 数据STUDIO -设为星标，干货速递

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

People who liked this content also liked

CNS级宝藏公众号：R语言从入门到大师！

...

R语言数据分析指南

不看的原因

- 内容质量低
- 不看此公众号

奇妙的 CSS MASK

...

iCSS前端趣闻

不看的原因

- 内容质量低
- 不看此公众号

50行Python代码绘制数据大屏，这个可视化框架真的太神了

...

关于数据分析与可视化

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___ad4a283b7a2a4b1ca.bmp"/>

Scan to Follow