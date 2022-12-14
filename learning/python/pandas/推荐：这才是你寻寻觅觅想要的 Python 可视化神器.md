# 推荐：这才是你寻寻觅觅想要的 Python 可视化神器

<a id="copyright_logo"></a>Original <a id="js_author_name"></a>我是阳哥 <a id="profileBt"></a><a id="js_name"></a>Python数据之道 *2019-03-23 14:05*

收录于话题

<a id="js_article_tag_name__1337047152119447553"></a>#Plotly可视化 <a id="js_article_tag_num__1337047152119447553"></a>14 <a id="js_article_tag_tips__1337047152119447553"></a>个

<a id="js_article_tag_name__1572925747453804545"></a>#Python数据之道精选 <a id="js_article_tag_num__1572925747453804545"></a>37 <a id="js_article_tag_tips__1572925747453804545"></a>个

<a id="js_article_tag_name__1339529968694525952"></a>#Python数据可视化 <a id="js_article_tag_num__1339529968694525952"></a>39 <a id="js_article_tag_tips__1339529968694525952"></a>个

点击上方“**Python数据之道**”，选择“星标公众号”

精品文章，第一时间送达

<img width="592" height="37" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_95462670d44f40baa.jpg"/>

<img width="556" height="237" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_0ae09eabb4054c0cb.jpg"/>

翻译 | Lemon

来源 | Plotly

译文出品 | Python数据之道 （ID：PyDataRoad）

**Plotly Express 入门之路**

Plotly Express 是一个新的高级 Python 可视化库：它是 Plotly.py 的高级封装，它为复杂的图表提供了一个简单的语法。 

受 Seaborn 和 ggplot2 的启发，它专门设计为具有简洁，一致且易于学习的 API ：只需一次导入，您就可以在一个函数调用中创建丰富的交互式绘图，包括分面绘图（faceting）、地图、动画和趋势线。 它带有数据集、颜色面板和主题，就像 Plotly.py 一样。

Plotly Express 完全免费：凭借其宽松的开源 MIT 许可证，您可以随意使用它（是的，甚至在商业产品中！）。 

最重要的是，Plotly Express 与 Plotly 生态系统的其他部分完全兼容：在您的 Dash 应用程序中使用它，使用 Orca 将您的数据导出为几乎任何文件格式，或使用JupyterLab 图表编辑器在 GUI 中编辑它们！

用 `pip install plotly_express` 命令可以安装 Plotly Express。

## 使用 Plotly Express 轻松地进行数据可视化

一旦导入Plotly Express（通常是 `px` ），大多数绘图只需要一个函数调用，接受一个整洁的Pandas dataframe，并简单描述你想要制作的图。 如果你想要一个基本的散点图，它只是 `px.scatter（data，x =“column_name”，y =“column_name”）`。

以下是 内置的 Gapminder 数据集 的示例，显示2007年按国家/地区的人均预期寿命和人均GDP 之间的趋势：

```


1.  `import plotly_express as px`
    
2.  
3.  `gapminder = px.data.gapminder()`
    
4.  `gapminder2007 = gapminder.query('year == 2007')`
    
5.  `px.scatter(gapminder2007, x='gdpPercap', y='lifeExp')`
    


```

<img width="677" height="464" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_ebf719da7f3341c68.jpg"/>

如果你想通过大陆区分它们，你可以使用 `color` 参数为你的点着色，由 `px` 负责设置默认颜色，设置图例等：

<img width="677" height="414" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_99cbc97f9d1f41bab.jpg"/>

这里的每一点都是一个国家，所以也许我们想要按国家人口来衡量这些点...... 没问题：这里也有一个参数来设置，它被称为 `size`：

<img width="677" height="419" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_f544f642b20a40c4b.jpg"/>

如果你好奇哪个国家对应哪个点？ 可以添加一个 `hover_name` ，您可以轻松识别任何一点：只需将鼠标放在您感兴趣的点上即可！ 事实上，即使没有 `hover_name` ，整个图表也是互动的：

<img width="677" height="420" src="../../../_resources/640_wx_fmt_gif_wxfrom_5_wx_lazy__363949e3810249d58.gif"/>

也可以通过 `facet_col =”continent“` 来轻松划分各大洲，就像着色点一样容易，并且让我们使用 x轴 对数（log_x）以便在我们在图表中看的更清晰：

<img width="677" height="435" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_e5b69fb3a75d4537a.jpg"/>

