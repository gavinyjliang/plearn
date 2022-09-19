# Github年度最受欢迎的TOP30 Python项目，点赞收藏

<a id="profileBt"></a><a id="js_name"></a>python数据分析之禅 *2022-02-06 13:13*

The following article is from 关于数据分析与可视化 Author 俊欣

<a id="copyright_info"></a>[![](../../../_resources/0_f807eef89d2941de91feabb01098788a.jpg)<br>**关于数据分析与可视化** .<br>本公众号定期分享数据分析与可视化干货文章，并有时结合热点话题进行深入讨论，希望您会喜欢，要是哪里写的不好，也渴望倾听您的想法和意见，感谢！❤️](#)

![](../../../_resources/0_wx_fmt_png_97f01cb5f9d943478d4e01c4a69deb50.png)

**python数据分析之禅**

点击领取pandas高清速查表，后台回复“速查表”获取

<a id="js_profile_article"></a>85篇原创内容

Official Account

今天小编整理归纳了2021年`Github`上面最受欢迎的30个`Python`项目，帮助大家在打磨技术与提升自我上面更进一步。

### 通过代码来获取

`Github`官网有开源的接口，因此数据的获取也就方便了许多，代码如下

```
`url = 'https://api.github.com/search/repositories?q=language:python&sort=stars&order=desc'
res = requests.get(url)
res_dict = res.json()
repos = res_dict['items']
`
```

我们整理到`Pandas`中的`DataFrame`数据集当中去，代码如下

```
`repo_df = pd.DataFrame(repos)
repo_df = repo_df[['name', 'full_name', 'html_url', 'created_at', 'stargazers_count', 'watchers', 'forks', 'open_issues']]
repo_df['created_at'] = pd.to_datetime(repo_df['created_at'])
repo_df['created_year'] = repo_df['created_at'].dt.year
repo_df['years_on_github'] = 2022 - repo_df['created_at'].dt.year
repo_df.head()
`
```

output

<img width="618" height="168" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__e1ea5739b55544e0a.png"/>

上面出来的结果包括了`Python`项目的项目名称、项目链接、创建的时间以及点赞的数量和拷贝的数量等等，我们可以根据一定的指标来进行排序，例如根据“点赞”以及“查阅”等指标依次从高到低来进行排序

```
`repo_df.sort_values(by = ["stargazers_count", "watchers"], ascending = False).head(10)
`
```

output

<img width="618" height="192" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__094a3973066848df8.png"/>

当然我们也可以根据"forks"这个指标来进行排序

```
`repo_df.sort_values(by = "forks", ascending = False).head(10)
`
```

output

<img width="618" height="190" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__360806f0c2d54c499.png"/>

下面小编就带大家罗列几个在`Github`上面受欢迎的`Python`项目

### `Python-cheatsheet`

当中集合了`Python`编程的语法以及各种数据类型的内置方法，`Python`编程的初学者倒是可以多看看里面的内容，项目地址

https://github.com/gto76/python-cheatsheet

### 面试内推项目

当中包含了国内几乎所有的互联网大厂的面经和答案，项目地址：

https://github.com/0voice/interview\_internal\_reference

### Python-100-Days

100天的时间完成从`Python`新手小白到大师的进阶，项目地址：

https://github.com/jackfrued/Python-100-Days

### `Rich`

`Python`当中的`Rich`库，可以为你在终端中提供富文本和漂亮、精美的格式，它可以绘制漂亮的表格、进度条、`markdown`，突出显示语法的源代码及回溯等等，优秀的功能有很多

项目地址：

https://github.com/Textualize/rich

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### `Python Web`开发

说到`Python`的`Web`开发，`Flask`以及`Django`这两个框架被广泛地应用到了实际工作当中。

- `Flask`项目地址：https://github.com/pallets/flask
    
- `Django`项目地址：https://github.com/django/django
    

在`Github`当中也是收获了相当数量的点赞与拷贝

当然还有`fastapi`框架，项目地址：

https://github.com/tiangolo/fastapi

### `Scrapy`

主要是用Python写的大规模的数据抓取的框架，项目地址

https://github.com/scrapy/scrapy

点赞量达到42.5K，拷贝的量有9.5K

### 人工智能

要是对深度学习和机器学习感兴趣的童鞋，可以去看这两个项目，

- keras，项目地址是：https://github.com/keras-team/keras
    
- models，项目地址是：https://github.com/tensorflow/models
    

它们分别用到了`keras`模块以及`tensorflow`框架来进行模型的训练与优化，而这两个框架正在被越来越多的算法工程师们接受与使用。

### `transformers`

项目地址：

https://github.com/huggingface/transformers

收获了57.4K的点赞量以及13.6K的拷贝，该项目主要是将一些已经训练好的模型运用在一些实际项目当中，包括自然语言处理当中的例如翻译、问答挑战，以及计算机视觉任务当中的图像识别、物体检测等等。

### 人脸识别项目

项目地址：

https://github.com/ageitgey/face_recognition

收获了42.9K的点赞以及11.9K的拷贝，包含了与人脸识别相关的一系列功能。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### `openpilot`项目

项目地址：

https://github.com/commaai/openpilot

收获了32.2K的点赞以及6K的拷贝数量，该项目是一个开源的辅助驾驶系统，并且支持150+种汽车，包括我们耳熟能详的奥迪、雷克萨斯、丰田、起亚、本田等车。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 深度学习论文集合

项目地址：

https://github.com/floodsung/Deep-Learning-Papers-Reading-Roadmap

收获了31.6K的点赞以及6.8K的拷贝数量，当中集合了一系列深度学习的优秀论文与书籍，对此感兴趣的童鞋可以根据链接前往阅读

People who liked this content also liked

Python 强大的模式匹配工具—Pampy

...

快学Python

不看的原因

- 内容质量低
- 不看此公众号

Python的哪个Web框架学习周期短，学习成本低？

...

Python大数据分析

不看的原因

- 内容质量低
- 不看此公众号

Python中最简单易用的并行加速技巧

...

Python大数据分析

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___59611f86c0b1451e8.bmp"/>

Scan to Follow