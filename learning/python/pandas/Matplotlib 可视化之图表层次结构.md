# Matplotlib 可视化之图表层次结构

<a id="copyright_logo"></a>Original <a id="js_author_name"></a>云朵君 <a id="profileBt"></a><a id="js_name"></a>数据STUDIO *2022-03-05 14:30*

收录于合集

<a id="js_article_tag_name__2154302528702726148"></a>#matplotlib <a id="js_article_tag_num__2154302528702726148"></a>13 <a id="js_article_tag_tips__2154302528702726148"></a>个

<a id="js_article_tag_name__1974991176839544834"></a>#可视化 <a id="js_article_tag_num__1974991176839544834"></a>41 <a id="js_article_tag_tips__1974991176839544834"></a>个

大家好，我是云朵君！
**关注和星标『数据STUDIO』**，和云朵君一起学习数据分析与挖掘！

![](../../../_resources/0_wx_fmt_png_10a81b8f405044d6b07e68dc7266ef74.png)

**数据STUDIO**

点击领取《Python学习手册》，后台回复「福利」获取。『数据STUDIO』专注于数据科学原创文章分享，内容以 Python 为核心语言，涵盖机器学习、数据分析、可视化、MySQL等领域干货知识总结及实战项目。

<a id="js_profile_article"></a>195篇原创内容

Official Account

👆点击关注｜设为星标｜干货速递👆

今天云朵君给大家系统介绍Matplotlib图表层次结构，通过步骤分解，详细了解一个图表绘制的过程 。

```


```

数据STUDIO

，赞 46

七步吃透 Matplotlib 图表层次结构

### Matplotlib图表层次结构

#### Figure图形

**Figure**中最重要的元素是**figure**本身。在调用**figure**方法时创建的，可以指定它的长宽(`figsize`)及分辨率(`dpi`)，也可以指定背景颜色(`facecolor`)和标题(`suptitle`)。另外，当保存图形时，背景颜色将不会被使用，因为**savefig**函数也有一个`faceccolor`参数(默认为白色)，它将覆盖您的图形背景颜色。如果不想要任何背景，可以在保存图形时指定`transparent=True`。

#### Axes轴

这是第二个最重要的元素，它对应于将呈现数据图表的实际区域。它也被称为`subplot`子图。每个**figure**可以有一个或多个**axes**轴，每个**axes**轴通常由四条边(左、上、右、下)包围，称为`spines`。每一根**spines**上都可以装饰有主刻度和次刻度(可以指向内部或外部)、刻度标签和标签。默认情况下，**matplotlib**只装饰左边和下面的**spines**边框。

#### Axis轴

有刻度的**spines**边线称为轴。水平的是`x轴`，垂直的是`y轴`。每个轴每一个都是由一个**spines**轴线，主刻度、次刻度、主刻度标签、次刻度标签和一个轴标签组成。

#### Spines轴线

**Spines**是连接轴刻度线和数据区域边界的轴线。它们可以被放置在任意位置，可以选择展示或隐藏它们。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

第一步，设置画布大小、调整坐标轴范围  
第二步，设置图表边框格式  
第三步，设置图表标题  
第四步，设置图表的网格  
第五步，设置轴刻度  
第六步，绘图  
第七步，配置图例

## Step1设置画布

**第一步，设置画布大小、调整坐标轴范围。**

首先需要有画布，才能在上面创作，就像写字需要先拿一张纸。画布的大小（长宽比、分辨率）及刻度范围可以先设置好，如果预先不知道刻度范围，可以等绘图结束后再做适当调整。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

设置画布

```
fig = plt.figure(figsize=(8, 8))
ax = fig.add_subplot(1, 1, 1, aspect=1)
ax.set_xlim(0, 4)
ax.set_ylim(0, 4)

```

**Matplotlib有两种画图接口**：①是便捷的 MATLAB 风格接口，②是功能更强大的面向对象接口。

### **MATLAB**风格接口

MATLAB 风格的工具位于**pyplot(plt)** 接口中。`plt.xx`之类的是 **函数式绘图**，通过将数据参数传入 **plt类** 的静态方法中并调用方法，从而绘图。

