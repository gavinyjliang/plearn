# Pandas三大利器-map、apply、applymap

<a id="profileBt"></a><a id="js_name"></a>早起Python *2022-02-25 10:10*

The following article is from 尤而小屋 Author Peter

<a id="copyright_info"></a>[![](../../../_resources/0_a989f7f90c844e59b9e5b8a128acde20.jpg)<br>**尤而小屋** .<br>尤而小屋，一个温馨且有爱的小屋🏡 小屋主人，一手代码谋求生存，一手掌勺享受生活，欢迎你的光临~](#)

实际工作中，我们在利用 `pandas`进行数据处理的时候，经常会对数据框中的单行、多行（列也适用）甚至是整个数据进行某种相同方式的处理，比如将数据中的 `sex`字段将 **男替换成1，女替换成0**。

在这个时候，很容易想到的是 `for`循环。用 `for`循环是一种很简单、直接的方式，但是运行效率很低。本文中介绍了 `pandas`中的三大利器： `map、apply、applymap` 来解决上述同样的需求。

- map
    
- apply
    
- applymap
    

— ***01***—

模拟数据

通过一个模拟的数据来说明3个函数的使用，在这个例子中学会了如何生成各种模拟数据。数据如下：

```


1.  `import pandas as pd`
    
2.  `import numpy as np`
    
3.  
4.  `boolean =  [True,  False]`
    
5.  `gender =  ["男","女"]`
    
6.  `color =  ["white","black","red"]`
    
7.  
8.  `# 好好学习如何生成模拟数据：非常棒的例子`
    
9.  `# 学会使用random模块中的randint方法`
    
10. 
11. `df = pd.DataFrame({"height":np.random.randint(160,190,100),`
    
12.  `"weight":np.random.randint(60,90,100),`
    
13.  `"smoker":[boolean[x]  for x in np.random.randint(0,2,100)],`
    
14.  `"gender":[gender[x]  for x in np.random.randint(0,2,100)],`
    
15.  `"age":np.random.randint(20,60,100),`
    
16.  `"color":[color[x]  for x in np.random.randint(0,len(color),100)]`
    
17.  `})`
    
18. `df.head()`
    


```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

— ***02***—

map

### demo

**map()** 会根据提供的函数对指定序列做映射。

第一个参数 function 以参数序列中的每一个元素调用 function 函数，返回包含每次 function 函数返回值的**新列表**。

```


1.  `map(function, iterable)`
    


```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 实际数据

将gender中男变成1，女变成0

```


1.  `# 方式1：通过字典映射实现`
    
2.  `dic =  {"男":1,  "女":0}  # 通过字典映射`
    
3.  `df1 = df.copy()  # 副本，不破坏原来的数据df`
    
4.  `df1["gender"]  = df1["gender"].map(dic)`
    
5.  `df1`
    
6.  
7.  `# 方式2：通过函数实现`
    
8.  `def map_gender(x):`
    
9.   `gender =  1  if x ==  "男"  else  0`
    
10.  `return gender`
    
11. 
12. `df2 = df.copy()`
    
13. `# 将df["gender"]这个S型数据中的每个数值传进去`
    
14. `df2["gender"]  = df2["gender"].map(map_gender)`
    
15. `df2`
    


```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

— ***03***—

apply

`apply`方法的作用原理和 `map`方法类似，区别在于 `apply`能够传入功能更为复杂的函数，可以说 `apply`是 `map`的高级版

pandas 的 `apply()` 函数可以作用于 `Series` 或者整个 `DataFrame`，功能也是自动遍历整个 `Series` 或者 `DataFrame`, 对每一个元素运行指定的函数。

在 `DataFrame`对象的大多数方法中，都会有 `axis`这个参数，它控制了你指定的操作是沿着0轴还是1轴进行。 `axis=0`代表操作对 `列columns`进行， `axis=1`代表操作对 `行row`进行

### demo

1.  上面的数据中将age字段的值都减去3，即加上-3
    

```


1.  `def apply_age(x,bias):`
    
2.   `return x + bias`
    
3.  
4.  `df4 = df.copy()`
    
5.  `# df4["age"]当做第一个值传给apply_age函数，args是第二个参数`
    
6.  `df4["age"]  = df4["age"].apply(apply_age,args=(-3,))`
    


```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

1.  计算BMI指数
    

```


1.  `# 实现计算BMI指数：体重/身高的平方(kg/m^2)`
    
2.  `def BMI(x):`
    
3.   `weight = x["weight"]`
    
4.   `height = x["height"]  /  100`
    
5.   `BMI = weight /  (height **2)`
    
6.  
7.   `return BMI`
    
8.  
9.  `df5 = df.copy()`
    
10. `df5["BMI"]  = df5.apply(BMI,axis=1)  # df5现在就相当于BMI函数中的参数x；axis=1表示在列上操作`
    
11. `df5`
    


```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

`DataFrame`型数据的 `apply`操作总结：

1.  当 `axis=0`时，对 `每列columns`执行指定函数；当 `axis=1`时，对 `每行row`执行指定函数。
    
2.  无论 `axis=0`还是 `axis=1`，其传入指定函数的默认形式均为 `Series`，可以通过设置 `raw=True`传入 `numpy数组`。
    
3.  对每个Series执行结果后，会将结果整合在一起返回（若想有返回值，定义函数时需要 `return`相应的值）
    

### apply实现需求

通过apply方法实现上面的性别转换需求。**apply方法中传进来的第一个参数一定是函数**

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

— ***04***—

applymap

### DF数据加1

applymap函数用于对DF型数据中的每个元素执行相同的函数操作，比如下面的加1：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 保留2位有效数字

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

****---------End**\-\-\-\-\-\-\-\-\-******

文末推荐一本**《OpenCV图像处理入门与实践》**，本书基于 Python，由浅入深、循序渐进地介绍了 OpenCV 从入门到实践的内容。本书共 13 章，内容涵盖OpenCV 基础知识、常见的图像操作、图像去噪、图像轮廓的提取与分析，以及人脸识别、目标追踪等计算机视觉的项目实战。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

最后还是老规矩，留言送 **3** 本，点赞前三将包邮获赠本书（时间截止3.4日）

People who liked this content also liked

《具体数学》 第七章 生成函数阅读总结

算法竞赛的日常

不看的原因

- 内容质量低
- 不看此公众号

R语言ggplot2 学习Nature communication的箱线图、小提琴图、点图混合的画法，以及绘图的实用技巧

R语言ggplot2科研绘图

不看的原因

- 内容质量低
- 不看此公众号

牛顿迭代法的可视化详解

深度学习初学者

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___0fd00a24527440468.bmp"/>

Scan to Follow