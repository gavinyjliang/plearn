# pandas100个骚操作：一行 pandas 代码搞定 Excel “条件格式”！

<a id="copyright_logo"></a>Original 东哥起飞 <a id="profileBt"></a><a id="js_name"></a>Python数据科学 *2021-01-26 13:46*

收录于合集

<a id="js_article_tag_name__1699019347278561282"></a>#pandas骚操作 <a id="js_article_tag_num__1699019347278561282"></a>25 <a id="js_article_tag_tips__1699019347278561282"></a>个

<a id="js_article_tag_name__1939519024426516485"></a>#Python办公自动化 <a id="js_article_tag_num__1939519024426516485"></a>13 <a id="js_article_tag_tips__1939519024426516485"></a>个

关注上方“Python数据科学”，选择星标，

关键时间，第一时间送达！

<img width="657" height="334" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__a7e54bb5279e471a9.png"/>

来源：Python数据科学

作者：东哥起飞

大家好，我是你们的东哥。

本篇是pandas100个骚操作系列的第 **7**篇：**一行 pandas 代码搞定 Excel “条件格式”！**

系列内容，请看👉「[pandas100个骚操作](https://mp.weixin.qq.com/mp/appmsgalbum?__biz=MzUzODYwMDAzNA==&action=getalbum&album_id=1699019347278561282#wechat_redirect)」话题，订阅后文章更新可第一时间推送。

* * *

条件格式

说实话，Excel的 **“条件格式”** 是东哥非常喜欢的功能之一，通过添加颜色条件可以让表格数据更加清晰的凸显出统计特性。

有的朋友在想，这样的操作在python可能会很复杂。但其实一点不复杂，而且只需一行代码即可。

为什么可以做到一行代码实现 “条件格式”？

一是使用了pandas的`style`方法，二是要得益于pandas的`链式法则`。

下面我们来一起看个例子，体验一下这个组合操作有多骚。

**实例**

首先，我们导入数据集，使用经典的titanic中抽样的部分数据。

```


import pandas as pd
df = pd.read_csv("test.csv")
df


```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

可以看到，现在这个dataframe是空白的，什么都没有的，现在要给表格添加一些条件。

1、比如我们想让Fare变量值呈现条形图，以清楚看出各个值得大小比较，那么可直接使用`bar`代码如下。

df.style.bar("Fare",vmin=0)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

2、再比如，我们想让Age变量呈现背景颜色的梯度变化，以体验映射的数值大小，那么可直接使用`background_gradient`，深颜色代表数值大，浅颜色代表数值小，代码如下。

df.style.background_gradient("Greens",subset="Age")

<img width="337" height="468" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__652a51d4f4e74fda9.png"/>

3、让所有缺失值都高亮出来，可使用`highlight_null`，表格所有缺失值都会变成高亮。

df.style.highlight_null()

<img width="349" height="484" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__46c060ba54ef47068.png"/>

以上就是`pandas`的`style`条件格式，用法非常简单。下面我们用链式法则将以上三个操作串起来，只需将每个方法加到前一个后面即可，代码如下。

df.style.bar("Fare",vmin=0).background_gradient("Greens",subset="Age").highlight_null()

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

当然，如果你希望加更多的条件格式效果，还可以继续让链式更长，但不论条件怎么多，都只是一行代码。

**其它操作**

上面仅仅是列举了三个`style`中常用的操作，还有很多其他操作比如高亮最大值、给所有负值标红等等，通过参数subset还可以指定某一列或者某几列的小范围内进行条件格式操作。

```


# 负值标为红色
applymap(color_negative_red)
# 高亮最大值
apply(highlight_max)
# 使某一列编程±前缀，小数点保留两位有效数字
format({"Coulumn": lambda x: "±{:.2f}".format(abs(x))})
# 使用subset进行dataframe切片，选择指定的列
applymap(color_negative_red,
                  subset=pd.IndexSlice[2:5, ['B', 'D']])


```

另外，还有很多的效果可以实现，比如结合`seaborn`的各种风格。

```


import seaborn as sns
cm = sns.light_palette("green", as_cmap=True)
df.style.background_gradient(cmap=cm)


```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

如果结合`Ipython`的`HTML`还可以实现炫酷的动态效果。

```


from IPython.display import HTML
def hover(hover_color="#ffff99"):
    return dict(selector="tr:hover",
                props=[("background-color", "%s" % hover_color)])
styles = [
    hover(),
    dict(selector="th", props=[("font-size", "150%"),
                               ("text-align", "center")]),
    dict(selector="caption", props=[("caption-side", "bottom")])
]
html = (df.style.set_table_styles(styles)
          .set_caption("Hover to highlight."))
html


```

<img width="367" height="279" src="../../../_resources/640_wx_fmt_gif_wxfrom_5_wx_lazy__455a170f657d4055b.gif"/>

关于`style`条件格式的所有用法，可以参考pandas的官方文档。

**链接：**https://pandas.pydata.org/pandas-docs/version/0.18/style.html

如果喜欢东哥的骚操作，请给我点个赞<img width="20" height="20" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__640d55f752384f5d9.png"/>

文中数据集可以在公众号后台回复：条件格式 获取。

* * *

```


我是东哥，最后给大家分享《100本Python电子书》，包括Python编程技巧、数据分析、爬虫、Web开发、机器学习、深度学习。

```


现在免费分享出来，有需要的读者可以下载学习，在下面的公众号「GitHuboy」里回复关键字：Python，就行。

<img width="191" height="191" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_4d9df659e59141f8b.jpg"/>

```


```


# 

```


爱点赞的人，运气都不会太差![Image](https://mmbiz.qpic.cn/mmbiz_jpg/C1uDMDqjn19wtuQpK3jmJW3bFGWI8Yz6FR17tl1MF8VfqYxPx990kv2J74Lvqwib26KayHdOXd6ebzrqYibbTjww/640?wx_fmt=jpeg&wxfrom=5&wx_lazy=1&wx_co=1)


```




```


```




```


```

收录于合集 #<a id="js_album_keep_read_title"></a>pandas骚操作

 <a id="js_album_keep_read_size"></a>25个

<a id="js_album_prev"></a>上一篇 <a id="js_album_keep_read_pre_title"></a>pandas100个骚操作：强大的 accessor 方法 <a id="js_album_next"></a>下一篇 <a id="js_album_keep_read_next_title"></a>pandas100个骚操作：transform 数据转换的 4 个常用技巧！

<a id="js_view_source"></a>Read more

People who liked this content also liked

超强图解pandas，建议收藏(文末赠书)

...

Python数据科学

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___6d96817c6be94703b.bmp"/>

Scan to Follow