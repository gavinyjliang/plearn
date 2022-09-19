# Pandas数据类型操作

<a id="copyright_logo"></a>Original Peter <a id="profileBt"></a><a id="js_name"></a>尤而小屋 *2021-06-21 20:37*

收录于话题

<a id="js_article_tag_name__1999223975448477698"></a>#python <a id="js_article_tag_num__1999223975448477698"></a>57 <a id="js_article_tag_tips__1999223975448477698"></a>个

<a id="js_article_tag_name__1999223975666581509"></a>#数据分析 <a id="js_article_tag_num__1999223975666581509"></a>104 <a id="js_article_tag_tips__1999223975666581509"></a>个

<a id="js_article_tag_name__1999223977696624640"></a>#机器学习 <a id="js_article_tag_num__1999223977696624640"></a>72 <a id="js_article_tag_tips__1999223977696624640"></a>个

<a id="js_article_tag_name__2000726468170940417"></a>#数据分析师 <a id="js_article_tag_num__2000726468170940417"></a>42 <a id="js_article_tag_tips__2000726468170940417"></a>个

<a id="js_article_tag_name__1999223977226862596"></a>#pandas <a id="js_article_tag_num__1999223977226862596"></a>50 <a id="js_article_tag_tips__1999223977226862596"></a>个

大家好，我是Peter~

这是Pandas系列的第8篇连载文章：Pandas数据类型操作。

数据处理、分析等操作的首要操作是我们正确地设置了数据类型，笔者自己经常也会遇到事先没有处理好数据类型，而造成无法进行后续操作的困扰。

本文总结了Pandas中进行数据类型转换的三种基本方法，同时介绍了基于数据类型取数的方法：

- 使用astype()函数进行强制类型转换
    
- 通过自定义函数来进行数据类型转换
    
- 使用Pandas提供的函数如to\_numeric()、to\_datetime()等进行转化
    
- select_dtypes函数的使用
    

<img width="657" height="252" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_334f803c411c48c48.jpg"/>

## 一、Pandas、Python、Numpy各自支持的数据类型

下表中展示的是Pandas、Python和Numpy中支持的数据类型，可以看到pandas中支持的类型是最丰富的的。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 二、模拟数据

### 2.1 导入数据

下面是模拟的一份数据，包含多个字段名称

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

```
`import pandas as pd
import numpy as np
# 读取数据
df = pd.read_csv("数据类型操作.csv")
df
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 2.2 数据类型查看

查看数据的字段类型：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

```
`df.dtypes # 各字段的数据类型
df.team.dtype # 某个字段的类型
s.dtype # Series 的类型
df.dtypes.value_counts() # 各类型有多少个字段
`
```

## 三、实际案例

### 3.1 字符型转成数值型

比如我们想在数据上进行一些操作，比如将"2019年"、和"2020年"的数据相加：很明显数据不是我们想要的结果。

根本原因：这两个字段是字符类型，进行+操作，是直接将里面的内容拼接在一起，而不是里面数值的相加。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

正确的操作：

1、先把这两个字段中的数字单独提取出来

```
`# 分割之后取出第1个元素
df["2020年_新"] = df["2020年"].apply(lambda x:x.split("元")[0])
df["2019年_新"] = df["2019年"].apply(lambda x:x.split("元")[0])
df
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

2、查看数据类型

生成的两个新字段仍然是字符类型，不能直接相加

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

3、将数字表现型的字符型数据转成数值型

有两种方法实现这种需求：

- pd.astype("float") ：指定类型
    
- to_numeric()：直接转化
    

```
`##  字符类型的数值转成纯数值型
# 等价写法：df["2020年_新"] = df.astype({"2020年_新":"int")  字典形式传入
df["2020年_新"] = df["2020年_新"].astype("int")
df['2019年_新'] = pd.to_numeric(df['2019年_新'], errors='coerce')  
df
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

3、将两个新的字段相加

```
`df["前两年之和"] = df["2020年_新"] + df["2019年_新"]
df
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

求两个年份之间的差值：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 3.2 数值型转成字符型

求出2020年的增长率：

```
`df["增长率"] = df["前两年之差"] / df["2019年_新"]
df
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

现在整个增长率是float的数值型，我们想把它转成%的形式，也就是字符类型的数据：

```
`顾客姓名        object
顾客编码         int64
客户部门        object
客户组别       float64
2019年       object
2020年       object
日            int64
月            int64
年            int64
是否大客户       object
2020年_新      int64
2019年_新      int64
前两年之和        int64
前两年之差        int64
增长率        float64     # 数值型数据
dtype: object
`
```

在这里也是两种方法满足上面的需求：

- 方法1：通过str函数的转化
    
- 方法2：通过format函数的格式化输出
    

```
`df["增长率1"] = df["增长率"].apply(lambda x: str(round(100*x,2)) + "%")
df["增长率2"] = df["增长率"].apply(lambda x: format(x,'.2%'))
df
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

```
`顾客姓名        object
顾客编码         int64
客户部门        object
客户组别       float64
2019年       object
2020年       object
日            int64
月            int64
年            int64
是否大客户       object
2020年_新      int64
2019年_新      int64
前两年之和        int64
前两年之差        int64
增长率        float64
增长率1        object   #  两个字符类型的数据
增长率2        object
dtype: object
`
```

### 3.3 数值型数据存在缺失值

如果某个字段中大部分的数据都是数值型，但是存在少量的缺失值的情况，可以使用下面的方法进行转化：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

```
`df["客户组别"] = pd.to_numeric(df['客户组别'], errors='coerce').fillna(0)  # 未知的组用0代替；0可以换成其他数值
df
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 3.4 数值型类型转成时间类型

