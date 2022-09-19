# Python的可等待对象在Asyncio的作用

<a id="profileBt"></a><a id="js_name"></a>Python编程时光 *2022-05-17 09:02* *Posted on <a id="js_ip_wording"></a>福建*

The following article is from 博海拾贝diary Author so1n

<a id="copyright_info"></a>[![](../../../_resources/0_d746b94dc1df4e4181d03b567219b7c4.jpg)<br>**博海拾贝diary** .<br>公众号写博客太麻烦了 还是弄个博客写好了 http://so1n.me](#)

## 前记

上一篇文章《初识Python协程的实现》<sup>\[1\]</sup> 介绍了Python是如何以生成器来实现协程的以及Python Asyncio通过Future和Task的封装来实现协程的调度，而在Python Asyncio之中Coroutines, Tasks和Future都属于可等待对象，在使用的Asyncio的过程中，经常涉及到三者的转换和调度，开发者容易在概念和作用上犯迷糊，本文主要阐述的是三者之间的关系以及他们的作用。

## 1.Asyncio的入口

协程是线程中的一种特例，协程的入口和切换都是靠事件循环来调度的，在新版的`Python`中协程的入口是`Asyncio.run`，当程序运行到`Asyncio.run`后，可以简单的理解为程序由线程模式切换为协程模式(只是方便理解，对于计算机而言，并没有这样区分)，以下是一个最小的协程例子代码：

```
import asyncio
async def main():
    await asyncio.sleep(0)
asyncio.run(main())
```

在这段代码中，`main`函数和`asyncio.sleep`都属于Coroutine，`main`是通过`asyncio.run`进行调用的，接下来程序也进入一个协程模式，`asyncio.run`的核心调用是`Runner.run`，它的代码如下：

```
class Runner:
    ...
    def run(self, coro, *, context=None):
        """Run a coroutine inside the embedded event loop."""
        # 省略代码
        ...
        # 把coroutine转为task
        task = self._loop.create_task(coro, context=context)
        # 省略代码
        ...
        try:
            # 如果传入的是Future或者coroutine，也会专为task
            return self._loop.run_until_complete(task)
        except exceptions.CancelledError:
        
        # 省略代码
        ...
```

这段代码中删去了部分其它功能和初始化的代码，可以看到这段函数的主要功能是通过`loop.create_task`方法把一个Coroutine对象转为一个Task对象，然后通过`loop.run_until_complete`等待这个Task运行结束。

> 在`Python3.5`之后，`asycnio`改为又C语言实现，所以本文是`asyncio`源码都来源于最后一个以`Python`实现的asycnio版本Python3.4.10/Lib/asyncio<sup>\[2\]</sup>

可以看到，`Asycnio`并不会直接去调度Coroutine，而是把它转为Task再进行调度，这是因为在`Asyncio`中事件循环的最小调度对象就是Task。不过在`Asyncio`中并不是所有的Coroutine的调用都会先被转为Task对象再等待，比如示例代码中的`asyncio.sleep`，由于它是在`main`函数中直接await的，所以它不会被进行转换，而是直接等待，通过调用工具分析展示的图如下：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

在这个图示中，从`main`函数到`asyncio.sleep`函数中没有明显的`loop.create_task`等把Coroutine转为Task调用，这里之所以不用进行转换的原因不是做了一些特殊优化，而是本因如此， 这个`await asyncio.sleep`函数实际上还是会被`main`这个Coroutine转换成的`Task`继续调度到。

## 2.两种Coroutine调用方法的区别

在了解`Task`的调度原理之前，还是先回到最初的调用示例，看看直接用Task调用和直接用Coroutine调用的区别是什么。如下代码，我们显示的执行一个Coroutine转为Task的操作再等待，那么代码会变成下面这样:

```
import asyncio
async def main():
    await asyncio.create_task(asyncio.sleep(0))
asyncio.run(main())
```

这样的代码看起来跟最初的调用示例很像，没啥区别，但是如果进行一些改变，比如增加一些休眠时间和Coroutine的调用，就能看出Task对象的作用了，现在编写两份文件，他们的代码如下:

```
# demo_coro.py
import asyncio
import time
async def main():
    await asyncio.sleep(1)
    await asyncio.sleep(2)
s_t = time.time()
asyncio.run(main())
print(time.time() - s_t)
# // Output: 3.0028765201568604
# demo_task.py
import asyncio
import time
async def main():
    task_1 = asyncio.create_task(asyncio.sleep(1))
    task_2 = asyncio.create_task(asyncio.sleep(2))
    await task_1
    await task_2
s_t = time.time()
asyncio.run(main())
print(time.time() - s_t)
# // Output: 2.0027475357055664
```

其中`demo_coro.py`进行了两次`await`调用，程序的运行总时长为3秒，而`demo_task.py`则是先把两个Coroutine对象转为Task对象，然后再进行两次`await`调用，程序的运行总时长为2秒。可以发现，`demo_task.py`的运行时长近似于其中运行最久的Task对象时长，而`demo_coro.py`的运行时长则是近似于两个Coroutine对象的总运行时长。

之所以会是这样的结果，是因为直接`await`Coroutine对象时，这段程序会一直等待，直到Coroutine对象执行完毕再继续往下走，而Task对象的不同之处就是在创建的那一刻，就已经把自己注册到事件循环之中等待被安排运行了，然后返回一个task对象供开发者等待，由于`asyncio.sleep`是一个纯IO类型的调用，所以在这个程序中，两个`asyncio.sleep`Coroutine被转为Task从而实现了并发调用。

## 3.Task与Future

上述的代码之所以通过Task能实现并发调用，是因为Task中出现了一些与事件循环交互的函数，正是这些函数架起了Coroutine并发调用的可能， 不过Task是Future的一个子对象，所以在了解Task之前，需要先了解Future。

## 3.1.Future

与Coroutine只有让步和接收结果不同的是Future除了让步和接收结果功能外，它还是一个只会被动进行事件调用且带有状态的容器，它在初始化时就是`Pending`状态，这时可以被取消，被设置结果和设置异常。而在被设定对应的操作后，Future会被转化到一个不可逆的对应状态，并通过`loop.call_sonn`来调用所有注册到本身上的回调函数，同时它带有`__iter__`和`__await__`方法使其可以被`await`和`yield from`调用，它的主要代码如下：

```
class Future:
    ...
    def set_result(self, result):
        """设置结果，并安排下一个调用"""
        if self._state != _PENDING:
            raise exceptions.InvalidStateError(f'{self._state}: {self!r}')
        self._result = result
        self._state = _FINISHED
        self.__schedule_callbacks()
    def set_exception(self, exception):
        """设置异常，并安排下一个调用"""
        if self._state != _PENDING:
            raise exceptions.InvalidStateError(f'{self._state}: {self!r}')
        if isinstance(exception, type):
            exception = exception()
        if type(exception) is StopIteration:
            raise TypeError("StopIteration interacts badly with generators "
                            "and cannot be raised into a Future")
        self._exception = exception
        self._state = _FINISHED
        self.__schedule_callbacks()
        self.__log_traceback = True
    def __await__(self):
        """设置为blocking，并接受await或者yield from调用"""
        if not self.done():
            self._asyncio_future_blocking = True
            yield self  # This tells Task to wait for completion.
        if not self.done():
            raise RuntimeError("await wasn't used with future")
        return self.result()  # May raise too.
    __iter__ = __await__  # make compatible with 'yield from'.
```

单看这段代码是很难理解为什么下面这个future被调用`set_result`后就能继续往下走:

```
async def demo(future: asyncio.Future):
    await future
    print("aha")
```

这是因为Future跟Coroutine一样，没有主动调度的能力，只能通过Task和事件循环联手被调度。

## 3.2.Task

Task是Future的子类，除了继承了Future的所有方法，它还多了两个重要的方法`__step`和`__wakeup`，通过这两个方法赋予了Task调度能力，这是Coroutine和Future没有的，Task的涉及到调度的主要代码如下(说明见注释):

```
class Task(futures._PyFuture):  # Inherit Python Task implementation
                                # from a Python Future implementation.
    _log_destroy_pending = True
    def __init__(self, coro, *, loop=None, name=None, context=None):
        super().__init__(loop=loop)
        # 省略部分初始化代码
        ...
        # 托管的coroutine
        self._coro = coro
        if context is None:
            self._context = contextvars.copy_context()
        else:
            self._context = context
        # 通过loop.call_sonn，在Task初始化后马上就通知事件循环在下次有空的时候执行自己的__step函数
        self._loop.call_soon(self.__step, context=self._context)
    def __step(self, exc=None):
        coro = self._coro
        # 方便asyncio自省
        _enter_task(self._loop, self)
        # Call either coro.throw(exc) or coro.send(None).
        try:
            if exc is None:
                # 通过send预激托管的coroutine
                # 这时候只会得到coroutine yield回来的数据或者收到一个StopIteration的异常
                # 对于Future或者Task返回的是Self
                result = coro.send(None)
            else:
                # 发送异常给coroutine 
                result = coro.throw(exc)
        except StopIteration as exc:
            # StopIteration代表Coroutine运行完毕
            if self._must_cancel:
                # coroutine在停止之前被执行了取消操作，则需要显示的执行取消操作
                self._must_cancel = False
                super().cancel(msg=self._cancel_message)
            else:
                # 把运行完毕的值发送到结果值中
                super().set_result(exc.value)
        # 省略其它异常封装
        ...
        else:
            # 如果没有异常抛出
            blocking = getattr(result, '_asyncio_future_blocking', None)
            if blocking is not None:
                # 通过Future代码可以判断，如果带有_asyncio_future_blocking属性，则代表当前result是Future或者是Task
                # 意味着这个Task里面裹着另外一个的Future或者Task
                # 省略Future判断
                ...
                if blocking:
                    # 代表这这个Future或者Task处于卡住的状态，
                    # 此时的Task放弃了自己对事件循环的控制权，等待这个卡住的Future或者Task执行完成时唤醒一下自己
                    result._asyncio_future_blocking = False
                    result.add_done_callback(self.__wakeup, context=self._context)
                    self._fut_waiter = result
                    if self._must_cancel:
                        if self._fut_waiter.cancel(msg=self._cancel_message):
                            self._must_cancel = False
                else:
                    # 不能被await两次
                    new_exc = RuntimeError(
                        f'yield was used instead of yield from '
                        f'in task {self!r} with {result!r}')
                    self._loop.call_soon(
                        self.__step, new_exc, context=self._context)
            elif result is None:
                # 放弃了对事件循环的控制权，代表自己托管的coroutine可能有个coroutine在运行，接下来会把控制权交给他和事件循环 
                # 当前的coroutine里面即使没有Future或者Task,但是子Future可能有
                self._loop.call_soon(self.__step, context=self._context)
        finally:
            _leave_task(self._loop, self)
            self = None  # Needed to break cycles when an exception occurs.
    def __wakeup(self, future):
        # 其它Task和Future完成后悔调用到该函数，接下来进行一些处理
        try:
            # 回收Future的状态，如果Future发生了异常，则把异常传回给自己
            future.result()
        except BaseException as exc:
            # This may also be a cancellation.
            self.__step(exc)
        else:
            # Task并不需要自己托管的Future的结果值，而且如下注释，这样能使调度变得更快
            # Don't pass the value of `future.result()` explicitly,
            # as `Future.__iter__` and `Future.__await__` don't need it.
            # If we call `_step(value, None)` instead of `_step()`,
            # Python eval loop would use `.send(value)` method call,
            # instead of `__next__()`, which is slower for futures
            # that return non-generator iterators from their `__iter__`.
            self.__step()
        self = None  # Needed to break cycles when an exception occurs.
```

这份源码的Task对象中的`__setp`方法比较长，经过精简后可以发现他主要做的工作有三个：

- • 1.通过`send`或者`throw`来驱动Coroutine进行下一步
    
- • 2.通过给被自己托管的Future或者Task添加回调来获得完成的通知并重新获取控制权
    
- • 3.通过`loop.call_soon`来让步，把控制权交给事件循环
    

单通过源码分析可能很难明白， 以下是以`两种Coroutine`的代码为例子，简单的阐述Task与事件循环调度的过程，首先是`demo_coro`，这个例子中只有一个Task：

```
# demo_coro.py
import asyncio
import time
async def main():
    await asyncio.sleep(1)
    await asyncio.sleep(2)
s_t = time.time()
asyncio.run(main())
print(time.time() - s_t)
# // Output: 3.0028765201568604
```

这个例子中第一步是把`main`转为一个Task，然后调用到了对应的`__step`方法，这时候`__step`方法会会调用`main()`这个Coroutine的`send(None)`方法。之后整个程序的逻辑会直接转到`main`函数中的`await asyncio.sleep(1)`这个Coroutine中，`await asyncio.sleep(1)`会先生成一个Future对象，并通过`loop.call_at`告诉事件循环在1秒后激活这个Future对象，然后把对象返回。这时候逻辑会重新回到Task的`__step`方法中，`__step`发现`send`调用得到的是一个Future对象，所以就在这个Future添加一个回调，让Future完成的时候来激活自己，然后放弃了对事件循环的控制权。接着就是事件循环在一秒后激活了这个Future对象，这时程序逻辑就会执行到Future的回调，也就是Task的`__wakeup`方法，于是Task的`__step`又被调用到了，而这次遇到的是后面的`await asyncio.sleep(2)`，于是又走了一遍上面的流程。当两个`asyncio.sleep`都执行完成后，Task的`__step`方法里在对Coroutine发送一个`send(None)`后就捕获到了`StopIteration`异常，这时候Task就会通过`set_result`设置结果，并结束自己的调度流程。

可以看到`demo_core.py`中只有一个Task在负责和事件循环一起调度，事件循环的开始一定是一个Task，并通过Task来调起一个Coroutine，通过`__step`方法把后续的Future，Task,Coroutine都当成一条链来运行，而`demo_task.py`则不一样了，它有两个Task：

```

# demo_task.py
import asyncio
import time
async def main():
    task_1 = asyncio.create_task(asyncio.sleep(1))
    task_2 = asyncio.create_task(asyncio.sleep(2))
    await task_1
    await task_2
s_t = time.time()
asyncio.run(main())
print(time.time() - s_t)
# // Output: 2.0027475357055664
```

这个例子中第一步还是跟`demo_coro`一样，当跳转到`main`函数后就开始有区别了，首先在这函数中创建了task1和task2两个Task，他们分别都会通过`__step`方法中的`send`激活对应的`asyncio.sleep`Coroutine，然后等待对应的Future来通知自己已经完成了。而对于创建了这两个Task的main Task来说，通过`main`函数的`awati task_1`和`await task_2`来获取到他们的“控制权“。首先是通过`await task_1`语句，main Task中的`__step`方法里在调用`send`后得到的是task\_1对应的Future，这时候就可以为这个Future添加一个回调，让他完成时通知自己，自己再走下一步，对于task\_2也是如此。直到最后两个task都执行完成，main Task也捕获到了`StopIteration`异常，通过`set_result`设置结果，并结束自己的调度流程。

可以看到`demo_task.py`与`demo_coro.py`有个明显的区别在于main Task在运行的生命周期中创建了两个Task，并通过`await`托管了两个Task，同时两个Task又能实现两个协程的并发，所以可以发现事件循环运行期间，当前协程的并发数永远小于事件循环中注册的Task数量。此外，如果在main Task中如果没有显示的进行`await`，那么子Task就会逃逸，不受main Task管理，如下：

```

# demo_task.py
import asyncio
import time
def mutli_task():
    task_1 = asyncio.create_task(asyncio.sleep(1))
    task_2 = asyncio.create_task(asyncio.sleep(2))
async def main():
    mutli_task()
    await asyncio.sleep(1.5) 
s_t = time.time()
asyncio.run(main())
print(time.time() - s_t)
# // Output: 1.5027475357055664 
```

在这段代码中，main Task在执行到`mutli_task`时，会创建出两个task，但是在`__step`中的`coro.send(None)`调用得到的结果却是`await asyncio.sleep(1.5)`返回的Future，所以main Task只能调用到这个Future的`add_don_callback`来装载自己的`__wakeup`方法，最终导致到main Task只能托管到`await asyncio.sleep(1.5)`的Future，而`mutli_task`创建的task则逃逸了，成为另一条链的顶点Task。

不过这个程序的事件循环只管理到了`main Task`所以事件循环会一直运行，直到`main Task`运行结束的时候才退出，这时程序会跟着一起退出，所以程序的运行时间只有1.5秒左右。此外由于另外的Task也是注册到这个事件循环上面，所以事件循环会帮忙把task\_1执行完毕，而task\_2定义的休眠时间是2秒，程序退出之前事件循环会发现有个Task尚未执行完毕，于是会对这个Task进行清理并打印一条警报。

## 4.总结

在深入了Task，Future的源码了解后，了解了Task和Future在`Asyncio`的作用，同时也发现Task和Future都跟loop有一定的耦合，而loop也可以通过一定的方法来创建Task和Future，所以如果要真正的理解到`Asyncio`的调度原理，还需要更进入一步，通过`Asyncio`的源码来了解整个`Asyncio`的设计.

#### 引用链接

`[1]` 《初识Python协程的实现》: *https://so1n.me/2021/11/08/%E5%88%9D%E8%AF%86Python%20Async%E7%9A%84%E5%AE%9E%E7%8E%B0/#3-%E5%9F%BA%E4%BA%8E%E7%94%9F%E6%88%90%E5%99%A8%E7%9A%84%E5%8D%8F%E7%A8%8B*
`[2]` Python3.4.10/Lib/asyncio: *https://github.com/python/cpython/tree/v3.4.10/Lib/asyncio*

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

[![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)](https://mp.weixin.qq.com/mp/appmsgalbum?__biz=MzkzMjMxMTg2NQ==&action=getalbum&album_id=2225391633284562944&scene=173&subscene=90&sessionid=1642653201&enterid=1642653281&from_msgid=2247483819&from_itemidx=1&count=3&nolastread=1#wechat_redirect)

[![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)](http://mp.weixin.qq.com/s?__biz=MzIzMzMzOTI3Nw==&mid=2247505072&idx=1&sn=3605fcc95b6c0c7b0b9ec5dd15480231&chksm=e885b452dff23d44649944d41a190d55955a9f8ea6987e1ed73363c10bf3711528f4f8524e8a&scene=21#wechat_redirect)

[![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)](http://mp.weixin.qq.com/s?__biz=MzIzMzMzOTI3Nw==&mid=2247505084&idx=1&sn=d3bc3a37cda2759c1a078ce9811fdec2&chksm=e885b45edff23d48744d42d9f6658cdddd2164af7042544815851a851dc710c70a1527a9b686&scene=21#wechat_redirect)

[![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)](http://mp.weixin.qq.com/s?__biz=MzIzMzMzOTI3Nw==&mid=2247505065&idx=1&sn=a8b98840693b8d57bc5422da6d68a25d&chksm=e885b44bdff23d5d93cd686803d091d4b280c6f8a33afcf9e38c3f92746a1c222d1b40a8ad9a&scene=21#wechat_redirect)

People who liked this content also liked

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

只需一个文件，Python 实现迷你 Web 框架！

...

Python猫

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___482859ec7431466b8.bmp"/>

Scan to Follow