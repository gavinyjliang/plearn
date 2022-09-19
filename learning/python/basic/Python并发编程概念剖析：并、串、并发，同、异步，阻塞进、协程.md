# Python并发编程概念剖析：并、串、并发，同、异步，阻塞进、协程

<a id="profileBt"></a><a id="js_name"></a>Python编程大全 *2022-01-26 19:19*

↓推荐关注↓

![](../../../_resources/0_wx_fmt_png_a3940c40f24d41dab638ea52686700e0.png)

**Python编程大全**

分享Python技术文章，实用案例，热点资讯。 你想了解的Python的那些事都在这里...... 当你的才华还撑不起你的野心的时候，那就安静下来学习吧！

<a id="js_profile_article"></a>6篇原创内容

Official Account

<img width="578" height="356" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_e1ff4542e5774434b.jpg"/>

1 引言

并发、并行、串行、同步、异步、阻塞、非阻塞、进程、线程、协程是并发编程中的常见概念，相似却也有却不尽相同，令人头痛，这一篇博文中我们来区分一下这些概念。

2 并发与并行

在解释并发与并行之前，我们必须先明确：单个处理器（一个单核CPU）在某一个时刻只能处理一个线程 。

并发是指在同一个处理器上通过时间片轮转的方式在多个线程之间频繁切换，由于切换速度极快，所以看似多个线程似乎被同时执行，但实际上每一个时刻都只有一个线程被执行，其他的线程出于阻塞状态。

并行是指多个处理器在同一时刻同时处理了多个不同的线程，这才是真正意义的同时被执行。

如下图所示，线程A与线程B同在一个CPU内执行，且任一t时刻内，都只有一个线程（A或者B）被执行，所以线程A与线程B是并发执行的。线程C和线程D分别在两个CPU内执行，且在某一个t时刻内同时都在执行，所以线程C和线程D是并行的。

![Image](../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_897f653de8af4f2c8.jpg)

3 并行与串行

上面已经说到，并行是指多个任务同时执行，而 串行 是指多个任务时，各个任务按顺序执行，完成一个之后才能进行下一个。

![Image](../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_3170b4d127044edc8.jpg)

所以，并发与并行是在某一时刻是否都在执行的区别。并行与串行是同时进行或一个结束才进行下一个的区别。

4 同步与异步

同步与异步的概念与消息的通知机制有关：

同步是指线程在访问某一资源时，获得了资源的返回结果之后才会执行其他操作，否则主动继续获取这一资源；

异步与同步相对，是指线程在访问某一资源时，无论是否取得返回结果，都进行下一步操作；当有了资源返回结果时，系统自会通知线程。

```
用一个比喻来说明：10多前的银行是没有业务取号的，我们去办理业务时，如果有很多人，那就先排队，然后关注着什么时候轮到自己，这就是同步；现在去银行，得先取一张小纸条，上面写着你的
业务号，轮到你的时候，银行会喊你，这就是异步。异步机制往往注册一个回调机制,在所等待的事件被触发时由触发机制(银行柜台业务员)通过某种机制(业务办理号码)找到等待该事件的人。

```

5 阻塞与非阻塞

阻塞是与非阻塞都是程序的一种运行状态。

线程在等待某个操作完成期间，自身无法继续执行别的操作，则称该线程在该操作上是 阻塞的。

线程在等待某个操作完成期间，自身可执行别的操作，则称该线程在该操作上是 非阻塞 的。

```
继续上面银行办理业务的例子，无论是10多年前的排队办理业务，还是现在的业务号办理业务，如果在我们在等待过程中，什么也不能做，那就是阻塞的；如果在等待过程中，可以做其他事情（看书
、玩手游），那就是非阻塞的。

```

同步和异步是个线程处理方式或手段，阻塞和非阻塞是线程的一种状态，两者并不相同也并不冲突。

同步、异步与阻塞非阻塞可以产生不同的组合：同步阻塞、同步非阻塞、异步阻塞、异步非阻塞。

```
还是银行办理业务的例子：如果排着队，且只能傻傻的排着队，看着什么时候到自己，那就是同步阻塞；如果排着队还能玩玩手机，偶尔抬头看看什么时候到自己，那就是同步非阻塞。如果是现在的
取票按业务号办理业务，拿到号码后就陷入懵逼状态，啥也不能做，直到银行根据业务号通知自己，那就是异步阻塞；如果拿到业务号之后，自己爱干嘛干嘛，那就是异步非阻塞。

```