如果在实际数据中，我们遇到类似年、月、日等时间的数据，可以进行转化：比如我们想根据数据中的年月日生成一个生日的字段

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

1、上面的日、月、年现在是数值类型的数据，不能直接相加，先进行转化：

```
`df["月"] = df["月"].astype(str)
df["年"] = df["年"].astype(str)
`
```

2、转成字符型数据之后，再进行相加：

```
`df["生日"] = df["年"] + "-" + df["月"] + "-" + df["日"]
df
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

3、通过pd.to_datetime转成pandas中的时间类型数据

```
`df["生日"] = df["生日"].apply(lambda x: pd.to_datetime(x,format="%Y-%m-%d"))
df.dtypes
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

经过检验：如果字段是用英文表示的，下面的方法可以直接转成datetime64\[ns\]类型，使用中文汉字当做属性名的时候，该方法不适用。

Pandas中的to_datetime()函数可以把单独的year、month、day三列合并成一个单独的时间戳：

```
`pd.to_datetime(df[['year', 'month', 'day']]) # 组合成日期
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 3.5 布尔值判断使用

比如在是否为大客户中，我们想将Y换成True，N换成False，可以通过np.where来是实现：

```
`df["是否大客户"] = np.where(df["是否大客户"] == "Y", True, False)
df
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 3.6 读取文件直接转换

在使用pandas读取文件的时候，我们可以直接改变数据类型，使用参数是converters：

```
`df0 = pd.read_csv("数据类型操作.csv",
                  converters={
                    "顾客编码":str,  # 指定改变的函数
                    "2019年":lambda x:float(x.split("元")[0]),  # 切割函数
                    "2020年":lambda x:float(x.replace("元","")),  # 替换函数
                    "客户组别":lambda x: pd.to_numeric(x, errors='coerce'), 
                    "是否大客户":lambda x:np.where(x == "Y",True,False)
                  }
                 )
df0 
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 四、根据数据类型取数

我们看看df数据中存在的数据类型：object、int64、float64、bool、datetime64\[ns\]

```
`df.dtypes
# 结果
顾客姓名               object
顾客编码                int64
客户部门               object
客户组别              float64
2019年              object
2020年              object
日                  object
月                  object
年                  object
是否大客户                bool
2020年_新             int64
2019年_新             int64
前两年之和               int64
前两年之差               int64
增长率               float64
增长率1               object
增长率2               object
生日         datetime64[ns]
dtype: object
`
```

### 4.1 包含数据类型

```
`df.select_dtypes(include=["object"])  # 包含object类型的数据
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

也可以同时筛选包含多个数据类型：

```
`df.select_dtypes(include=["object","bool"])
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 4.2 不包含数据类型

```
`df.select_dtypes(exclude=["object"])   # 不包含object类型的数据
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

同时排除多个字段数据类型：

```
`df.select_dtypes(exclude=["object","bool"])   # 两个类型的数据同时排除
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 五、总结

对数据进行操作的第一步就是保证我们设置了正确的数据类型，然后才能进行后续的数据处理、数据分析、可视化等一系列的操作。不用的数据类型可以用不同的处理方法。

注意，一个列只能有一个总数据类型。本文中介绍了Pandas中常见的数据类型转化和基于数据类型取数的方法，希望对读者有所帮助。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

推荐阅读

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

[最后一篇：玩转Pandas取数](http://mp.weixin.qq.com/s?__biz=MzI4NjI3MzI2Ng==&mid=2247490519&idx=1&sn=7447e317984e06fdc479d054c6bac322&chksm=ebde204adca9a95cabf5d64c419c945792780178472b3cbf126488510ea8eddb7713e75740e5&scene=21#wechat_redirect)

[赞！五花八门的Pandas筛选数据](http://mp.weixin.qq.com/s?__biz=MzI4NjI3MzI2Ng==&mid=2247490346&idx=1&sn=8d120b0a63a51a6f65846c165c8da210&chksm=ebde20b7dca9a9a1578535bb80921b271b6e3deb5ac2d592cd628ade01e3bcc22d59b1dc5fe3&scene=21#wechat_redirect)

[各种骚气的Pandas取数操作](http://mp.weixin.qq.com/s?__biz=MzI4NjI3MzI2Ng==&mid=2247490184&idx=1&sn=6ad8f316354d07eabd2bf58fa1a3c8fd&chksm=ebde2115dca9a803b20533d225ce18bf10782715712acffc4d013758b5e62663f81ee11de56c&scene=21#wechat_redirect)

[创建DataFrame：10种方式任你选](http://mp.weixin.qq.com/s?__biz=MzI4NjI3MzI2Ng==&mid=2247489661&idx=1&sn=c8f2d2c30038801371c065720fe6460e&chksm=ebde23e0dca9aaf6b6d79d44b472cc933b7ad754bf75e3dcf116f75ffabb5314056155c6f3c0&scene=21#wechat_redirect)

> 尤而小屋，一个温馨的小屋。小屋主人，一手代码谋求生存，一手掌勺享受生活，欢迎你的光临

收录于话题 #<a id="js_album_keep_read_title"></a>python

 <a id="js_album_keep_read_size"></a>57个

<a id="js_album_prev"></a>上一篇 <a id="js_album_keep_read_pre_title"></a>从理论到实战：基于Python的用户增长Cohort分析 <a id="js_album_next"></a>下一篇 <a id="js_album_keep_read_next_title"></a>图解Pandas的排名rank机制

People who liked this content also liked

Python关联规则挖掘情侣、基友、渣男和狗

尤而小屋

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___295f823f29904c4a8.bmp"/>

Scan to Follow