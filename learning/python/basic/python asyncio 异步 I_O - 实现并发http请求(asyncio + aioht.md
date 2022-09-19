# python asyncio 异步 I/O - 实现并发http请求(asyncio + aiohttp)

<a id="copyright_logo"></a>Original 上海悠悠 <a id="profileBt"></a><a id="js_name"></a>从零开始学自动化测试 *2022-03-01 09:00*

<a id="js_article-tag-card__left"></a>收录于话题 #协程 <a id="js_article-tag-card__right"></a>2个

# 前言

如果需要并发 http 请求怎么办呢？requests库是同步阻塞的，必须等到结果才会发第二个请求，这里需使用http请求异步库 aiohttp。

# 环境准备

aiohttp 用于 asyncio 和 Python 的异步 HTTP 客户端/服务器。
使用pip安装对应的包。当前使用版本v3.8.1

```
pip install aiohttp
```

# 并发http请求

如果使用requests 库，发10个请求访问我的博客，那么这10个请求是串行的。

```
import requests
import time
url = "https://www.cnblogs.com/yoyoketang/"
start_time = time.time()
for i in range(10):
    r = requests.get(url)
    print(r)
print('总耗时：', time.time()-start_time)
```

运行结果,总共耗时1秒左右

```
<Response [200]>
<Response [200]>
<Response [200]>
<Response [200]>
<Response [200]>
<Response [200]>
<Response [200]>
<Response [200]>
<Response [200]>
<Response [200]>
总耗时：1.0870192050933838
```

使用asyncio + aiohttp 并发请求

```
import asyncio
from aiohttp import ClientSession
import time
async def bai_du(url):
    print(f'启动时间: {time.time()}')
    async with ClientSession() as session:
        async with session.get(url) as response:
            res = await response.text()
            return res
async def main():
    url = "https://www.cnblogs.com/yoyoketang/"
    task_list = []
    for i in range(10):
        task = asyncio.create_task(bai_du(url))
        task_list.append(task)
    done, pending = await asyncio.wait(task_list, timeout=None)
    # 得到执行结果
    for done_task in done:
        print(f"{time.time()} 得到执行结果 {done_task.result()}")
# asyncio.run(main())
start_time = time.time()
loop = asyncio.get_event_loop()
loop.run_until_complete(main())
print("总耗时: ", time.time()-start_time)
```

运行结果

```
启动时间: 1646028822.3952098
启动时间: 1646028822.4161537
启动时间: 1646028822.4171515
启动时间: 1646028822.4171515
启动时间: 1646028822.4171515
启动时间: 1646028822.4171515
启动时间: 1646028822.4181483
启动时间: 1646028822.4181483
启动时间: 1646028822.4181483
启动时间: 1646028822.4181483
....
总耗时:  0.17400002479553223
```

从运行结果可以看到，启动时间非常接近，也就是并发的请求，总过耗时 0.17 秒。

# asyncio.run

需注意的是这里使用 `asyncio.run(main())` 会报错RuntimeError: Event loop is closed

```
Exception ignored in: <function _ProactorBasePipeTransport.__del__ at 0x00000238675A8F70>
Traceback (most recent call last):
  File "D:\python3.8\lib\asyncio\proactor_events.py", line 116, in __del__
    self.close()
  File "D:\python3.8\lib\asyncio\proactor_events.py", line 108, in close
    self._loop.call_soon(self._call_connection_lost, None)
  File "D:\python3.8\lib\asyncio\base_events.py", line 719, in call_soon
    self._check_closed()
  File "D:\python3.8\lib\asyncio\base_events.py", line 508, in _check_closed
    raise RuntimeError('Event loop is closed')
RuntimeError: Event loop is closed
```

解决办法，把执行方式 `asyncio.run(main())`改成

```
# asyncio.run(main())
loop = asyncio.get_event_loop()
loop.run_until_complete(main())
```

注意原因是asyncio.run()会自动关闭循环,并且调用*ProactorBasePipeTransport.*_del**报错, 而asyncio.run\_until\_complete()不会.
详情参考https://zhuanlan.zhihu.com/p/365815189

[2022年第 10 期《python接口web自动化+测试开发》课程，2月13号开学!](http://mp.weixin.qq.com/s?__biz=MzI5ODU1MzkwMA==&mid=2247489884&idx=1&sn=03c8083c94645360d7b8ca18c116d36b&chksm=eca55e1fdbd2d709e5fb1145f1d68cf0ce7a358d0516005250f0eae0502b3ab0b6f1249dfe42&scene=21#wechat_redirect)

加量不加价（**新增postman, 赠送selenium和python基础2个课**）

本期上课时间：**2月13号**-5月21号，每周六、周日晚上20:30-22:30

联系微信/QQ：283340479

<a id="js_view_source"></a>Read more

People who liked this content also liked

有了这篇 Docker 网络原理，彻底爱了~

高效运维

不看的原因

- 内容质量低
- 不看此公众号

前端竟然用Golang 动态生成图片？

前端技术江湖

不看的原因

- 内容质量低
- 不看此公众号

FastAPI学习-6.POST请求 JSON 格式 body

从零开始学自动化测试

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___e69b5b9910fa46fb8.bmp"/>

Scan to Follow