# 强大的 Python 信号库：blinker 入门教程

<a id="profileBt"></a><a id="js_name"></a>Python工程师 *2022-02-15 07:30*

收录于话题

<a id="js_article_tag_name__2269177291329077250"></a>#信号 <a id="js_article_tag_num__2269177291329077250"></a>1 <a id="js_article_tag_tips__2269177291329077250"></a>个

<a id="js_article_tag_name__2269177293157793793"></a>#信号处理 <a id="js_article_tag_num__2269177293157793793"></a>1 <a id="js_article_tag_tips__2269177293157793793"></a>个

<a id="js_article_tag_name__2269177290725097474"></a>#函数 <a id="js_article_tag_num__2269177290725097474"></a>1 <a id="js_article_tag_tips__2269177290725097474"></a>个

<a id="js_article_tag_name__2258445776639066115"></a>#python <a id="js_article_tag_num__2258445776639066115"></a>11 <a id="js_article_tag_tips__2258445776639066115"></a>个

<img width="55" height="63" src="../../../_resources/640_wx_fmt_gif_wxfrom_5_wx_lazy__0b14916006cd47129.gif"/>

点击上方「蓝字」关注我们

<img width="17" height="22" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__d46f828fa85246aa8.png"/>

## \# 1 信号

信号是一种通知或者说通信的方式，信号分为发送方和接收方。发送方发送一种信号，接收方收到信号的进程会跳入信号处理函数，执行完后再跳回原来的位置继续执行。

常见的 Linux 中的信号，通过键盘输入 Ctrl+C，就是发送给系统一个信号，告诉系统退出当前进程。

信号的特点就是发送端通知订阅者发生了什么。使用信号分为 3 步：定义信号，监听信号，发送信号。

<img width="661" height="265" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__c5285eac6e5442a28.png"/>

Python 中提供了信号概念的通信模块，就是`blinker`。

Blinker 是一个基于 Python 的强大的信号库，它既支持简单的点对点通信，也支持点对多点的组播。Flask 的信号机制就是基于它建立的。Blinker 的内核虽然小巧，但是功能却非常强大，它支持以下特性：

- 支持注册全局命名信号
    
- 支持匿名信号
    
- 支持自定义命名信号
    
- 支持与接收者之间的持久连接与短暂连接
    
- 通过弱引用实现与接收者之间的自动断开连接
    
- 支持发送任意大小的数据
    
- 支持收集信号接收者的返回值
    
- 线程安全
    

## \# 2 blinker 使用

安装方法：

```
pip install blinker

```

###  2.1 命名信号

```
from blinker import signal
# 定义一个信号
s = signal('king')
def animal(args):
    print('我是小钻风，大王回来了，我要去巡山')
# 信号注册一个接收者
s.connect(animal)
if "__main__" == __name__:
    # 发送信号
    s.send()

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

###  2.2 匿名信号

blinker 也支持匿名信号，就是不需要指定一个具体的信号值。创建的每一个匿名信号都是互相独立的。

```
from blinker import Signal
s = Signal()
def animal(sender):
    print('我是小钻风，大王回来了，我要去巡山')
s.connect(animal)
if "__main__" == __name__:
    s.send()

```

###  2.3 组播信号

组播信号是比较能体现出信号优点的特征。多个接收者注册到信号上，发送者只需要发送一次就能传递信息到多个接收者。

```
from blinker import signal
s = signal('king')
def animal_one(args):
    print(f'我是小钻风，今天的口号是: {args}')
def animal_two(args):
    print(f'我是大钻风，今天的口号是: {args}')
s.connect(animal_one)
s.connect(animal_two)
if "__main__" == __name__:
    s.send('大王叫我来巡山，抓个和尚做晚餐！')

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

###  2.4 接收方订阅主题

接受方支持订阅指定的主题，只有当指定的主题发送消息时才发送给接收方。这种方法很好的区分了不同的主题。

```
from blinker import signal
s = signal('king')
def animal(args):
    print(f'我是小钻风，{args} 是我大哥')
s.connect(animal, sender='大象')
if "__main__" == __name__:
    for i in ['狮子', '大象', '大鹏']:
        s.send(i)

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

###  2.5 装饰器用法

除了可以函数注册之外还有更简单的信号注册方法，那就是装饰器。

```
from blinker import signal
s = signal('king')
@s.connect
def animal_one(args):
    print(f'我是小钻风，今天的口号是: {args}')
@s.connect
def animal_two(args):
    print(f'我是大钻风，今天的口号是: {args}')
if "__main__" == __name__:
    s.send('大王叫我来巡山，抓个和尚做晚餐！')

```

###  2.6 可订阅主题的装饰器

`connect`的注册方法用着装饰器时有一个弊端就是不能够订阅主题，所以有更高级的`connect_via`方法支持订阅主题。

```
from blinker import signal
s = signal('king')
@s.connect_via('大象')
def animal(args):
    print(f'我是小钻风，{args} 是我大哥')
if "__main__" == __name__:
    for i in ['狮子', '大象', '大鹏']:
        s.send(i)

```

###  2.7 检查信号是否有接收者

如果对于一个发送者发送消息前要准备的耗时很长，为了避免没有接收者导致浪费性能的情况，所以可以先检查某一个信号是否有接收者，在确定有接收者的情况下才发送，做到精确。

```
from blinker import signal
s = signal('king')
q = signal('queue')
def animal(sender):
    print('我是小钻风，大王回来了，我要去巡山')
