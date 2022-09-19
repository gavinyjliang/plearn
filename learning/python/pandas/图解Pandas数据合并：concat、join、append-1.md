# 图解Pandas数据合并：concat、join、append

<a id="copyright_logo"></a>Original Peter <a id="profileBt"></a><a id="js_name"></a>尤而小屋 *2021-08-01 18:50*

收录于合集

<a id="js_article_tag_name__1999223975448477698"></a>#python <a id="js_article_tag_num__1999223975448477698"></a>80 <a id="js_article_tag_tips__1999223975448477698"></a>个

<a id="js_article_tag_name__1999223975666581509"></a>#数据分析 <a id="js_article_tag_num__1999223975666581509"></a>129 <a id="js_article_tag_tips__1999223975666581509"></a>个

<a id="js_article_tag_name__1999223977696624640"></a>#机器学习 <a id="js_article_tag_num__1999223977696624640"></a>96 <a id="js_article_tag_tips__1999223977696624640"></a>个

<a id="js_article_tag_name__1999223977226862596"></a>#pandas <a id="js_article_tag_num__1999223977226862596"></a>64 <a id="js_article_tag_tips__1999223977226862596"></a>个

> 公众号：尤而小屋
> 作者：Peter
> 编辑：Peter

大家好，我是Peter~

预告：吴某某事件持续发酵，明天继续吃瓜🍉

