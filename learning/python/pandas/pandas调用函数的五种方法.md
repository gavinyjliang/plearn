# pandas调用函数的五种方法

<a id="profileBt"></a><a id="js_name"></a>数据STUDIO *2021-10-27 11:30*

The following article is from 可以叫我才哥 Author 道才

<a id="copyright_info"></a>[![](../../../_resources/0_44d5a45f5efc4265a61028eb4d6ccfeb.jpg)<br>**可以叫我才哥** .<br>学python，玩转数据分析、网络爬虫以及可视化等等](#)

大家好，我是才哥。

最近咱们的交流群很活跃，每天都有不少朋友提出技术问题引来大家的热烈讨论探究。才哥也参与其中，然后发现很多`pandas`相关的**数据处理问题**都可以通过**调用函数**的方法来快速处理。

那么，今天我们就来介绍`Pandas`常用的几种**调用函数的方法**吧。

这里我们以曾经用于《[对比Excel，用Pandas轻松搞定IF函数操作](http://mp.weixin.qq.com/s?__biz=MzUzODA5MjM5NQ==&mid=2247490636&idx=2&sn=f914d7f0300389a988977f1828612b25&chksm=faddaa48cdaa235eeb8ced28ede10f01c9d1565388f6d99d42735672ecb2e37939e9f387863a&scene=21#wechat_redirect)》的案例数据来演示~

**目录：**

- 0\. 数据预览
    
- 1\. apply
    
- 2\. applymap
    
- 3\. map
    
- 4\. agg
    
- 5\. pipe
    

## 0\. 数据预览

这里的数据是虚构的语数外成绩，大家在演示的时候拷贝一下就好啦。

```
`import pandas as pd
df = pd.read_clipboard()
df
`
```

|     | 姓名  | 语文  | 数学  | 英语  | 性别  | 总分  |
| --- | --- | --- | --- | --- | --- | --- |
| 0   | 才哥  | 91  | 95  | 92  | 1   | 278 |
| 1   | 小明  | 82  | 93  | 91  | 1   | 266 |
| 2   | 小华  | 82  | 87  | 94  | 1   | 263 |
| 3   | 小草  | 96  | 55  | 88  | 0   | 239 |
| 4   | 小红  | 51  | 41  | 70  | 0   | 162 |
| 5   | 小花  | 58  | 59  | 40  | 0   | 157 |
| 6   | 小龙  | 70  | 55  | 59  | 1   | 184 |
| 7   | 杰克  | 53  | 44  | 42  | 1   | 139 |
| 8   | 韩梅梅 | 45  | 51  | 67  | 0   | 163 |

## 1\. apply

`apply`可以对`DataFrame`类型数据按照**列或行**进行函数处理，**默认**情况下是按照**列**（单独对`Series`亦可）。

在案例数据中，比如我们想将**性别**列中的`1`替换为男，`0`替换为女，那么可以这样搞定。

先自定义一个函数，这个函数有一个参数 s（Series类型数据）。

```
`def getSex(s):
    if s==1:
        return '男'
    elif s==0:
        return '女'
`
```

上述函数还有更简洁写法，这里方便理解采用最直观的写法哈。

然后，我们直接使用`apply`去调用这个函数即可。

```
`df['性别'].apply(getSex)
`
```

可以看到输出结果如下：

```
`0    男
1    男
2    男
3    女
4    女
5    女
6    男
7    男
8    女
Name: 性别, dtype: object
`
```

当然，我们也可以直接用调用匿名函数`lambda`的形式：

```
`df['性别'].apply( lambda s: '男' if s==1 else '女' )
`
```

可以看到结果是一样的：

```
`0    男
1    男
2    男
3    女
4    女
5    女
6    男
7    男
8    女
Name: 性别, dtype: object
`
```

以上是单纯根据**一列**的值条件进行的数据处理，我们也可以根据**多列组合条件**（可以了解为**按行**）进行处理，需要注意这种情况下需要指定参数`axis=1`，具体看下面案例。

案例中，我们认为**总分高于200**且**数学分数高于90**为高分

```
`# 多列条件组合
df['level'] = df.apply(lambda df: '高分' if df['总分']>=200 and df['数学']>=90 else '其他', axis=1)
df
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

同样，上述用`apply`调用的函数都是自定义的，实际上我们也可以调用**内置**或者`pandas`/`numpy`等**自带**的函数。

比如，**求语数外和总分最高分：**

```
`# python内置的函数
df[['语文','数学','英语','总分']].apply(max)
`
```

```
`语文     96
数学     95
英语     94
总分    278
dtype: int64
`
```

**求语数外和总分平均分：**

```
`# numpy自带的函数
import numpy as np
df[['语文','数学','英语','总分']].apply(np.mean)
`
```

```
`语文     69.777778
数学     64.444444
英语     71.444444
总分    205.666667
dtype: float64
`
```

## 2\. applymap

`applymap`则是对每个元素的函数处理，变量是每个元素值。

比如对语数外三科超过`90`分认为是科目高分

```
`df[['语文','数学','英语']].applymap(lambda x:'高分' if x>=90 else '其他')
`
```

|     | 语文  | 数学  | 英语  |
| --- | --- | --- | --- |
| 0   | 高分  | 高分  | 高分  |
| 1   | 其他  | 高分  | 高分  |
| 2   | 其他  | 其他  | 高分  |
| 3   | 高分  | 其他  | 其他  |
| 4   | 其他  | 其他  | 其他  |
| 5   | 其他  | 其他  | 其他  |
| 6   | 其他  | 其他  | 其他  |
| 7   | 其他  | 其他  | 其他  |
| 8   | 其他  | 其他  | 其他  |

## 3\. map

`map`则是根据输入对应关系映射值返回最终数据，作用于`某一列`。传入的值可以是字典，键值为原始值，值为需要替换的值。也可以传入一个函数或者字符格式化表达式等等。

以上面性别列中的`1`替换为男，`0`替换为女为例，还可以通过`map`来实现

```
`df['性别'].map({1:'男', 0:'女'})
`
```

输出结果也是一致的：

```
`0    男
1    男
2    男
3    女
4    女
5    女
6    男
7    男
8    女
Name: 性别, dtype: object
`
```

比如总分列想变成格式化字符：

```
`df['总分'].map('总分：{}分'.format)
`
```

```
`0    总分：278分
1    总分：266分
2    总分：263分
3    总分：239分
4    总分：162分
5    总分：157分
6    总分：184分
7    总分：139分
8    总分：163分
Name: 总分, dtype: object
`
```

## 4\. agg

`agg`一般用于**聚合**，在分组或透视操作中常见到，用法是和`apply`比较接近。

比如，求语数外和总分的最高分、最低分和平均分

```
`df[['语文','数学','英语','总分']].agg(['max','min','mean'])
`
```

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__a4837066c13b411eb.png)

我们还可以**对不同的列进行不同的运算**（用字典形式指定）

```
`# 语文最高分、数学最低分和英文最高最低分
df.agg({'语文':['max'],'数学':'min','英语':['max','min']})
`
```

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__c544e8bbefe84c18b.png)

