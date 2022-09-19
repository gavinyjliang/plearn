# 强化版的 requests，这个库真牛 x

<a id="profileBt"></a><a id="js_name"></a>Python数据之道 *2021-09-05 11:01*

![](../../../_resources/0_wx_fmt_png_8c91e5e0e429482d9826db12593d2f65.png)

**Python数据之道**

点击领取《Python知识手册》高清电子版，回复数字 “600” 获取。「Python数据之道」秉承“让数据更有价值”的理念​，聚焦于 Python 数据分析、数据可视化、AI、机器学习、深度学习等领域。

<a id="js_profile_article"></a>235篇原创内容

Official Account

最近公司 Python 后端项目进行重构，整个后端逻辑基本都变更为采用"异步"协程的方式实现。看着满屏幕经过 async await（协程在 Python 中的实现）修饰的代码，我顿时感到一脸懵逼，不知所措。

虽然之前有了解过"协程"是什么东西，但并没有深入探索，于是正好借着这次机会可以好好学习一下。

###  什么是协程？

简单来说，协程是一种基于线程之上，但又比线程更加轻量级的存在。对于系统内核来说，协程具有不可见的特性，所以这种由 程序员自己写程序来管理的轻量级线程又常被称作 "**用户空间线程**"。

###  协程比多线程好在哪呢？

1.  线程的控制权在操作系统手中，而 **协程的控制权完全掌握在用户自己手中**，因此利用协程可以减少程序运行时的上下文切换，有效提高程序运行效率。
    
2.  建立线程时，系统默认分配给线程的 栈 大小是 1 M，而协程更轻量，接近 1 K 。因此可以在相同的内存中开启更多的协程。
    
3.  由于协程的本质不是多线程而是单线程，所以不需要多线程的锁机制。因为只有一个线程，也不存在同时写变量而引起的冲突。在协程中控制共享资源不需要加锁，只需要判断状态即可。所以协程的执行效率比多线程高很多，同时也有效避免了多线程中的竞争关系。
    

###  协程的适用 & 不适用场景

**适用场景：协程适用于被阻塞的，且需要大量并发的场景。**

不适用场景：协程不适用于存在大量计算的场景（因为协程的本质是单线程来回切换），如果遇到这种情况，还是应该使用其他手段去解决。

###  初探异步 http 框架 httpx

至此我们对 "协程" 应该有了个大概的了解，但故事说到这里，相信有朋友还是满脸疑问："协程" 对于接口测试有什么帮助呢？不要着急，答案就在下面。

相信用过 Python 做接口测试的朋友都对 requests 库不陌生。requests 中实现的 http 请求是同步请求，**但其实基于 http 请求 IO 阻塞的特性，非常适合用协程来实现 "异步" http 请求从而提升测试效率。**

相信早就有人注意到了这点，于是在 Github 经过了一番探索后，果不其然，最终寻找到了支持协程 "异步" 调用 http 的开源库: **httpx**

###  什么是 httpx

httpx 是一个几乎继承了所有 requests 的特性并且支持 "异步" http 请求的开源库。简单来说，可以认为 httpx 是强化版 requests。

下面大家可以跟着我一起见识一下 httpx 的强大

###  安装

httpx 的安装非常简单，在 Python 3.6 以上的环境执行

```


pip install httpx



```

###  最佳实践

俗话说得好，效率决定成败。我分别使用了 httpx 异步 和 同步 的方式对批量 http 请求进行了耗时比较，来一起看看结果吧～

首先来看看同步 http 请求的耗时表现：

```


import asyncio
import httpx
import threading
import time
def sync_main(url, sign):
    response = httpx.get(url).status_code
    print(f'sync_main: {threading.current_thread()}: {sign}2 + 1{response}')
sync_start = time.time()
[sync_main(url='http://www.baidu.com', sign=i) for i in range(200)]
sync_end = time.time()
print(sync_end - sync_start)



```

代码比较简单，可以看到在 sync_main 中则实现了同步 http 访问百度 200 次。

运行后输出如下（截取了部分关键输出…）：

```


sync_main: <_MainThread(MainThread, started 4471512512)>: 192: 200
sync_main: <_MainThread(MainThread, started 4471512512)>: 193: 200
sync_main: <_MainThread(MainThread, started 4471512512)>: 194: 200
sync_main: <_MainThread(MainThread, started 4471512512)>: 195: 200
sync_main: <_MainThread(MainThread, started 4471512512)>: 196: 200
sync_main: <_MainThread(MainThread, started 4471512512)>: 197: 200
sync_main: <_MainThread(MainThread, started 4471512512)>: 198: 200
sync_main: <_MainThread(MainThread, started 4471512512)>: 199: 200
16.56578803062439



```

可以看到在上面的输出中, 主线程没有进行切换（因为本来就是单线程啊喂！）请求按照顺序执行（因为是同步请求）。

程序运行共耗时 **16.6 秒**

下面我们试试 "异步" http 请求：

