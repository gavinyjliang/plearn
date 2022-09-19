# 100个必备的 Python 函数

<a id="profileBt"></a><a id="js_name"></a>机器学习与生成对抗网络 *2022-04-06 11:58*

![](../../../_resources/0_wx_fmt_png_5eced26c56fb4d8e93219235c4770d72.png)

**弹新事**

推荐最新生活中关注的热点问题，分享生活中点滴成长经历与经验。

<a id="js_profile_article"></a>145篇原创内容

Official Account

来源：CSDN—退休的龙叔

新手在做写代码的时候容易卡壳，尤其当接触的函数以及其他知识比较多的时候，经常会看完需求之后不知道自己该用什么方法来实现它，实现的逻辑可能你有，但怎么该用什么函数给忘了，这其实就是知识的储备不够，你记不住哪个函数有什么作用，自然一头雾水。

这几天我专门整理了Python常用的一些函数，从最基础的输入输出函数到正则等12个板块的，总共100多个常用函数，方便小伙伴们进行快速地记忆，每天快速过一遍，用的时候再加深一下，慢慢地你就会摆脱写代码卡壳的状况。

虽说自学编程的时候我们强调更多的东西是理解和实际去敲代码，但有些东西你是要必须牢记的，否则你写代码将寸步难行。老手当然已经烂记于心，新手想要快速得心应手开发，记住高频使用的函数就是一个好法子。

**01**

**基础函数**

<img width="501" height="161" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__a99b5ad18b27496c8.png"/>

案例：将浮点型数值转换为字符串，输出转换后的数据类型

```


```
`f = 30.5``ff = str(f)``print(type(ff))``#输出结果为 class 'str'`
```

02

流程控制


```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

案例：根据用户输入的分数判断成绩，低于50分时提示“你的分数低于50分”，5059分时提示“你的分数在60分左右”，大于等于60分为及格，8090分为优秀，大于90分为非常优秀。

```


```
`s = int(input("请输入分数:"))``if 80 >= s >= 60:` `print("及格")``elif 80 < s <= 90:` `print("优秀")``elif 90 < s <= 100:` `print("非常优秀")``else:` `print("不及格")` `if s > 50:` `print("你的分数在60分左右")` `else:` `print("你的分数低于50分")`
```


```

**03**

**列表**

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

案例：判断6这个数在列表\[1,2,2,3,6,4,5,6,8,9,78,564,456\]中的位置，并输出其下标。

```
`l = [1,2,2,3,6,4,5,6,8,9,78,564,456]``n = l.index(6, 0, 9)``print(n)``#输出结果为  4`
```

```


```

**04**

**元组**

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

案例：修改元组

```
`#取元组下标在1~4之间的3个数，转换成列表``t = (1,2,3,4,5)``print(t[1:4])``l = list(t)``print(l)``#在列表下标为2的位置插入1个6``l[2]=6``print(l)``#讲修改后的列表转换成元组并输出``t=tuple(l)``print(t)`
```

```
`#运行结果为：``(2, 3, 4)``[1,` `2, 3, 4, 5]``[1, 2, 6, 4, 5]``(1, 2, 6, 4, 5)`
```

## 

**05**

**字符串**

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

案例：用format（）的三种方式输出字符串

方式1：用数字占位（下标）

```


```
`"{0} 嘿嘿".format("Python")``a=100``s = "{0}{1}{2} 嘿嘿"``s2 = s.format(a,"JAVA","C++")``print(s2)``#运行结果为：100JAVAC++ 嘿嘿`
```




```

方式2：用{} 占位

```
`a=100``s = "{}{}{} 嘿嘿"``s2 = s.format(a,"JAVA","C++","C# ")``print(s2)``#运行结果为：100JAVAC++ 嘿嘿`
```

方式3：用字母占位

```


```
`s = "{a}{b}{c} 嘿嘿"``s2 = s.format(b="JAVA",a="C++",c="C# ")``print(s2)``#运行结果为：C++JAVAC#  嘿嘿`
```

06

字典


```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

案例：在字典中查找数据

```
`d = {"name": "小黑"}``print(d.get("name2", "没有查到"))``print(d.get("name"))`
```

```
`#运行结果为：``没有查到``小黑`
```

```


```

**07**

**函数**

函数这块重头戏更多的是自定义函数，常用的内置函数不是很多，主要有以下几个：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

案例：在函数中定义一个局部变量，跳出函数仍能调用该变量

```


```
`def fun1():` `global b` `b=100` `print(b)``fun1()``print(b)`
```




```

```


```
`#运行结果为：``100``100`
```

08

进程和线程


```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

案例：继承Thread类实现

```


```
`#多线程的创建``class MyThread(threading.Thread):` `def __init__(self,name):` `super().__init__()` `self.name = name` `def run(self):` `#线程要做的事情` `for i in range(5):` `print(self.name)` `time.sleep(0.2)` `#实例化子线程``t1 = MyThread("凉凉")``t2 = MyThread("最亲的人")``t1.start()``t2.start()`
```

09

模块与包


```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

