# 只需一个文件，Python 实现迷你 Web 框架！

<a id="profileBt"></a><a id="js_name"></a>Python猫 *2022-05-26 17:20* *Posted on <a id="js_ip_wording"></a>江苏*

The following article is from HelloGitHub Author 点击关注→

<a id="copyright_info"></a>[![](../../../_resources/0_bec068ef34d54b3c96b9714a9c212547.jpg)<br>**HelloGitHub** .<br>分享 GitHub 上有趣、入门级的开源项目。](#)

<img width="657" height="438" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_d8fe1d6391154718a.jpg"/>

当下网络就如同空气一样在我们的周围，它以无数种方式改变着我们的生活，但要说网络的核心技术变化甚微。

随着开源文化的蓬勃发展，诞生了诸多优秀的开源 Web 框架，让我们的开发变得轻松。但同时也让我们不敢停下学习新框架的脚步，其实万变不离其宗，只要理解了 Web 框架的核心技术部分，当有一个新的框架出来的时候，基础部分大同小异只需要重点了解：它有哪些特点，用到了哪些技术解决了什么痛点？这样接受和理解起新技术来会更加得心应手，不至于疲于奔命。

还有那些只会用 Web 框架的同学，是否无数次打开框架的源码，想学习提高却无从下手？

今天我们就抽丝剥茧、去繁存简，**用一个文件，实现一个迷你 Web 框架**，从而把其核心技术部分清晰地讲解清楚，配套的源码均已开源。

> GitHub 地址：https://github.com/521xueweihan/OneFile
> 
> 在线查看：https://hellogithub.com/onefile/

如果你觉得我做的这件事对你有帮助，就请给我一个 ✨Star，多多转发让更多人受益。

闲言少叙，下面就开始我们今天的提高之旅。

## 一、介绍原理

说到 Web 不得不提的就是网络协议，如果我们从 OSI 七层网络模型开始，我敢断定看完的绝对不超过三成！

所以今天我们就直接聊最上面的一层，也就是 Web 框架接触最多的 HTTP 应用层，至于 TCP/IP 部分会在聊 socket 的时候粗略带过。期间我会刻意打码非必要讲解技术的细枝末节，切断远离本期主题的技术话题，一个文件只讲一个技术点！**绝不拖堂**请大家放心阅读。

首先让我们先回忆下，平常浏览网站的流程。

如果我们把在网上冲浪，比做在一间教室听课，那么老师就是服务器（server），学生就是客户端（client）。当同学有问题的时候会先举手（请求建立 TCP），老师发现学生的提问请求，同意学生回答问题后，学生起立提出问题（发送请求），如果老师承诺会给提问的学生加课堂表现分，那么提问的时候就需要有个高效的提问方式（请求格式），即：

- 先报学号
    
- 再提问题
    

师接收到学生的提问后就可以立即回答问题（返回响应）无需再问学号，回答格式（响应格式）如下：

- 回答问题
    
- 根据学号加分！
    

有了约定好的提问格式（协议），就可以省去老师每次询问学生的学号，即高效又严谨。最后，老师回答完问题让学生坐下（关闭连接）。

其实，我们在网络上通信流程也大致如此：

<img width="558" height="263" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__e08903e7d6654ca89.png"/>

只不过机器执行起来更加严格，大家都是遵循某种协议来开发软件，这样就可以实现在某种协议下进行通信，而这种网络通信协议就叫做 HTTP（超文本传输协议）。

而我们要做的 Web 框架就是处理上面的流程：建立连接、接收请求、解析请求、处理请求、返回请求。

原理部分就聊这么多，目前你只需要记住网络上通信分为两大步：**建立连接（用于通信）和处理请求。**

所谓框架就是处理大多数情况下要处理的事情，所以我们要写的 Web 框架也就是处理两件事，即：

- 处理连接（socket）
    
- 处理请求（request）
    

一定要记住：**连接和请求是两个东西，建立起连接才能发送请求。**

而想要建立连接发起通信，就需要通过 socket 来实现（建立连接），socket 可以理解为两个虚拟的本子（文件句柄），通信的双方人手一个，它既能读也能写，只要把传输的内容写到本子上（处理请求），对方就可以看到了。

<img width="657" height="310" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__0410e4a46b9d4da89.png"/>

下面我把 Web 框架分为两部分进行讲解，所有代码将采用简单易懂的 Python3 进行实现。

## 二、编写 Web 框架

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__afa023d5b4fd4594b.png)