当然也支持自定义函数的调用

关于`agg`的更多技巧可以参考《[Pandas学习笔记05-分组与透视](http://mp.weixin.qq.com/s?__biz=MzUzODA5MjM5NQ==&mid=2247484320&idx=1&sn=dd0ecca958d3d96633db35b78217bf72&chksm=faddb5a4cdaa3cb28a50bd6b16def53eb1caa3581c696de9a2f1aa15f4378a05038afe7ad098&scene=21#wechat_redirect)》。

## 5\. pipe

以上四个调用函数的方法，我们发现被调用的函数的参数就是 `DataFrame`或`Serise`数据，如果我们被调用的函数**还需要别的参数**，那么该如何做呢？

所以，`pipe`就出现了。

pipe又称管道方法，可以将我们的处理分析过程标准化、流程化。它在调用函数的时候可以带被调用函数的其他参数，这样就方便自定义函数的功能扩展了。

比如，我们需要**获取总分大于n，性别为sex的同学的数据，其中n和sex是可变参数**，那么用`apply`等就不太好处理。这个时候，就可以用到`pipe`方法来搞事了！

我们先定义一个函数

```
`# 定义一个函数，总分大于等于n，性别为sex的同学数据（sex为2表示不分性别）
def total(df, n, sex):
    dfT = df.copy()
    if sex == 2:
        return dfT[(dfT['总分']>=n)]
    else:
        return dfT[(dfT['总分']>=n) & (dfT['性别']==sex)]
`
```

如果我们要找到**总分大于200，不分性别**的学生成绩，可以这样：

```
`df.pipe(total,200,2)
`
```

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__9761233304ab470fa.png)

再找**总分大于150，性别为男生**（1）的学生成绩，可以这样：

```
`df.pipe(total,150,1)
`
```

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__fea96c1480434d68b.png)

再找总分大于200，性别为女生（0）的学生成绩，可以这样：

```
`df.pipe(total,200,0)
`
```

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__28657045e5b34a4cb.png)

搞定！

以上就是本次我们介绍个5种调用函数的方法，这些操作技巧可以让我们在处理数据时更加灵活自如~~

长按👇关注\- 数据STUDIO - 设为星标，干货速递

<img width="578" height="194" src="../../../_resources/640_wx_fmt_gif_wxfrom_5_wx_lazy__d3ea5ae2f8ad43249.gif"/>

<img width="19" height="19" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__9f4fb9b4f15f44dbb.png"/>

分享

<img width="19" height="20" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__ba9dc048497846cfa.png"/>

收藏

<img width="19" height="19" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__15bf9cd891d54c3b8.png"/>

点赞

<img width="19" height="20" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__8e4832361c7e407db.png"/>

在看

People who liked this content also liked

字符数组转成字符串 Set接口实现类元素不能重复

大土转圈

不看的原因

- 内容质量低
- 不看此公众号

Jmeter——HTTP Request 一

3天时间

不看的原因

- 内容质量低
- 不看此公众号

某大学sql注入到getshell

天驿安全

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___894b63674cce4f4da.bmp"/>

Scan to Follow