这种接口最重要的特性是有状态的：它会持续跟踪 **"当前的"** 图形和坐标轴，所有 **plt** 命令都可以应用。可以用 `plt.gcf()` (获取当前图形)和 `plt.gca()`(获取当前坐标轴)来查看具体信息。

### 面向对象接口

`fig,ax=plt.subplots()`是**对象式编程**，这里`plt.subplots()`是返回一个元组，包含了 **figure** 对象(控制总体图形大小)和 **axes** 对象(控制绘图，坐标之类的)。此外`fig.add_subplot()`也是相同的道理。

进行对象式绘图，首先是要通过`plt.subplots()`将 **figure** 类和 **axes** 类实例化也就是代码中的`fig,ax`，然后通过 **fig** 调整整体图片大小，通过 **ax** 绘制图形，设置坐标，函数式绘图最大的好处就是直观。

面向对象接口可以适应更复杂的场景，更好地控制你自己的图形。在面 向对象接口中，画图函数不再受到当前 **"活动"** 图形或坐标轴的限制，而 变成了显式的 **Figure** 和 **Axes** 的方法。

## Step2 设置轴线

**第二步，设置图表Spines轴线。**

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

设置轴线

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

隐藏轴线

图形的轴线可以通过坐标轴属性`ax.spines`设置，最常见的设置方法是选择隐藏，通过属性`['top', 'bottom', 'left', 'right']`分别设置上下左右的轴线。

```
ax.spines.right.set_visible(False)
ax.spines.bottom.set_visible(False)

```

还有另一种经常使用的情况，根据绘图需要，调整 **spines** 轴线在图中位置。如绘制正余弦函数时：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

移动轴线

```
# 移动 left 和 bottom spines 到 (0,0) 位置
ax.spines["left"].set_position(("data", 0))
ax.spines["bottom"].set_position(("data", 0))
# 隐藏 top 和 right spines.
ax.spines["top"].set_visible(False)
ax.spines["right"].set_visible(False)

```

## Step3 设置标题

**第三步，设置标题，** 就是用几个简短的字高度概括该图形所要传达的信息。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

设置标题

**通过`ax.set_title`函数设置标题。**

```
ax.set_title("Anatomy of a figure (层次结构)", 
             fontsize=20, 
             verticalalignment="bottom")

```

#### matplotlib.axes.Axes.set_title()

`ax.set_title()`是给`ax`这个子图设置标题，当子图存在多个的时候，可以通过`ax`设置不同的标题。如果需要设置一个总的标题，可以通过`fig.suptitle('Total title')`方法设置。

```
Axes.set_title(label, fontdict=None,
               loc='center', pad=None,
               **kwargs)

```

**参数：**

- **label**：此参数是用于标题的文本。
    
- **fontdict**：此参数是控制标题文本外观的字典。
    
- **loc**：此参数用于设置标题`{'center'，'left'，'right'}`的位置。
    
- **pad**：此参数是标题距轴顶部的偏移量(以磅为单位)。
    

## Step4 设置网格

**第四步，设置图表的网格，** 图表网格属于图形配置的一种。网格可以辅助读者更好直观地量化图形。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

设置网格

**通过方法`ax.grid()`添加网格线。**

```
ax.grid(linestyle="--", linewidth=0.5, 
        color=".25", zorder=-10)

```

#### matplotlib.axes.Axes.grid()

```
Axes.grid(b=None, which='major',
          axis='both', **kwargs)

```

**参数：**

- **b**：是否显示网格线。布尔值或`None`，可选参数。如果没有关键字参数，则**b**为`True`，如果b为`None`且没有关键字参数，相当于切换网格线的可见性。
    
- **which**：网格线显示的尺度。字符串，可选参数，取值范围为`{'major', 'minor', 'both'}`，默认为`'major'`。`'major'`为主刻度、`'minor'`为次刻度。没有输入的方向则不会显示网格刻度。
    
- **axis**：选择网格线显示的轴。字符串，可选参数，取值范围为`{'both', 'x', 'y'}`，默认为`'both'`。
    
- ****kwargs**：Line2D线条对象属性。常用的
    