代码+注释一共 457 行，请放心绝对简单易懂。

### 2.1 处理连接（HTTPServer）

这里需要简单聊一下 socket 这个东西，在编程语言层面它就是一个类库，负责搞定连接建立网络通信。但本质上是系统级别提供通信的进程，而一台电脑可以建立多条通信线路，所以每一个端口号后面都是一个 socket 进程，它们相互独立、互不干涉，这也是为什么我们在启动服务的时候要指定端口号的原因。

最后，上面所说的服务器其实就是一台性能好一点、一直开着的电脑，而客户端就是浏览器、手机、电脑，它们都有 socket 这个东西（操作系统级别的一个进程）。

如果上面这段话没有看懂也不碍事，能看懂下面的图就行，得搞明白 socket 处理连接的步骤和流程，才能编写 Web 框架处理连接的部分。

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__c91e6753bbb247158.png)

下面分别展示基于 socket 编写的 server.py 和 client.py 代码。

```
# coding: utf-8
# 服务器端代码（server.py）
import socket
print('我是服务端！')
HOST = ''
PORT = 50007
s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)  # 创建 TCP socket 对象
s.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)  # 重启时释放端口
s.bind((HOST, PORT))  # 绑定地址
s.listen(1)  # 监听TCP，1代表：操作系统可以挂起(未处理请求时等待状态)的最大连接数量。该值至少为1
print('监听端口：', PORT)
while 1:
    conn, _ = s.accept()  # 开始被动接受TCP客户端的连接。
    data = conn.recv(1024)  # 接收TCP数据，1024表示缓冲区的大小
    print('接收到:', repr(data))
    conn.sendall(b'Hi, '+data)  # 给客户端发送数据
    conn.close()

```

因为 HTTP 是建立在相对可靠的 TCP 协议上，所以这里创建的是 TCP socket 对象。

```
# coding: utf-8
# 客户端代码（client.py）
import socket
print('我是客户端！')
HOST = 'localhost'    # 服务器的IP
PORT = 50007              # 需要连接的服务器的端口
s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.connect((HOST, PORT))
print("发送'HelloGitHub'")
s.sendall(b'HelloGitHub')  # 发送‘HelloGitHub’给服务器
data = s.recv(1024)
s.close()
print('接收到', repr(data))  # 打印从服务器接收回来的数据

```

运行效果如下：

<img width="657" height="233" src="../../../_resources/640_wx_fmt_gif_wxfrom_5_wx_lazy__861112ceaf59475fa.gif"/>

结合上面的代码，可以更加容易理解 socket 建立通信的流程：

1.  socket：创建socket
    
2.  bind：绑定端口号
    
3.  listen：开始监听
    
4.  accept：接收请求
    
5.  recv：接收数据
    
6.  close：关闭连接
    

所以，Web 框架中处理连接的 HTTPServer 类要做的事情就呼之欲出了。即：一开始在 `__init__`方法中创建 socket，接着绑定端口（`server_bind`）然后开始监听端口（`server_activate`)

```
# 处理连接进行数据通信
class HTTPServer(object):
    def __init__(self, server_address, RequestHandlerClass):
        self.server_address = server_address # 服务器地址
        self.RequestHandlerClass = RequestHandlerClass # 处理请求的类
        # 创建 TCP Socket
        self.socket = socket.socket(socket.AF_INET,
                                    socket.SOCK_STREAM)
        # 绑定 socket 和端口
        self.server_bind()
        # 开始监听端口
        self.server_activate()

```

