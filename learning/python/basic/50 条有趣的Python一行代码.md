# 50 条有趣的Python一行代码

<a id="profileBt"></a><a id="js_name"></a>数据STUDIO *2021-11-26 08:35*

The following article is from 法纳斯特 Author 小F

<a id="copyright_info"></a>[![](../../../_resources/0_cf5abbf2191a4eba9edeb401e6bb5058.jpg)<br>**法纳斯特** .<br>分享学习Python爬虫、数据分析、数据挖掘的点滴。](#)

大家好，我是云朵君！**关注和星标**『数据STUDIO』，和云朵君一起学习数据分析与挖掘！

![](../../../_resources/0_wx_fmt_png_15ae7ae032614f45acd6b401cbbbf95e.png)

**数据STUDIO**

点击领取《Python学习手册》，后台回复「福利」获取。『数据STUDIO』专注于数据科学原创文章分享，内容以 Python 为核心语言，涵盖机器学习、数据分析、可视化、MySQL等领域干货知识总结及实战项目。

<a id="js_profile_article"></a>173篇原创内容

Official Account

在学习Python的过程中，总会发现Python能够轻易的解决许多问题。

一些复杂的任务，甚至可以使用一行Python代码就能搞定。

下面，云朵君给大家介绍50个有趣的Python一行代码，都很实用。希望大家能从中找到对自己有帮助的技巧。

▍1、字母异位词

两个单词如果包含相同的字母，次序不同，则称为字母易位词(anagram)。

例如，“silent”和“listen”是字母易位词，而“apple”和“aplee”不是易位词。

```


from collections import Counter
s1 = 'below'
s2 = 'elbow'
print('anagram') if Counter(s1) == Counter(s2) else print('not an anagram')



```

使用一行Python代码，就能判断出来了。

▍2、二进制转十进制

```


decimal = int('1010', 2)
print(decimal) #10



```

▍3、将字符串转换为小写

```


print("Hi my name is XiaoF".lower())
# 'hi my name is xiaof'
print("Hi my name is XiaoF".casefold())
# 'hi my name is xiaof'



```

▍4、将字符串转换为大写

```


print("hi my name is XiaoF".upper())
# 'HI MY NAME IS XIAOF'



```

▍5、将字符串转换为字节

```


print("convert string to bytes using encode method".encode())
# b'convert string to bytes using encode method'



```

▍6、拷贝文件

```


import shutil
shutil.copyfile('source.txt', 'dest.txt')



```

▍7、快速排序

```


qsort = lambda l: l if len(l) <= 1 else qsort([x for x in l[1:] if x < l[0]]) + [l[0]] + qsort([x for x in l[1:] if x >= l[0]])
print(qsort([17, 29, 11, 97, 103, 5]))
# [5, 11, 17, 29, 97, 103]



```

▍8、n个连续数的和

```


n = 10
print(sum(range(0, n+1)))
# 55



```

▍9、交换两个变量的值

```


a,b = b,a



```

▍10、斐波纳契数列

```


fib = lambda x: x if x<=1 else fib(x-1) + fib(x-2)
print(fib(20))
# 6765



```

▍11、将嵌套列表合并为一个列表

```


main_list = [[0, 1, 2], [11, 12, 13], [52, 53, 54]]
result = [item for sublist in main_list for item in sublist]
print(result)
>
[0, 1, 2, 11, 12, 13, 52, 53, 54]



```

▍12、运行一个HTTP服务器

```


python3 -m http.server 8000
python2 -m SimpleHTTPServer



```

▍13、反转列表

```


numbers = [0, 1, 2, 11, 12, 13, 52, 53, 54]
print(numbers[::-1])
# [54, 53, 52, 13, 12, 11, 2, 1, 0]



```

▍14、阶乘

```


import math
fact_5 = math.factorial(5)
print(fact_5)
# 120



```

▍15、在列表推导式中使用for和if

```


even_list = [number for number in [1, 2, 3, 4] if number % 2 == 0]
print(even_list)
# [2, 4]



```

▍16、列表中最长的字符串

```


words = ['This', 'is', 'a', 'list', 'of', 'words']
result = max(words, key=len)
print(result)
# 'words'



```

▍17、列表推导式

```


li = [num for num in range(0, 10)]
print(li)
# [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]



```

▍18、集合推导式

```


num_set = {num for num in range(0, 10)}
print(num_set)
# {0, 1, 2, 3, 4, 5, 6, 7, 8, 9}



```

▍19、字典推导式

```


dict_numbers = {x: x*x for x in range(1, 5)}
print(dict_numbers)
# {1: 1, 2: 4, 3: 9, 4: 16}



```

▍20、if-else

```


print("even") if 4 % 2==0 else print("odd")



```

▍21、无限循环

```


while 1:0



```

▍22、检查数据类型

```


print(isinstance(2, int))
# True
print(isinstance("allwin", str))
# True
print(isinstance([3, 4, 1997], list))
# True



```

▍23、While循环

```


a = 5
while a > 0:
    a = a - 1
print(a)
# 0



```

▍24、使用print语句写入文件

```


print("Hello, World!", file=open('file.txt', 'w'))



```

既可打印出信息，还能将信息保存文件。

▍25、计算一个字符在字符串中出现的频率

```


print("umbrella".count('l'))
# 2



```

▍26、合并列表

```


list1 = [1, 2, 4]
list2 = ['XiaoF']
list1.extend(list2)
print(list1)
# [1, 2, 4, 'XiaoF']



```

▍27、合并字典

```


dict1 = {'name': 'weiwei', 'age': 23}
dict2 = {'city': 'Beijing'}
dict1.update(dict2)
print(dict1)
# {'name': 'weiwei', 'age': 23, 'city': 'Beijing'}



```

▍28、合并集合

```


set1 = {0, 1, 2}
set2 = {11, 12, 13}
set1.update(set2)
print(set1)
# {0, 1, 2, 11, 12, 13}



```

▍29、时间戳

```


import time
print(time.time())



```

▍30、列表中出现次数最多的元素

```


test_list = [9, 4, 5, 4, 4, 5, 9, 5, 4]
most_frequent_element = max(set(test_list), key=test_list.count)
print(most_frequent_element)
# 4



```

▍31、嵌套列表

```


numbers = [[num] for num in range(10)]
print(numbers)
# [[0], [1], [2], [3], [4], [5], [6], [7], [8], [9]]



```

▍32、八进制转十进制

```


print(int('30', 8)) 
# 24



```

▍33、将键值对转换为字典

```


result = dict(name='XiaoF', age=23)
print(result)
# {'name': 'XiaoF', 'age': 23}


```

▍34、求商和余数

```


quotient, remainder = divmod(4, 5)
print(quotient, remainder)
# 0 4



```

divmod()函数返回当参数1除以参数2时，包含商和余数的元组。

▍35、删除列表中的重复项

```


print(list(set([4, 4, 5, 5, 6])))
# [4, 5, 6]



```

▍36、按升序排序列表

```


print(sorted([5, 2, 9, 1]))
# [1, 2, 5, 9]



```

▍37、按降序排序列表

```


print(sorted([5, 2, 9, 1], reverse=True))
# [9, 5, 2, 1]



```

▍38、获取小写字母表

```


import string
print(string.ascii_lowercase)
# abcdefghijklmnopqrstuvwxyz



```

▍39、获取大写字母表

```


import string
print(string.ascii_uppercase)
# ABCDEFGHIJKLMNOPQRSTUVWXYZ



```

▍40、获取0到9字符串

```


import string
print(string.digits)
# 0123456789



```

▍41、十六进制转十进制

```


print(int('da9', 16))
# 3497



```

▍42、日期时间

```


import time
print(time.ctime())
# Thu Aug 13 20:00:00 2021



```

▍43、将列表中的字符串转换为整数

```


print(list(map(int, ['1', '2', '3'])))
# [1, 2, 3]



```

▍44、用键对字典进行排序

```


d = {'one': 1, 'four': 4, 'eight': 8}
result = {key: d[key] for key in sorted(d.keys())}
print(result)
# {'eight': 8, 'four': 4, 'one': 1}



```

▍45、用键值对字典进行排序

```


x = {1: 2, 3: 4, 4: 3, 2: 1, 0: 0}
result = {k: v for k, v in sorted(x.items(), key=lambda item: item[1])}
print(result)
# {0: 0, 2: 1, 1: 2, 4: 3, 3: 4}



```

▍46、列表旋转

```


li = [1, 2, 3, 4, 5]
# li[n:] + li[:n], 右变左
print(li[2:] + li[:2])
# [3, 4, 5, 1, 2]
# li[-n:] + li[:-n], 左变右
print(li[-1:] + li[:-1])
# [5, 1, 2, 3, 4]



```

▍47、将字符串中的数字移除

```


message = ''.join(list(filter(lambda x: x.isalpha(), 'abc123def4fg56vcg2')))
print(message)
# abcdeffgvcg



```

▍48、矩阵变换

```


old_list = [[1, 2, 3], [3, 4, 6], [5, 6, 7]]
result = list(list(x) for x in zip(*old_list))
print(result)
# [[1, 3, 5], [2, 4, 6], [3, 6, 7]]



```

▍49、列表过滤

```


result = list(filter(lambda x: x % 2 == 0, [1, 2, 3, 4, 5, 6]))
print(result)
# [2, 4, 6]



```

▍50、解包

```


a, *b, c = [1, 2, 3, 4, 5]
print(a) # 1
print(b) # [2, 3, 4]
print(c) # 5



```

往期推荐 点击查看

[数据科学家提高效率的 40 个 Python 技巧](http://mp.weixin.qq.com/s?__biz=Mzk0OTI1OTQ2MQ==&mid=2247506024&idx=1&sn=a311f30c7a7a96c50fdf6de48d3db463&chksm=c35985c2f42e0cd475eeea307e61028641654101f8c9575f9e6a759c571fc978a8ea3da67c5c&scene=21#wechat_redirect)

[Python Bokeh 库进行数据可视化实用指南](http://mp.weixin.qq.com/s?__biz=Mzk0OTI1OTQ2MQ==&mid=2247506011&idx=1&sn=fc2f26fa115a8d9c234af124d4f004ba&chksm=c35985f1f42e0ce741bd2870dd039718e3ca548540d8542125a14ee8fee8107e83b74c38d059&scene=21#wechat_redirect)

[数据分析最有用的25个Matplotlib图](http://mp.weixin.qq.com/s?__biz=Mzk0OTI1OTQ2MQ==&mid=2247505997&idx=2&sn=2a53b914c9e5f080dc57db3856c47a97&chksm=c35985e7f42e0cf10d3114dd41f11918e1b0f7ff5e905046ddaedcc5887a0fa787e1bcc60d3e&scene=21#wechat_redirect)

[精选 15 个顶级 Python 库](http://mp.weixin.qq.com/s?__biz=Mzk0OTI1OTQ2MQ==&mid=2247505824&idx=2&sn=e10b8206f7ad8ca87a67ce91c1690757&chksm=c3598a0af42e031c355b94b83b8b287c39759644a7f71ca09f3965c1d1c65f7410532b3d79e3&scene=21#wechat_redirect)

[学习Python的 11 个顶级Github库](http://mp.weixin.qq.com/s?__biz=Mzk0OTI1OTQ2MQ==&mid=2247505698&idx=2&sn=80047589984b3374d35c06e309b4557a&chksm=c3598a88f42e039e8123efe877fde647f0a195faa466bd68c8d6ae2e8fd87b96040b736435b1&scene=21#wechat_redirect)

[24 个好用到爆的 Python 实用技巧！](http://mp.weixin.qq.com/s?__biz=Mzk0OTI1OTQ2MQ==&mid=2247505087&idx=1&sn=2c17139efd3e7103e088fe3103e90337&chksm=c3598915f42e0003b566dfd65865969b9ce663445587729790418c96231168dbbf6c4f3dd460&scene=21#wechat_redirect)

[21 分钟 MySQL 入门教程完整版](http://mp.weixin.qq.com/s?__biz=Mzk0OTI1OTQ2MQ==&mid=2247504752&idx=2&sn=391ed23365a9616822784231998aa4a8&chksm=c3598edaf42e07cc28f0f6553008fb7fd6574b8ca05c13af52b871878d5f4533505c1f2b594a&scene=21#wechat_redirect)

[68 个 Python 内置函数详解](http://mp.weixin.qq.com/s?__biz=Mzk0OTI1OTQ2MQ==&mid=2247503551&idx=2&sn=1ff63e062e893999cafda88cc4751ec4&chksm=c3599315f42e1a035be08c0ef25cc59be905cc213a306f1c891c62b14472c0701af48b728ba8&scene=21#wechat_redirect)

[过瘾！100道Python入门练习题](http://mp.weixin.qq.com/s?__biz=Mzk0OTI1OTQ2MQ==&mid=2247501855&idx=2&sn=c603f9a7e25517847f6ac3993659505b&chksm=c35995b5f42e1ca3c2c1613c26f7288bd220cfb435f32c24a26e94aea787bdc8563a83ac2ecf&scene=21#wechat_redirect)

[SQL语法速成手册](http://mp.weixin.qq.com/s?__biz=Mzk0OTI1OTQ2MQ==&mid=2247501356&idx=1&sn=6cb818c95355374e75b16ab9dc3d596f&chksm=c3599b86f42e1290ec9cb863efce40d2af9866d62a412661fd5ef6c138cec0f2412d9d4354d3&scene=21#wechat_redirect)

[Seaborn 绘制 21 种超实用精美图表](http://mp.weixin.qq.com/s?__biz=Mzk0OTI1OTQ2MQ==&mid=2247499555&idx=1&sn=9f4e46d4c9c563fdd559f354dfc5f34e&chksm=c359a289f42e2b9f31df218e43175503a4e2b11aeb05646f4f91267c82b3310cdc34e656a86e&scene=21#wechat_redirect)

长按👇关注\- 数据STUDIO - 设为星标，干货速递

<img width="677" height="227" src="../../../_resources/640_wx_fmt_gif_wxfrom_5_wx_lazy__64a785f48001419ab.gif"/>

<a id="js_view_source"></a>Read more

People who liked this content also liked

SpringBoot-20-Mybatis代码生成

springboot葵花宝典

不看的原因

- 内容质量低
- 不看此公众号

10行python代码做出哪些酷炫的事情？

信息安全修炼手册

不看的原因

- 内容质量低
- 不看此公众号

使用简单的python代码生成复杂的数学公式

GitHub开源指北

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___3907a46448bb43458.bmp"/>

Scan to Follow