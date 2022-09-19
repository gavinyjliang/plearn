# 用 Python 写的 web 页面，如何让所有人都能访问？

<a id="copyright_logo"></a>Original 刘早起 <a id="profileBt"></a><a id="js_name"></a>早起Python *2021-11-07 12:28*

收录于话题

<a id="js_article_tag_name__2112489489896898561"></a>#PyWebIO开发 <a id="js_article_tag_num__2112489489896898561"></a>4 <a id="js_article_tag_tips__2112489489896898561"></a>个

<a id="js_article_tag_name__2125351522912763904"></a>#用服务器做好玩的事儿 <a id="js_article_tag_num__2125351522912763904"></a>5 <a id="js_article_tag_tips__2125351522912763904"></a>个

大家好，我是早起。

之前分享过两篇用 `PyWebIO` 纯 Python 写网站的教程

- [PyWebIO常见操作讲解](https://mp.weixin.qq.com/s?__biz=Mzg5OTU3NjczMQ==&mid=2247520529&idx=1&sn=16f3f2644f1bdb41780707ca017ac541&scene=21#wechat_redirect)
    
- [二次封装 pandas 功能到 web](https://mp.weixin.qq.com/s?__biz=Mzg5OTU3NjczMQ==&mid=2247520657&idx=1&sn=ca8d3cd24a32dfea4fa0d1a111712ce9&scene=21#wechat_redirect)
    

在文章的最后，我都说到如果想要让所有人、任意设备都能访问你写的页面，就需要将你的项目部署到服务器了。

还是先宣传下，**双十一免费送阿里云服务器活动还在继续**，如果还没上车的可以扫描下方二维码添加我的微信（备注服务器）

<img width="336" height="318" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__ca51dc4dc93547198.png"/>

本文就将介绍如何在服务器上配置你的项目，**以下教程基于小白视角讲解**，适用于任何Python脚本（爬虫、web、数据分析、自动化等都行）

## 同步环境

其实整体思路就是，在本地开发，然后在服务器配置和你本地一样的环境，并将全部项目文件上传到服务器，之后就像部署远程 `Jupyter Notebook` 一样，使用 `nohup` 命令将程序挂在后台即可。

所以假设你现在已经在本地写好你的网站下面可以将你本地开发用到的库整理到 `requirements.txt` 中（可以通过`pip freeze > requirements.txt`）并ssh连接上服务器之后，执行下方命令

```
pip install -r requirements.txt

```

但如果你在服务器上安装了 `anaconda` 的话，更多情况下，只需要进入服务器执行 `pip install pywebio` 即可。

现在，你的服务器开发环境就和本地一致了，下一步自然是**将整个项目文件同步到服务器**，使用 `git` 是比较方便的方法，使用命令行命令是常见的方法，但是对于大多数小白来说，通过 `ftp` 软件点点可能更方便。

所以下面是在 `mac` 上使用 `Termius` 的同步文件过程（`Windows` 可以用 `filezilla`）首先打开软件<img width="669" height="341" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__e44dc3d1b7564c319.png"/>

点击 sftp 并选择一个服务器，之后输入你的服务器账号密码进入你的服务器文件夹（默认root目录下），然后创建一个新的文件夹用于你的项目<img width="669" height="624" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__a61d4d8713e34d9ea.png"/>

接下来只要双击进入你创建的文件夹，并将你本地的整个文件夹拖入，等待上传完毕即可。

## 挂载程序

现在，你的服务器有和你本地一致的开发环境与完整的项目文件，下面让我们把它启动起来。

首先还是通过ssh工具连上服务器，之后cd进入刚刚的新建的目录，比如我的文件夹名称为`aliyun`，我的命令就是

```
cd aliyun

```

接下来同样适用 `nohup` 启动并将脚本挂在后台执行即可，例如我的脚本为 `aliyun.py`，我的命令就是

```
nohup python aliyun.py &

```

这样就将该命令，挂在后台执行了，最后一步同样是打开短端口，例如你在 `PyWebIO` 中使用的端口是 `8888` 就要去阿里云后台防火墙/安全组开放这个端口<img width="669" height="188" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__e11abf66adbd44d1b.png"/>

至此，你就可以通过你的公网IP:端口，访问你的web项目，还有一个常见的问题，如何关闭这个端口/程序？

我们可以通过端口反查进程号<img width="669" height="61" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__b7ef1888912d4495a.png"/>

也可以根据命令查找进程pid（`ps -ef|grep python`）<img width="669" height="152" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__2fa9784c9abf4be2a.png"/>

然后将对应的进程kill掉，并在修改完代码后重新启动即可，这些就属于运维相关知识，网上资料很多，感兴趣的可以自己查阅。

## 彩蛋 \- 自定义页脚

最近很多人问，怎么修改默认页面中页脚的显示文字，或者去掉默认页面的`Powered by PyWebIO`<img width="669" height="95" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__52f21e005368455ba.png"/>

就像我的页面一样，显示早起Python，并且点击跳转到我设置好的页面<img width="669" height="180" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__a27bac4cb38e46f0a.png"/>

其实稍微对web开发懂一点的，都会知道这是额外加入了html元素，既然没有修改的命令，说明这个html肯定是写死在某个文件夹中。

在mac下，我们可以打开anaconda安装目录，并根据下面的路径中找到 `index.html`<img width="669" height="226" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__c7d430b1ef7949699.png"/>

之后使用任意代码编辑器修改框中部分即可<img width="669" height="243" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__668c735d5e364ad29.png"/>

在 Windows 或者 centos 下也是类似，找到 `pywebio` 对应的目录，修改对应文件即可，大家可以自己研究！

## 文末福利

如果你还没有服务器，恰好双十一来了，**我和阿里云的同学申请了 1000 台云服务器送给大家作为福利**，目前已经送出近 **200** 台！

只要你没有在阿里云买过服务器，咱早起Python的粉丝都可以**无套路直接领取**，扫描下方二维码添加我的微信并备注「**服务器**」即可

<img width="413" height="391" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__ca51dc4dc93547198.png"/>

收录于话题 #<a id="js_album_keep_read_title"></a>用服务器做好玩的事儿

 <a id="js_album_keep_read_size"></a>5个

<a id="js_album_next"></a>下一篇 <a id="js_album_keep_read_next_title"></a>PyWebIO，让 Pandas 原地起飞的神器！

People who liked this content also liked

牛！阿里&蚂蚁正式开源 IDE 研发框架 OpenSumi 帮助开发者搭建自己的IDE产品

大厂杂谈

不看的原因

- 内容质量低
- 不看此公众号

保姆级教程，使用Docker+Jenkins+Git技术搭建完整的CI流程（图文详解）

自动化测试研习社

不看的原因

- 内容质量低
- 不看此公众号

面向对象设计

小路板块

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___1b9a1e783d9348559.bmp"/>

Scan to Follow