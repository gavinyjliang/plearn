# Python 中有 3 个不可思议的返回功能

<a id="profileBt"></a><a id="js_name"></a>Python工程师 *2022-02-28 08:14*

<a id="js_article-tag-card__left"></a>收录于话题 #python <a id="js_article-tag-card__right"></a>11个

今天给大家分享 3 个比较冷门的知识

## 第一个：神奇的字典键

```
some_dict = {}
some_dict[5.5] = "Ruby"
some_dict[5.0] = "JavaScript"
some_dict[5] = "Python"

```

Output:

```
>>> some_dict[5.5]
"Ruby"
>>> some_dict[5.0]
"Python"
>>> some_dict[5]
"Python"

```

"Python" 消除了 "JavaScript" 的存在？

#### 💡 说明:

- Python 字典通过检查键值是否相等和比较哈希值来确定两个键是否相同.
    
- 具有相同值的不可变对象在Python中始终具有相同的哈希值.
    

注意: 具有不同值的对象也可能具有相同的哈希值（哈希冲突）.

```
>>> 5 == 5.0
True
>>> hash(5) == hash(5.0)
True

```

当执行 `some_dict[5] = "Python"` 语句时, 因为Python将 `5` 和 `5.0` 识别为 `some_dict` 的同一个键, 所以已有值 "JavaScript" 就被 "Python" 覆盖了

## 第二个：异常处理中的return

```
def some_func():
    try:
        return 'from_try'
    finally:
        return 'from_finally'

```

Output:

```
>>> some_func()
'from_finally'

```

#### 💡 说明:

- 当在 "try…finally" 语句的 `try` 中执行 `return`, `break` 或 `continue` 后, `finally` 子句依然会执行.
    
- 函数的返回值由最后执行的 `return` 语句决定. 由于 `finally` 子句一定会执行, 所以 `finally` 子句中的 `return` 将始终是最后执行的语句.
    

## 第三个：相同对象的判断

```
class WTF:
  pass

```

Output:

```
>>> WTF() == WTF() # 两个不同的对象应该不相等
False
>>> WTF() is WTF() # 也不相同
False
>>> hash(WTF()) == hash(WTF()) # 哈希值也应该不同
True
>>> id(WTF()) == id(WTF())
True

```

#### 💡 说明:

- 当调用 `id` 函数时, Python 创建了一个 `WTF` 类的对象并传给 `id` 函数. 然后 `id` 函数获取其id值 (也就是内存地址), 然后丢弃该对象. 该对象就被销毁了.
    
- 当我们连续两次进行这个操作时, Python会将相同的内存地址分配给第二个对象. 因为 (在CPython中) `id` 函数使用对象的内存地址作为对象的id值, 所以两个对象的id值是相同的.
    
- 综上, 对象的id值仅仅在对象的生命周期内唯一. 在对象被销毁之后, 或被创建之前, 其他对象可以具有相同的id值.
    
- 那为什么 `is` 操作的结果为 `False` 呢? 让我们看看这段代码.
    

```
class WTF(object):
  def __init__(self): print("I")
  def __del__(self): print("D")

```

Output:

```
>>> WTF() is WTF()
I
I
D
D
False
>>> id(WTF()) == id(WTF())
I
D
I
D
True

```

正如你所看到的, 对象销毁的顺序是造成所有不同之处的原因.

原文链接：https://github.com/leisurelicht/wtfpython-cn

以上就是今天的分享，如果你觉得文章还不错，请大家 **点赞、分享、留言** 下，因为这将是我持续输出更多优质文章的最强动力！

\- EOF -

推荐阅读  点击标题可跳转

<ins>1、[10行python代码做出哪些酷炫的事情？](http://mp.weixin.qq.com/s?__biz=MzkzMDMyMzIzNw==&mid=2247483810&idx=1&sn=d16480fb7cc186b65fe7149e0536f780&chksm=c27d4fe8f50ac6fe0500bc66b9297a668011661a84c374fc16cf17c1a999d860e3ad7e37b98b&scene=21#wechat_redirect)</ins>

<ins>2、</ins><ins>相见恨晚的 Python 内置库：itertools</ins>

<ins>3、</ins><ins>用Python开发了3个有趣的小游戏，上班摸鱼再也不无聊了</ins>

觉得本文对你有帮助？请分享给更多人

点赞和在看就是最大的支持❤️

People who liked this content also liked

Go语言学习笔记05 -- 复合数据类型之数组与切片

奶昔的学习笔记

不看的原因

- 内容质量低
- 不看此公众号

Python基础(下)

啊颜的学习场所

不看的原因

- 内容质量低
- 不看此公众号

9个好用的python技巧分享

AI算法之道

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___d5217cdd384149eb9.bmp"/>

Scan to Follow