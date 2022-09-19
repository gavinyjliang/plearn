# Python入门：55个案例讲透列表的索引和切片！

<a id="copyright_logo"></a>Original Peter <a id="profileBt"></a><a id="js_name"></a>尤而小屋 *2021-08-16 09:00*

收录于话题

<a id="js_article_tag_name__2001714341183553538"></a>#Python入门 <a id="js_article_tag_num__2001714341183553538"></a>16 <a id="js_article_tag_tips__2001714341183553538"></a>个

<a id="js_article_tag_name__1999223975666581509"></a>#数据分析 <a id="js_article_tag_num__1999223975666581509"></a>104 <a id="js_article_tag_tips__1999223975666581509"></a>个

<a id="js_article_tag_name__1999223977696624640"></a>#机器学习 <a id="js_article_tag_num__1999223977696624640"></a>72 <a id="js_article_tag_tips__1999223977696624640"></a>个

![](../../../_resources/0_wx_fmt_png_54582c13fbd14fb39ddca16d6bb8902a.png)

**尤而小屋**

尤而小屋，一个温馨且有爱的小屋🏡 小屋主人，一手代码谋求生存，一手掌勺享受生活，欢迎你的光临~

<a id="js_profile_article"></a>216篇原创内容

Official Account

大家好，我是Peter~

在Python入门的上一篇文章中介绍了Python列表的各种操作和相关函数。本文介绍的Python列表中的索引和切片问题。

* * *

