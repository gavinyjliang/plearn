# 如何将 Python 数据管道的速度提高到 91 倍？

Murallie <a id="profileBt"></a><a id="js_name"></a>AI前线 *2021-10-19 13:30*

<img width="563" height="375" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_75f044234f2c4862b.jpg"/>

作者 | Thuwarakesh Murallie

译者 | Sambodhi

策划 | 刘燕

数据科学家们最大的烦恼就是等待大数据管道的完成。

虽然 Python 是数据科学家的浪漫语言，但是它速度还不够快。这个脚本语言是在执行时进行解释的，这使它变慢，并且难以并行执行。遗憾的是，并非所有数据科学家都是 C++ 专家。

假如有一种 Python 代码以并行执行的方式运行，并以编译代码的速度运行，该怎么办？那是 Tuplex 要解决的问题。

Tuplex 是用 Python 编写的并行大数据处理框架。如果你使用过 Apache Spark，你可能对此比较熟悉。但是，不像 Spark，Tuplex 不会调用 Python 解释器。该算法优化管道，并将其转换成 LLVM 字节码，运行速度极快，与手工优化的 C++ 代码一样快。

Python 使用 multiprocessing（多处理）库来并行化执行。这个库的缺点在于它无法在任何 REPL 环境中工作。但是，我们的数据科学家喜欢 Jupyter Notebook。实际上，multiprocessing 根本就不是并行执行技术。这只是多个子进程的启动，而操作系统负责进程的并行执行。事实上，无法保证操作系统允许它们并行运行。

本文将讨论：

- 怎样安装 Tuplex。
    
- 怎样运行简单的数据管道。
    
- Tuplex 中方便的异常处理。
    
- 高级配置是如何提供帮助的。
    
- 对照通常的 python 代码进行基准测试。
    

我敢肯定这会是一件容易的事。

使用 Tuplex 开始运行

虽然 Tuplex 很有用，但是设置它非常简单。PyPI 就是这样。

```
pip install tuplex

```

尽管 Linux 推荐使用此方法，但在 Mac 上你可能必须使用 docker 容器。

需要注意的是，它还没有在 Windows PC 上进行过测试。至少 Tuplex 的文档 没有提到这一点。如果你有幸在 Windows PC 上测试过，请与我们分享。

使用 Tuplex 的第一个数据管道

一旦你安装了 Tuplex，运行一个并行任务就很容易了。下面是 Tuplex 官方文档页面上的示例。

quickstart.py :

```
from tuplex import *
c = Context ( )
# access elements via tuple syntax
# will print [11,22,33]
c.parallelize ([( 1,10 ) , ( 2,20 ) , ( 3,30 )] ) \
 .map ( lambda x: x [0] + x [1] ) \
 .collect ( )

```

在开始的时候，你必须创建一个 Tuplex 上下文（context）。通过从 Tuplex 模块导入，你可以完成此操作。

从这里开始，运行并行函数执行只需要三个步骤：并行化（parallelize）、映射（map）和收集（collect）。

Tuplex context 对象的 `parallelize` 方法是你的起点。它以函数的输入值列表作为参数。这个列表中的每个元素都将与其他元素并行地在函数中运行。

你可以传递一个用户定义的函数，使用 map 函数对每个输入进行转换。最后，使用 `collect` 方法收集所有并行执行的输出。

Tuplex 中方便的异常处理

我喜欢 Tuplex 的一点就是，它可以轻松地管理异常。在数据管道中的错误处理是一种可怕的经历。想象一下，你花了几个小时来处理一个数据流，却发现了一个细微的“被零除”（division by zero）错误， 这会让你的所作所为化为乌有。

error_unhandled.py：

```
from tuplex import *
c = Context()
c.parallelize([(1, 0), (2, 1), (3, 0), (4, -1)]) \
 .map(lambda x, y: x / y) \
 .collect()

```

上面的代码会引发一个“被零除”的错误。至少，如果你使用 Spark 或任何标准 Python 模块进行处理，至少会出现这种情况。

错误处理是 Tuplex 中的一种自动操作。它将忽略有错误的那一个，并返回其他的。上面的代码将返回 \[2,-4\]，因为不能执行列表中的第一个和第三个输入。

然而，有时候忽略错误是有问题的。你经常需要用不同的方法来处理它们，而 Tuplex 的 API 非常灵活，足以完成此任务。实际上， Tuplex 方法非常方便。

error_handling.py：

```
from tuplex import *
c = Context()
c.parallelize([(1, 0), (2, 1), (3, 0), (4, -1)]) \
 .map(lambda x, y: x / y) \
 .resolve(ZeroDivisionError, lambda a, b: 0) \
 .collect()

```

Tuplex 可以轻松地进行错误处理。你只需将一个 `resolve` 方法链接到 `map` 和 `collect` 方法之间。对于上例，我们传递了 ZeroDivisionError 类型，然后通过替换零传递它。

`resolve` 方法的第二个参数是一个函数。通过这个函数，你可以告诉 Tuplex 在出现错误类型时如何处理。

为高级用例配置 Tuplex

有两种方式可以配置 Tuplex。第一种是直接的解决方案；只需将字典传递到 Context 初始化即可。下面是一个将执行内存设置为一个更高的值的示例。

passing\_config\_dict.py：

```
from tuplex import *
c = Context(executorMemory="2G")

```

Tuplex 还支持通过 YAML 文件传递配置。你可能需要将配置存储在生产环境中的文件中。YAML 文件是一种处理不同配置以及在开发和测试团队之间传递的极佳方法。