[滚！滚？](http://mp.weixin.qq.com/s?__biz=MzI4NjI3MzI2Ng==&mid=2247493205&idx=1&sn=0987053ab38fea908a19804bae765d4b&chksm=ebddddc8dcaa54de7dbc1eb8affbc45bc7bd1ba6d6e35d90eb6d53c4fb6e082ce731f0bdb8dc&scene=21#wechat_redirect)

## 图解pandas数据合并：concat+join+append

在上一篇文章中介绍过pandas中最为常用的一个合并函数merge的使用，本文中介绍的是另外3个与合并操作相关的函数：

- concat
    
- join
    
- append
    

[挑战SQL：图解Pandas的数据合并merge](http://mp.weixin.qq.com/s?__biz=MzI4NjI3MzI2Ng==&mid=2247493135&idx=1&sn=ff3c46e206fe19986506ca14805a0bb1&chksm=ebdddd92dcaa548442cc2599f731a42115d28b00c814a65f492b110b4e8d7e9cef9ea318a8ad&scene=21#wechat_redirect)

<img width="657" height="252" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_8191ab92addd48b09.jpg"/>

## Pandas连载

本文是Pandas数据分析库的第15篇，欢迎阅读：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 模拟数据

首先是模拟几份不同的数据：

```
`import pandas as pd
import numpy as np
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## concat

concat也是一个常用的合并函数，下面通过具体例子来介绍它的使用。

### 参数

```
`pandas.concat(objs,  # 合并对象
              axis=0,   # 合并方向，默认是0纵轴方向
              join='outer', # 合并取的是交集inner还是并集outer
              ignore_index=False, # 合并之后索引是否重新
              keys=None, # 在行索引的方向上带上原来数据的名字；主要是用于层次化索引，可以是任意的列表或者数组、元组数据或者列表数组
              levels=None, # 指定用作层次化索引各级别上的索引，如果是设置了keys
              names=None, # 行索引的名字，列表形式
              verify_integrity=False, # 检查行索引是否重复；有则报错
              sort=False, # 对非连接的轴进行排序
              copy=True   # 是否进行深拷贝
             )
`
```

### 默认情况

默认情况是直接在纵向上进行合并

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### axis

指定合并的方向

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

如果某个数据框中不存在，则会显示为NaN：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

- 根据实际数据调整合并的方向，默认是axis=0
    
- 某个数据库中不存在的数据，用NaN代替
    

### 参数ignore_index

是否保留原表索引，默认保留，为 True 会自动增加自然索引。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 参数join

指定取得交集inner还是并集outer，默认是并集outer

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

df3和df4只有地址这个字段是相同的，所以保留了它，其他的舍弃：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 参数keys

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

当我们设置了索引重排(ignore_index=True)，keys参数就无效啦

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 参数name

指定每个层级索引的名字

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

我们可以检查下df6的索引，发现是层级索引：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 合并多个DataFrame

同时合并df1、df2、df5

```
`pd.concat([pd.concat([df1,df2],axis=0,ignore_index=True),df5],axis=1)
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

分两步来实现：先合并df1、df2，将得到的结果和df5合并

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## join

### 参数

来自官网的参数说明：

```
`dataframe.join(other,  # 待合并的另一个数据框
        on=None,  # 连接的键
        how='left',   # 连接方式：‘left’, ‘right’, ‘outer’, ‘inner’ 默认是left
        lsuffix='',  # 左边（第一个）数据框相同键的后缀
        rsuffix='',  # 第二个数据框的键的后缀
        sort=False)  # 是否根据连接的键进行排序；默认False
`
```

### 模拟数据

为了解释join的操作，再模拟下数据：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 参数 lsuffix、rsuffix

功能是为了添加指定的后缀

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

如果不指定的话，会报错：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 参数how

how参数默认是left，保留左边的全部字段。右边不存在的数据用NaN

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

改成right之后，保留右边的全部数据：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

可以在默认的参数结果中，name字段被分成了name\_left和name\_right，如何进行字段的合并呢？？？

1.  先把键当做行索引
    
2.  通过join合并
    
3.  通过reset_index()重新设置索引
    

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

合并两个数据：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

最后进行索引重置的功能：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

还有一种更为简便的方法：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 合并多个DataFrame

利用join来实现多个DataFrame的合并：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

如果我们想要用merge函数来实现呢？

使用how="outer"，保留全部字段的数据信息

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## append

字面意思是“追加”。向dataframe对象中添加新的行，如果添加的列名不在dataframe对象中，将会被当作新的列进行添加

### 参数

```
`DataFrame.append(other, 
                 ignore_index=False, 
                 verify_integrity=False, 
                 sort=False)
`
```

参数解释：

- other：待合并的数据。可以是pandas中的DataFrame、series，或者是Python中的字典、列表这样的数据结构
    
- ignore_index：是否忽略原来的索引，生成新的自然数索引
    
- verify_integrity：默认是False，如果值为True，创建相同的index则会抛出异常的错误
    
- sort：boolean，默认是None。如果self和other的列没有对齐，则对列进行排序，并且属性只在版本0.23.0中出现。
    

### 模拟数据

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 添加不同类型数据

1、Python字典

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

2、Series类型

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

3、最常用的DataFrame

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 默认合并

df12和df13默认合并的结果：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 参数ignore_index

改变生成的索引值

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 参数verify_integrity

默认是False，如果值为True，创建相同的index则会抛出异常的错误

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 案例实战

假设现在一个excel表中有3个sheet：订单表、订单商品表、商品信息表：

1、订单表

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

2、订单商品表

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

3、商品信息表

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

现在我们通过合并函数将3个sheet中的内容关联起来：

```
`import pandas as pd
import numpy as np
# 读取订单表中的内容
df1 = pd.read_excel("水果订单商品信息3个表.xlsx",sheet_name=0)  # 第一个sheet的内容，索引从0开始
df1
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

```
`#  读取订单商品表
df2 = pd.read_excel("水果订单商品信息3个表.xlsx",sheet_name=1)
# 商品信息表
df3 = pd.read_excel("水果订单商品信息3个表.xlsx",sheet_name="商品信息")  # 可以直接指定sheet的名字name，不通过索引
df3
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

第一步：订单表和订单商品表的合并

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

第二步：将上面的结果和商品信息表合并

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

当我们得到上面的结果后，就可以完成很多的需求，举2个例子说明：

1、不同水果的销量和订单数：根据水果进行分组统计数量和订单数

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

2、不同区域的水果销售额和客户数

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 总结

两篇关于pandas数据合并的文章，详细介绍了4个函数：merge、concat、join、append的使用；其中，**merge和concat最为常用**。它们可以是实现SQL中join的功能。不管是交集、并集、还是左右连接，甚至是全连接都是可以直接实现的。

上面的实战案例数据是存放在一个Excel表中。在pandas中，我们可以从不同的来源：Excel、数据库、本地文件夹等获取来进行数据合并，方便后续实现我们的需求，希望本文对读者有所帮助。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

推荐阅读

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

[滚！滚？](http://mp.weixin.qq.com/s?__biz=MzI4NjI3MzI2Ng==&mid=2247493205&idx=1&sn=0987053ab38fea908a19804bae765d4b&chksm=ebddddc8dcaa54de7dbc1eb8affbc45bc7bd1ba6d6e35d90eb6d53c4fb6e082ce731f0bdb8dc&scene=21#wechat_redirect)

[挑战SQL：图解Pandas的数据合并merge](http://mp.weixin.qq.com/s?__biz=MzI4NjI3MzI2Ng==&mid=2247493135&idx=1&sn=ff3c46e206fe19986506ca14805a0bb1&chksm=ebdddd92dcaa548442cc2599f731a42115d28b00c814a65f492b110b4e8d7e9cef9ea318a8ad&scene=21#wechat_redirect)

[可视化神器Plotly玩转子图](http://mp.weixin.qq.com/s?__biz=MzI4NjI3MzI2Ng==&mid=2247492837&idx=1&sn=7013845b002325130f66e5f569f2d29d&chksm=ebdddf78dcaa566ea65b31c08193319c95bc085df6af272a54f8929e062979641745f1dc8f32&scene=21#wechat_redirect)

[数据分析师=7大主题，24份资料](http://mp.weixin.qq.com/s?__biz=MzI4NjI3MzI2Ng==&mid=2247492664&idx=1&sn=4136051d1be47a616d52fcb22ff49a49&chksm=ebdddfa5dcaa56b3eb370532daaf6deb74886599a8cc08e010bfddf0562ffedc0ce89f43f18f&scene=21#wechat_redirect)

[图解Pandas重复值处理](http://mp.weixin.qq.com/s?__biz=MzI4NjI3MzI2Ng==&mid=2247492519&idx=1&sn=c4974450fc9ea8b84106f65de6501028&chksm=ebddd83adcaa512c788ae612b0ebc6b4846ad43981aa00fa38a2c56710496651ae69ce180f1a&scene=21#wechat_redirect)

[图解Pandas的缺失值处理](http://mp.weixin.qq.com/s?__biz=MzI4NjI3MzI2Ng==&mid=2247492160&idx=1&sn=9315ad29a2130ebce8df256d21b70fe3&chksm=ebddd9dddcaa50cb7c3882fd4673ebde2598f13497d26c82e0de0e51bf20df2050ab3e8d31bb&scene=21#wechat_redirect)

> 尤而小屋，一个温馨的小屋。小屋主人，一手代码谋求生存，一手掌勺享受生活，欢迎你的光临

收录于合集 #<a id="js_album_keep_read_title"></a>python

 <a id="js_album_keep_read_size"></a>80个

<a id="js_album_prev"></a>上一篇 <a id="js_album_keep_read_pre_title"></a>Python入门-列表初相识 <a id="js_album_next"></a>下一篇 <a id="js_album_keep_read_next_title"></a>熬夜制作的瓜：8月份无疫烦！哈哈哈哈

People who liked this content also liked

妙呀！一行Python代码

...

尤而小屋

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___bfecba45980641809.bmp"/>

Scan to Follow