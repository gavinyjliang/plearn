# Python 编程的最好搭档—VSCode 详细指南

<a id="copyright_logo"></a>Original <a id="js_author_name"></a>Ckend <a id="profileBt"></a><a id="js_name"></a>Python实用宝典 *2020-03-09 08:00*

刚学Python的同学可能会觉得每次写Python的时候都得打开Cmd有点烦躁，直接上手Pycharm的同学可能会觉得这软件太笨重了，晦涩难用。那么有没有省去打开CMD的步骤，又能弥补Pycharm笨重的特点的软件呢？——答案是VSCode.

诞生于2015年的VSCode编辑器，现在可以说是目前最强的编辑器之一，在微软的背书下，比各位历史悠久的老大哥成长快得多，不到5年的时间里便坐到了市场占有率第一的位置。这么短的时间里，它是怎么成功的？答案是：**简单，可扩展性强**。

**编辑器，简单很重要**。还记得我多年前第一次用Vim编辑器时搜索的第一个问题：**怎么退出Vim？**一个工具的学习曲线会直接影响该工具的受众数量，对于编辑器而言尤其如此。任何使用起来复杂的东西最终都会被更容易使用的东西替代掉，不过Vim有其在运维方面的独特优势，所以它暂时是不可替代的。

Vim的不可替代是在服务器层面，对于我们在桌面端编程而言，越简单好用的编辑器越好，不要搞骚操作，最终烦的是自己。这就是为什么VSCode越来越火爆，而且它不仅简单易用，还能覆盖几乎所有语言的编写，如果我有一个小项目需要涉及到前后端所有代码，用VSCode一个编辑器就能解决了，而不是前端切Webstorm，后端切Pycharm.

好了，接下来就让我们来上手VSCode.

## **1.安装**

毕竟是微软大爷的产品，安装VSCode你几乎不会遇到问题，打开：
https://code.visualstudio.com/

选择适合自己系统的版本下载安装，一路默认即可：

<img width="677" height="406" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_6fccbf0575d743db8.jpg"/>

## **2.使用**

如果你看不惯英文版的编辑器，下面教你怎么装中文插件：

### **2.1 中文插件**

**1\.** 点击**View - Command Palette** (或输入 Ctrl + shift + P) 进入命令面板.

<img width="677" height="407" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__e7a20352cf6043b99.jpg"/>

**2. ****输入 configure language**, 选择**Configure Display Language** (配置显示语言)。

<img width="677" height="406" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__1597802ba8a448e88.jpg"/>

**3\.** 检查有没有zh-cn的选项，如果有，直接选择zh-cn替换。然后按照提示重启vscode就能看到界面变回中文了。

如果没有zh-cn的选项，则选择**install additional languages** (添加其他语言选项)，左边会弹出扩展窗口，扩展窗口找到中文简体，点击 install 安装，重复 **第 1, 2 步骤** 选择中文即可。

<img width="677" height="496" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__ed195ef0353d45228.jpg"/>

### **2.2 使用终端(Terminal)**

这是用VSCode编写Python最核心的地方，你不用打开丑丑的CMD，直接在VSCode中就可以运行Python。

点击 【**查看—终端** 】 或直接快捷键 【**Ctrl + `** 】 打开终端，会在下方产生一个CMD控制台：

<img width="677" height="573" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_b45ebf9d168c48798.jpg"/>

在这里你做的最新修改都可以直接 python xx.py 运行：

<img width="677" height="573" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_5684e9102bea45afb.jpg"/>

不过要注意一下当前的文件夹是否和Python脚本文件处于同一个目录，如果不在同一个目录则要cd进去。

### **2.3 一键运行**

很多同学都想一键运行Python，而非以命令的形式运行，这时候就需要Python扩展了，打开扩展页，输入Python，选择第一个进行安装 install 即可：

<img width="677" height="537" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_da92c3776cd543039.jpg"/>

重新加载VSCode生效，在这后编辑完代码按F5即可运行（如果你不需要输入参数的话），初次运行可能会让你选环境，选择Python即可。

默认按F5后进入DEBUG模式，需要再按一次F5程序才会运行，如果要按F5马上运行需要将launch.json文件的 "stopOnEntry": true,改为 "stopOnEntry": false。launch.json文件在设置中可以找到，如下图所示：

<img width="677" height="550" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_7a2cd334fc8040fcb.jpg"/>

## **3.其他扩展**

### **3.1 语法提示，配置flake8**

写代码没有语法提示，其实是很难受的一件事情，IDE直接帮你做了这件事，不过VSCode需要你稍微配置一下：

**1.** 打开终端，输入 pip install flake8 安装flake8，我已经装过了，你的提示应该跟我的不一样:

<img width="677" height="255" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_8ad0cb510c0145b5a.jpg"/>

**2. **在settings.json文件中输入"python.linting.flake8Enabled": true

<img width="677" height="483" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_a53ca90f6f744216a.jpg"/>

### **3.2 自动格式化代码**

Yapf是谷歌开源的一个用于格式化Python代码的工具，可以一键美化代码。支持两种规范：PEP8和Google Style，下面的步骤和3.1类似的就不再补图啦：

**1. **打开终端

**2. **输入 "pip install yapf" 安装yapf

**3. **在settings.json文件中输入"python.formatting.provider": "yapf"

**4.** 用一个看看， 按下快捷键 **Alt+Shift+F** 即可自动格式化代码。

### **3.3 文件及文件夹图标**

默认的VSCode图标没有那么详细，只有几个重要文件类型的图标提示，可以安装vscode-icons解决，Mac的有vscode-icons-mac版本。如图所示：

<img width="677" height="550" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_469494cc2a0a4b61a.jpg"/>

之后的文件显示就详细多了：

<img width="274" height="278" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_b7977d8f326543beb.jpg"/>

### **3.4 生成注释格式**

这个是我强烈推荐的插件，搜索docstring，目前排在第四位，由Nils Werner开发的autoDocstring，优秀的代表：

<img width="677" height="483" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_da9d532f6716402cb.jpg"/>

之后，你只需要在函数名后面输入三个双引号然后回车，即可生成docstring注释：

![Image](../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_3dc6f9db5e0640038.jpg)

按Tab可以直接切换需要输入的位置，而不用鼠标去点击，加快了注释效率。不过，我有点不喜欢它comment出现的位置直接在三个双引号的后面，有点不太雅观，可能这就是东西方美感的差异？不过即便如此，它还是一个非常方便的插件。

如果你喜欢今天的Python 教程，请持续关注Python实用宝典，如果对你有帮助，麻烦在下面点一个赞/在看<img width="36" height="23" src="../../../_resources/640_wx_fmt_gif_wxfrom_5_wx_lazy__0dbdaa4c62884a168.jpg"/>，有任何问题都可以在下方留言，我们会耐心解答的！

**点击下方阅读原文可以获取所有代码和链接哦！**

Python实用宝典 (pythondict.com)

不只是一个宝典

欢迎关注公众号：Python实用宝典

![Image](../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_729bf27bb8dd40bea.jpg)

<a id="js_view_source"></a>Read more

People who liked this content also liked

Python 一行代码算出每个省面积的神器—Geopandas

...

Python实用宝典

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___57cb2d0a610940539.bmp"/>

Scan to Follow