6 进程 线程 协程

6.1 基本概念

进程是具有一定独立功能的程序关于某个数据集合上的一次运行活动,进程是系统进行资源分配和调度的一个独立单位，是资源（内存）分配的最小单位。每个进程都有自己的独立内存空间，不同进程通过进程间通信来通信。由于进程比较重量，占据独立的内存，所以上下文进程间的切换开销（栈、寄存器、虚拟内存、文件句柄等）比较大，但相对比较稳定安全。

线程是进程的一个实体,是CPU调度和分派的基本单位,它是比进程更小的能独立运行的基本单位.线程自己基本上不拥有系统资源,只拥有一点在运行中必不可少的资源(如程序计数器,一组寄存器和栈),但是它可与同属一个进程的其他的线程共享进程所拥有的全部资源。线程间通信主要通过共享内存，上下文切换很快，资源开销较少，但相比进程不够稳定容易丢失数据。

协程是一种用户态的轻量级线程，协程的调度完全由用户控制。协程拥有自己的寄存器上下文和栈。协程调度切换时，将寄存器上下文和栈保存到其他地方，在切回来的时候，恢复先前保存的寄存器上下文和栈，直接操作栈则基本没有内核切换的开销，可以不加锁的访问全局变量，所以上下文的切换非常快。

6.2 进程与线程

线程是指进程内的一个执行单元,也是进程内的可调度实体。线程与进程的区别:

1) 地址空间:线程是进程内的一个执行单元，进程内至少有一个线程，它们共享进程的地址空间，而进程有自己独立的地址空间；

2) 资源拥有:进程是资源分配和拥有的单位,同一个进程内的线程共享进程的资源；

3) 线程是处理器调度的基本单位,但进程不是；

4) 二者均可并发执行

5) 每个独立的线程有一个程序运行的入口、顺序执行序列和程序的出口，但是线程不能够独立执行，必须依存在应用程序中，由应用程序提供多个线程执行控制。

6.3 协程多与线程进行比较

1) 一个线程可以多个协程，一个进程也可以单独拥有多个协程，这样python中则能使用多核CPU。

2) 线程进程都是同步机制，而协程则是异步

3) 协程能保留上一次调用时的状态，每次过程重入时，就相当于进入上一次调用的状态。

*声明：本文于网络整理，版权归原作者所有，如来源信息有误或侵犯权益，请联系我们删除或授权事宜。

*关注： Python编程大全 ，加星标精彩内容不迷路*

```


![Image](https://mmbiz.qpic.cn/mmbiz_gif/b96CibCt70iabwjyojLhA03PtxUnkNPREnt2F48ywfXLpDdDAjicOTPI8Q94tVLbJ58tbRs12iaXDKhUOW9gd4NlFA/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

如果喜欢我的文章，那么

“在看”和转发是对我最大的支持！

```


![Image](../../../_resources/640_wx_fmt_gif_wxfrom_5_wx_lazy__301812e129d84f72b.gif)

 （戳下面蓝字阅读）