通过传入的 `RequestHandlerClass` 参数可以看出，处理请求与建立连接是分开处理。

下面就要开始启动服务接收请求了，也就是 `HTTPServer` 的启动方法 `serve_forever`，这里包含了**接收请求、接收数据、开始处理请求、结束请求**的全过程。

```
def serve_forever(self):
    while True:
        ready = selector.select(poll_interval)
        # 当客户端请求的数据到位，则执行下一步
        if ready:
            # 有准备好的可读文件句柄，则与客户端的链接建立完毕
            request, client_address = self.socket.accept()
            # 可以进行下面的处理请求了，通过 RequestHandlerClass 处理请求和连接独立
            self.RequestHandlerClass(request, client_address, self)
            # 关闭连接
            self.socket.close()

```

如此循环下去，就是 HTTPServer 处理连接、建立起 HTTP 连接的全部代码，就这？对！是不是很简单？

代码中的 `RequestHandlerClass` 形参是处理请求的类，下面将深入讲解其对应的 `HTTPRequestHandler` 是如何处理 HTTP 请求。

### 2.2 处理请求（HTTPRequestHandler）

还记得上面介绍的 socket 如何实现两端通信吗？通过两个可读写的“虚拟本子”。

再加上还要保证通信的高效和严谨，就需要有对应的“通信格式”。

所以，处理请求只需要三步走：

1.  setup：初始化两个本子
    

- 读请求的文件句柄（rfile）
    
- 写响应的文件句柄（wfile）
    

3.  handle：读取并解析请求、处理请求、构造响应并写入
    
4.  finish：返回响应，销毁两个本子释放资源，然后尘归尘土归土，等待下个请求
    

对应的代码：

```
# 处理请求
class HTTPRequestHandler(object):
    def __init__(self, request, client_address, server):
        self.request = request # 接收来的请求（socket）
        # 1、初始化两个本子
        self.setup()
        try:
            # 2、读取、解析、处理请求，构造响应
            self.handle()
        finally:
            # 3、返回响应，释放资源
            self.finish()
    
    def setup(self):
        self.rfile = self.request.makefile('rb', -1) # 读请求的本子
        self.wfile = self.request.makefile('wb', 0) # 写响应的本子
    def handle(self):
        # 根据 HTTP 协议，解析请求
        # 具体的处理逻辑，即业务逻辑
        # 构造响应并写入本子
    def finish(self):
        # 返回响应
        self.wfile.flush()
        # 关闭请求和响应的句柄，释放资源
        self.wfile.close()
        self.rfile.close()

```

以上就是处理请求的整体流程，下面将详细介绍 `handle` 如何解析 HTTP 请求和构造 HTTP 响应，以及如何实现把框架和具体的业务代码（处理逻辑）分开。

在解析 HTTP 之前，需要先看一个实际的 HTTP 请求，当我打开 hellogithub.com 网站首页的时候，浏览器发送的 HTTP 请求如下：

<img width="657" height="265" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__e05da61259b444f2a.png"/>

整理归纳可得 HTTP 请求格式，如下：

```
{HTTP method} {PATH} {HTTP version}\r\n
{header field name}:{field value}\r\n
...
\r\n
{request body}

```

得到了请求格式，那么 `handle` 解析请求的方法也就有了。

