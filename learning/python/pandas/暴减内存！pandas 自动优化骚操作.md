# 暴减内存！pandas 自动优化骚操作

<a id="copyright_logo"></a>Original 东哥起飞 <a id="profileBt"></a><a id="js_name"></a>Python数据科学 *2021-11-07 23:22*

<a id="js_article-tag-card__left"></a>收录于合集 #pandas骚操作 <a id="js_article-tag-card__right"></a>25个

大家好，我是东哥。

本篇是pandas骚操作系列的第 **24** 篇：自动优化数据类型，暴省内存！

系列内容，请看👉「[pandas骚操作](https://mp.weixin.qq.com/mp/appmsgalbum?__biz=MzUzODYwMDAzNA==&action=getalbum&album_id=1699019347278561282&scene=21#wechat_redirect)」话题，订阅后文章更新可第一时间推送至订阅号。内容也同步我的GitHub，欢迎star！

> https://github.com/xiaoyusmd/PythonDataScience

* * *

平日工作里经常会听到周边小伙伴说：**我X，内存又爆了！**

对于这样的话我听了不下百遍。正因为如此，在资源有限的情况下，我们都是变着法的减少内存占用，一些常用的方法如：

1.  `gc.collect`和`del`回收
    
2.  使用`csv`的替代品，如[feather](https://mp.weixin.qq.com/s?__biz=MzUzODYwMDAzNA==&mid=2247543807&idx=1&sn=9977c51c32af6118c765a0adf20905a3&chksm=fad75cf2cda0d5e4584e23f931e12d0292db6eb34e5e1aaaaaf4a31dfc6099ab294b9340e079&token=2078782947&lang=zh_CN&scene=21#wechat_redirect)、`Parquet`
    
3.  优化代码，[尽量使用Numpy矩阵代替for循环和apply](https://mp.weixin.qq.com/s?__biz=MzUzODYwMDAzNA==&mid=2247517636&idx=2&sn=4225c9dcbd9604453620be5bbe6b76f8&chksm=fad7f2c9cda07bdf4b759ec1ac0e0c168538fc570c1c687679ffd1b54bfb850f2b8e0d4cd371&token=2078782947&lang=zh_CN&scene=21#wechat_redirect)
    
4.  ...
    

本次再分享一个骚操作，就是通过改变数据类型来压缩内存空间。之前也和大家介绍过[category](https://mp.weixin.qq.com/s?__biz=MzUzODYwMDAzNA==&mid=2247529624&idx=1&sn=472958500eb54f607b54b20cfdf9a6f6&chksm=fad70395cda08a83e4c95dbea4ba616df44a7db6025ad2ab811ae52ceded3d293960feec5257&token=2078782947&lang=zh_CN&scene=21#wechat_redirect)类型，也可以减少一些内存占用。和这个方法一样，我们可以延伸到所有数据类型。

正常情况下，`pandas` 会给数据列自动设置默认的数据类型，其中最令人讨厌并且最消耗内存的数据类型就是`object(O)`，这也恰好限制了 `pandas` 的一些功能。下面是 `pandas` 、`Python`、`Numpy`的数据类型列表，对比你就发现`pandas`的数据类型是有很大优化空间的。

| Pandas dtype | Python type | NumPy type | Usage |
| --- | --- | --- | --- |
| object | str | string_,unicode | Text |
| int64 | int | int,int8,intl6,int32,int64,uint8,uint16,uint32,uint64 | Integer numbers |
| float64 | float | float,float16,float32,float64 | Floating point numbers |
| bool | bool | bool_ | True/False values |
| datetime64 | NA  | datetime64\[ns\] | Date and time values |
| timedelta\[ns\] | NA  | NA  | Differences between two datetimes |
| category | NA  | NA  | Finite list of text values |

> 来源：http : //pbpython.com/pandas_dtypes.html

很多默认的数据类型占用很多内存空间，其实根据没有必要，我们完全可以压缩到可能小的子类型。

| Data type | Description |
| --- | --- |
| bool_ | Boolean(True or False) stored as a byte |
| int_ | Default integer type(same as C 1ong ; normally either int64or int32) |
| intc | ldentical to C int(normally int32 or int64) |
| intp | Integer used for indexing(same as C ssize_t; normally either int32 or int64) |
| int | 8Byte(-128 to 127) |
| int16 | Integer(-32768 to 32767) |
| int32 | Integer(-2147483648 to 2147483647) |
| int64 | Integer(-9223372036854775808 to 9223372036854775807) |
| uint8 | Unsigned integer(0 to 255) |
| uint16 | Unsigned integer(0 to 65535) |
| uint32 | Unsigned integer(0 to 4294967295) |
| uint64 | Unsigned integer(0 to 18446744073709551615) |
| float_ | Shorthand for float64. |
| float16 | Half precision float: sign bit,5 bits exponent,10 bits mantissa |
| float32 | Single precision float: sign bit,8 bits exponent,23 bits mantissa |
| float64 | Double precision float: sign bit,11 bits exponent,52 bits mantissa |
| complex_ | Shorthand for complex128. |
| complex64 | Complex number, represented by two 32-bit floats(real and imaginary components) |
| complex128 | Complex number, represented by two 64-bit floats(real and imaginary components) |

> 来源：https : //docs.scipy.org/doc/numpy-1.13.0/user/basics.types.html

上面是`scipy`文档中列出的所有数据类型，从简单到复杂。我们希望将类型简单化，以此节省内存，比如将浮点数转换为`float16/32`，或者将具有正整数和负整数的列转为`int8/16/32`，还可以将布尔值转换为`uint8`，甚至仅使用正整数来进一步减少内存消耗。

基于上面所说的变量类型简化的思考，写出一个自动转化的函数，它可以根据上表**将浮点数和整数转换为它们的最小子类型**：

```
def reduce_memory_usage(df, verbose=True):
    numerics = ["int8", "int16", "int32", "int64", "float16", "float32", "float64"]
    start_mem = df.memory_usage().sum() / 1024 ** 2
    for col in df.columns:
        col_type = df[col].dtypes
        if col_type in numerics:
            c_min = df[col].min()
            c_max = df[col].max()
            if str(col_type)[:3] == "int":
                if c_min > np.iinfo(np.int8).min and c_max < np.iinfo(np.int8).max:
                    df[col] = df[col].astype(np.int8)
                elif c_min > np.iinfo(np.int16).min and c_max < np.iinfo(np.int16).max:
                    df[col] = df[col].astype(np.int16)
                elif c_min > np.iinfo(np.int32).min and c_max < np.iinfo(np.int32).max:
                    df[col] = df[col].astype(np.int32)
                elif c_min > np.iinfo(np.int64).min and c_max < np.iinfo(np.int64).max:
                    df[col] = df[col].astype(np.int64)
            else:
                if (
                    c_min > np.finfo(np.float16).min
                    and c_max < np.finfo(np.float16).max
                ):
                    df[col] = df[col].astype(np.float16)
                elif (
                    c_min > np.finfo(np.float32).min
                    and c_max < np.finfo(np.float32).max
                ):
                    df[col] = df[col].astype(np.float32)
                else:
                    df[col] = df[col].astype(np.float64)
    end_mem = df.memory_usage().sum() / 1024 ** 2
    if verbose:
        print(
            "Mem. usage decreased to {:.2f} Mb ({:.1f}% reduction)".format(
                end_mem, 100 * (start_mem - end_mem) / start_mem
            )
        )
    return df

```

当然，这个函数不是固定，东哥只是提供个模板，大家可以直接复制拿过去改成自己习惯的方式。

下面来看一下这个转化函数能给我们具体带来多少内存占用的减少。这里我用了一个加载进来会占用`2.2GB`内存的数据集，使用`reduce_memory_usage`以后的情况是这样的。

```
>>> reduce_memory_usage(tps_october)
Mem. usage decreased to 509.26 Mb (76.9% reduction)

```

数据集的内存占用从原来的 `2.2GB` 压缩到 `510MB`。不要小看这个压缩量，因为数据分析或者建模的过程中，要做很多数据处理操作，就这导致数据集会被重复使用很多次。**如果开始的数据集就很大，那么后面的内存占用也会跟着大，这样一算下来整个就放大了很多倍。**

但有一点需要提示一下，尽管在我们运行时会减少内存，但当我们保存数据时，内存减少的效果会丢失掉，不过磁盘空间往往是够用的，这个影响没那么大。

**原创不易，欢迎点赞、留言、分享，支持我继续写下去****![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)**

* * *

**推荐阅读**

[*1. *pandas100个骚操作](https://mp.weixin.qq.com/mp/appmsgalbum?__biz=MzUzODYwMDAzNA==&action=getalbum&album_id=1699019347278561282#wechat_redirect)

*2. *[机器学习原创系列](https://mp.weixin.qq.com/mp/appmsgalbum?__biz=MzUzODYwMDAzNA==&action=getalbum&album_id=1838255778403581970#wechat_redirect)

*3. *[数据科学干货下载](https://mp.weixin.qq.com/mp/appmsgalbum?__biz=MzUzODYwMDAzNA==&action=getalbum&album_id=1816443218394218499#wechat_redirect)

👇 点击「**阅读原文**」有惊喜

收录于合集 #<a id="js_album_keep_read_title"></a>pandas骚操作

 <a id="js_album_keep_read_size"></a>25个

<a id="js_album_prev"></a>上一篇 <a id="js_album_keep_read_pre_title"></a>吹爆这个 pandas GUI 神器，自动转代码！ <a id="js_album_next"></a>下一篇 <a id="js_album_keep_read_next_title"></a>pandas 变量类型转换的 6 种方法

<a id="js_view_source"></a>Read more

People who liked this content also liked

如何在云原生中监控JVM指标

...

YP小站

不看的原因

- 内容质量低
- 不看此公众号

打造一款基于Golang的高交互渗透测试框架（上） | 技术精选0133

...

酒仙桥六号部队

不看的原因

- 内容质量低
- 不看此公众号

这款刚开源的云原生时代的开发工具有何魔力？

...

k8s技术圈

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___dc8bd43b735f4279a.bmp"/>

Scan to Follow