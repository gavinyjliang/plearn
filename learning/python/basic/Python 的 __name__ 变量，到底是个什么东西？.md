# Python 的 \_\_name\_\_ 变量，到底是个什么东西？

<a id="profileBt"></a><a id="js_name"></a>小白学视觉 *2022-03-02 10:05*

```


点击上方“小白学视觉”，选择加"星标"或“置顶”

重磅干货，第一时间送达![Image](https://mmbiz.qpic.cn/mmbiz_jpg/ow6przZuPIENb0m5iawutIf90N2Ub3dcPuP2KXHJvaR1Fv2FnicTuOy3KcHuIEJbd9lUyOibeXqW8tEhoJGL98qOw/640?wx_fmt=jpeg&wxfrom=5&wx_lazy=1&wx_co=1)


```

 原文链接：

https://medium.freecodecamp.org/whats-in-a-python-s-name-506262fe61e8

大家应该已经在很多 Python 脚本里见到过 \_\_name\_\_ 变量了吧？它经常是以类似这样的方式出现在我们的程序里：

```
if __name__ == '__main__':
    main()
```

今天，我就带大家详细扒一扒这个内置变量的用法，示范一下在你写的 Python 模组里要怎么用到它。

# 

**这个 \_\_name\_\_ 拿来做什么的？**

作为 Python 的内置变量，**\_\_name\_\_**变量（前后各有两个下划线）还是挺特殊的。它是每个 Python 模块必备的属性，但它的值取决于你是如何执行这段代码的。

在许多情况下，你的代码不可能全部都放在同一个文件里，或者你在这个文件里写的函数，在其他地方也可以用到。为了更高效地重用这些代码，你需要在 Python 程序中导入来自其他文件的代码。

所以，在**\_\_name\_\_ **变量的帮助下，你可以判断出这时代码是被直接运行，还是被导入到其他程序中去了。

**这个 \_\_name\_\_ 变量可能取什么值？**

当你直接执行一段脚本的时候，这段脚本的 **\_\_name\_\_**变量等于** '\_\_main\_\_'**，当这段脚本被导入其他程序的时候，**\_\_name\_\_ **变量等于脚本本身的名字。

下面，让我举两个栗子来说明一下：

<img width="638" height="208" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_e7972a44d82c49e49.jpg"/>

## 

**情况 1 - 直接运行脚本**

假设我们有一个**nameScript.py**，代码如下：

```
def myFunction():
    print('变量 __name__ 的值是 ' + __name__)
def main():
    myFunction()
if __name__ == '__main__':
    main()
```

当你直接执行** nameScript.py** 时，流程是这样处理的：

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__8c25e148e10143b6b.png)

在所有其他代码执行之前，**\_\_name\_\_**变量就被设置为** '\_\_main\_\_' **了。在此之后，通过执行 def 语句，函数** main() **和 **myFunction() **的本体被载入。

接着，因为这个 if 语句后面的表达式为真** true**，函数** main() **就被调用了。而 main() 函数又调用了**myFunction()**，打印出变量的值**'\_\_main\_\_'**。

## 

**情况 2 - 从其他脚本里导入**

如果你需要在其他脚本里重用这个** myFunction() **函数，比如在** importingScript.py **里，我们可以将** nameScript.py **作为一个模组导入。

假设** importingScript.py **的内容如下：

```
import nameScript as ns
ns.myFunction()
```

这时，我们就有了两个不同的作用域：一个是** importingScript** 的，一个是** nameScript** 的。让我画个示意图，你就能看出这和之前的区别了：

<img width="677" height="385" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__41b72f588ecf46159.png"/>

在** importingScript.py **里，**\_\_name\_\_ **变量就被设置为** '\_\_main\_\_'**。当导入** nameScript **的时候，Python 就在本地和环境变量** PATH **指向的路径中寻找对应名称的 .py 文件，找到之后，将会运行导入的文件中的代码。

但这一次，在导入的时候，它自身的** \_\_name\_\_ **变量就被设置为了** 'nameScript'**，接下来还是一样，函数 **main() **和** myFunction() **的本体被载入。然而，这一次 if 语句后面的表达式结果为假 false，所以** main() **函数没有被调用。

导入完毕之后，回到** importingScript.py **中。现在** nameScript** 模块中的函数定义已经被导入到当前的作用域中，于是我们通过 **ns.myFunction() **的方式调用模块中的函数，这个函数返回的是模块内的变量的值** 'nameScript'**。

如果你试着在** importingScript **中打印** \_\_name\_\_ **变量的值，那当你直接执行** importingScript **的时候，它也会输出** '\_\_main\_\_'**。原因在于，这个变量是在** importingScript **的作用域中的。

**总结**

今天和大家一起讨论了 **\_\_name\_\_ **变量在模组中的特性，分析了不同的调用方式对它的值有什么影响。利用这个特性，你既可以在程序中导入模组来使用，也可以直接把模组本身作为程序来运行。

**下载1：OpenCV-Contrib扩展模块中文版教程**

在「**小白学视觉**」公众号后台回复：**扩展模块中文教程****，**即可下载全网第一份OpenCV扩展模块教程中文版，涵盖**扩展模块安装、SFM算法、立体视觉、目标跟踪、生物视觉、超分辨率处理**等二十多章内容。

**下载2：Python视觉实战项目52讲**

在「**小白学视觉**」公众号后台回复：**Python视觉实战项目****，**即可下载包括**图像分割、口罩检测、车道线检测、车辆计数、添加眼线、车牌识别、字符识别、情绪检测、文本内容提取、面部识别**等31个视觉实战项目，助力快速学校计算机视觉。

**下载3：OpenCV实战项目20讲**

在「**小白学视觉**」公众号后台回复：**OpenCV实战项目20讲****，**即可下载含有**20**个基于**OpenCV**实现20个**实战项目**，实现OpenCV学习进阶。

交流群

欢迎加入公众号读者群一起和同行交流，目前有SLAM、三维视觉、传感器、自动驾驶、计算摄影、检测、分割、识别、医学影像、GAN、算法竞赛等微信群（以后会逐渐细分），请扫描下面微信号加群，备注：”昵称+学校/公司+研究方向“，例如：”张三 + 上海交大 + 视觉SLAM“。请按照格式备注，否则不予通过。添加成功后会根据研究方向邀请进入相关微信群。**请勿**在群内发送广告，否则会请出群，谢谢理解~

<img width="134" height="134" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_c9e9c1c8137b41d79.jpg"/>

<img width="677" height="392" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_ae12f0e415344d518.jpg"/>

People who liked this content also liked

前端竟然用Golang 动态生成图片？

前端技术江湖

不看的原因

- 内容质量低
- 不看此公众号

会写代码的AI开源了！C语言写得比Codex还要好，掌握12种编程语言丨CMU

量子位

不看的原因

- 内容质量低
- 不看此公众号

优化算法 | Jaya算法（附MATLAB代码）

优化算法交流地

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___242f06d486a048a4b.bmp"/>

Scan to Follow