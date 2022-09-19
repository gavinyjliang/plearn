# 在 Python 中如何实现类的继承，方法重载及重写？

<a id="profileBt"></a><a id="js_name"></a>AI科技大本营 *2022-05-27 18:30* *Posted on <a id="js_ip_wording"></a>北京*

<img width="661" height="117" src="../../../_resources/640_wx_fmt_gif_wxfrom_5_wx_lazy__3e5209310b3546689.gif"/>

作者 | 苏凉.py

来源 | CSDN博客

今天我们将进入类的继承以及对类的方法重写及重载的学习！话不多说直接进入正题！！

**<img width="661" height="70" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__c474c509fe54442d8.png"/>**

**类的继承**

如果要编写的类是另一个现成类的特殊版本，那我们就可以使用继承。一个类继承另一个类时，将自动获得另一个类的所有属性和方法，原有的类称为父类，而新的类称为子类，子类继承父类的所有属性和方法，同时还可以定义自己的属性和方法。

**继承的特点**

- 如果在子类中需要父类的构造方法就需要调用父类的构造方法，或者不重写父类的构造方法。
    
- 在调用父类的方法时，需要加上父类的类名前缀，且需要带上 self 参数变量。区别在于类中调用普通函数时并不需要带上 self 参数。
    
- Python 总是首先查找对应类型的方法，如果它不能在子类中找到对应的方法，它才开始到父类中逐个查找。（先在子类中查找调用的方法，找不到才去夫类中找）。
    

**子类不重写__ init __ 的继承（子类需要自动调用父类的方法）**

子类不重写 __ init __，实例化子类时，会 自动调用父类定义的 __ init __。

```
`# 创建一个父类``class Base_father:` `def __init__(self,name,age):` `self.name = name` `self.age = age` `print('调用了父类的name')``# 创建子类``class Base_son(Base_father):` `def getname(self):` `print(f'姓名：{self.name}')` `print(f'年龄：{self.age}')` `return '运行完毕！！'``num1 = Base_son('suliang',21)``print(num1.getname())`
```

运行结果：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

在子类中没有重写 __ init __方法，在调用子类进行实例化时，就默认调用父类的 __ init __ 方法。

### **子类重写__ init __ 的继承（子类不需要自动调用父类的方法）**

> 如果重写了__ init __ 时，实例化子类，就不会调用父类已经定义的 __ init __。

```


`# 创建一个父类class Base_father:    def __init__(self,name,age):        self.name = name        self.age = age` `print('调用了父类的name')``# 创建子类``class Base_son(Base_father):` `def __init__(self,name,age):` `self.name = name` `self.age = age` `print('调用了我自己定义的方法！！')` `def getname(self):` `print(f'姓名：{self.name}')` `print(f'年龄：{self.age}')` `return '运行完毕！！'``num1 = Base_son('suliang',21)``print(num1.getname())`