- **color :** 这就不用多说了，就是设置网格线的颜色。或者直接用c来代替color也可以。
    
- **linestyle :** 也可以用ls来代替linestyle， 设置网格线的风格，是连续实线，虚线或者其它不同的线条。`| '-' | '--' | '-.' | ':' | 'None' | ' ' | ''|`
    
- **linewidth :** 设置网格线的宽度
    
- **zorder：** 设置层次顺序
    

**另外还有几种不同设置网格线的方法：**

```
plt.grid(true)
# 设置网格线格式：
plt.grid(color='r',  linestyle='--',
         linewidth=1, alpha=0.3) 
# 使用 axes 类面向对象命令
# 同时设置横竖坐标轴上的网格线
ax.grid(color='r', linestyle='--',
        linewidth=1,alpha=0.3)
# 单独设置X坐标轴上(垂直方向)的网格线
ax.xaxis.grid(color='r', linestyle='--',
              linewidth=1, alpha=0.3)
# 单独设置Y坐标轴上(水平方向)的网格线
ax.yaxis.grid(color='r', linestyle='--',
              linewidth=1,alpha=0.3)

```

### 图形配置

#### 手动设置背景色的几种方法

##### ① 设置 figure 背景颜色

```
# 方法 I：
plt.figure(facecolor='blue',    # 图表区的背景色
           edgecolor='black')    # 图表区的边框线颜色
# 方法 II：
fig=plt.gcf()
fig.set_facecolor('green')

```

##### ② 设置 axes 背景颜色

```
# 方法 I：
a = plt.axes([.65, .6, .2, .2],
             facecolor='k')  # pyplot api 命令-黑色背景
# 方法 II：
ax1=plt.gca()
ax1.patch.set_facecolor("gray")    # 设置 ax1 区域背景颜色               
ax1.patch.set_alpha(0.5)    # 设置 ax1 区域背景颜色透明度  

```

##### ③ 修改 matplotlib 默认参数

Matplotlib 每次加载时，都会定义一个运行时配置(rcParams)，其中包含了 所有你创建的图形元素的默认风格。你可以用 mpl.rcParams 简便方法随时修 改这个配置。

```
mpl.rcParams['axes.facecolor']='red'
mpl.rcParams['savefig.facecolor']='red'

```

#### 手动配置

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

手动配置

```
# 用灰色背景
fig = plt.figure(figsize=(8, 8),
                 facecolor='#E6E6E6',    # 图表区的背景色
                 edgecolor='black')
# 画上白色的网格线
ax.grid(color='grey', linestyle='-.')
# 隐藏坐标轴的线条
for spine in ax.spines.values():
    spine.set_visible(False)
# 隐藏上边与右边的刻度 ax.xaxis.tick_bottom() ax.yaxis.tick_left()
# 弱化刻度与标签
ax.tick_params(colors='gray', direction='out')
for tick in ax.get_xticklabels():
    tick.set_color('gray')
for tick in ax.get_yticklabels():
    tick.set_color('gray')

```

#### 使用默认配置样式表

即使你不打算创建自己的绘图风格，样式表包含的默认内容也非常有 用。通过 plt.style.available 命令可以看到所有可用的风格，

```
plt.style.available[:5]

```

```
[ 'fivethirtyeight', 'seaborn-pastel',
'seaborn-whitegrid', 'ggplot',  
'grayscale']

```

**使用某种样式表的基本方法如下所示:**

