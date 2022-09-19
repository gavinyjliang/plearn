# Python 强大的模式匹配工具—Pampy

<a id="profileBt"></a><a id="js_name"></a>简说Python *2022-05-23 22:25* *Posted on <a id="js_ip_wording"></a>浙江*

The following article is from Python实用宝典 Author Ckend

<a id="copyright_info"></a>[![](../../../_resources/0_87731495b3344446a5386c6a0aad04a7.jpg)<br>**Python实用宝典** .<br>如此Python，怎能不爱](#)

![](../../../_resources/0_wx_fmt_png_525a0a61b312468b8c6664c043490986.png)

**简说Python**

号主老表，自学，分享Python，SQL零基础入门、数据分析、数据挖掘、机器学习优质文章以及学习经验。

<a id="js_profile_article"></a>218篇原创内容

Official Account

在自然语言处理界，**模式匹配**可以说是最常用的技术。甚至可以说，将NLP技术作为真实生产力的项目都少不了**模式匹配**。

什么是模式匹配呢？在计算机科学中，往往是检查给定的序列或字符串中是否有符合**某种模式**的片段。比如说：“啊，你的**AK-47**打得真准”，如果我们将 “啊，你的**\_\_\_\_\_**打得真准 ” 作为一种模式，则会将AK-47匹配出来。

实现模式匹配往往都是用正则表达式，但是如果你想识别特别复杂的模式，编写正则表达式就会变得非常非常麻烦。而Pampy这个项目能解决你不少的烦恼。https://github.com/santinic/pampy

下面是一个使用例子:

<img width="677" height="381" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__235a03bc1a244728b.png"/>

***1.准备***

首先需要你的电脑安装好了Python环境，并且安装好了Python开发工具。

如果你还没有安装，可以参考以下文章：

如果仅用Python来处理数据、爬虫、数据分析或者自动化脚本、机器学习等，建议使用Python基础环境+jupyter即可，安装使用参考[Windows/Mac 安装、使用Python环境+jupyter notebook](https://mp.weixin.qq.com/s?__biz=MzUyOTAwMzI4NA==&mid=2247510991&idx=1&sn=8331416e8c762c26c43c08691736ee64&scene=21#wechat_redirect)

如果想利用Python进行web项目开发等，建议使用Python基础环境+Pycharm，安装使用参考 ：[Windows下安装、使用Pycharm教程，这下全了](https://mp.weixin.qq.com/s?__biz=MzUyOTAwMzI4NA==&mid=2247516661&idx=2&sn=6170d4b6d90d14422c88473a1295d573&scene=21#wechat_redirect) 和 [Mac下玩转Python-安装&使用Python/PyCharm](https://mp.weixin.qq.com/s?__biz=MzUyOTAwMzI4NA==&mid=2247486567&idx=1&sn=84804c803d998403f522a6e3e9f07fbd&scene=21#wechat_redirect) 。

**请选择以下任一种方式输入命令安装依赖**：

1\. Windows 环境 打开 Cmd (开始-运行-CMD)。
2\. MacOS 环境 打开 Terminal (command+空格输入Terminal)。
3\. 如果你用的是 VSCode编辑器 或 Pycharm，可以直接使用界面下方的Terminal.

```
pip install pampy
```

看到 Successfully installed pampy-0.3.0 则说明安装成功。

***2.使用***

**特性1：****HEAD 和 TAIL**

HEAD和TAIL能代表某个模式的前面部分或后面部分。

比如将特定模式后的元素都变成元组：

```
from pampy import match, HEAD, TAIL, _
x = [-1, -2, -3, 0, 1, 2, 3]
print(match(x, [-1, TAIL], lambda t: [-1, tuple(t)]))
# => [-1, (-2, -3, 0, 1, 2, 3)]
```

将特定模式前的元素设为集合，后面的元素设为元组：

```
from pampy import match, HEAD, TAIL, _
x = [-1, -2, -3, 0, 1, 2, 3]
print(match(x, [HEAD, _, _, 0, TAIL], lambda h, a, b, t: (set([h, a, b]), tuple(t))))
# => ({-3, -1, -2}, (1, 2, 3))
```

**特性2：****甚至能匹配字典中的键**

在你不知道哪个键下有某个值的时候，这招非常好用：

```
from pampy import match, HEAD, TAIL, _
my_dict = {
    'global_setting': [1, 3, 3],
    'user_setting': {
        'face': ['beautiful', 'ugly'],
        'mind': ['smart', 'stupid']
    }
}
result = match(my_dict, { _: {'face': _}}, lambda key, son_value: (key, son_value))
print(result)
# => ('user_setting', ['beautiful', 'ugly'])
```

**特性3: 搭配正则**

不仅如此，它还能搭配正则一起使用哦：

```
import re
from pampy import match, HEAD, TAIL, _
def what_is(pet):
    return match(
        pet, re.compile('(\w+)，(\w)\w+鳕鱼$'), lambda mygod, you: you + "像鳕鱼"
    )
print(what_is('我的天，你长得真像鳕鱼'))
# => '你像鳕鱼'
```

如果你喜欢今天的Python 教程，请持续关注Python实用宝典，如果对你有帮助，麻烦在下面点一个赞/在看，有任何问题都可以在下方留言，我们会耐心解答的！

#### --end--

```


![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

扫码即可加我微信

学习交流

老表朋友圈经常有赠书/红包福利活动


```

People who liked this content also liked

再也不用敲SQL DDL了！数据湖时代Google的元数据自动管理技术

...

Flink

不看的原因

- 内容质量低
- 不看此公众号

只需一个文件，Python 实现迷你 Web 框架！

...

Python猫

不看的原因

- 内容质量低
- 不看此公众号

用Go重写Node.js服务：项目性能提升5倍，内存减少40%

...

OSC开源社区

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___72f7ff100b3348e98.bmp"/>

Scan to Follow