```
def handle(self):
    # --- 开始解析 --- #
    self.raw_requestline = self.rfile.readline(65537) # 读取请求第一行数据，即请求头
    requestline = str(self.raw_requestline, 'iso-8859-1') # 转码
    requestline = requestline.rstrip('\r\n') # 去换行和空白行
    # 就可以得到 "GET / HTTP/1.1" 请求头了，下面开始解析
    self.command, self.path, self.request_version = requestline.split() 
    # 根据空格分割字符串，可得到("GET", "/", "HTTP/1.1")
    # command 对应的是 HTTP method，path 对应的是请求路径
    # request_version 对应 HTTP 版本，不同版本解析规则不一样这里不做展开讲解
    self.headers = self.parse_headers() # 解析请求头也是处理字符串，但更为复杂标准库有工具函数这里略过
    # --- 业务逻辑 --- #
    # do_HTTP_method 对应到具体的处理函数
    mname = ('do_' + self.command).lower()
    method = getattr(self, mname)
    # 调用对应的处理方法
    method()
    # --- 返回响应 --- #
    self.wfile.flush()
def do_GET(self):
    # 根据 path 区别处理
    if self.path == '/':
        self.send_response(200)  # status code
        # 加入响应 header
        self.send_header("Content-Type", "text/html; charset=utf-8")
        self.send_header("Content-Length", str(len(content)))
        self.end_headers() # 结束头部分，即：'\r\n'
        self.wfile.write(content.encode('utf-8')) # 写入响应 body，即：页面内容
def send_response(self, code, message=None):
    # 响应体格式
    """
    {HTTP version} {status code} {status phrase}\r\n
    {header field name}:{field value}\r\n
    ...
    \r\n
    {response body}
    """
    # 写响应头行
    self.wfile.write("%s %d %s\r\n" % ("HTTP/1.1", code, message))
    # 加入响应 header
    self.send_header('Server', "HG/Python ")
    self.send_header('Date', self.date_time_string())

```

以上就是 `handle` 处理请求和返回响应的核心代码片段了，至此 `HTTPRequestHandler` 全部内容均已讲解完毕，下面将演示运行效果。

### 2.3 运行

```
class RequestHandler(HTTPRequestHandler):
    # 处理 GET 请求
    def do_get(self):
        # 根据 path 对应到具体的处理方法
        if self.path == '/':
            self.handle_index()
        elif self.path.startswith('/favicon'):
            self.handle_favicon()
        else:
            self.send_error(404)
if __name__ == '__main__':
    server = HTTPServer(('', 8080), RequestHandler)
    # 启动服务
    server.serve_forever()

```

这里通过继承 Web 框架的 `HTTPRequestHandler` 实现的子类 `RequestHandler` 重写 `do_get` 方法，实现业务代码和框架的分离。这样保证了框架的灵活性和解耦。

接下来服务毫无意外地运行起来了，效果如下：

<img width="657" height="638" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__987171a153f24985b.png"/>

本文中涉及 Web 框架的代码，为方便阅读都经过了简化。如果想要获取完整可运行的代码，可前往 GitHub 地址获取：

<ins>https://github.com/521xueweihan/OneFile/blob/main/src/python/web-server.py</ins>

该框架并不包含 Web 框架应有的丰富功能，旨在通过最简单的代码，实现一个迷你 Web 框架，让不了解基本 Web 框架结构的同学，得以一探究竟。

如果本文的内容勾起了你对 Web 框架的兴趣，你还想更加深入的了解更加全面、适用于生产环境、代码和结构同样的简洁的 Web 框架。我建议的学习路径：

1.  Python3 的 HTTPServer、BaseHTTPRequestHandler
    
2.  bottle：单文件、无三方依赖、持续更新，可用于生产环境的开源 Web 框架：
    

- 地址：https://github.com/bottlepy/bottle
    

4.  werkzeug -> flask
    
5.  starlette -> uvicorn -> fastapi
    

有的时候阅读框架源码不是为了写一个新的框架，而是向前辈学习和靠拢。

## 最后

新的技术总是学不完的，掌握核心的技术原理，不仅可以在接受新的知识时快人一步，还可以在排查问题时一针见血。

不知道这种一个文件讲解一个技术点，力求通过简单的文字和精简的代码描述原理，期间抹去了细枝末节的技术专注于一门技术，最后给出完整可运行的开源代码的文章，是否符合你的胃口？

本文是我对新的系列一种尝试，接受任何指点和批评。

如果你喜欢此类文章，就请点赞给我一点鼓励，还可以留言提建议或者“点餐”。

> 不要想你为开源做了什么，你只需要清楚你为自己做了什么。

