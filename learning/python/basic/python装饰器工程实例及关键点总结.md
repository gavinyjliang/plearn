# python装饰器工程实例及关键点总结

<a id="profileBt"></a><a id="js_name"></a>机器学习与生成对抗网络 *2022-05-31 14:19* *Posted on <a id="js_ip_wording"></a>广东*

![](../../../_resources/0_wx_fmt_png_7a8e030b43d4445792acb6cfdf6fee93.png)

**机器学习与生成对抗网络**

记录分享通俗、有趣的AI科技知识，包括不限于CV、GAN等等，还有程序员求职面试、内推等资料，偶尔分享诗词歌赋、陶冶情操，一起做个有趣、前沿的人！

<a id="js_profile_article"></a>130篇原创内容

Official Account

来源：知乎—苹果姐

地址：https://zhuanlan.zhihu.com/p/465334776

## 

**00**

**工程实例**

装饰器是一个基础概念也是让初学者很迷糊的概念，但每个中大型工程里面都会用到，搞不清楚的话就会看得云里雾里，很多功能找不到在哪里实现的。比如以下这个工程实例：（出自Featdepth模型源码）

```
`from mono.model.registry import MONO``from mmcv import Config``def main():` `args = parse_args()` `cfg = Config.fromfile(args.config)`` model_name = cfg.model['name']    # 返回模型名称字符串` `model = MONO.module_dict[model_name](https://mp.weixin.qq.com/s/cfg.model)` `……``if __name__ == '__main__':``    main()`
```

看到这里就会疑惑 model = MONO.module\_dict\[modelname\](https://mp.weixin.qq.com/s/cfg.model)这一句中module\_dict是哪里来的，来看看导入的MONO模块：

```
`class Registry(object):` `def __init__(self, name):` `self._name = name` `self._module_dict = dict()` `@property` `def name(self):` `return self._name` `@property` `def module_dict(self):` `return self._module_dict` `def _register_module(self, module_class):` `"""Register a module.` `Args:` ``module (:obj:`nn.Module`): Module to be registered.`` `"""` `if not issubclass(module_class, nn.Module):` `raise TypeError(` `'module must be a child of nn.Module, but got {}'.format(` `module_class))` `module_name = module_class.__name__` `if module_name in self._module_dict:` `raise KeyError('{} is already registered in {}'.format(` `module_name, self.name))` `self._module_dict[module_name] = module_class` `def register_module(self, cls):` `self._register_module(cls)` `return cls``MONO = Registry('mono')`
```

原来MONO是一个实例化的Registry对象，里面有module_dict这个属性，但是这个字典里面目前是空的，如何取到里面的元素呢？

发现mono.model有个\_init\_.py文件，这个文件在模块导入时候会自动执行，里面内容是：

```
`from .mono_baseline.net import Baseline``from .mono_autoencoder.net import autoencoder``from .mono_fm.net import mono_fm``from .mono_fm_joint.net import mono_fm_joint`
```

说明这些.net文件在导入的时候也被执行了。随便打开其中一个.net文件：

```
`rom ..registry import MONO``@MONO.register_module``class autoencoder(nn.Module):` `def __init__(self, options):` `super(autoencoder, self).__init__()` `self.opt = options` `……`
```

这里的@MONO.register\_module就是用到了装饰器，这个函数在Registry类中定义，功能就是将被装饰的类以类名为key，存入module\_dict中，所以在本文最开始的代码中：

```
model = MONO.module_dict[model_name](https://mp.weixin.qq.com/s/cfg.model)
```

这一句就可以用模型名称（model\_name）从module\_dict中取到，同时进行实例化了。

可以说这是装饰器的一个很典型的应用，可以实现设计模式中的工厂模式，很方便地将各个模块在定义时即注册到工厂中，从而可以很灵活地使用。通过这个例子深入研究装饰器，结合网上的各种教程，可以总结出以下关键点：

**01**

**装饰器是闭包的语法糖**

所谓语法糖及语法上完全等价的表达方式。装饰器和闭包是完全等价的，及本身是一个函数，接收的参数是函数，返回的还是一个函数：

```
`def funcA(func):` `def funcB():` `print('funcA')` `func()``    return funcB`
```

此时如果用funcA来装饰一个函数：

```
`@funcA``def funcC():` `print('funcC')`
```

等价于闭包的写法：

```
funcC = funcA(funcC)
```

运行结果：

```


```
`funcC()``输出：``funcA``funcC`
```




```

可见装饰器和闭包的功能都是对函数功能进行增强，变成一个新的增强函数。

## 

**02**

**装饰器在定义时及执行，在被装饰函数被调用时不再执行**

在funcA中加一句打印：

```
`def funcA(func):` `print('funcAAA')` `def funcB():` `print('funcA')` `func()` `return funcB``@funcA``def funcC():``    print('funcC')`
```

在此模块被导入或执行时（funcA和funcC均未被调用），funcA及被执行：

```
`输出：``funcAAA`
```

而在funcC被调用时不会再输出funcAAA：

```
`funcC()``输出：``funcA``funcC`
```

这是因为此时funC已经变成了funcA返回的新函数funcB，被调用的时候和funcA已经没有关系了，而且funcC也不存在了：

```
`print(funcC.__name__)``输出：``funcB`
```

```


```

**03**

**多个装饰器的执行顺序：定义时从下往上，调用时从上往下**

```
`def func_a(func):` `print(1)` `def func_a1():` `print(11)` `func()` `return func_a1``def func_b(func):` `print(2)` `def func_b1():` `print(22)` `func()` `return func_b1``@func_a``@func_b``def func_c():` `print(3)`
```

直接执行输出：

```
`2``1`
```

调用时：

```
`func_c()``输出：``11``22``3`
```

这个其实也好理解，写成闭包的形式：

```
func_c = func_a(func_b(func_c))
```

因为内层优先级高，所以定义时从内向外执行，而在调用时，func\_c已经变成了func\_a的内层函数funca1，里面又包含了func\_b的内层函数func\_b1，所以从外向内执行，相当于入栈和出栈。

**04**

**带参数和返回值的装饰函数写法**

**1\. 被装饰函数有参数和返回值：**

```
`def func_a(func):` `def func_a1(*args, **kwargs):` `a = func(*args, **kwargs) + 1` `return a` `return func_a1``@func_a``def func_b(num):` `return num`
```

调用：

```
`m = func_b(100)``print(m)``输出：``101`
```

可见如果被装饰函数有参数和返回值，则在装饰器的内层函数中设置相应的参数和返回值，和被装饰函数保持一致即可。参数可写成通用模式：\*args, \*\*kwargs

**2\. 装饰器有参数和返回值：**

```
`def func_a(num):` `def func_a1(func):` `def func_a2(*args, **kwargs):` `a = func(*args, **kwargs) + num` `return a` `return func_a2` `return func_a1``@func_a(100)``def func_b(a):``    return a`
```

调用：

```
`m = func_b(1000)``print(m)``输出：``1100`
```

这时候看看被装饰函数func_b变成了什么：

```
`print(func_b.__name__)``输出：``func_a2`
```

可见如果装饰器有参数和返回值，则需要在装饰器中再加一层，也就是一共三层，第外层接收装饰器的参数，中间层接收被装饰函数，最内层接收被装饰函数的参数。这个时候被装饰函数还是变成了装饰器的最内层函数，只是装饰器要多一层接收自己的参数而已。

## 

**05**

**装饰器或被装饰对象是类的情况**

**1\. 被装饰对象是类：**

```
`def wrapClass(cls):` `def inner(para):` `print('param name:', para)` `return cls(para)` `return inner``@wrapClass``class Foo():` `def __init__(self, a):` `self.a = a` `def fun(self):` `print('self.a =', self.a)`
```

调用：

```
`Foo('apple')``输出：``param name: apple`
```

可见如果被装饰对象是类，那装饰器返回的也应该是类

**2\. 装饰器是类：**

先看类装饰类的情况：

```
`class ShowClassName(object):` `def __init__(self, cls):` `self._cls = cls` `def __call__(self, a):` `print('class name:', self._cls.__name__)` `return self._cls(a)``@ShowClassName``class Foobar(object):` `def __init__(self, a):` `self.value = a` `def fun(self):` `print(self.value)`
```

调用：

```
`a = Foobar('apple')``输出：``class name: Foobar`
```

再看类装饰函数的情况：

```
`class ShowFunName():` `def __init__(self, func):` `self._func = func` `def __call__(self, a):` `print('function name:', self._func.__name__)` `return self._func(a)``@ShowFunName``def Bar(a):` `return a`
```

调用：

```
`print(Bar('apple'))``输出：``function name: Bar``apple`
```

可见如果装饰器是类，在调用被装饰对象的时候会调用装饰器的\_\_call\_\_方法，相当于装饰器是函数情况的内层函数，如果被装饰对象是类则返回类，是函数则返回函数。

总之无论装饰器还是被装饰对象是类，本质上和函数装饰函数是一样的，都是返回对被装饰对象的增强。

关于装饰器就介绍这么多，希望帮到大家

![](../../../_resources/0_wx_fmt_png_7675093a042e4f119a0c8fc6aa254ffe.png)

**摸鱼吧算法工程师**

初入职场、仍旧保持有好奇心的老学姐 /(ㄒoㄒ)/~~

<a id="js_profile_article"></a>7篇原创内容

Official Account

**猜您喜欢：**

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)<ins>[戳我，查看GAN的系列专辑~！](https://mp.weixin.qq.com/mp/appmsgalbum?action=getalbum&album_id=1338452847133917185&__biz=MzU5MTgzNzE0MA==#wechat_redirect)</ins>

<ins>**[一顿午饭外卖，成为CV视觉前沿弄潮儿！](http://mp.weixin.qq.com/s?__biz=MzU5MTgzNzE0MA==&mid=2247495588&idx=1&sn=55b0953b85a6c0b003b0c341872ba4d1&chksm=fe2a4d1fc95dc4092c17841bff234069675fbf2c9321adc4f790c3ae5156e970e232e258bcbd&scene=21#wechat_redirect)**</ins>

**<ins>[CVPR 2022 | 25+方向、最新50篇GAN论文](http://mp.weixin.qq.com/s?__biz=MzU5MTgzNzE0MA==&mid=2247496616&idx=1&sn=9433eb1890e84457ce192e506458e389&chksm=fe2a5113c95dd8057764d2cf1e696b06b0517aa58258eea1e2d3e5e02b8ad45ae31239c2eaae&scene=21#wechat_redirect)
</ins>**

**<ins>[ICCV 2021 | 35个主题GAN论文汇总](http://mp.weixin.qq.com/s?__biz=MzU5MTgzNzE0MA==&mid=2247496449&idx=1&sn=49c3f04c2fd7d0ccfdd56da1dac081de&chksm=fe2a51bac95dd8ace9abd4a863b876dc12475d19488845cb6e7daa4aab3b12e2dbd8436d74fa&scene=21#wechat_redirect)
</ins>**

<ins>**[超110篇！CVPR 2021最全GAN论文梳理](http://mp.weixin.qq.com/s?__biz=MzU5MTgzNzE0MA==&mid=2247494947&idx=1&sn=62419b9131b0cf37b236b8b91c8dac40&chksm=fe2a4f98c95dc68eccdf79ab203418186b6870e5893b9990c6a725e84d32b7797a87fb756810&scene=21#wechat_redirect)**</ins>

<ins>**超100篇！CVPR 2020最全GAN论文梳理**</ins>

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

着急毕业，却还没发论文······

...

机器学习与生成对抗网络

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___027a4221c5e64deeb.bmp"/>

Scan to Follow