passing\_config\_yaml.py：

```
from tuplex import *
c = Context(conf="/conf/tuplex.yaml")

```

下面是一个配置文件的示例，其中包含了你可以从 Tuplex 文档中完成的各种定制。

config.yaml：

```
# FastETL configuration file
#     created 2019-02-17 16:45:09.940033 UTC
tuplex:
    -   allowUndefinedBehavior: false
    -   autoUpcast: false
    -   csv:
            -   comments: ["#", "~"]
            -   generateParser: true
            -   maxDetectionMemory: 256KB
            -   maxDetectionRows: 100
            -   quotechar: "\""
            -   selectionPushdown: true
            -   separators: [",", ;, "|", "\t"]
    -   driverMemory: 1GB
    -   executorCount: 4
    -   executorMemory: 1GB
    -   logDir: .
    -   normalcaseThreshold: 0.9
    -   partitionSize: 1MB
    -   runTimeLibrary: tuplex_runtime
    -   runTimeMemory: 32MB
    -   runTimeMemoryBlockSize: 4MB
    -   scratchDir: /tmp
    -   useLLVMOptimizer: true

```

性能基准测试

Tuplex 的承诺耐人寻味。现在是时候看看它的性能提升情况了。

在这个基准测试中，我使用了这个简单的素数计数器函数。先用 for 循环来运行这个函数，然后使用 Python 内置的 multiprocessing 模块，最后使用 Tuplex。

count_primes.py：

```
def count_primes(max_num):
    """This function counts of prime numbers below the input value.
    Input values are in thousands, ie. 40, is 40,000.
    """
    count = 0
    for num in range(max_num * 1000 + 1):
        if num > 1:
            for i in range(2, num):
                if num % i == 0:
                    break
                else:
                    count += 1
    return count

```

在标准 Python 中执行密集型任务。

在 for 循环中运行函数最简单。我使用 Jupyter Notebook 的 `%time' 助手程序来跟踪执行时间。

for_count.py：

```
%%time
for val in [10, 20, 30, 40]:
    print(count_primes(val))

```

执行多次以上代码，平均完成时间为 51.2 秒。

在 for 循环执行中，执行速度较慢是可以预料的。但是让我们尝试一下 Python 内置的 multiprocessing 模块。无法在 Jupyter Notebook 等 REPL 上运行以下代码。你必须把它放在一个 .py 文件中，并 在命令行中执行。

multiprocessing.py：

```
from multiprocessing import Pool
from datetime import datetime
def count_primes(max_num):
    """This function counts of prime numbers below the input value.
    Input values are in thousands, ie. 40, is 40,000.
    """
    count = 0
    for num in range(max_num * 1000 + 1):
        if num > 1:
            for i in range(2, num):
                if num % i == 0:
                    break
                else:
                    count += 1
    return count
if __name__ == "__main__":
    start_time = datetime.now()
    with Pool(5) as p:
        print(p.map(count_primes, [10, 20, 30, 40]))
    end_time = datetime.now()
    print(f"It took {end_time - start_time} to run")

```

执行此 multiprocessing 脚本的平均耗时为 30.76 秒。相对于 for 循环方式，减少了 20.44 秒。

Tuplex 并行处理密集型任务。

最后，我们执行相同的素数计数函数，这次是用 Tuplex。下面这段简洁的代码平均花了 0.000040 秒，也产生了同样的结果。

tuplex.py：

```
from tuplex import *
c = Context()
c.parallelize([10, 20, 30, 40]).map(count_primes).collect()

```

Tuplex 在性能方面比其他标准 Python 方法有很大提高。这是一个简单的例子，执行时间比 multiprocessing 短 769000 倍，比普通的 for 循环快 1280000 倍。

我们将坚持 Tuplex 团队的承诺，即 5~91 倍。尽管如此，Tuplex 敦促我在再写一个 for 循环之前三思而行。

结语

Tuplex 是一个易于设置的 Python 包，可以节省你很多时间。它通过将数据管道转换为字节码，并并行执行，从而加快了数据管道的速度。

性能基准表明，它对代码执行的改进意义重大。不过，它的设置很简单，其语法和配置也非常灵活。

Tuplex 最酷的地方在于它方便地异常处理。在数据管道中的错误处理从未如此简单。它很好地结合了交互式外壳和 Jupiter Notebook。这种情况对于编译语言而言并不常见。就连 Python 本身也没有能力在 Jupyter Notebook 这样的 REPL 里面处理并行处理。

**作者介绍：**

Thuwarakesh Murallie，Medium 博客写手，志在简化数据科学和生活。

**原文链接：**

https://towardsdatascience.com/how-to-speed-up-python-data-pipelines-up-to-91x-80d7accfe7ec

<img width="661" height="367" src="../../../_resources/640_wx_fmt_gif_wxfrom_5_wx_lazy__5db09493b89a4df8b.gif"/>

**你也「在看」吗？**👇

People who liked this content also liked

在Python中使用Greppo构建的地理空间仪表板

大邓和他的Python

不看的原因

- 内容质量低
- 不看此公众号

天啦！从 MongoDB 到 ClickHouse，我竟然经历了这些。。。

高效运维

不看的原因

- 内容质量低
- 不看此公众号

【进阶】嫌弃Python慢，试试这几个方法？

深度学习算法与计算机视觉

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___ecbd46eef62b47f49.bmp"/>

Scan to Follow