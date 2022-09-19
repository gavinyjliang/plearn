# Python标准库UUID(生成唯一标识)

<a id="profileBt"></a><a id="js_name"></a>数据STUDIO *2022-05-23 08:35* *Posted on <a id="js_ip_wording"></a>四川*

![Image](../../../_resources/640_wx_fmt_gif_wxfrom_5_wx_lazy__dae2ef4ea01e4177b.gif)

<a id="js_appmsg_search_keywords_head"></a>![](../../../_resources/0_wx_fmt_png_d0ff2634478d41b2b919f3bf811ed2e7.png)<a id="appmsg_search_keywords_nickname"></a>数据STUDIO<a id="appmsg_search_keywords_title"></a>推荐搜索<a id="appmsg_search_keywords_tips"></a>关键词列表：<a id="appmsg_search_keywords_keywords_list"></a>Python数据挖掘数据可视化SQL

<img width="520" height="79" src="../../../_resources/640_wx_fmt_gif_wxfrom_5_wx_lazy__e476c35bbac349619.gif"/>

##### 本文中，介绍一个常用的python标准库，用于生成唯一标识，一个非常常见的一种方法，没啥特别的，就是给大家扫个盲，希望能够帮助到大家！

## UUID是什么

**UUID：** 通用唯一标识符 ( Universally Unique Identifier )，对于所有的UUID它可以保证在空间和时间上的唯一性，也称为GUID，全称为：

- UUID —— Universally Unique IDentifier Python中称为 UUID
    
- GUID —— Globally Unique IDentifier   C#中称为 GUID
    

它是通过MAC地址、 时间戳、 命名空间、 随机数、 伪随机数来保证生成ID的唯一性,，有着固定的大小 ( 128 bit位 )，通常由 32 字节的字符串（十六进制）表示。

它的唯一性和一致性特点，使得可以无需注册过程就能够产生一个新的UUID；UUID可以被用作多种用途, 既可以用来短时间内标记一个对象，也可以可靠的辨别网络中的持久性对象。

## UUID有什么用

很多应用场景需要一个id，但是又不要求这个id 有具体的意义，仅仅用来标识一个对象。常见的用处有数据库表的id字段；另一个例子是前端的各种UI库，因为它们通常需要动态创建各种UI元素，这些元素需要唯一的id， 这时候就需要使用UUID了。

例如：一个网站在存储视频、图片等格式的文件时，这些文件的命名方式就可以采用 UUID生成的随机标识符，避免重名的出现。

### UUID模块提供的UUID类和函数

python的uuid模块提供的UUID类和函数`uuid1()，uuid3()，uuid4()，uuid5()` 来生成1, 3, 4, 5各个版本的UUID ( 需要注意的是：python中没有uuid2()这个函数)。

对uuid模块中最常用的几个函数总结如下:

### uuid.uuid1()

`uuid.uuid1([node[, clock_seq]])` \-\- 基于时间戳

由 MAC 地址（主机物理地址）、当前时间戳、随机数生成。可以保证全球范围内的唯一性， 但 MAC 的使用同时带来安全性问题，局域网中可以使用 IP 来代替MAC。

该函数有两个参数, 如果 node 参数未指定, 系统将会自动调用 getnode() 函数来获取主机的硬件地址. 如果 clock_seq 参数未指定系统会使用一个随机产生的14位序列号来代替.

注意：uuid1() 返回的不是普通的字符串，而是一个 uuid 对象，其内含有丰富的成员函数和变量。

### uuid.uuid2()

`uuid.uuid2()` \-\- 基于分布式计算环境DCE（Python中没有这个函数）

算法与uuid1相同，不同的是把时间戳的前 4 位置换为 POSIX 的 UID。实际中很少用到该方法。

### uuid.uuid3()

`uuid.uuid3(namespace, name)` \-\- 基于名字的MD5散列值

通过计算名字和命名空间的MD5散列值得到，保证了同一命名空间中不同名字的唯一性， 和不同命名空间的唯一性，但同一命名空间的同一名字生成相同的uuid。

### uuid.uuid4()

`uuid.uuid4()` \-\- 基于随机数

由伪随机数得到，有一定的重复概率，该概率可以计算出来。

### uuid.uuid5()

`uuid.uuid5()` \-\- 基于名字的SHA-1散列值

算法与uuid3相同，不同的是使用 Secure Hash Algorithm 1 算法。

### 上述几个函数的使用方法：