也许你不仅仅对 2007年 感兴趣，而且你想看看这张图表是如何随着时间的推移而演变的。 可以通过设置 `animation_frame=“year”` （以及 `animation_group =“country”` 来标识哪些圆与控制条中的年份匹配）来设置动画。 

在这个最终版本中，让我们在这里调整一些显示，因为像“gdpPercap” 这样的文本有点难看，即使它是我们的数据框列的名称。 我们可以提供更漂亮的“标签” （labels），可以在整个图表、图例、标题轴和悬停（hovers）中应用。 我们还可以手动设置边界，以便动画在整个过程中看起来更棒：

<img width="677" height="442" src="../../../_resources/640_wx_fmt_gif_wxfrom_5_wx_lazy__2d488fd4aa4a47ea8.gif"/>

因为这是地理数据，我们也可以将其表示为动画地图，因此这清楚地表明 Plotly Express 不仅仅可以绘制散点图（不过这个数据集缺少前苏联的数据）。

<img width="677" height="427" src="../../../_resources/640_wx_fmt_gif_wxfrom_5_wx_lazy__408085690c514efea.gif"/>

事实上，Plotly Express 支持三维散点图、三维线形图、极坐标和地图上三元坐标以及二维坐标。 条形图（Bar）有二维笛卡尔和极坐标风格。

进行可视化时，您可以使用单变量设置中的直方图（histograms）和箱形图（box）或小提琴图（violin plots），或双变量分布的密度等高线图（density contours）。 大多数二维笛卡尔图接受连续或分类数据，并自动处理日期/时间数据。 可以查看我们的图库 (ref-3) 来了解每个图表的例子。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 上述动态图包含 10多张 图片的可视化，『Python数据之道』已将代码整合到 jupyter notebook 文件中，在公号回复 “code” 即可获得**源代码**。

下图即是其中的一个图形：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 可视化分布

数据探索的主要部分是理解数据集中值的分布，以及这些分布如何相互关联。 Plotly Express 有许多功能来处理这些任务。

使用直方图（histograms），箱形图（box）或小提琴图（violin plots）可视化单变量分布：

直方图：

<img width="677" height="417" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_bfbc9b87859545eeb.jpg"/>

箱形图：

<img width="677" height="428" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_c9bc0a145a22448eb.jpg"/>