[Python入门-列表初相识](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247493104&idx=1&sn=9eeee899fbdfd4376b875f2f2bf79e62&chksm=cf12f52af8657c3c6eaa4cd0a0a1e8f9dcd2252f5af309eec93d925b5458f43c69e721dd1244&scene=21#wechat_redirect)

[55个案例：吃透Python字符串格式化](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247493094&idx=1&sn=e62f809024edad67a0bdfefa70405c5e&chksm=cf12f53cf8657c2a0eef022f32ea263b050d2e58ee3f170bcd1ac033c87d6c3f930c78ca95bd&scene=21#wechat_redirect)

[Python入门-字符串初相识](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247493092&idx=1&sn=d4c88b7ba2057d55efb4a6298e8ed635&chksm=cf12f53ef8657c28a8a9740aee25ca900c53b58831077d54849e7b4db9e804c29b146c5572a5&scene=21#wechat_redirect)

* * *

列表和之前介绍的数据类型字符串一样，都是有序的数据结构，存在索引和切片的概念。通过给定的索引号或者使用切片，我们就可以获取我们想要的数据。

在本文将会详细介绍Python中索引和切片的使用。

<img width="657" height="540" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_99a4eebe3ba744c6b.jpg"/>

## 一、索引

在python中，索引可正可负。正索引表示从左边的0开始，负索引表示从右边的-1开始。

在列表中，元素的索引表示的就是该元素在列表中的位置。

### 1.1查看数据信息

```
`# 给定一个列表
number = [-1,1,2,3,4,5,6,7,8,9,10,5,6,7,8,9]
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

- 索引左边从0开始，右边从-1开始
    
- 同一个元素有两种表示方法
    

```
`type(number)  # 查看数据类型为列表
`
```

结果为list列表类型

```
list

```

查看内存地址，使用id函数；

```
`id(number)  # 查看列表的内存地址
`
```

```
4600162736

```

查看列表的长度：

```
`len(number)  # 查看列表的长度
`
```

```
16

```

### 1.2指定索引号

```
`number[0]  # 第一个数据
`
```

```
-1

```

```
`number[-16]  # 倒过来数
`
```

```
-1

```

倒数第16个数也是-1，因为刚好长度是16

```
`number[-1]  # 最后的数据
`
```

```
9

```

```
`number[3]
`
```

```
3

```

```
`number[-4]
`
```

```
6

```

如果指定的索引号超过了列表的长度，则会报错：

```
`number[18]  # 超出长度则会报错
`
```

```
---------------------------------------------------------------------------
IndexError                                Traceback (most recent call last)
<ipython-input-10-983efb3d1bf6> in <module>
----> 1 number[18]  # 超出长度则会报错
IndexError: list index out of range

```

### 1.3index函数

index函数是用来查找某个元素在列表中出现的第一个索引位置。

在上面创建的列表中，部分元素是重复的，比如56789，我们使用index来查看它们的位置：

```
`number.index(-1) 
`
```

```
0

```

```
`number.index(6)  # 多次出现的话，只显示第一次出现的索引位置
`
```

```
6

```

```
`number.index(7)
`
```

```
7

```

```
`number.index(9)
`
```

```
9

```

index函数还可以指定查找的起止索引位置：index(x,start,stop)

- x：查找的对象。
    
- start：可选，查找的起始位置。
    
- end：可选，查找的结束位置。
    

```
`number.index(7,8,16)  # 查找7的第一个位置；从索引8开始到16
`
```

```
13

```

```
`number.index(9,13,16)
`
```

```
15

```

## 二、切片

### 2.1切片规则

list\[start:stop:step\]，其中：

- start表示开始的索引位置（包含）；如果不写，表示从头开始切
    
- stop表示结束的位置（不包含）；如果不写，表示切片操作执行到末尾
    
- step表示步长，可正可负；如果不写，默认为1
    

### 2.2正索引

```
`number   # 原列表
`
```

```
[-1, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 5, 6, 7, 8, 9]

```

```
`len(number)  # 列表长度为16
`
```

```
16

```

```
`# 1、默认步长1
# # 不包含索引11位置的数据
number[0:11]  
`
```

```
[-1, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

```

```
`# 2、改变步长
number[0:15:2]
`
```

```
[-1, 2, 4, 6, 8, 10, 6, 8]

```

```
`number[0:15:4]
`
```

```
[-1, 4, 8, 6]

```

```
`# 3、不写起始位置
number[:15]  # 从索引为0开始
`
```

```
[-1, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 5, 6, 7, 8]

```

```
`number[:15:3]  # 从索引0开始且步长为3
`
```

```
[-1, 3, 6, 9, 6]

```

```
`# 4、不写结束位置
number[0::]  # 切到末尾
`
```

```
[-1, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 5, 6, 7, 8, 9]

```

```
`number[1::2]  # 从索引1开始，步长为2
`
```

```
[1, 3, 5, 7, 9, 5, 7, 9]

```

```
`number[1::4]  # 步长改成4
`
```

```
[1, 5, 9, 7]

```

```
`# 5、不写开头和结束位置
number[::]  
`
```

```
[-1, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 5, 6, 7, 8, 9]

```

这样的写法得到的结果原列表相同，相当于是复制了一份

```
`number[::3]  # 步长为3
`
```

```
[-1, 3, 6, 9, 6, 9]

```

### 2.3负索引

```
`# 1、默认步长
number[-16:-2:]
`
```

```
[-1, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 5, 6, 7]

```

```
`#  2、改变步长
number[-16:-2:3]
`
```

```
[-1, 3, 6, 9, 6]

```

```
`number[-10:-2:2]  # 包含-10的位置，不包含-2的位置，步长为2
`
```

```
[6, 8, 10, 6]

```

```
`# 3、不指定开头
number[:-2:2]
`
```

```
[-1, 2, 4, 6, 8, 10, 6]

```

```
`# 4、不指定结束
number[-16::3]
`
```

```
[-1, 3, 6, 9, 6, 9]

```

### 2.4同时使用正负索引

```
`number[-16:9:]  # -16的位置其实就是开头的元素位置，不包含索引9的位置
`
```

```
[-1, 1, 2, 3, 4, 5, 6, 7, 8]

```

```
`number[-15:13:2]  # 改变步长
`
```

```
[1, 3, 5, 7, 9, 5]

```

```
`number[3:-3:]
`
```

```
[3, 4, 5, 6, 7, 8, 9, 10, 5, 6]

```

```
`number[4:-1:2]
`
```

```
[4, 6, 8, 10, 6, 8]

```

### 2.5使用负步长

上面的切片操作中步长都是整数，或者默认的1，现在我们改成负数作为索引。

当使用负步长的时候，start必须大于等于stop的索引

```
`number[::-2]  # 反过来切，步长-2
`
```

```
[9, 7, 5, 9, 7, 5, 3, 1]

```

```
`number[-3:-16:-3]
`
```

```
[7, 10, 7, 4, 1]

```

```
`number[-2::-2]
`
```

```
[8, 6, 10, 8, 6, 4, 2, -1]

```

```
`number[:-15:-3]  # 从最后一个元素（索引-1），步长为-3切到-15的位置
`
```

```
[9, 6, 9, 6, 3]

```

```
`number[15:4:-3]
`
```

```
[9, 6, 9, 6]

```

## 三、反转列表

通过将步长设置成-1，即可反转整个列表

```
`number[::-1]  # 步长设置为-1
`
```

```
[9, 8, 7, 6, 5, 10, 9, 8, 7, 6, 5, 4, 3, 2, 1, -1]

```

## 四、切片赋值

```
`number   # 原数据
`
```

```
[-1, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 5, 6, 7, 8, 9]

```

```
`number[10:]
`
```

```
[10, 5, 6, 7, 8, 9]

```

```
`id(number)
`
```

```
4600162736

```

我们现在改变索引从10开始之后的所有值：

```
`number[10:] = [20,25,30,35,40,45]
`
```

```
`number
`
```

```
[-1, 1, 2, 3, 4, 5, 6, 7, 8, 9, 20, 25, 30, 35, 40, 45]

```

```
`id(number)  # 改变了数据内存地址仍不变
`
```

```
4600162736

```

## 五、删除切片数据

通过del关键字来删除列表中一部分数据；删除列表中的部分数据不改变其在内存的地址

```
`number
`
```

```
[-1, 1, 2, 3, 4, 5, 6, 7, 8, 9, 20, 25, 30, 35, 40, 45]

```

```
`number[12:]
`
```

```
[30, 35, 40, 45]

```

```
`id(number)
`
```

```
4600162736

```

```
`del number[12:]  # 直接原地删除，不生成新的数据
`
```

```
`number  # 少了删除的部分
`
```

```
[-1, 1, 2, 3, 4, 5, 6, 7, 8, 9, 20, 25]

```

```
`id(number)  # 内存地址不变
`
```

列表的内存地址并没有变化

```
4600162736
```

<img width="17" height="36" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__a0df187f05b14095a.png"/>

推荐阅读

<img width="17" height="36" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__09e390fd9ae34f1f9.png"/>

[开启Pandas进阶：图解Pandas透视表、交叉表](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247493182&idx=1&sn=9b37f4aee2cfe3ae52f90ceff9296b0a&chksm=cf12f6e4f8657ff27e45a212d2e29e6733cc37f347828bdf12c7573ae3f6d56d484a300c8833&scene=21#wechat_redirect)

[27000字，103天，16篇文章！](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247493112&idx=1&sn=2c153674d25f43175bd2dccfb71e99dd&chksm=cf12f522f8657c34ec09f6805c3a1f14b04fccd46b002c2390395a19332230ffce73e9dd49dc&scene=21#wechat_redirect)

[图解Pandas的轴旋转函数：stack和unstack](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247493111&idx=1&sn=e3c785fc786c061d36675d1bd596ad38&chksm=cf12f52df8657c3b0ac8927717234607008da17c63128f47031eadc9b2fe98d85070b292fa2e&scene=21#wechat_redirect)

[挑战SQL：图解Pandas的数据合并merge](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247493106&idx=1&sn=7aefa2284d6db3bf2cceedf5db177aaf&chksm=cf12f528f8657c3ee9065f6d6d33b70790be38c833bc0b52b4856609384847a007a3ca0d484d&scene=21#wechat_redirect)

[Python入门-列表初相识](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247493104&idx=1&sn=9eeee899fbdfd4376b875f2f2bf79e62&chksm=cf12f52af8657c3c6eaa4cd0a0a1e8f9dcd2252f5af309eec93d925b5458f43c69e721dd1244&scene=21#wechat_redirect)

> 尤而小屋，一个温馨的小屋。小屋主人，一手代码谋求生存，一手掌勺享受生活，欢迎你的光临

![](../../../_resources/0_wx_fmt_png_54582c13fbd14fb39ddca16d6bb8902a.png)

**尤而小屋**

尤而小屋，一个温馨且有爱的小屋🏡 小屋主人，一手代码谋求生存，一手掌勺享受生活，欢迎你的光临~

<a id="js_profile_article"></a>216篇原创内容

Official Account

收录于话题 #<a id="js_album_keep_read_title"></a>Python入门

 <a id="js_album_keep_read_size"></a>16个

<a id="js_album_prev"></a>上一篇 <a id="js_album_keep_read_pre_title"></a>Python入门：4000字能把元组tuple讲透吗？ <a id="js_album_next"></a>下一篇 <a id="js_album_keep_read_next_title"></a>Python入门-列表初相识

People who liked this content also liked

Python关联规则挖掘情侣、基友、渣男和狗

尤而小屋

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___86b38ebe76214c20a.bmp"/>

Scan to Follow