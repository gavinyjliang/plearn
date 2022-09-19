# 18 个 Python 坏习惯，你一定要抛弃

<a id="profileBt"></a><a id="js_name"></a>早起Python *2021-11-24 10:24*

The following article is from Python七号 Author somenzz

<a id="copyright_info"></a>[![](../../../_resources/0_82844e8ad32045279cccc61c50c11078.jpg)<br>**Python七号** .<br>一个玩鲁班七号的程序员，带你玩转 Python。专注于分享 Python 编程和实战，关注后回复【2048】，获取我珍藏的学习资源，交个朋友，一起用 Python 超神。](#)

大家好，我是早起。

今天分享 18 个 Python 坏习惯，这些坏习惯会暴露开发者在 Python 方面经验不足。通过摒弃这些习惯并以 Pythonic 的方式编写代码，可以提高你的代码质量，给看代码的人留下好印象。

## 1、拼接字符串用 + 号

坏的做法：

```
def manual_str_formatting(name, subscribers):
    if subscribers > 100000:
        print("Wow " + name + "! you have " + str(subscribers) + " subscribers!")
    else:
        print("Lol " + name + " that's not many subs")

```

好的做法是使用 f-string，而且效率会更高：

```
def manual_str_formatting(name, subscribers):
    # better
    if subscribers > 100000:
        print(f"Wow {name}! you have {subscribers} subscribers!")
    else:
        print(f"Lol {name} that's not many subs")

```

## 2、使用 finaly 而不是上下文管理器

坏的做法：

```
def finally_instead_of_context_manager(host, port):
    s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    try:
        s.connect((host, port))
        s.sendall(b'Hello, world')
    finally:
        s.close()

```

好的做法是使用上下文管理器，即使发生异常，也会关闭 socket：：

```
def finally_instead_of_context_manager(host, port):
    # close even if exception
    with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as s:
        s.connect((host, port))
        s.sendall(b'Hello, world')

```

## 3、尝试手动关闭文件

坏的做法:

```
def manually_calling_close_on_a_file(filename):
    f = open(filename, "w")
    f.write("hello!\n")
    f.close()

```

好的做法是使用上下文管理器，即使发生异常，也会自动关闭文件，凡是有上下文管理器的，都应该首先采用：

```
def manually_calling_close_on_a_file(filename):
    with open(filename) as f:
        f.write("hello!\n")
    # close automatic, even if exception

```

## 4、except 后面什么也不写

坏的做法：

```
def bare_except():
    while True:
        try:
            s = input("Input a number: ")
            x = int(s)
            break
        except:  # oops! can't CTRL-C to exit
            print("Not a number, try again")

```

这样会捕捉所有异常，导致按下 CTRL-C 程序都不会终止，好的做法是

```
def bare_except():
    while True:
        try:
            s = input("Input a number: ")
            x = int(s)
            break
        except Exception:  # 比这更好的是用 ValueError
            print("Not a number, try again")

```

## 5、函数参数使用可变对象

如果函数参数使用可变对象，那么下次调用时可能会产生非预期结果，坏的做法

```
def mutable_default_arguments():
    def append(n, l=[]):
        l.append(n)
        return l
    l1 = append(0)  # [0]
    l2 = append(1)  # [0, 1]

```

好的做法:

```
def mutable_default_arguments():
    def append(n, l=None):
        if l is None:
            l = []
        l.append(n)
        return l
    l1 = append(0)  # [0]
    l2 = append(1)  # [1]

```

## 6、从不用推导式

坏的做法

```
squares = {}
for i in range(10):
    squares[i] = i * i

```

好的做法

```
odd_squares = {i: i * i for i in range(10)}

```

## 7、推导式用的上瘾

推导式虽然好用，但是不可以牺牲可读性，坏的做法

```
c = [
    sum(a[n * i + k] * b[n * k + j] for k in range(n))
    for i in range(n)
    for j in range(n)
]

```

好的做法:

```
c = []
for i in range(n):
    for j in range(n):
        ij_entry = sum(a[n * i + k] * b[n * k + j] for k in range(n))
        c.append(ij_entry)

```

## 8、检查类型是否一致用 ==

坏的做法

```
def checking_type_equality():
    Point = namedtuple('Point', ['x', 'y'])
    p = Point(1, 2)
    if type(p) == tuple:
        print("it's a tuple")
    else:
        print("it's not a tuple")

```

好的做法

```
def checking_type_equality():
    Point = namedtuple('Point', ['x', 'y'])
    p = Point(1, 2)
    # probably meant to check if is instance of tuple
    if isinstance(p, tuple):
        print("it's a tuple")
    else:
        print("it's not a tuple")

```

## 9、用 == 判断是否单例

坏的做法

```
def equality_for_singletons(x):
    if x == None:
        pass
    if x == True:
        pass
    if x == False:
        pass

```

好的做法

```
def equality_for_singletons(x):
    # better
    if x is None:
        pass
    if x is True:
        pass
    if x is False:
        pass

```

## 10、判断一个变量用 bool(x)

坏的做法

```
def checking_bool_or_len(x):
    if bool(x):
        pass
    if len(x) != 0:
        pass

```

好的做法

```
def checking_bool_or_len(x):
    # usually equivalent to
    if x:
        pass

```

## 11、使用类 C 风格的 for 循环

坏的做法

```
def range_len_pattern():
    a = [1, 2, 3]
    for i in range(len(a)):
        v = a[i]
        ...
    b = [4, 5, 6]
    for i in range(len(b)):
        av = a[i]
        bv = b[i]
        ...

```

好的做法

```
def range_len_pattern():
    a = [1, 2, 3]
    # instead
    for v in a:
        ...
    # or if you wanted the index
    for i, v in enumerate(a):
        ...
    # instead use zip
    for av, bv in zip(a, b):
        ...

```

## 12、不实用 dict.items

坏的做法

```
def not_using_dict_items():
    d = {"a": 1, "b": 2, "c": 3}
    for key in d:
        val = d[key]
        ...

```

好的做法

```
def not_using_dict_items():
    d = {"a": 1, "b": 2, "c": 3}
    for key, val in d.items():
        ...

```

## 13、解包元组使用索引

坏的做法

```
mytuple = 1, 2
x = mytuple[0]
y = mytuple[1]

```

好的做法

```
mytuple = 1, 2
x, y = mytuple

```

## 14、使用 time.time() 统计耗时

坏的做法

```
def timing_with_time():
    start = time.time()
    time.sleep(1)
    end = time.time()
    print(end - start)

```

好的做法是使用 time.perf_counter()，更精确：

```
def timing_with_time():
   # more accurate
    start = time.perf_counter()
    time.sleep(1)
    end = time.perf_counter()
    print(end - start)

```

## 15、记录日志使用 print 而不是 logging

坏的做法

```
def print_vs_logging():
    print("debug info")
    print("just some info")
    print("bad error")

```

好的做法

```
def print_vs_logging():
    # versus
    # in main
    level = logging.DEBUG
    fmt = '[%(levelname)s] %(asctime)s - %(message)s'
    logging.basicConfig(level=level, format=fmt)
    # wherever
    logging.debug("debug info")
    logging.info("just some info")
    logging.error("uh oh :(")

```

## 16、调用外部命令时使用 shell=True

坏的做法

```
subprocess.run(["ls -l"], capture_output=True, shell=True)

```

如果 shell=True，则将 `ls -l` 传递给/bin/sh(shell) 而不是 Unix 上的 ls 程序，会导致 subprocess 产生一个中间 shell 进程， 换句话说，使用中间 shell 意味着在命令运行之前，命令字符串中的变量、glob 模式和其他特殊的 shell 功能都会被预处理。比如，$HOME 会在在执行 echo 命令之前被处理处理。

好的做法是拒绝从 shell 执行：

```
subprocess.run(["ls", "-l"], capture_output=True)

```

## 17、从不尝试使用 numpy

坏的做法

```
def not_using_numpy_pandas():
    x = list(range(100))
    y = list(range(100))
    s = [a + b for a, b in zip(x, y)]

```

好的做法:

```
import numpy as np
def not_using_numpy_pandas():
    # 性能更快
    x = np.arange(100)
    y = np.arange(100)
    s = x + y

```

## 18、喜欢 import *

坏的做法

```
from itertools import *
count()

```

这样的话，没有人直到这个脚本到底有多数变量， 好的做法：

```
from mypackage.nearby_module import awesome_function
def main():
    awesome_function()
if __name__ == '__main__':
    main()

```

**点击下载「pandas进阶修炼300题」👇**

[![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)](http://mp.weixin.qq.com/s?__biz=Mzg5OTU3NjczMQ==&mid=2247519402&idx=1&sn=535dddd429bf383a1eda9f0316507ec4&chksm=c053ea5ef72463488c4fdd99282d9d4d3922c0ad9fe034865f351d4ec19b375672b3e41350ec&scene=21#wechat_redirect)

* * *

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

People who liked this content also liked

使用python绘制散点密度图（用颜色标识密度）

Chemocoder

不看的原因

- 内容质量低
- 不看此公众号

Python其实很简单！从零基础到大佬，超详细知识点汇总，附教程

编程学习部

不看的原因

- 内容质量低
- 不看此公众号

Python unittest单元测试框架及断言方法

软件测试unittest

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___fe63bd377ddc48b1b.bmp"/>

Scan to Follow