案例：包的使用方式4

```


```
`from my_package1 import my_module3``print(my_module3.a)``my_module3.fun4()`
```

10

文件操作


```

（1）常规文件操作

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

关于文件操作的常规模式：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

file的对象属性

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

file对象的方法

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

（2）OS模块

- 关于文件的功能
    

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

- 关于文件夹的功能
    

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 

**11**

**修饰器/装饰器**

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

案例：classmethod的用法举例

```


```
`class B:` `age = 10` `def __init__(self,name):` `self.name = name` `@classmethod` `def eat(cls): #普通函数` `print(cls.age)` `def sleep(self):` `print(self)``b = B("小贱人")``b.eat()``#运行结果为:10`
```

12

正则


```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

案例：用split()函数分割一个字符串并转换成列表

```


```
`import re``s = "abcabcacc"``l = re.split("b",s)``print(l)``#运行结果为：['a', 'ca', 'cacc']`
```




```

这篇文章的目的，不是为了教大家怎么使用函数，而是为了快速、便捷地记住常用的函数名，所以没有把每个函数的用法都给大家举例，你只有记住了函数名字和它的作用之后，你才会有头绪，至于函数的用法，百度一下就出来，用了几次你就会了。

如果连函数名和它的用途都不知道，你要花的时间和精力就更多了，必然不如我们带着目的性地去查资料会更快些。

**猜您喜欢：**

![](../../../_resources/0_wx_fmt_png_c10c89c4353c49cf9efa40a575ab5d91.png)

**机器学习与生成对抗网络**

记录分享通俗、有趣的AI科技知识，包括不限于CV、GAN等等，还有程序员求职面试、内推等资料，偶尔分享诗词歌赋、陶冶情操，一起做个有趣、前沿的人！

<a id="js_profile_article"></a>130篇原创内容

Official Account

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)<ins>[戳我，查看GAN的系列专辑~！](https://mp.weixin.qq.com/mp/appmsgalbum?action=getalbum&album_id=1338452847133917185&__biz=MzU5MTgzNzE0MA==#wechat_redirect)</ins>

<ins>**[一顿午饭外卖，成为CV视觉的前沿弄潮儿！](http://mp.weixin.qq.com/s?__biz=MzU5MTgzNzE0MA==&mid=2247495588&idx=1&sn=55b0953b85a6c0b003b0c341872ba4d1&chksm=fe2a4d1fc95dc4092c17841bff234069675fbf2c9321adc4f790c3ae5156e970e232e258bcbd&scene=21#wechat_redirect)**</ins>

<ins>**[超110篇！CVPR 2021最全GAN论文汇总梳理！](http://mp.weixin.qq.com/s?__biz=MzU5MTgzNzE0MA==&mid=2247494947&idx=1&sn=62419b9131b0cf37b236b8b91c8dac40&chksm=fe2a4f98c95dc68eccdf79ab203418186b6870e5893b9990c6a725e84d32b7797a87fb756810&scene=21#wechat_redirect)**</ins>

<ins>**超100篇！CVPR 2020最全GAN论文梳理汇总！**</ins>

<ins>拆解组新的GAN：解耦表征MixNMatch</ins>

<ins>StarGAN第2版：多域多样性图像生成</ins>

<ins>[附下载 | 《可解释的机器学习》中文版](http://mp.weixin.qq.com/s?__biz=MzU5MTgzNzE0MA==&mid=2247484929&idx=1&sn=523220858287ed3e6105919703c8e833&chksm=fe29a4bac95e2dac758c4aac10a3aac49eb062bc0829d4ef2ebc8e6d5069d246385c5084a2b4&scene=21#wechat_redirect)</ins>

<ins>附下载 |《TensorFlow 2.0 深度学习算法实战》</ins>

<ins>附下载 |《计算机视觉中的数学方法》分享</ins>

<ins>[《基于深度学习的表面缺陷检测方法综述》](http://mp.weixin.qq.com/s?__biz=MzU5MTgzNzE0MA==&mid=2247485492&idx=1&sn=6f8af433521b5fee5aceab62e0111115&chksm=fe29aa8fc95e2399a93dae3af3b13626643b18cf42842d1a7ada0de46486fa8b865aa2421d2f&scene=21#wechat_redirect)</ins>

<ins>《零样本图像分类综述: 十年进展》</ins>

<ins>《基于深度神经网络的少样本学习综述》</ins>

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

People who liked this content also liked

GO富集分析及可视化，一把子拿捏！

...

基迪奥生物

不看的原因

- 内容质量低
- 不看此公众号

用Python去除图片背景：​Rembg库

...

Python大数据分析

不看的原因

- 内容质量低
- 不看此公众号

Python 强大的模式匹配工具—Pampy

...

快学Python

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___e692d38fbe684e028.bmp"/>

Scan to Follow