- [Python 的十大特性](http://mp.weixin.qq.com/s?__biz=Mzg5NzcxMzA0NA==&mid=2247483803&idx=1&sn=7c7a6b9bdd5d4506d49d583dc7b9a52e&chksm=c06cd760f71b5e7690e1ee8092c59a24609c71fdaee13789edfac2e1f29c9cbb46c9d16b6d35&scene=21#wechat_redirect)
    
- [30 个 Python 代码实现的常用功能](http://mp.weixin.qq.com/s?__biz=Mzg5NzcxMzA0NA==&mid=2247484085&idx=1&sn=b00838903213cb1dc72d3bc01e27b113&chksm=c06cd44ef71b5d582710d50abf1f4cb5b28962b5696a555f544ec8ea5510b541adacf58ed31b&scene=21#wechat_redirect)
    
- [终于把所有的Python库，都整理出来啦！](http://mp.weixin.qq.com/s?__biz=Mzg5NzcxMzA0NA==&mid=2247484001&idx=1&sn=9be186c81f09819c519f9e5a3cdf8666&chksm=c06cd49af71b5d8cf87a165fa0e615538a60fd487b2587fcb4ee6f3f8131f09634940f1f0b41&scene=21#wechat_redirect)
    
- [两年撸代码总结出的21个Python高效代码，学到了很省事！](http://mp.weixin.qq.com/s?__biz=Mzg5NzcxMzA0NA==&mid=2247484206&idx=1&sn=32b16d17dbe761f0afa62a1282f589ce&chksm=c06cd5d5f71b5cc34081a210cd390f6ee19162c5892626dcb5f491188d38858c86c51a180269&scene=21#wechat_redirect)
    
- [Python中使用构造方法创建对象](http://mp.weixin.qq.com/s?__biz=Mzg5NzcxMzA0NA==&mid=2247483871&idx=1&sn=85cacff5a50fd77aa71be4fa8544fd1f&chksm=c06cd724f71b5e321e503fa1d69ce53f53afe0bd2c3b84ba52e98f79504bdea479c7ce9bc4a5&scene=21#wechat_redirect)
    
- [一些著名的软件都用什么语言编写？](http://mp.weixin.qq.com/s?__biz=Mzg5NzcxMzA0NA==&mid=2247483934&idx=1&sn=1a67d7e67521bd719dde09d9e02bc068&chksm=c06cd4e5f71b5df3857ede19be1627c5e3176be13224cd03ab9d30868c86040a1f066aa45978&scene=21#wechat_redirect)
    
- [Python超车，C#错失年度编程语言](http://mp.weixin.qq.com/s?__biz=Mzg5NzcxMzA0NA==&mid=2247483887&idx=1&sn=c9c39d8a239f405017ba8eac1665fc9b&chksm=c06cd714f71b5e0233062e2a1722dd95a49cf3f4da9bec3b96d0bbe07e50a1b8096b2dd3f077&scene=21#wechat_redirect)
    
- [Python再获年度编程语言，微软或成最大赢家](http://mp.weixin.qq.com/s?__biz=Mzg5NzcxMzA0NA==&mid=2247483886&idx=1&sn=b5fe0bde24ae244dfc90ef6b68e096cb&chksm=c06cd715f71b5e03f7109a13cc306f060e8837bb84aa4e8f4c92aed042508affe4443cfe02d9&scene=21#wechat_redirect)
    
- [Python 应该怎么学？](http://mp.weixin.qq.com/s?__biz=Mzg5NzcxMzA0NA==&mid=2247484170&idx=1&sn=4b3cc59a6c12b5f0c6de3778d44e94a1&chksm=c06cd5f1f71b5ce76e2845a08526d03df025b92f3e25d5933f881add7cabb32b218926ea4ae9&scene=21#wechat_redirect)
    
- [](http://mp.weixin.qq.com/s?__biz=Mzg5NzcxMzA0NA==&mid=2247484231&idx=1&sn=ed22cd660565b5af255ea8093116dc9d&chksm=c06cd5bcf71b5caa2b69e9b8f2440acac77b95ba1fb13e668d05bfddadc268d1eba012b59bae&scene=21#wechat_redirect)[最全Python入门算法来了，GitHub超6.8万星](http://mp.weixin.qq.com/s?__biz=Mzg5NzcxMzA0NA==&mid=2247484231&idx=1&sn=ed22cd660565b5af255ea8093116dc9d&chksm=c06cd5bcf71b5caa2b69e9b8f2440acac77b95ba1fb13e668d05bfddadc268d1eba012b59bae&scene=21#wechat_redirect)
    

<img width="206" height="206" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_7fee1d23439248eb8.jpg"/>

Python编程大全

进群学习交流加 : mm1552923

<img width="24" height="23" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__27873d8d5de84d268.png"/>觉得不错，请点个在看呀














```


```

```


![Image](https://mmbiz.qpic.cn/mmbiz_png/eGU4MMop1ayFNFNmlvqbFaePA7KqafKnD4pSCwWiaraQgOAVxyAibx7D3gT50kRH8DwGI3o7UtWVf8GcicFAojVibg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

分享














```

People who liked this content also liked

python也能轻松实现界面编程

简易编程网

不看的原因

- 内容质量低
- 不看此公众号

Python 高级编程

笑嘻嘻的小书屋

不看的原因

- 内容质量低
- 不看此公众号

Python编程基础入门学习（七）--if语句的应用

Ingemar

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___d0e8546a884941a9b.bmp"/>

Scan to Follow