小提琴图： ![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

还可以创建联合分布图（marginal rugs），使用直方图，箱形图（box）或小提琴来显示双变量分布，也可以添加趋势线。 Plotly Express 甚至可以帮助你在悬停框中添加线条公式和R²值！ 它使用 statsmodels 进行普通最小二乘（OLS）回归或局部加权散点图平滑（LOWESS）。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 颜色面板和序列

在上面的一些图中你会注意到一些不错的色标。 在 Plotly Express 中， px.colors 模块包含许多有用的色标和序列：定性的、序列型的、离散的、循环的以及所有您喜欢的开源包：ColorBrewer、cmocean 和 Carto 。 我们还提供了一些功能来制作可浏览的样本供您欣赏（ref-3）：

定性的颜色序列：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

众多内置顺序色标中的一部分：

<img width="677" height="448" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_204285a40be84a7aa.jpg"/>

## 用一行 Python 代码进行交互式多维可视化

我们特别为我们的交互式多维图表感到自豪，例如散点图矩阵（SPLOMS）、平行坐标和我们称之为并行类别的并行集。 通过这些，您可以在单个图中可视化整个数据集以进行数据探索。 在你的Jupyter 笔记本中查看这些单行及其启用的交互：

<img width="677" height="398" src="../../../_resources/640_wx_fmt_gif_wxfrom_5_wx_lazy__f93d14f4d61e44a39.gif"/>

散点图矩阵（SPLOM）允许您可视化多个链接的散点图：数据集中的每个变量与其他变量的关系。 数据集中的每一行都显示为每个图中的一个点。 你可以进行缩放、平移或选择操作，你会发现所有图都链接在一起！

<img width="677" height="398" src="../../../_resources/640_wx_fmt_gif_wxfrom_5_wx_lazy__c1b88e46e6d94d97b.gif"/>

平行坐标允许您同时显示3个以上的连续变量。 dataframe 中的每一行都是一行。 您可以拖动尺寸以重新排序它们并选择值范围之间的交叉点。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

并行类别是并行坐标的分类模拟：使用它们可视化数据集中多组类别之间的关系。

## Plotly 生态系统的一部分

Plotly Express 之于 Plotly.py 类似 Seaborn 之于 matplotlib：Plotly Express 是一个高级封装库，允许您快速创建图表，然后使用底层 API 和生态系统的强大功能进行修改。 对于Plotly 生态系统，这意味着一旦您使用 Plotly Express 创建了一个图形，您就可以使用Themes，使用 FigureWidgets 进行命令性编辑，使用 Orca 将其导出为几乎任何文件格式，或者在我们的 GUI JupyterLab 图表编辑器中编辑它 。

主题（Themes）允许您控制图形范围的设置，如边距、字体、背景颜色、刻度定位等。 您可以使用模板参数应用任何命名的主题或主题对象：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

有三个内置的 Plotly 主题可以使用， 分别是 plotly， plotly*white 和 plotly*dark

`px` 输出继承自 Plotly.py 的 `Figure` 类 `ExpressFigure` 的对象，这意味着你可以使用任何 `Figure` 的访问器和方法来改变 `px`生成的绘图。 例如，您可以将 `.update（）` 调用链接到 `px` 调用以更改图例设置并添加注释。 `.update（）` 现在返回修改后的数字，所以你仍然可以在一个很长的 Python 语句中执行此操作：

<img width="677" height="492" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_0e91ef7f447e4e1d8.jpg"/>

在这里，在使用 Plotly Express 生成原始图形之后，我们使用 Plotly.py 的 API 来更改一些图例设置并添加注释。

## 能够与 Dash 完美匹配

Dash 是 Plotly 的开源框架，用于构建具有 Plotly.py 图表的分析应用程序和仪表板。Plotly Express 产生的对象与 Dash 100％兼容，只需将它们直接传递到 `dash_core_components.Graph`，如下所示： `dcc.Graph（figure = px.scatter（...））`。 这是一个非常简单的 50行 Dash 应用程序的示例，它使用 `px` 生成其中的图表：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

这个 50 行的 Dash 应用程序使用 Plotly Express 生成用于浏览数据集的 UI 。

## 设计理念：为什么我们创建 Plotly Express ？

可视化数据有很多原因：有时您想要提供一些想法或结果，并且您希望对图表的每个方面施加很多控制，有时您希望快速查看两个变量之间的关系。 这是交互与探索的范畴。

Plotly.py 已经发展成为一个非常强大的可视化交互工具：它可以让你控制图形的几乎每个方面，从图例的位置到刻度的长度。 不幸的是，这种控制的代价是冗长的：有时可能需要多行 Python 代码才能用 Plotly.py 生成图表。

我们使用 Plotly Express 的主要目标是使 Plotly.py 更容易用于探索和快速迭代。

我们想要构建一个库，它做出了不同的权衡：在可视化过程的早期牺牲一些控制措施来换取一个不那么详细的 API，允许你在一行 Python 代码中制作各种各样的图表。 然而，正如我们上面所示，该控件并没有消失：你仍然可以使用底层的 Plotly.py 的 API 来调整和优化用 Plotly Express 制作的图表。

支持这种简洁 API 的主要设计决策之一是所有 Plotly Express 的函数都接受“整洁”的 dataframe 作为输入。 每个 Plotly Express 函数都体现了dataframe 中行与单个或分组标记的清晰映射，并具有图形启发的语法签名，可让您直接映射这些标记的变量，如 x 或 y 位置、颜色、大小、 facet-column 甚至是 动画帧到数据框（dataframe）中的列。 当您键入 `px.scatter（data，x ='col1'，y='col2'）` 时，Plotly Express 会为数据框中的每一行创建一个小符号标记 - 这就是 `px.scatter` 的作用 \- 并将 “col1” 映射到 x 位置（类似于 y 位置）。 这种方法的强大之处在于它以相同的方式处理所有可视化变量：您可以将数据框列映射到颜色，然后通过更改参数来改变您的想法并将其映射到大小或进行行分面（facet-row）。

接受整个整洁的 dataframe 的列名作为输入（而不是原始的 `numpy` 向量）也允许 `px` 为你节省大量的时间，因为它知道列的名称，它可以生成所有的 Plotly.py 配置用于标记图例、轴、悬停框、构面甚至动画帧。 但是，如上所述，如果你的 dataframe 的列被笨拙地命名，你可以告诉 `px` 用每个函数的 `labels` 参数替换更好的。

仅接受整洁输入所带来的最终优势是它更直接地支持快速迭代：您整理一次数据集，从那里可以使用 `px` 创建数十种不同类型的图表，包括在 SPLOM 中可视化多个维度 、使用平行坐标、在地图上绘制，在二维、三维极坐标或三维坐标中使用等，所有这些都不需要重塑您的数据！

我们没有以权宜之计的名义牺牲控制的所有方面，我们只关注您想要在数据可视化过程的探索阶段发挥的控制类型。 您可以对大多数函数使用 `category_orders` 参数来告诉 `px` 您的分类数据“好”、“更好”、“最佳” 等具有重要的非字母顺序，并且它将用于分类轴、分面绘制 和图例的排序。 您可以使用 `color_discrete_map` （以及其他 `* _map` 参数）将特定颜色固定到特定数据值（如果这对您的示例有意义）。 当然，你可以在任何地方重构 `color_discrete_sequence` 或 `color_continuous_scale` （和其他 `*_sequence` 参数）。

在 API 级别，我们在 `px` 中投入了大量的工作，以确保所有参数都被命名，以便在键入时最大限度地发现：所有 `scatter` -类似的函数都以 `scatter` 开头（例如 `scatter_polar`， `scatter_ternary`）所以你可以通过自动补全来发现它们。 我们选择拆分这些不同的散点图函数，因此每个散点图函数都会接受一组定制的关键字参数，特别是它们的坐标系。 也就是说，共享坐标系的函数集（例如 `scatter`， `line` ＆ `bar`，或 `scatter_polar`， `line_polar` 和 `bar_polar` ）也有相同的参数，以最大限度地方便学习。 我们还花了很多精力来提出简短而富有表现力的名称，这些名称很好地映射到底层的 Plotly.py 属性，以便于在工作流程中稍后调整到交互的图表中。

最后，Plotly Express 作为一个新的 Python 可视化库，在 Plotly 生态系统下，将会迅速发展。所以不要犹豫，立即开始使用 Plotly Express 吧！

## 『Python数据之道』已将代码整合到 jupyter notebook 文件中，在公号回复 “code” 即可获得**源代码**。

> 文章来源：
> 
> https://medium.com/@plotlygraphs/introducing-plotly-express-808df010143d
> 
> 参考文献：
> 
> ref-1：https://nbviewer.jupyter.org/github/plotly/plotly_express/blob/master/walkthrough.ipynb
> 
> ref-2：https://mybinder.org/v2/gh/plotly/plotly_express/master?filepath=walkthrough.ipynb
> 
> ref-3：https://plotly.github.io/plotly_express/

**-------------------End-------------------**

<img width="677" height="122" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_879fa380d2ea4a66a.jpg"/>

[<img width="556" height="57" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_827583ea52bf494f9.jpg"/>](http://mp.weixin.qq.com/s?__biz=MzI2NjY5NzI0NA==&mid=2247485586&idx=1&sn=30bb5c26ec90898207550e56e61fefaf&chksm=ea8b67e1ddfceef726acd541d32281a10b257383a8830810cc6c9be2171c9eeb567d35316292&scene=21#wechat_redirect)

[<img width="556" height="57" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_1fe3b48f3e2e4a999.jpg"/>](http://mp.weixin.qq.com/s?__biz=MzI2NjY5NzI0NA==&mid=2247485238&idx=1&sn=f2d6b9136ff94697bdce9c9f48397e78&chksm=ea8b6845ddfce15301f810293b5541b8637aed7c80ec501298b9ce4414de1a0d604c33bac614&scene=21#wechat_redirect)

<img width="556" height="313" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_5e1bdece1c444656a.jpg"/>

收录于话题 #<a id="js_album_keep_read_title"></a>Python数据可视化

 <a id="js_album_keep_read_size"></a>39个

<a id="js_album_prev"></a>上一篇 <a id="js_album_keep_read_pre_title"></a>30000字 Matplotlib 实操干货，38个案例带你从入门到进阶！ <a id="js_album_next"></a>下一篇 <a id="js_album_keep_read_next_title"></a>玩转词云图，推荐一个Pyecharts和Plotly数据分析实战项目

<a id="js_view_source"></a>Read more

Modified on 2019-03-23

People who liked this content also liked

一个悄然成为世界最流行的操作系统诞生！

Python数据之道

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___d1b3a98d3be04eb19.bmp"/>

Scan to Follow