OneFile 期待你的加入，轻点{阅读原文}贡献一份力量。

![Image](../../../_resources/640_wx_fmt_gif_wxfrom_5_wx_lazy__36deb8620d054dfba.gif "音符")

**Python猫技术交流群开放啦！**群里既有国内一二线大厂在职员工，也有国内外高校在读学生，既有十多年码龄的编程老鸟，也有中小学刚刚入门的新人，学习氛围良好！想入群的同学，请在公号内回复『**交流群**』，获取猫哥的微信（谢绝广告党，非诚勿扰！）~

**还不过瘾？试试它们**

**▲****[Python 程序调试分析大杀器合集](http://mp.weixin.qq.com/s?__biz=MzUyOTk2MTcwNg==&mid=2247496021&idx=1&sn=6bd4580a1f7a8ea1d4c5377c7664887e&chksm=fa5bb6d0cd2c3fc6b0ca72209e13653c25b1d01293778fa3f432c7c116356b1139eaf3935ac0&scene=21#wechat_redirect)**

**▲****[如何让你的 Python 代码经得起时间检验？](http://mp.weixin.qq.com/s?__biz=MzUyOTk2MTcwNg==&mid=2247495971&idx=1&sn=ed0948921bc5764c8780a39af78a84f0&chksm=fa5bb6a6cd2c3fb033e265d61bf545fcdc7d8bd1f78d6202dd0b9369b45595dc76df7595dda9&scene=21#wechat_redirect)**

**▲****[重写 500 Lines or Less 项目：流水线调度问题](http://mp.weixin.qq.com/s?__biz=MzUyOTk2MTcwNg==&mid=2247494559&idx=1&sn=4694e5d27af76cdb7bc895f75b4c0091&chksm=fa5bac1acd2c250cf3eb910253f1bbc6afdaf68b8bcce3f123f86d89c81695fe822583ce2373&scene=21#wechat_redirect)**

**▲****[Python之父：为什么要使用操作符？](http://mp.weixin.qq.com/s?__biz=MzUyOTk2MTcwNg==&mid=2247491657&idx=1&sn=b97df777272bc5bbda65643d5678d97e&chksm=fa5ba7cccd2c2edae47561f7afac162c9b6396df1efa98a73f7f1e88f40e44203204b85d88a6&scene=21#wechat_redirect)**

**▲****[这有 73 个例子，彻底掌握 f-string 用法！](http://mp.weixin.qq.com/s?__biz=MzUyOTk2MTcwNg==&mid=2247487897&idx=1&sn=0719226b98a2512c7433415cba5db7e9&chksm=fa58561ccd2fdf0a7dfd1e7b86499f159c511e35b86a5c9387b3e5e41a2fdab863ed61c30080&scene=21#wechat_redirect)**

**▲****[Python 中的数字到底是什么？](http://mp.weixin.qq.com/s?__biz=MzUyOTk2MTcwNg==&mid=2247486618&idx=1&sn=1b4c013c6ee27235b8f8f6a99305e09b&chksm=fa584b1fcd2fc2090a0d7da8e63a3a196315e413c1d8865133b03a00297c06a724b396aced8f&scene=21#wechat_redirect)**

**如果你觉得本文有帮助**

**请慷慨分享****和****点赞****，感谢啦****！**

![](../../../_resources/0_wx_fmt_png_2232664080f24a22bc9fefebc08de0ee.png)

**Python猫**

分享Python进阶、Python哲学、文章翻译、资源工具等内容

<a id="js_profile_article"></a>152篇原创内容

Official Account

<a id="js_view_source"></a>Read more

People who liked this content also liked

为什么要有协程，协程是如何实现的？

...

Python猫

不看的原因

- 内容质量低
- 不看此公众号

支持300+常用功能的开源GO语言工具函数库

...

GoCN

不看的原因

- 内容质量低
- 不看此公众号

像Python一样玩C/C++

...

光城

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___7215a52e3d864d449.bmp"/>

Scan to Follow