```
plt.style.use('ggplot')

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

默认配置

## Step5 设置轴刻度

### 坐标轴定位器与格式生成器

虽然 Matplotlib 默认的坐标轴定位器(locator)与格式生成器 (formatter)可以满足大部分需求，但是并非对每一幅图都合适。

#### Tick Locator

**Tick Locator** 主要设置刻度位置，这在我的绘图教程中主要是用来设置副刻度(`minor`)，而 **Formatter** 则是主要设置刻度形式。Matplotlib 对这两者则有着多种用法，其中 **Locator** 的子类主要如下：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

Tick Locator

#### Tick formatters

**Tick formatters** 设置刻度标签格式，主要对绘图刻度标签定制化需求时，matplotlib 可支持修改的刻度标签形式如下。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

Tick formatters

**部分代码如下**

```
import matplotlib.ticker as ticker
# Multiple Locator
axs[1].xaxis.set_major_locator(ticker.MultipleLocator(0.5))
axs[1].xaxis.set_minor_locator(ticker.MultipleLocator(0.1))
# Index Locator
axs[4].plot(range(0, 5), [0]*5, color='white')
axs[4].xaxis.set_major_locator(ticker.IndexLocator(base=0.5, offset=0.25))
# Auto Locator
axs[5].xaxis.set_major_locator(ticker.AutoLocator())
axs[5].xaxis.set_minor_locator(ticker.AutoMinorLocator())
# Log Locator
axs[7].set_xlim(10**3, 10**10)
axs[7].set_xscale('log')
axs[7].xaxis.set_major_locator(ticker.LogLocator(base=10, numticks=15))
# StrMethod formatter
setup(axs1[1], title="StrMethodFormatter('{x:.3f}')")
axs1[1].xaxis.set_major_formatter(ticker.StrMethodFormatter("{x:.3f}"))
# FuncFormatter can be used as a decorator
@ticker.FuncFormatter
def major_formatter(x, pos):
    return f'[{x:.2f}]'
setup(axs1[2], title='FuncFormatter("[{:.2f}]".format')
axs1[2].xaxis.set_major_formatter(major_formatter)

```

### 刻度标签参数

更改刻度、刻度标签和网格线的外观。

#### matplotlib.axes.Axes.tick_params()

```
Axes.tick_params(axis='both', **kwargs)

```

**主要参数：**

```
axis : 可选{'x', 'y', 'both'} ，选择对哪个轴操作，默认是'both'
reset : bool，如果为True，则在处理其他参数之前将所有参数设置为默认值。它的默认值为False。
which : 可选{'major', 'minor', 'both'} 选择对主or副坐标轴进行操作
direction/tickdir : 可选{'in', 'out', 'inout'}刻度线的方向
size/length : float, 刻度线的长度
width : float, 刻度线的宽度
color : 刻度线的颜色，我一般用16进制字符串表示，eg：'#EE6363'
pad : float, 刻度线与刻度值之间的距离
labelsize : float/str, 刻度值字体大小
labelcolor : 刻度值颜色
colors : 同时设置刻度线和刻度值的颜色
zorder : float ，Tick and label zorder.
bottom, top, left, right : bool, 分别表示上下左右四边，是否显示刻度线，True为显示
labelbottom, labeltop, labelleft, labelright:bool, 分别表示上下左右四边，是否显示刻度值，True为显示
labelrotation : 刻度值逆时针旋转给定的度数，如20
gridOn: bool ,是否添加网格线；
grid_alpha:float网格线透明度 ；
grid_color: 网格线颜色; 
grid_linewidth:float网格线宽度；
grid_linestyle: 网格线型
tick1On, tick2On : bool分别表表示是否显示axis轴的(左/下、右/上)or(主、副)刻度线
label1On,label2On : bool分别表表示是否显示axis轴的(左/下、右/上)or(主、副)刻度值

```

可以将每个 Matplotlib 对象都看成是子对象(**sub- object**)的容器，例如每个 **figure** 都会包含一个或多个 **axes** 对象，每个 **axes** 对象又会包含其他表示图形内容的对象。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

设置轴刻度

```
# 设置主次刻度轴
ax.xaxis.set_major_locator(MultipleLocator(1.000))
ax.xaxis.set_minor_locator(AutoMinorLocator(4))
ax.yaxis.set_major_locator(MultipleLocator(1.000))
ax.yaxis.set_minor_locator(AutoMinorLocator(4))
ax.xaxis.set_minor_formatter(FuncFormatter(minor_tick))
# 设置刻度轴范围
ax.set_xlim(0, 4)
ax.set_ylim(0, 4)
# 设置刻度参数
ax.tick_params(which="major", width=1.0)
ax.tick_params(which="major", length=10)
ax.tick_params(which="minor", width=1.0, labelsize=10)
ax.tick_params(which="minor", length=5, labelsize=10, labelcolor="0.25")
# 设置轴标签
ax.set_xlabel("X axis label")
ax.set_ylabel("Y axis label")

