# Pandas 实用技能，将列（column）排序的几种方法

<a id="profileBt"></a><a id="js_name"></a>AI科技大本营 *2022-05-31 18:00* *Posted on <a id="js_ip_wording"></a>北京*

The following article is from Python数据之道 Author 阳哥

<a id="copyright_info"></a>[![](../../../_resources/0_d8b04e47df5a4bfe9a4c5332a6989439.jpg)<br>**Python数据之道** .<br>点击领取《Python知识手册》高清电子版，回复数字 “600” 获取。「Python数据之道」秉承“让数据更有价值”的理念​，聚焦于 Python 数据分析、数据可视化、AI、机器学习、深度学习等领域。](#)

<img width="661" height="117" src="../../../_resources/640_wx_fmt_gif_wxfrom_5_wx_lazy__04b30251a421413ca.gif"/>

作者 | 阳哥

来源 | Python数据之道

Pandas 可以说是 在Python数据科学领域应用最为广泛的工具之一。

Pandas是一种高效的数据处理库，它以 `dataframe` 和 `series` 为基本数据类型，呈现出类似excel的二维数据。

在数据处理过程中，咱们经常需要将列按照一定的要求进行排序，以方便展示。

这里，给大家分享下 在 Pandas 中将列排序的几种常用方法。

### 数据准备

文中主要使用了 `pandas` 和 `akshare` ，首先导入 Python 库，如下：

```


import pandas as pd
import akshare as ak
print(f'pandas version: {pd.__version__}')



```

本次使用的数据如下：

```


data = {
    'brand':['Python数据之道','价值前瞻','菜鸟数据之道','Python','Java'],
    'B':[4,6,8,12,10],
    'A':[10,2,5,20,16],
    'D':[6,18,14,6,12],
    'years':[4,1,1,30,30],
    'C':[8,12,18,8,2],
}
df = pd.DataFrame(data=data)
df



```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

现将现有的 `columns` 输出，方便后面 copy 使用。

```


df.columns
# Index(['brand', 'B', 'A', 'D', 'years', 'C'], dtype='object')



```

### Method 1

第一种方法，也是我自己常用的方法，就是自己将列的名称按需要进行手动排序，然后运行代码如下：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### Method 2

第二种方法，是使用 `.iloc` 方法，通过列的位置来进行排序，如下：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### Method 3

第三种方法，是使用 `.loc` 方法，通过列的名称来进行排序，如下：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

这种方法跟第一种方法类似，个人觉得第一种方法更简洁些。

### Method 4

第四种是 逆序 排序，算是排序中一种特定的排序方式。

```


# Method 4 ,逆序
cols = list(df.columns)
cols.reverse()
df[cols]



```

上述代码中，`cols.reverse()` 是将列表（list）进行逆序排序。

此外，列表（list）的逆序排序，还可以用 `cols[::-1]` 来实现。因此，下面的方法也可以实现逆序排序。

```


# Method 4 ,逆序
cols = list(df.columns)
df[cols[::-1]]



```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 实战案例：自由排序

有时候，当存在变量、列的数量较多，或者不同的dataframe中列的名称不完全一致等情况出现时，咱们不一定会通过列名称来实现排序。

这里分享一个实战案例，是关于制作基金的十大持仓数据表的，具体过程我就不在这里描述了，下面给出实现的函数，有兴趣的同学可以研究下。

自定义函数如下：

```


# 需要安装 akshare
# pip install akshare
years = ['2019','2020','2021']
def fund_stock_holding(years,code):
    data = pd.DataFrame()
    for yr in years:
        df_tmp = ak.fund_em_portfolio_hold(code=code,year=yr)
        data = data.append(df_tmp)
    data['季度']=data['季度'].apply(lambda x:x[:8])
    data['占净值比例'] = pd.to_numeric(data['占净值比例'])
    data = data.sort_values(['季度','持仓市值'],ascending=[True,False])
    df = data.set_index(['序号','季度']).stack().unstack([1,2]).head(10)
    df = df.loc[:,(slice(None), '股票名称')]
    df = df.droplevel(None,axis=1)
    df.columns.name=None
    df = df.reset_index()
#     df.index.name = None
    df['基金代码'] = code
    return df
    
df = fund_stock_holding(years,'005669')
df



```

得到的数据表格如下：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

上面的表格中，我需要将 `基金代码` 这一列移动到 `序号` 这列的后面，由于 `years = ['2019','2020','2021']` 这是一个变量，当具体的值不同时，会导致列名称不一样，因此，在这种情况下我们不能直接使用列的具体名称，但咱们可以通过 列的位置组合来实现，列的调整具体如下：

```


    cols = df.columns.tolist()
    cols = cols[:1] + cols[-1:] + cols[1:-1]  # 将基金代码列名放前面
    df = df[cols]



```

将上面的调整过程整合到自定义函数中，完整的代码如下：

```


# 需要安装 akshare
# pip install akshare
years = ['2019','2020','2021']
def fund_stock_holding_update(years,code):
    data = pd.DataFrame()
    for yr in years:
        df_tmp = ak.fund_em_portfolio_hold(code=code,year=yr)
        data = data.append(df_tmp)
    data['季度']=data['季度'].apply(lambda x:x[:8])
    data['占净值比例'] = pd.to_numeric(data['占净值比例'])
    data = data.sort_values(['季度','持仓市值'],ascending=[True,False])
    df = data.set_index(['序号','季度']).stack().unstack([1,2]).head(10)
    df = df.loc[:,(slice(None), '股票名称')]
    df = df.droplevel(None,axis=1)
    df.columns.name=None
    df = df.reset_index()
#     df.index.name = None
    df['基金代码'] = code
    cols = df.columns.tolist()
    cols = cols[:1] + cols[-1:] + cols[1:-1]  # 将基金代码列名放前面
    df = df[cols]
    return df
    
df = fund_stock_holding_update(years,'005669')
df



```

效果如下：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

当然，我最后实现的效果是将基金代码换成基金名称，这个可以想办法实现，效果如下：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 小结

以上就是关于 Pandas 中 列名称排序的介绍，看似很简单的内容，在最后的实践中，也还是有些小技巧的。

欢迎大家来畅聊，Pandas 中有哪些实用的小技巧~~

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

**往期回顾**

[介绍Pandas实战中的一些高端玩法](http://mp.weixin.qq.com/s?__biz=Mzg4NDQwNTI0OQ==&mid=2247560261&idx=2&sn=840f1c8524764beba14b032beb2c28b3&chksm=cfbb092bf8cc803d3967d1fcb3cb33236a0d118413af06e3ec438e6f5d5dc66b8f2060e6c649&scene=21#wechat_redirect)

[用Python+Excel制作一个视频下载器~](http://mp.weixin.qq.com/s?__biz=Mzg4NDQwNTI0OQ==&mid=2247560560&idx=2&sn=8a0bd49afbabd75442d06641ea602bd5&chksm=cfbb081ef8cc8108edeb66adc804d553431c5f293047d231bd22ee47669e378e442ca4e330e5&scene=21#wechat_redirect)

[Pandas表格样式设置指南，爆赞！](http://mp.weixin.qq.com/s?__biz=Mzg4NDQwNTI0OQ==&mid=2247560525&idx=3&sn=7d88a115ac198cf9eff70a43de1a36fb&chksm=cfbb0823f8cc8135fa119ee7aca66fad894de51f18230541cb3c00c2dd62e48e21894b4d16e0&scene=21#wechat_redirect)

[如何用一行Python代码制作一个GUI？](http://mp.weixin.qq.com/s?__biz=Mzg4NDQwNTI0OQ==&mid=2247560214&idx=3&sn=9ccf13326a163c9cc3f2e02fe0d7e184&chksm=cfbb0978f8cc806ef7eb68e55b203e62b0372fc92f10f7c587bbff6b1aff740b9fac445e27fd&scene=21#wechat_redirect)

```


![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

分享

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

点收藏

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

点点赞

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

点在看












```

People who liked this content also liked

Python 实现 GIF 动图以及视频卡通化，两脚踢碎次元壁

...

AI科技大本营

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___5e1a77200339457bb.bmp"/>

Scan to Follow