```


import asyncio
import httpx
import threading
import time
client = httpx.AsyncClient()
async def async_main(url, sign):
    response = await client.get(url)
    status_code = response.status_code
    print(f'async_main: {threading.current_thread()}: {sign}:{status_code}')
loop = asyncio.get_event_loop()
tasks = [async_main(url='http://www.baidu.com', sign=i) for i in range(200)]
async_start = time.time()
loop.run_until_complete(asyncio.wait(tasks))
async_end = time.time()
loop.close()
print(async_end - async_start)



```

上述代码在 async_main 中用 async await 关键字实现了"异步" http，通过 asyncio ( 异步 io 库请求百度首页 200 次并打印出了耗时。

运行代码后可以看到如下输出（截取了部分关键输出…）

```


async_main: <_MainThread(MainThread, started 4471512512)>: 56: 200
async_main: <_MainThread(MainThread, started 4471512512)>: 99: 200
async_main: <_MainThread(MainThread, started 4471512512)>: 67: 200
async_main: <_MainThread(MainThread, started 4471512512)>: 93: 200
async_main: <_MainThread(MainThread, started 4471512512)>: 125: 200
async_main: <_MainThread(MainThread, started 4471512512)>: 193: 200
async_main: <_MainThread(MainThread, started 4471512512)>: 100: 200
4.518340110778809



```

可以看到顺序虽然是乱的（56，99，67…） (这是因为程序在协程间不停切换) 但是主线程并没有切换 （协程本质还是单线程 ）。

程序共耗时 **4.5 秒**

比起同步请求耗时的 16.6 秒 缩短了接近 73 %！

俗话说得好，**一步快，步步快。** 在耗时方面，"异步" http 确实比同步 http 快了很多。当然，"协程" 不仅仅能在请求效率方面赋能接口测试， 掌握 "协程"后，相信小伙伴们的技术水平也能提升一个台阶，从而设计出更优秀的测试框架。

**---------End---------**

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 精选资料

回复关键词，获取对应的资料：

| 关键词 | 资料名称 |
| --- | --- |
| **600** | [《Python知识手册》](https://mp.weixin.qq.com/mp/appmsgalbum?__biz=MzI2NjY5NzI0NA==&action=getalbum&album_id=1370549534602133504#wechat_redirect) |
| **md** | [《Markdown速查表》](https://mp.weixin.qq.com/s?__biz=MzI2NjY5NzI0NA==&mid=2247495631&idx=1&sn=720c931b1f5cdb82ceccd9b34b182a95&scene=21#wechat_redirect) |
| **time** | [《Python时间使用指南》](https://mp.weixin.qq.com/s?__biz=MzI2NjY5NzI0NA==&mid=2247492370&idx=1&sn=1dc6b3edef0fcb241d07757fb9e2ae03&scene=21#wechat_redirect) |
| **str** | [《Python字符串速查表》](https://mp.weixin.qq.com/s?__biz=MzI2NjY5NzI0NA==&mid=2247496716&idx=1&sn=8ec7a6b373059fa49f04433990581aa6&scene=21#wechat_redirect) |
| **pip** | [《Python：Pip速查表》](https://mp.weixin.qq.com/s?__biz=MzI2NjY5NzI0NA==&mid=2247500405&idx=1&sn=c5a760279babd1075c6153858af84af8&scene=21#wechat_redirect) |
| **style** | [《Pandas表格样式配置指南》](https://mp.weixin.qq.com/s?__biz=MzI2NjY5NzI0NA==&mid=2247501026&idx=1&sn=378292e5435b7ef5eede36192812da3b&scene=21#wechat_redirect) |
| **mat** | [《Matplotlib入门100个案例》](https://mp.weixin.qq.com/s?__biz=MzI2NjY5NzI0NA==&mid=2247493274&idx=1&sn=323662b49f3b8e0d619d44315537a76a&scene=21#wechat_redirect) |
| **px** | [《Plotly Express可视化指南》](https://mp.weixin.qq.com/s?__biz=MzI2NjY5NzI0NA==&mid=2247501349&idx=1&sn=04502491758816e83d43525e911cce56&scene=21#wechat_redirect) |

### 精选内容

- [神器 VS Code，超详细Python配置使用指南](https://mp.weixin.qq.com/s?__biz=MzI2NjY5NzI0NA==&mid=2247495918&idx=1&sn=6b06eadc1604b693f48a127e7b5986a4&scene=21#wechat_redirect)
    
- [神器Tushare，财经数据必备工具！](https://mp.weixin.qq.com/s?__biz=MzI2NjY5NzI0NA==&mid=2247497358&idx=1&sn=e9574b69ca3c05142edd534e438e855e&scene=21#wechat_redirect)
    
- [Matplotlib 可视化最有价值的 50 个图表](https://mp.weixin.qq.com/s?__biz=MzI2NjY5NzI0NA==&mid=2247485238&idx=1&sn=f2d6b9136ff94697bdce9c9f48397e78&scene=21#wechat_redirect)
    
- [视频：Plotly 和 Dash 在投资领域的应用](https://mp.weixin.qq.com/s?__biz=MzI2NjY5NzI0NA==&mid=2247496637&idx=1&sn=91910ea5f9034fc6685d28ef0f00a70c&scene=21#wechat_redirect)
    

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

People who liked this content also liked

一个悄然成为世界最流行的操作系统诞生！

Python数据之道

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___92b7ae67815f4f22b.bmp"/>

Scan to Follow