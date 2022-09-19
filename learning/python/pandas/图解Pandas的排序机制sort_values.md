# 图解Pandas的排序机制sort_values

<a id="copyright_logo"></a>Original Peter <a id="profileBt"></a><a id="js_name"></a>尤而小屋 *2021-07-03 19:08*

收录于话题

<a id="js_article_tag_name__1999223975448477698"></a>#python <a id="js_article_tag_num__1999223975448477698"></a>57 <a id="js_article_tag_tips__1999223975448477698"></a>个

<a id="js_article_tag_name__1999223975666581509"></a>#数据分析 <a id="js_article_tag_num__1999223975666581509"></a>104 <a id="js_article_tag_tips__1999223975666581509"></a>个

<a id="js_article_tag_name__1999223977696624640"></a>#机器学习 <a id="js_article_tag_num__1999223977696624640"></a>72 <a id="js_article_tag_tips__1999223977696624640"></a>个

<a id="js_article_tag_name__2000726468170940417"></a>#数据分析师 <a id="js_article_tag_num__2000726468170940417"></a>42 <a id="js_article_tag_tips__2000726468170940417"></a>个

<a id="js_article_tag_name__1999223977226862596"></a>#pandas <a id="js_article_tag_num__1999223977226862596"></a>50 <a id="js_article_tag_tips__1999223977226862596"></a>个

> 公众号：尤而小屋
> 作者：Peter
> 编辑：Peter

大家好，我是Peter~

在上一篇pandas的文章中已经介绍排名机制rank函数的使用。其实在实现排名的过程，已经顺带实现了排序的功能；但是pandas中还有一个重要的方法来解决排序问题：sort_values。

<img width="657" height="252" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_99219afcbf934ccdb.jpg"/>

## 一、Pandas连载

Pandas文章已经形成连载，前10篇文章分别是：

<img width="657" height="699" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_a2381a1f964348fea.jpg"/>

## 二、参数解释

```
`DataFrame.sort_values(by, 
               axis=0, 
               ascending=True, 
               inplace=False, 
               kind='quicksort', 
               na_position='last', # last，first；默认是last
               ignore_index=False, 
               key=None)
`
```

参数的具体解释为：

- by：表示根据什么字段或者索引进行排序，可以是一个或多个
    
- axis：排序是在横轴还是纵轴，默认是纵轴axis=0
    
- ascending：排序结果是升序还是降序，默认是升序
    
- inplace：表示排序的结果是直接在原数据上的就地修改还是生成新的DatFrame
    
- kind：表示使用排序的算法，快排quicksort,，归并mergesort， 堆排序heapsort，稳定排序stable ，默认是 ：快排quicksort
    
- na_position：缺失值的位置处理，默认是最后，另一个选择是首位
    
- ignore_index：新生成的数据帧的索引是否重排，默认False（采用原数据的索引）
    
- key：排序之前使用的函数
    

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 三、参数案例说明

下面会通过不同的案例来说明各个参数的使用方法和含义，模拟的第一份数据如下，里面有一个缺失值：

```
`df = pd.DataFrame({
    '姓名': ['Tom', 'Peter', 'Jack', 'Jim', 'Mike', 'Nob'],
    '语文': [112, 112, 131, 98, 117, 124],
    '数学': [127, 139, 100, 133, 122, 114],
    '外语': [139, 88, 140, np.nan, 122, 114],   # 存在缺失值
    '住址': ['深圳', '广州', '深圳', '珠海', '广州', '东莞']}
)
df  
`
```

<img width="657" height="363" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_4ee4f3701264491e8.jpg"/>

### by

by参数是必需的，也是最简单的参数：我们必须指定按照哪个列属性或者行索引来排序，可以是一个或多个，by关键字可以省略。

```
`df.sort_values(by="数学")   
df.sort_values(by=["语文","数学"])  # 多个字段的排序
`
```

### axis

axis参数表示的是在哪个方向上排序，axis=0纵轴，axis=1横轴，看一份新的模拟数据：

```
`data = pd.DataFrame({'a':[7,4,6,3],
                     'b':[4,3,2,1],
                     'c':[8,9,5,1]}) 
data
# 结果
  a b c
0 7 4 8
1 4 3 9
2 6 2 5
3 3 1 1
`
```

<img width="657" height="518" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_129f6db29df24a999.jpg"/>

### inplace

当inplace=True的时候，原数据在排序之后会直接修改：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

<img width="657" height="557" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_ff4c8082d5d74cd08.jpg"/>

### ascending

可以是一个或者多个字段的排序，通过列表的形式指定：

