# 图解Pandas，这篇文章是真的强！

<a id="profileBt"></a><a id="js_name"></a>数据不吹牛 *2022-06-01 22:03* *Posted on <a id="js_ip_wording"></a>浙江*

![](../../../_resources/0_wx_fmt_png_e59a9aacb0c54e61952717fff5a4fa04.png)

**数据不吹牛**

有趣+干货的数据分析宝藏

<a id="js_profile_article"></a>69篇原创内容

Official Account

来源：https://pandastutor.com/index.html

大家伙，我是小z，也可以叫我阿粥

`Pandas`是数据挖掘常见的工具，掌握使用过程中的函数是非常重要的。本文将借助可视化的过程，讲解`Pandas`的各种操作。

## sort_values

```
(dogs[dogs['size'] == 'medium']
 .sort_values('type')
 .groupby('type').median()
)
```

执行步骤：

- size列筛选出部分行
    
- 然后将行的类型进行转换
    
- 按照type列进行分组，计算中位数
    

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## selecting a column

```
dogs['longevity']
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## groupby + mean

```
dogs.groupby('size').mean()
```

执行步骤：

- 将数据按照size进行分组
    
- 在分组内进行聚合操作
    

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## grouping multiple columns

```
dogs.groupby(['type', 'size'])
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## groupby + multi aggregation

```
(dogs
  .sort_values('size')
  .groupby('size')['height']
  .agg(['sum', 'mean', 'std'])
)
```

执行步骤

- 按照size列对数据进行排序
    
- 按照size进行分组
    
- 对分组内的height进行计算
    

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## filtering for columns

```
df.loc[:, df.loc['two'] <= 20]
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## filtering for rows

```
dogs.loc[(dogs['size'] == 'medium') & (dogs['longevity'] > 12), 'breed']
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## dropping columns

```
dogs.drop(columns=['type'])
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## joining

```
ppl.join(dogs)
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## merging

```
ppl.merge(dogs, left_on='likes', right_on='breed', how='left')
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## pivot table

```
dogs.pivot_table(index='size', columns='kids', values='price')
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## melting

```
dogs.melt()
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## pivoting

```
dogs.pivot(index='size', columns='kids')
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## stacking column index

```
dogs.stack()
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## unstacking row index

```
dogs.unstack()
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## resetting index

```
dogs.reset_index()
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## setting index

```
dogs.set_index('breed')
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

以上。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

```


![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

<ins>●[分析师如何正确的提建议？](http://mp.weixin.qq.com/s?__biz=MzU5Mjg2OTQ1MA==&mid=2247509523&idx=1&sn=5612dab06f2dd041fb6b192bd35fe881&chksm=fe1bc736c96c4e20cc6123fd651544c8d9995e5e0c4c962b1094a5aa760c4eaea2951ff0136b&scene=21#wechat_redirect)</ins>

<ins>●[品牌知名度](http://mp.weixin.qq.com/s?__biz=MzU5Mjg2OTQ1MA==&mid=2247501207&idx=1&sn=b17fc4d67f0794da55c15ca5765bf884&chksm=fe1ba4b2c96c2da49e694c8a403807526e87b1e54b1f7d266efe3e31f657435248c2073efb30&scene=21#wechat_redirect)分析</ins>




























```

People who liked this content also liked

五分钟搞定Flink双流JOIN面试题

...

大数据技术与数仓

不看的原因

- 内容质量低
- 不看此公众号

又一款python开发神器

...

基因学苑

不看的原因

- 内容质量低
- 不看此公众号

10种聚类算法的完整python操作示例

...

Python大数据分析

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___edbe5fc5a4dd47c09.bmp"/>

Scan to Follow