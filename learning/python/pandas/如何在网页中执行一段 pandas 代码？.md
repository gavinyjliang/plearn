# 如何在网页中执行一段 pandas 代码？

<a id="profileBt"></a><a id="js_name"></a>SQL数据库开发 *2022-05-17 08:30* *Posted on <a id="js_ip_wording"></a>广东*

The following article is from 早起Python Author 刘早起

<a id="copyright_info"></a>[![](../../../_resources/0_060463dc61b44980b3766fda94d49da8.jpg)<br>**早起Python** .<br>点击领取pandas数据分析300题](#)

![](../../../_resources/0_wx_fmt_png_a9bb41f29d064108920520bd251b450e.png)

**SQL数据库开发**

专注数据相关领域，主要分享MySQL，数据分析，Python，Linux ，大数据等相关技术内容，关注回复「1024」获取资源大礼包。

<a id="js_profile_article"></a>426篇原创内容

Official Account

很多粉丝对**如何在线执行 pandas 代码**感兴趣，那么今天就简单来说一下我探索这一功能的过程。

首先在设计这一功能时，需要先明确大致需求：

- ⭐⭐⭐用户可以在当前页面执行
    
- 不同用户之间独立运行
    
- 不需要加载额外代码或操作
    

其中最重要的一点就是用户可以**在当前网站、当前单元格执行代码**，其次尽可能的减少其他操作。

其实为了实现这个功能，我探索了大半个月，不断修改方案，删掉了几个写了很久但是不能完美实现的代码，几度放弃，最后还是磕磕碰碰的做出来，下面是我的一些经验，仅供参考。

## 方案1

首先最简单的思路就是用自己的服务器，**前端写一个输入框，然后将用户提交的代码到后台，执行后再返回前端**，就像这样👇![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

但是思索了一番还是放弃了，除了要防止恶意用户执行`sudo rm - rf /*`之类的代码，为了满足第二个需求就要**给每个用户分配一定的空间**，这就很吃服务器的配置，例如前天最高100+用户同时运行，我的 4c8g 服务器肯定是带不动的。

并且如果采取这个的方案，理论上可以实现，但除了升级服务器要钱，我也没有开发类似产品的经验，时间成本不好预估，遂放弃。

## 方案2

之后又是一番面向 `stackoverflow` 编程，我了解到很多可以在线执行代码的网站，就像这样![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

确实可以在线执行一段代码，但是除去我是否能做出来，如何控制权限等问题，这样的网站主要是**以执行代码为主，无法完成 pandas 教程**的任务。

并且**代码不能预设置**，只能进入页面后手动输入，本地数据也不好加载，而且执行一次就要跳转到一个新的页面，十分繁琐（写一个爬虫接口也是一个办法，但是就太依赖对方网站），于是很快放弃了这条思路。

## Jupyterhub

继续一番搜索后，我发现了一个神器 —— `Jupyterhub`

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

如上图架构展示的一样，使用`Jupyterhub` 可以给每个用户分配一个独立的`Jupyter Notebook`，并且**无需考虑权限等问题**，我也可以提前将代码和数据进行预设。

但问题在于采取此方案**无法满足教程需求**，因为全部内容都需要放在 `Jupyter Notebook`中，整体上就是将 pandas300题做成了在线版，而**我想要的是一个网站**。

并且使用`Jupyterhub`不可避免的要进行一些 `docker` 或 `k8s` 操作，这也不是我熟悉的领域，虽热在这条思路上走了一段时间，但还是放弃了。

## JupyterBook

之后又是一番检索，但无非都是上面几种方案，在我感觉要放弃做这个网站时，无意中发现一个项目`JupyterBook`![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

简单来说，他可以将你的 `Jupyter Notebook` 转换为 `html` 页面（基于 `sphinx`），并且一个很重要的特点就是可以**在线、交互式**执行代码。![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

具体怎么实现的呢？首先需要将你的项目上传到一个公共资源平台`binder`，这个网站会为你的项目创建一个**镜像**，这样可以方便给不同用户使用![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

简单来说，可以理解为将你的 `Jupyter Notebook` 挂在这个网站，别人就能去在线执行，但是很明显，我们都需要跳转到这个页面去使用，而我希望**在当前页面执行代码。**

这时就需要在使用另一个项目（`Thebe`）![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

它使用`JupyterLab` API，通**过加载一段JS代码，再指定一个执行后端（上面提到的binder），就可以在当前页面执行代码。**

听起来很复杂，但是实现起来很简单，上面我们说到，`JupyterBook` 是基于 `Sphinx`制作页面的，所以只需要提前在配置 `Sphinx`时加载  `sphinx_thebe`插件即可，![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

至此，开头我需求中的1、2就完美实现了，还剩最后一个问题就是**如何让用户更少的执行代码？**

如果你体验过我的网站，你会发现执行一个 `pandas` 操作连 `import pandas as pd`和读取数据的操作都不用！

其实这些代码在启动`jupyter notebook`时就**预先加载了**，只需要在对应单元格上加上 `thebe-init`的 tag 即可。

当然，使用 `JupyterBook` 还是有很多坑，消耗我最多的时间就是在**修改样式**上，默认的样式如下，可能英文状态下表现还行，但是到中文并不是很适配![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

为了大家不仅用的爽，我**对网站颜值的要求也很高**，于是爆改了几千行的 `css` 和 `js` 代码，甚至组件的位置都调整到小数点后两位才让我满意，磕磕碰碰一个多月终于将整个网站做出来

最后，本文仅是对在线执行代码做了一个**快速、不完整**的总结。由于篇幅限制，还有很多搭建、部署网站细节的内容没有涉及到，如果你觉得不错，欢迎**点赞、转发.**

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg== "音符")

最后给大家分享我写的SQL两件套：**《SQL基础知识第二版》**和**《SQL高级知识第二版》**的PDF电子版。里面有各个语法的解释、大量的实例讲解和批注等等，非常通俗易懂，方便大家跟着一起来实操。

有需要的可以下载学习，只需要在下面的公众号「**数据前线**」(非本号)，后台回复关键字：**SQL**，就行

```


数据前线

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

#### 

```


后台回复关键字：1024，获取一份精心整理的技术干货

后台回复关键字：进群，带你进入高手如云的交流群。

```


```


推荐阅读

- [SQL中去重的三种方法，还有谁不会？](http://mp.weixin.qq.com/s?__biz=MzA3MTg4NjY4Mw==&mid=2457327227&idx=1&sn=aeb9c82d0c39780fd74ece083e852043&chksm=88a5c44fbfd24d59d8db70b995cd313ca7ec9e4009b0e239545b049031bd84791232089978f4&scene=21#wechat_redirect)
    
- [SQL 学习最强刷题网站！](http://mp.weixin.qq.com/s?__biz=MzA3MTg4NjY4Mw==&mid=2457327043&idx=1&sn=97769773d037b8318df61da616814a11&chksm=88a5c7f7bfd24ee111caffe2a37f2b219fd9a0e52857dca9df9d1284941c1f5cbca02e8cc467&scene=21#wechat_redirect)
    
- [一千行 MySQL 学习笔记](http://mp.weixin.qq.com/s?__biz=MzA3MTg4NjY4Mw==&mid=2457326778&idx=2&sn=955e62a5136fc2b31c31bde258084982&chksm=88a5c68ebfd24f986416e16c41afd82fb88fa5c486812f93e8f9f4489ec6c5f0d29ea5277448&scene=21#wechat_redirect)
    
- [SQL自定义排序](http://mp.weixin.qq.com/s?__biz=MzA3MTg4NjY4Mw==&mid=2457326649&idx=2&sn=f84c02633c15cbac9527ec85818b8266&chksm=88a5c60dbfd24f1b2d4df7f3be8486def1f3a63f978f30bc1691d5fad58dbb05678aa1f43869&scene=21#wechat_redirect)![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg== "分享点赞在看.gif")
    


```


```


```




```

<a id="js_view_source"></a>Read more

People who liked this content also liked

MySQL8.0 InnoDB并行查询特性

...

yangyidba

不看的原因

- 内容质量低
- 不看此公众号

作为前端，你必须要知道的meta标签知识

...

大迁世界

不看的原因

- 内容质量低
- 不看此公众号

一次非常有意思的 SQL 优化经历：从 30248.271s 到 0.001s

...

技术最前线

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___c7de1275212b45768.bmp"/>

Scan to Follow