```
`df.sort_values(by="数学",ascending=False)   # 一个字段排序
df.sort_values(by=["语文","数学"],  # 多个字段的不同排序方式
               ascending=[True,False]
              )  
`
```

<img width="657" height="327" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_fd9694b282cc49c78.jpg"/>

### na_position

缺失值的位置处理：默认是最后，也可以放到最前面：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

上面默认是在末尾。也可以放在首位：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### ignore_index

表示新生成的数据中的索引是采用原数据的索引还是新生成的0,1,2,3…...

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### key

表示在排序之前使用的函数：

> Apply the key function to the values before sorting. This is similar to the key argument in the builtin `sorted()` function, with the notable difference that this key function should be *vectorized*. It should expect a `Series` and return a Series with the same shape as the input. It will be applied to each column in by independently.

为了解释key参数，我们再模拟一份数据：

```
`data1 = pd.DataFrame({
    'col1': [2, 1, 9, 8, 7, 4],
    'col2': [0, 1, 9, 4, 2, 3],
    'col3': ['a', 'e', 'F', 'B', 'c', 'D']
})
data1
`
```

<img width="657" height="405" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_6f20ffa7bb7140e68.jpg"/>

<img width="657" height="545" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_3a05ec97d3a14fe99.jpg"/>

上面的例子很好地显示了参数key的使用，解释下上面两行代码的运行结果，我们对col3字段排序：

- 默认情况下，字母是按照它们对应的ASCII码进行比较的(A-65,a-97)；所以升序的结果就是：BDFace
    
- 加上了key参数，我们写了一个匿名函数lambda，作用是将col3中的字符串全部变成小写字母，这样升序自然是aBcDeF，因为此时的BDF变成了bdf
    

## 总结

排序sort_values函数在平时使用的频率是非常高的，经常需要对销售数据做TopN分析。它能够很快地运用于电商领域，包含TopN的销售额、用户、商品、销售员业绩等；不同学科的排名，学生的成绩等诸多场景，希望对读者有所帮助。

<img width="10" height="10" src="../../../_resources/640_wx_fmt_gif_wxfrom_5_wx_lazy__bb6e91f573f949b7b.gif"/>

推荐阅读

<img width="14" height="14" src="../../../_resources/640_wx_fmt_gif_wxfrom_5_wx_lazy__77cf0a2df5474995b.gif"/>

[图解Pandas的排名rank机制](http://mp.weixin.qq.com/s?__biz=MzI4NjI3MzI2Ng==&mid=2247492004&idx=1&sn=d0602ee2d71f3a23417209cb8da519ec&chksm=ebddda39dcaa532f1fa6e9f10d067ad2de0997d31a31ca104f017caf74168186883e70eac4e8&scene=21#wechat_redirect)

[图解Pandas的groupby机制](http://mp.weixin.qq.com/s?__biz=MzI4NjI3MzI2Ng==&mid=2247491765&idx=1&sn=e63fe83db79cdb8b74f403c458a135cc&chksm=ebdddb28dcaa523e3e9392a792776fc2f2a511a0d2e546014ee2e32d753c120f2aac79aed9da&scene=21#wechat_redirect)

[Pandas数据类型操作](http://mp.weixin.qq.com/s?__biz=MzI4NjI3MzI2Ng==&mid=2247491606&idx=1&sn=71ca0fdd7a24b9dd34125c7995df6e98&chksm=ebdddb8bdcaa529d3ac4a32f4393dbb5a792f7134fda628c5ce778c6416a724f719d48965428&scene=21#wechat_redirect)

[数据处理基石：数据探索](http://mp.weixin.qq.com/s?__biz=MzI4NjI3MzI2Ng==&mid=2247491098&idx=1&sn=5bc104d393b8725c857142ac24c2550a&chksm=ebde2587dca9ac919f11c5d7fc153a261f42b9de8af3e66603efaf9ec8c0bd6c33812a7663a8&scene=21#wechat_redirect)

> 尤而小屋，一个温馨的小屋。小屋主人，一手代码谋求生存，一手掌勺享受生活，欢迎你的光临

收录于话题 #<a id="js_album_keep_read_title"></a>python

 <a id="js_album_keep_read_size"></a>57个

<a id="js_album_prev"></a>上一篇 <a id="js_album_keep_read_pre_title"></a>图解Pandas的排名rank机制 <a id="js_album_next"></a>下一篇 <a id="js_album_keep_read_next_title"></a>图解Pandas的缺失值处理

People who liked this content also liked

Python关联规则挖掘情侣、基友、渣男和狗

尤而小屋

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___93e33cc9e899447cb.bmp"/>

Scan to Follow