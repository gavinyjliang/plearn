# 我发现了pandas的黄金搭档！

<a id="profileBt"></a><a id="js_name"></a>python数据分析之禅 *2022-03-24 12:33*

The following article is from Python大数据分析 Author 费弗里

<a id="copyright_info"></a>[![](../../../_resources/0_924bf348505349d4810c7c67d91c2a88.jpg)<br>**Python大数据分析** .<br>分享python编程、可视化设计、大数据分析、机器学习等技术以及数据分析案例，包括但不限于pandas、numpy、spark、matplotlib、sklearn、tensorflow、keras、tableau等](#)

> ❝
> 
> 本文示例代码及文件已上传至我的`Github`仓库https://github.com/CNFeffery/DataScienceStudyNotes
> 
> ❞

# 1 简介

`pandas`发展了如此多年，所包含的功能已经覆盖了大部分数据清洗、分析场景，但仍然有着相当一部分的应用场景`pandas`中尚存空白亦或是现阶段的操作方式不够简洁方便。

今天我要给大家介绍的`Python`库`pyjanitor`就内置了诸多功能方法，可以在兼容`pandas`中数据框等数据结构的同时为`pandas`补充更多功能。它是对`R`中著名的数据清洗包`janitor`的移植，就如同它的名字那样，帮助我们完成数据处理的清洁工作：

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__b8f61dc90e564f95b.png)

# 2 pyjanitor中的常用功能

对于使用`conda`的朋友，推荐使用下列命令完成`pyjanitor`的安装，其中使用到上海交大的`conda-forge`镜像：

```
`conda install pyjanitor -c https://mirrors.sjtug.sjtu.edu.cn/anaconda/cloud/conda-forge -y
`
```

完成安装后`import janitor`即可进行导入，接着我们就可以直接在`pandas`的代码逻辑中穿插`pyjanitor`的各种API接口。

`pyjanitor`中的很多功能实际上跟`pandas`中的一些功能存在重叠，作为一位`pandas`老手，这部分功能费老师我还是倾向于使用`pandas`完成，因此下面我只给大家介绍一些`pyjanitor`中颇具特色的功能：

## 2.1 利用also()方法穿插执行任意函数

熟悉`pandas`链式写法的朋友应该知道这种写法对于处理数据和理清步骤有多高效，`pyjanitor`中的`also()`方法允许我们在链式过程中随意插入执行任意函数，接受上一步状态的数据框运算结果，且不影响对下一步处理逻辑的数据输入，我非常喜欢这个功能，下面是一个简单的例子：

```
`df = (
    # 构造示例数据框
    pd.DataFrame({"a": [1, 2, 3], "b": list("abc")})
    .query("a > 1")
    # 利用also()插入lambda函数接受上一步的输入对象
    .also(lambda df: print(f"a字段<=1的记录有{df.query('a <= 1').shape[0]}行"))
    .rename(columns={'a': 'new_a'})
    # 利用also()实现中间计算结果的导出
    .also(lambda df: df.to_csv("temp.csv", index=False))
    # 利用also()打印到这一步时数据框计算结果的字段名
    .also(
        lambda df: print(f"字段名：{df.columns.tolist()}")
    )
    .drop(columns='b')
)
df
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 2.2 利用case_when()方法实现多条件分支

`pyjanitor`中的`case_when()`方法可以帮助我们针对数据框实现类似`SQL`中的的多条件分支运算，注意，因为是多条件分支，所以包含最后的“其他”条件在内，需要至少定义3条分支规则，参考下面的例子：

```
`df = pd.DataFrame(
    {
        "a": [0, 0, 1, 2],
        "b": [0, 3, 4, 5],
        "c": [6, 7, 8, 9],
    }
)
df.case_when(
    ((df.a == 0) & (df.b == 0)), '类别1',
    ((df.a == 0) & (df.b != 0)), '类别2',
    # 其他情况
    '类别3',
    column_name="类别",
)
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 2.3 利用conditional_join()实现条件连接

`pyjanitor`中的`conditional_join()`非常地好用，它弥补了`pandas`一直以来都未完善的“条件连接”功能，即我们对两张表进行**「连接」**的条件，不只`pandas`中的`merge()`、`join()`之类的方法所实现的，左表与右表的指定字段之间`相等`这样简单的条件判断，而是可高度自定义的条件判断。

`conditional_join()`在作为方法使用时，其第一个参数应传入连接中的**「右表」**数据框，紧接着的是若干个格式为`(左表字段, 右表字段, 判断条件)`这样的三元组来定义单条或多条条件判断的**「且」**组合，之后再用于定义连接方式`how`参数。

下面是一个示例，这里我们实现生信中常见的一种数据分析操作，左表和右表各自定义了一些区间段，我们利用条件连接来为左表找到右表中完全被其包住的区间：

```
`# 定义示例左表
df_left = pd.DataFrame({
    'id': list('abcd'),
    'left_range_start': [2, 9, 14, 30],
    'left_range_end': [5, 11, 21, 35]
})
# 定义示例右表
df_right = pd.DataFrame({
    'id': list('ijxy'),
    'right_range_start': [2, 6, 15, 28],
    'right_range_end': [3, 10, 18, 31]
})
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

进行条件连接：

```
`(
    df_left
    .conditional_join(
        df_right,
        # 满足left_range_start <= right_range_start
        ('left_range_start', 'right_range_start', '<='),
        # 且满足left_range_end >= right_range_end
        ('left_range_end', 'right_range_end', '>=')
    )
)
`
```

连接结果如下：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 2.4 利用move()方法快捷完成字段位置调整

`pyjanitor`中的`move()`方法用于快捷调整某行或某列数据的位置，通过`source`参数指定需要移动的数据行`index`或列的字段名，`target`参数用于指定移动的目标位置数据行`index`或列的字段名，`position`用于设置移动方式（`'before'`表示移动到目标之前一个位置，`after`表示后一个位置），`axis`用于设定移动方式（`0`表示行移动，`1`表示列移动）。

以最常用的列移动为例：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

而除了上述这些颇具特色的功能外，`pyjanitor`中还针对生信、化学、金融、机器学习、数学等领域内置了一些特别的功能，感兴趣的朋友可以前往其官网`https://pyjanitor-devs.github.io/pyjanitor/`进一步了解相关内容。

* * *

以上就是本文的全部内容，欢迎在评论区与我进行讨论~

END

People who liked this content also liked

分享 63 个面向前端开发人员的开源项目工具

前端达人

不看的原因

- 内容质量低
- 不看此公众号

pandas 文本处理大全（附代码）

Python数据科学

不看的原因

- 内容质量低
- 不看此公众号

Splunk系列：Splunk字段提取篇（三）

Bypass

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___9ccca80e0cdc43258.bmp"/>

Scan to Follow