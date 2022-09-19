# 如何让 Jupyter Notebook 自动导入代码？

<a id="copyright_logo"></a>Original 刘早起 <a id="profileBt"></a><a id="js_name"></a>早起Python *2022-05-05 12:05* *Posted on <a id="js_ip_wording"></a>安徽*

<img width="677" height="286" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_f5a8afce38474d5bb.jpg"/>

大家好，我是早起。

作为使用 `Python` 工作的数据科学家。每天我们都会启动多个新的`Jupyter`笔记本，并且在会用到多个不同的库，例如`pandas`、`matplotlib`等。

但是，在开始实际工作之前，我们总是需要为每一个 `Notebook` 写一堆的导入代码，虽然这不困难，但是却很繁琐，有时还需要查找对应的导入语句例如

```
from sklearn.preprocessing import OneHotEncoder, LabelEncoder
from sklearn import feature_selection

```

怎样才能在启动`Jupyter` 笔记本时自动加载这些代码，让我们只专注于使用这些库？本文介绍两种办法。

## 方法一 : 修改配置文件

一个常见的方法就是通过修改`Jupyter`的配置文件来实现，这也是我在[之前文章中介绍过的方法](https://mp.weixin.qq.com/s?__biz=Mzg5OTU3NjczMQ==&mid=2247510307&idx=2&sn=a50f30ae0d443ffcdbb4ea26711ad4c7&chksm=c053cfd7f72446c15b9399789625d620e5d6e6ed85b775dd5baa5e633fbca166537f6371ccdb&token=951385889&lang=zh_CN&scene=21#wechat_redirect)。

以`macOS`为例，你可以进入`~/.ipython/profile_default`文件夹(Windows下也可以在安装目录中找到对应的文件夹)，如果找不到该目录需在命令行执行`ipython profile create`生成配置文件
![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

如上图所示，在该文件夹下新建一个名为`startup`的文件夹(如果有则不用新建)，之后进入`startup`文件夹新建一个`Python`脚本`start.py`

现在你可以在`start.py`中尽情的添加你每次启动`jupyter notebook`后都需要手动敲入的那段代码，之后保存即可

```
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from sklearn import svm, tree, linear_model, neighbors, naive_bayes, ensemble, discriminant_analysis, gaussian_process
from xgboost import XGBClassifier
from sklearn.preprocessing import OneHotEncoder, LabelEncoder
from sklearn import feature_selection
from sklearn import model_selection
.......

```

现在重启`Jupyter Notebook`后就可以直接使用`pandas、numpy`等我们配置好的库！
![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

但这个方法也有一个弊端，就是由于文件缺少相关导入代码，因此可能打包发给别人用时会无法执行，我们也不可能再次检查所用的代码然后手动导入一遍，所以只能在自己修改了配置文件的设备上用用。

## 方法二 : 使用 pyforest

这是我最近新发现的一个方法，由国外大神开发的一个插件，相比较修改配置文件，更适合小白操作。

我们只需要在终端（命令行）执行以下代码

```
pip install --upgrade pyforest
python -m pyforest install_extensions

```

之后重启`Jupyter Notebook`后便可以实现自动导入相关库。
![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

可以看到，这个方法和方法一的差别在于，他不是默认导入全部的依赖库（避免了过多的内存占用），而是在你使用到这个库时，自动在`Notebook`头部添加对应的导入代码，是不是很酷！

以`pandas`为例，当我们使用到`pd.xxx`便会在头部添加`import pandas as pd`，而在使用它之前，变量`pd`只是`pyforest`占位符。

但使用别人配置好的缺点就是自己想额外添加一些第三方库会比较困难，好在开发者已经预设了上百个常用库，从数据分析到机器学习、深度学习都有，基本上不用额外设置，感兴趣的话可以尝试一下~

如果你也想快速上手pandas，你可以打开我的网站进行学习👇

[![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)](http://mp.weixin.qq.com/s?__biz=Mzg5OTU3NjczMQ==&mid=2247521606&idx=1&sn=07f869da7bedb1662576f79c13a4a20f&chksm=c053f3b2f7247aa441279f6da189cf93f1a14e5b3bd002f1db071104eb393a197fb3083d72bc&scene=21#wechat_redirect)

  文末赠书  

文末推荐一本**《自然语言处理NLP从入门到项目实战（Python语言实现）》**，本书分为12章，主要包括学习人工智能原理、自然语言处理技术、掌握深度学习模型、NLP开源技术实战、Python神经网络计算实战、AI语音合成有声小说实战、玩转词向量、近义词查询系统实战、机器翻译系统实战、文本情感分析系统实战、电话销售语义分析系统实战人工智能辅助写作系统（独家专利技术解密）。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

最后还是老规矩，留言送 **3** 本，点赞前三将包邮获赠本书（时间截止5.15日，仅限2022年未获得过赠书的粉丝参与）

People who liked this content also liked

一文弄懂Python中的 if \_\_name\_\_==\_\_main\_\_

...

AI算法之道

不看的原因

- 内容质量低
- 不看此公众号

​有了这份小抄，你还学不会Python？再也不怕不记得Python的语法了

...

编程小小

不看的原因

- 内容质量低
- 不看此公众号

奇妙的 CSS MASK

...

iCSS前端趣闻

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___d2271f7c760441b0a.bmp"/>

Scan to Follow