```
# 记得关注和星标 @公众号：数据STUDIO import uuid # 导入UUID模块
# make a UUID based on the host ID and current time
uuid.uuid1()
UUID('a8098c1a-f86e-11da-bd1a-00112444be1e')
# make a UUID using an MD5 hash of a namespace UUID and a name
uuid.uuid3(uuid.NAMESPACE_DNS, 'python.org')
UUID('6fa459ea-ee8a-3ca4-894e-db77e160355e')
# make a random UUID
uuid.uuid4()
UUID('16fd2706-8baf-433b-82eb-8c7fada847da')
# make a UUID using a SHA-1 hash of a namespace UUID and a name
uuid.uuid5(uuid.NAMESPACE_DNS, 'python.org')
UUID('886313e1-3b8a-5372-9b90-0c9aee199e5d')
# make a UUID from a string of hex digits (braces and hyphens ignored)
x = uuid.UUID('{00010203-0405-0607-0809-0a0b0c0d0e0f}')
# convert a UUID to a string of hex digits in standard form
str(x)
'00010203-0405-0607-0809-0a0b0c0d0e0f'
# get the raw 16 bytes of the UUID
x.bytes
'\x00\x01\x02\x03\x04\x05\x06\x07\x08\t\n\x0b\x0c\r\x0e\x0f'
# make a UUID from a 16-byte string
uuid.UUID(bytes=x.bytes)
UUID('00010203-0405-0607-0809-0a0b0c0d0e0f')

```

### 实例

- 首先，Python中没有基于 DCE 的，所以uuid2可以忽略；
    
- 其次，uuid4存在概率性重复，由无映射性，最好不用；
    
- 再次，若在Global的分布式计算环境下，最好用uuid1；
    
- 最后，若有名字的唯一性要求，最好用uuid3或uuid5。
    

```
import uuid
name = "test_name"
namespace = "test_namespace"
print uuid.uuid1()  # 带参的方法参见Python Doc
print uuid.uuid3(namespace, name)
print uuid.uuid4()
print uuid.uuid5(namespace, name)

```

> uuid模块文档：https://docs.python.org/2/library/uuid.html
> 原文链接：https://www.cnblogs.com/hellojesson/p/6410445.html
> 编辑：@公众号：数据STUDIO

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

##### 🏴‍☠️宝藏级🏴‍☠️ 原创公众号『**数据STUDIO**』内容超级硬核。公众号以Python为核心语言，垂直于数据科学领域，包括可戳👉[**Python**](https://mp.weixin.qq.com/mp/appmsgalbum?__biz=Mzk0OTI1OTQ2MQ==&action=getalbum&album_id=1974978822768771072&scene=173&from_msgid=2247519294&from_itemidx=1&count=3&nolastread=1#wechat_redirect)**｜**[**MySQL**](https://mp.weixin.qq.com/mp/appmsgalbum?__biz=Mzk0OTI1OTQ2MQ==&action=getalbum&album_id=2023684574089658370&scene=173&from_msgid=2247519619&from_itemidx=2&count=3&nolastread=1#wechat_redirect)**｜**[**数据分析**](https://mp.weixin.qq.com/mp/appmsgalbum?__biz=Mzk0OTI1OTQ2MQ==&action=getalbum&album_id=1974978820940054530&scene=173&from_msgid=2247518366&from_itemidx=1&count=3&nolastread=1#wechat_redirect)**｜**[**数据可视化**](https://mp.weixin.qq.com/mp/appmsgalbum?__biz=Mzk0OTI1OTQ2MQ==&action=getalbum&album_id=1974991176839544834&scene=173&from_msgid=2247519244&from_itemidx=1&count=3&nolastread=1#wechat_redirect)**｜**[**机器学习与数据挖掘**](https://mp.weixin.qq.com/mp/appmsgalbum?__biz=Mzk0OTI1OTQ2MQ==&action=getalbum&album_id=1963494160565354497&scene=173&from_msgid=2247512171&from_itemidx=1&count=3&nolastread=1#wechat_redirect)**｜**[**爬虫**](https://mp.weixin.qq.com/mp/appmsgalbum?__biz=Mzk0OTI1OTQ2MQ==&action=getalbum&album_id=2318258648965644288&scene=173&from_msgid=2247518366&from_itemidx=1&count=3&nolastread=1#wechat_redirect) 等，从入门到进阶！

长按👇关注\- 数据STUDIO -设为星标，干货速递

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

People who liked this content also liked

推荐七个Python效率工具

...

Python丹卿

不看的原因

- 内容质量低
- 不看此公众号

Python高级用法，代码精进! (文末赠书）

...

尤而小屋

不看的原因

- 内容质量低
- 不看此公众号

21张让你Python代码能力突飞猛进的速查表

...

Python丹卿

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___43fa5838daff404bb.bmp"/>

Scan to Follow