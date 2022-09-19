# Python程序调试分析大杀器！

<a id="profileBt"></a><a id="js_name"></a>早起Python *2022-05-25 10:28* *Posted on <a id="js_ip_wording"></a>安徽*

The following article is from RandomGenerator Author zijie0

<a id="copyright_info"></a>[![](../../../_resources/0_deafb6ee8d934f8aabc0c0564845add7.jpg)<br>**RandomGenerator** .<br>机器学习工程师笔记](#)

# 问题背景

在之前的 [debug 系列文章](http://mp.weixin.qq.com/s?__biz=MzkzNTIzNTYzMA==&mid=2247484204&idx=1&sn=c84ce6f50a4c5a342f4df57870e15ef3&chksm=c2b05a6df5c7d37b6bb6c45400fbdc30ebaafedd78239fa70677852b1a0eba51cf031d844208&scene=21#wechat_redirect)<sup>\[1\]</sup> 里，我们曾经介绍过一个 Python 进程 hang 住的问题的排查。最近正好又碰到了类似的问题，来简单调研总结一下处理这类问题的排查处理方法，以及各种 Python 调试，性能分析中可以使用的强大工具。

# 思路

对于程序 hang 住，但 CPU 等使用率较低的情况，最常用的做法是获取程序当前的 callstack，查看是否在“等待”什么东西。理想情况下，我们希望能直接得到 Python 程序当前在执行哪一行代码，定位根源。

对于程序跑得慢，且 CPU 使用率相对正常或比较高，说明程序还是在“干活”的。这时候一般会采用各种 profiling 工具，去多次“采样” callstack，查看程序在各行代码上的所耗费时间的百分比，以此来定位性能瓶颈。

可见获取 callstack 是一个共性需求，我们接下来会以此展开，来看看各种处理方法。

# 常规方法

先介绍一下不需要额外库的排查方法，也是很多文章中会提到的常见做法。

## 如果可以改代码

如果这个 hang 住的程序是我们自己控制的，比如自己开发的项目，线下的测试环境可以修改重启等，那么我们可以通过植入“信号 handler”的方法来获取到程序的 callstack，进而定位到底 hang 在哪一行代码执行上。具体代码如下：

```
`import code, traceback, signal
def debug(sig, frame):
    """Interrupt running process, and provide a python prompt for
    interactive debugging."""
    d={'_frame':frame}         # Allow access to frame object.
    d.update(frame.f_globals)  # Unless shadowed by global
    d.update(frame.f_locals)
    i = code.InteractiveConsole(d)
    message  = "Signal received : entering python shell.\nTraceback:\n"
    message += ''.join(traceback.format_stack(frame))
    i.interact(message)
def listen():
    signal.signal(signal.SIGUSR1, debug)  # Register handler
`
```

然后只要在程序开始执行前的代码里先调用一下`listen()`，后面当程序卡住时，可以通过另一个 Python 进程来发送信号：

```
`os.kill(pid, signal.SIGUSR1)
`
```

然后就能得到当时的 callstack 了。不过这个方法只能在 Linux/Unix 系的操作系统上使用。

## 如果不能改代码

很多情况下，我们并不能修改相应的 Python 程序，比如是在线上运行的程序，不能随意关掉；或者并不是自己开发的项目（虽然 Python 库一般来说都是代码开放的）。这时候我们可以使用一些 debug 通用进程的方式来进行排查，例如：

- 可以用`pstack`，`gdb`来打印 native callstack。
    
- 用`strace`来做进程调用的追踪。
    
- 使用`perf`等动态追踪工具来进行 profiling。
    

同样的，这些方法也基本只能在 Linux 系统上使用，且拿到的信息大多数都是系统调用，不容易直接映射到对于的 Python 代码上。

# 利用专有工具

Java 中自带的 JDK 里有很多强大的开发者工具，可以帮助我们来对 Java 程序做各类排查诊断，另外也衍生出了包括 arthas<sup>\[2\]</sup> 这样优秀的第三方工具。可惜 Python 原生并没有如此完善的工具体系，但好在类似的第三方工具还是有不少的，我们这就来尝试一下。

## 示例代码

首先我们先来“人造”一个非常简单的程序 hang 住的代码：

```
`import time
def func1():
    print('hello')
    func2()
    print('bye')
def func2():
    print('enter func2')
    time.sleep(100)
    print('leave func2')
def main():
    func1()
if __name__ == '__main__':
    main()
`
```

执行这个程序，程序会在`sleep`处 hang 住，然后可以用`ps`命令获取到相应的 pid。后续我们希望各类工具都能直接使用这个 pid，而不需要对程序本身进行修改或重启操作，就能获取到相关的 callstack 或更丰富的 profiling 信息。

大家如果对性能 profiling 的排查感兴趣，也可以自己构造一个消耗较高 CPU 资源的程序（例如启动 2 个子进程来计算圆周率）来进行后面的各种实验。

## 系统工具

我们先用`gdb`来看下获取的 callstack 长啥样，执行：

```
`gdb attach <pid>
`
```

然后通过`bt`命令打印出 callstack：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

gdb 获取的 callstack

可见 native 的 callstack 包含的内容比较“细节”，虽然大致可以看出程序是 hang 在了 sleep 操作上，但 Python 内部代码的调用路径基本是不可见的。

## pystack-debugger

找这个项目的出发点是想看看能否在`gdb`里支持 Python 调用 frame 的信息获取。其它网站有看到过一些 gdb 的宏的介绍，不过安装配置比较麻烦，还是这个工具用起来最简单直接。

项目地址：pystack<sup>\[3\]</sup>

安装：

```
`pip install pystack-debugger
`
```

执行：

```
`pystack <pid>
`
```

返回信息：

```
`❯ pystack 3570
Dumping Threads....
  File "hang.py", line 17, in <module>
    main()
  File "hang.py", line 14, in main
    func1()
  File "hang.py", line 5, in func1
    func2()
  File "hang.py", line 10, in func2
    time.sleep(100)
  File "<string>", line 1, in <module>
`
```

Wow，效果很好，直接就找到了我们 hang 在了 sleep 这一行上。

## hypno

项目地址：hypno<sup>\[4\]</sup>

安装同样用 pip：

```
`pip install hypno
`
```

这个库的目标不太一样，主要是为了 inject 到正在运行的 Python 进程里跑一些代码，所以灵活性比单纯打 callstack 更高，比如可以打印一些变量之类。我们用如下的命令来实现获取 callstack 的功能：

```
`hypno <pid> "import traceback; traceback.print_stack()"
`
```

可以看到原来的进程中打印出了我们所需要的信息：

```
`❯ python hang.py
hello
enter func2
  File "hang.py", line 17, in <module>
    main()
  File "hang.py", line 14, in main
    func1()
  File "hang.py", line 5, in func1
    func2()
  File "hang.py", line 10, in func2
    time.sleep(100)
  File "<string>", line 1, in <module>
leave func2
bye
`
```

这里也能直接定位到进程当时运行到的位置，而且“入侵”完成后还不影响原先程序的运行，会一直跑完正常退出。我琢磨着这个工具应该还可以做不少其它的骚操作……

## py-spy

在搜寻 profiling 相关工具时看到了这个项目，感觉非常强大。

项目地址：py-spy<sup>\[5\]</sup>

安装：

```
`pip install py-spy
`
```

获取 callstack 可以使用 dump 命令：

```
`❯ py-spy dump --pid 5103
Process 5103: python hang.py
Python v3.8.8 (/home/zijie0/miniconda3/envs/playground/bin/python3.8)
Thread 5103 (idle)
    func2 (hang.py:10)
    func1 (hang.py:5)
    main (hang.py:14)
    <module> (hang.py:17)
`
```

此外`py-spy`还支持`top`形式的实时 profiling，以及生成火焰图（操作稍麻烦）的功能，除了程序 hang 之外也能做性能优化的监控工具。不过易用性上来说比下面要介绍的`austin`还是稍微差了些。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

py-spy top 模式

在我的电脑上看这个 sample 百分比的记录好像也有些问题。

## austin

本以为`py-spy`已经足够强大了，没想到又找到了这个无敌的`austin`！

项目地址：austin<sup>\[6\]</sup>，austin-tui<sup>\[7\]</sup>

安装：

```
`conda install -c conda-forge austin-tui
`
```

这是一个集大成的 profiling 工具，不光可以看 callstack，还可以看对应的采样（近似理解为程序时间开销）百分占比信息，以及实时更新的 flamegraph 等，竟然还是跨平台的！官网上还有更多高级特性，感觉已经可以跟 JDK 里的 visualvm 来媲美了！这个例子里的 callstack 比较简单，生成出来长这样：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

austin-tui

官网上给的树状图和 flamegraph 对于复杂程序的性能问题排查会很有帮助：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

austin tree view

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

austin 火焰图

作者还很贴心的提供了 cheatsheet 来帮助你更好的使用这个工具，以后可以在项目里多尝试一下了。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

austin cheatsheet

值得一提的是在调研跟`austin`类似的工具框架时，还找到了这个 pyroscope<sup>\[8\]</sup>，把 profiling 做成了个平台（也支持 Python），还成立了家公司进入了 YC 的孵化器，未来说不定也是个不错的细分市场呀。

## 其它项目

搜了一圈下来发现 Python 这方面的工具远比我想象中的要丰富，还有很多相对冷门或者有些时间没有维护的项目，这里也列举出来供大家参考：

- pyinstrument<sup>\[9\]</sup>
    
- pystuck<sup>\[10\]</sup>
    
- PyFlame<sup>\[11\]</sup>
    
- pyrasite<sup>\[12\]</sup>
    
- pydbattach<sup>\[13\]</sup>
    

大家在平时工作中有找到过什么好用的 Python 开发者工具吗？也欢迎在评论区留言讨论 :)

感谢收看，我们下期再见。

### 参考资料

\[1\]

debug 系列文章: *https://zhuanlan.zhihu.com/p/385962965*

\[2\]

arthas: *https://github.com/alibaba/arthas*

\[3\]

pystack: *https://github.com/wooparadog/pystack*

\[4\]

hypno: *https://github.com/kmaork/hypno*

\[5\]

py-spy: *https://github.com/benfred/py-spy*

\[6\]

austin: *https://github.com/P403n1x87/austin*

\[7\]

austin-tui: *https://github.com/p403n1x87/austin-tui*

\[8\]

pyroscope: *https://github.com/pyroscope-io/pyroscope*

\[9\]

pyinstrument: *https://github.com/joerick/pyinstrument*

\[10\]

pystuck: *https://github.com/alonho/pystuck*

\[11\]

PyFlame: *https://github.com/uber-archive/pyflame*

\[12\]

pyrasite: *https://github.com/lmacken/pyrasite*

\[13\]

pydbattach: *https://github.com/albertz/pydbattach*

![](../../../_resources/0_wx_fmt_png_0cdc5617003b4280b0e99de9e1f23b08.png)

**早起Python**

点击领取pandas数据分析300题

<a id="js_profile_article"></a>290篇原创内容

Official Account

People who liked this content also liked

只需一个文件，Python 实现迷你 Web 框架！

...

Python猫

不看的原因

- 内容质量低
- 不看此公众号

推荐七个Python效率工具

...

Python丹卿

不看的原因

- 内容质量低
- 不看此公众号

21张让你Python代码能力突飞猛进的速查表

...

Python丹卿

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___101fd97333b8486da.bmp"/>

Scan to Follow