```

运行结果：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

通过上面两个例子就可以清楚的看到，若是子类自己定义了一个初始化方法 __ init __，那么将不在调用父类已经调用好的 __ init __ 方法。

### **子类重写 \_\_init \_\_ ，并且继承父类的构造方法（super）**

```
`如果重写了__ init __ 时，要继承父类的构造方法，可以使用 super关键字。``语法：super(子类，self).__ init __(参数)`
```

```
`# 创建一个父类``class Base_father:` `def __init__(self,name,age):` `self.name = name` `self.age = age` `print('调用了父类的name')``# 创建子类``class Base_son(Base_father):` `def __init__(self,name,age):` `#利用super调用父类的构造函数` `super(Base_son, self).__init__(name ,age)` `print('-'*50)` `self.name = name` `self.age = age` `print('调用了我自己定义的方法！！')` `def getname(self):` `print(f'姓名：{self.name}')` `print(f'年龄：{self.age}')` `return '运行完毕！！'``num1 = Base_son('suliang',21)``print(num1.getname())`
```

运行结果：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

**![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)**

## **方法的重写及重载**

### **重写**

子类对父类的允许访问的方法的实现过程进行重新编写, 返回值和形参都不能改变。

**优点：**子类可以根据需要，定义特定于自己的行为。也就是说子类能够根据需要实现父类的方法。

**重载**

重载(overloading) 是在一个类里面，==方法名字相同，而参数不同==。返回类型可以相同也可以不同。每个重载的方法（或者构造函数）都必须有一个独一无二的参数类型列表。

**二者区别**

1.  方法重载是一个类中定义了多个方法名相同，而他们的参数的数量不同或数量相同而类型和次序不同，则称为方法的重载。
    
2.  方法重写是在子类存在方法与父类的方法的名字相同，而且参数的个数与类型一样，返回值也一样的方法，就称为重写。
    
3.  方法重载是一个类的多态性表现，而方法重写是子类与父类的一种多态性表现。
    

**对方法重写**

如果父类方法的功能不能满足你的需求，就可以在子类重写你父类的方法。

```
`class Father:` `def __init__(self,name):` `self.name = name` `def list(self):` `print(f'name:{self.name}')``class Son(Father):` `def list(self):` `print(f'姓名：{self.name}')` `return  '执行完毕！！'``num1 = Son('suliang')``print(num1.list())`
```

运行结果：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

**基础重载方法**

- 构造函数
    

> __ init __ ( self \[,args\] )

- 析构方法，删除一个对象
    

> __ del __( self )

- 转化为供解释器读取的形式
    

> __ repr __( self )

- 用于将值转化为适于人阅读的形式
    

> __ str __( self )

- 对象比较
    

> __ cmp __ ( self, x )

方法重载的具体方法将在下一章进行详细介绍。在此之作简单说明！！

**![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)**

**类的属性和方法（）**

**类的私有属性**

在定义类的属性时，在前面加入__（两个下划线）即代表私有属性，==只能在类的内部调用==，而不能在外部调用。

```
`class List:` `a = 5 #类的公有属性` `__b = 6 #类的私有属性``obj = List()``print(obj.a)``print(obj.__b)`
```

运行结果：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### **类的私有方法**

在定义方法时，在前面加入 __ (两个下划线)即可定义一个私有方法，只能在类的内部调用，语法为self.__方法名

```
`class List:` `def __init__(self ,a,b ):` `self.a = a` `self.b =b` `def pri1(self):` `# 定义一个公有方法` `print(f'{self.a + self.b}')` `return '  '` `def __pri2(self):` `# 定义一个私有方法` `print(f'{self.a *self.b}')` `def pri3(self):` `self.__pri2()  # 在内部调用私有方法` `return '  '``obj = List(5,10)``print(obj.pri1())``print(obj.pri3())`
```

运行结果：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

**![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)**

### **面向对象中下划线的说明**

### **_前面单下划线**

以单下划线开头的表示的是 ==protected 类型的变量==，即保护类型==只能允许其本身与子类进行访问==。

### **_前面双下划线**

双下划线的表示的是==私有类型(private)的变量==, 只能是允许这个类本身进行访问了。

### **_前面双下划线****_**

### ==定义的是特殊方法==，一般是系统定义名字 ，类似 __ init __() 之类的。

**![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)**

**小结**

本篇文章带大家了解了类的继承，方法的重写以及重载的内容。一顿操作下来是不是觉得并不难呢，当然这都是基础语法，深入的还需大家理解这其中的内涵，再慢慢的去实践。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

**往期回顾**

[介绍Pandas实战中的一些高端玩法](http://mp.weixin.qq.com/s?__biz=Mzg4NDQwNTI0OQ==&mid=2247560261&idx=2&sn=840f1c8524764beba14b032beb2c28b3&chksm=cfbb092bf8cc803d3967d1fcb3cb33236a0d118413af06e3ec438e6f5d5dc66b8f2060e6c649&scene=21#wechat_redirect)

[从Python可视化图表中探究王心凌的流量密码](http://mp.weixin.qq.com/s?__biz=Mzg4NDQwNTI0OQ==&mid=2247560469&idx=1&sn=9e139ceee84cc4fe0acd2f7654fe5f5a&chksm=cfbb087bf8cc816dab09eef93d5b44731ded958e59baa9cbbc5ee1e3d3349c9596af040ac624&scene=21#wechat_redirect)

[Python 实现 GIF 动图以及视频卡通化](http://mp.weixin.qq.com/s?__biz=Mzg4NDQwNTI0OQ==&mid=2247560380&idx=1&sn=c85a86800762b8da9a35923dad46f4c8&chksm=cfbb09d2f8cc80c4f950d636463ccb4ec6323c1a9526a005312a973fc9a9c79e9da9077eefba&scene=21#wechat_redirect)

[如何用一行Python代码制作一个GUI？](http://mp.weixin.qq.com/s?__biz=Mzg4NDQwNTI0OQ==&mid=2247560214&idx=3&sn=9ccf13326a163c9cc3f2e02fe0d7e184&chksm=cfbb0978f8cc806ef7eb68e55b203e62b0372fc92f10f7c587bbff6b1aff740b9fac445e27fd&scene=21#wechat_redirect)

```


![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

分享

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

点收藏

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

点点赞

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

点在看












```

People who liked this content also liked

一文弄懂Python中的pprint模块

...

AI算法之道

不看的原因

- 内容质量低
- 不看此公众号

Python 中最简单易用的并行加速技巧

...

AI科技大本营

不看的原因

- 内容质量低
- 不看此公众号

推荐七个Python效率工具

...

Python丹卿

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___9bf31d5302d446429.bmp"/>

Scan to Follow