```

## Step6 绘图

#### matplotlib.axes.Axes.plot()

```
Axes.plot([x], y, [fmt], data=None, **kwargs)

```

用于绘制XY坐标系的点、线或其他标记形状。

**参数：**

- **x, y：** 类数组或极坐标。水平/垂直坐标系中的数据点，x是可选参数，默认为`[0,..., N-1]`。 通常，参数`x,y`是长度为N的数组，也支持极坐标（相当于一个常数值数组）。 参数也可以是二维的，此时，每一列代表一个数据集。
    
- **fmt：** 字符串，可选参数。格式化字符串，例如`'ro'`代表红色圆圈。格式字符串是用于快速设置基本线条样式的缩写，这些样式或更多的样式可通过关键字参数来实现。
    

```
fmt = '[color][marker][line]'

```

`color`（颜色）、`marker`（标记点）、`line`（线条）都是可选的，例如如果指定 **line** 而不指定 **marker**

```
ax.plot(X, Y1, c=(0.25, 0.25, 1.00), lw=2, 
        label="Blue signal", zorder=10)
ax.plot(X, Y2, c=(1.00, 0.25, 0.25), lw=2, 
        label="Red signal")
ax.plot(X, Y3, linewidth=0, marker="o", 
        markerfacecolor="w", markeredgecolor="k")

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

绘图

## Step7 配置图例

想在可视化图形中使用图例，可以为不同的图形元素分配标签。

#### matplotlib.axes.Axes.legend()

可以用 `Axes.legend()`命令来创建最简单的图例。

```
Axes.legend(*args, **kwargs)

```

**参数：**

- **labels**：这个参数是在旁边显示的标签列表。
    
- **handles**：这个参数列表是要添加到示例的。
    
- **loc：** 位置参数，常用参数，可以传入位置字符串或位置代码，如下：
    

| Location String | Location Code |
| --- | --- |
| 'best' | 0   |
| 'upper right' | 1   |
| 'upper left' | 2   |
| 'lower left' | 3   |
| 'lower right' | 4   |
| 'right' | 5   |
| 'center left' | 6   |
| 'center right' | 7   |
| 'lower center' | 8   |
| 'upper center' | 9   |
| 'center' | 10  |

### 同时显示多个图例

有时，我们可能需要在同一张图上显示多个图例。用 **Matplotlib** 通过标准的 **legend** 接口只能为一张图建一个图例。如果你想用 `plt.legend()` 或 `ax.legend()` 方法创建第二个图例，那么第一个图例就会被覆盖。但是，我们可以通过从头开始创建一个新的图例对象(legend artist)，然后用底层的(**lower- level**)`ax.add_artist()` 方法在图上添加第二个图例。

```
fig, ax = plt.subplots(figsize=(10,6))
lines = []
styles = ['-', '--', '-.', ':'] 
x = np.linspace(0, 10, 1000)
for i in range(4):
    lines += ax.plot(x, np.sin(x - i * np.pi / 2), styles[i], color='black')
ax.axis('equal')
# 设置第一个图例要显示的线条和标签 
ax.legend(lines[:2], ['line A', 'line B'],
          loc='upper right', frameon=False,fontsize=15)
# 创建第二个图例，通过add_artist方法添加到图上
from matplotlib.legend import Legend
leg = Legend(ax, lines[2:], ['line C', 'line D'],
             loc='lower right', frameon=False, fontsize=15) 
ax.add_artist(leg)

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 参考资料

\[1\]

* Scientific Visualisation-Python & Matplotlib   
 *Python数据科学手册**

长按👇关注\- 数据STUDIO -设为星标，干货速递

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

People who liked this content also liked

Matplotlib类别比较图（3）

...

python数据分析实践

不看的原因

- 内容质量低
- 不看此公众号

谁说数据图表不能可爱啦？Python 绘制漫画风图表

...

喜悦家学Python

不看的原因

- 内容质量低
- 不看此公众号

使用 Hyperopt 和 Plotly 可视化超参数优化

...

python数据大师

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___a8f3846a3f5e471b9.bmp"/>

Scan to Follow