s.connect(animal)
if "__main__" == __name__:
    res = s.receivers
    print(res)
    if res:
        s.send()
    res = q.receivers
    print(res)
    if res:
        q.send()
    else:
        print("孩儿们都出去巡山了")
{4511880240: <weakref at 0x10d02ae80; to 'function' at 0x10cedd430 (animal)>}
我是小钻风，大王回来了，我要去巡山
{}
孩儿们都出去巡山了

```

###  2.8 检查订阅者是否订阅了某个信号

也可以检查订阅者是否由某一个信号

```
from blinker import signals = signal('king')q = signal('queue')def animal(sender):    print('我是小钻风，大王回来了，我要去巡山')s.connect(animal)if "__main__" == __name__:        res = s.has_receivers_for(animal)    print(res)    res = q.has_receivers_for(animal)    print(res)TrueFalse

```

## \# 3 基于 blinker 的 Flask 信号

Flask 集成 blinker 作为解耦应用的解决方案。在 Flask 中，信号的使用场景如：请求到来之前，请求结束之后。同时 Flask 也支持自定义信号。

###  3.1 简单 Flask demo

```
from flask import Flask
app = Flask(__name__)
@app.route('/',methods=['GET','POST'],endpoint='index')
def index():
    return 'hello blinker'
if __name__ == '__main__':
    app.run()

```

访问`127.0.0.1:5000`时，返回给浏览器`hello blinker`。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

###  3.2 自定义信号

因为 Flask 集成了信号，所以在 Flask 中使用信号时从 Flask 中引入。

```
from flask import Flask
from flask.signals import _signals
app = Flask(__name__)
s = _signals.singal('msg')
def QQ(args):
    print('you have msg from QQ')
s.connect(QQ)
@app.route('/',methods=['GET','POST'],endpoint='index')
def index():
    s.send()
    return 'hello blinker'
if __name__ == '__main__':
    app.run()

```

###  3.3 Flask自带信号

在 Flask 中除了可以自定义信号，还可以使用自带信号。Flask 中自带的信号有很多种，具体如下：

```
# 请求
request_started = _signals.signal('request-started')                # 请求到来前执行
request_finished = _signals.signal('request-finished')              # 请求结束后执行
# 模板渲染
before_render_template = _signals.signal('before-render-template')  # 模板渲染前执行
template_rendered = _signals.signal('template-rendered')            # 模板渲染后执行
# 请求执行
got_request_exception = _signals.signal('got-request-exception')    # 请求执行出现异常时执行
request_tearing_down = _signals.signal('request-tearing-down')      # 请求执行完毕后自动执行（无论成功与否）
appcontext_tearing_down = _signals.signal('appcontext-tearing-down') # 请求上下文执行完毕后自动执行（无论成功与否）
# 请求上下文中
appcontext_pushed = _signals.signal('appcontext-pushed')            # 请求上下文push时执行
appcontext_popped = _signals.signal('appcontext-popped')            # 请求上下文pop时执行
message_flashed = _signals.signal('message-flashed')                # 调用flask在其中添加数据时，自动触发

```

下面以请求到来之前为例，看 Flask 中信号如何使用

```
from flask import Flask
from flask.signals import _signals, request_started
import time
app = Flask(__name__)
def wechat(args):
    print('you have msg from wechat')
# 从flask中引入已经定好的信号，注册一个函数
request_started.connect(wechat)
@app.route('/',methods=['GET','POST'],endpoint='index')
def index():
    return 'hello blinker'
if __name__ == '__main__':
    app.run()

```

当请求到来时，Flask 会经过`request_started` 通知接受方，就是函数`wechat`，这时`wechat`函数先执行，然后才返回结果给浏览器。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

但这种使用方法并不是很地道，因为信号并不支持异步方法，所以通常在生产环境中信号的接收者都是配置异步执行的框架，如 Python 中大名鼎鼎的异步框架 celery。

## \# 4 总结

信号的优点：

1.  解耦应用：将串行运行的耦合应用分解为多级执行
    
2.  发布订阅者：减少调用者的使用，一次调用通知多个订阅者
    

信号的缺点：

1.  不支持异步
    
2.  支持订阅主题的能力有限
    

> 作者：金色旭光
> 
> https://www.cnblogs.com/goldsunshine/p/15426970.html

\- EOF -

推荐阅读  点击标题可跳转

<ins>1、[煎蛋网全站妹子图爬虫](http://mp.weixin.qq.com/s?__biz=MzkzMDMyMzIzNw==&mid=2247483745&idx=1&sn=2ab2be90bc00d27954fd738bdfda6bf0&chksm=c27d4f2bf50ac63d5990b26c02eebc00e9bb9781a4273edbd89a25d5df4503b9e1b79171a04b&scene=21#wechat_redirect)</ins>

<ins>2、</ins><ins>用python实现四个完整游戏开发小项目</ins>

<ins>3、</ins><ins>30个极简Python项目代码，拿走即用（真干货）</ins>

觉得本文对你有帮助？请分享给更多人

People who liked this content also liked

CAN SPI I2S I2C等总线比较

硬件测试杂谈

不看的原因

- 内容质量低
- 不看此公众号

支持Python 3.10，Gym迎来史上最大更新

Python丹卿

不看的原因

- 内容质量低
- 不看此公众号

LAMMPS模拟教程和相关小工具

慕然分子模拟

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___4e9237ad871d47aab.bmp"/>

Scan to Follow