# Python里面的self，是谁啊？

<a id="profileBt"></a><a id="js_name"></a>机器学习算法那些事 *2022-04-07 14:30*

**正文**

大家学Python面向对象的时候，总会遇到一个让人难以理解的存在：self

这个self到底是谁啊，为什么每个类实例方法都有一个参数self，它到底有什么作用呢？

**「先下结论：类实例化后，self即代表着实例（对象）本身」**

想要理解self有个最简单的方法，就是你把self当做**「实例（对象)**的**身份证。」**

Python的类不能直接使用，只有通过创建实例（对象）才能发挥它的功能，每个实例（对象）都是独一无二的，它可以调用类的方法、属性。类就像灵魂附体一样，让实例（对象）有了自己（self）的功能。<img width="677" height="528" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__5346a736738c4a68a.png"/>

初学者会发现，类的方法（构造方法和实例方法）中都会有一个固定参数self，其实这个参数就是代表着实例（对象）本身，就像是一个身份证，实例可以凭着身份证去调用类方法。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

类比人类，人类就是一个Python类，每个个体的人代表着实例（对象），而每个人的身份证代表的Python中self，每个人可以凭借身份证去上大学、坐高铁、住酒店...（方法），而Python中的实例（对象）也可以凭着self去调用类的方法。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

上面是用类比的方法解释了下self的含义，说到底self就是代表着实例本身，**「当某个实例（对象）调用类方法时，该对象会把自身的引用作为第一个参数自动传给该方法，而这第一个参数就是self。」**

而且self只是约定俗成的写法，你可以用任何其他名称代替self，不会改变代码含义，只不过我们一般不这样做。另外，搜索公众号顶级架构师后台回复“面试”，获取一份惊喜礼包。

为了更好的说明self的作用，我们来举个例子，下面有一个Students类：

```
`class Students:
    # 构造方法
    def __init__(self,name):
        self.name = name
    # 实例方法
    def study(self,examination_results):
        self.examination_results = examination_results
        print("同学{}的考试分数是{}".format(self.name,self.examination_results))
        print("该实例对象的地址是{}".format(self))
`
```

先来个实例student_a

```
`studend_a = Students('studend_a')
print(studend_a.name)
`
```

> ❝
> 
> 结果打印出：studend_a
> 
> ❞

再来个实例student_b

```
`studend_b = Students('studend_b')
print(studend_b.name)
`
```

> ❝
> 
> 结果打印出：studend_b
> 
> ❞

可以看出，实例（对象）不一样，打印出的结果也不一样，当类被实例化后，`self.name`其实就等于`实例（对象）.name`

还是以刚刚的代码为例，我们再来调用里面的实例方法，里面会打印出self，就能看得更加明显了

实例student_a：

```
`studend_a = Students('studend_a')
print(studend_a.study(80))
`
```

输出结果：

> ❝
> 
> 同学studend_a的考试分数是80 该实例对象的地址是<**「main」**.Students object at 0x00000129EB0F6A90>
> 
> ❞

实例student_b：

```
`studend_b = Students('studend_b')
print(studend_b.study(80))
`
```

输出结果：

> ❝
> 
> 同学studend_b的考试分数是80 该实例对象的地址是<**「main」**.Students object at 0x00000129EB0F6B38>
> 
> ❞

大家能清楚看到两个实例打印出的self是不一样的，因为self代表着实例（对象）本身。

以实例student_b来说，打印self出现下面对象信息

`<__main__.Students object at 0x00000129EB0F6B38>`

如果再打印 student_b，会出现同样的结果

```
`print(student_b)
`
```

输出：

`<__main__.Students object at 0x00000129EB0F6B00>`

这个时候是不是就清楚了，类实例化后，self即代表着实例（对象)本身

****你还有什****么想要补充的吗？****

> 免责声明：本文内容来源于网络，文章版权归原作者所有，意在传播相关技术知识&行业趋势，供大家学习交流，若涉及作品版权问题，请联系删除或授权事宜。编辑：机器学习算法与Python实战

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

People who liked this content also liked

【Docker】命令使用大全

...

小白学视觉

不看的原因

- 内容质量低
- 不看此公众号

Python中最简单易用的并行加速技巧

...

Python大数据分析

不看的原因

- 内容质量低
- 不看此公众号

用Python去除图片背景：​Rembg库

...

Python大数据分析

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___5daa2896e1b6405f8.bmp"/>

Scan to Follow