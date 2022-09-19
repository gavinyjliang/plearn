# 对比 Excel，学习 pandas 数据透视表

<a id="profileBt"></a><a id="js_name"></a>数据分析与开发 *2021-05-31 12:10*

The following article is from 凹凸数据 Author 黄同学

<a id="copyright_info"></a>[![](../../../_resources/0_4605f4b4e7464fd49bcbf0dce32b2242.jpg)<br>**凹凸数据** .<br>一个不务正业的数据🐶！爬虫、数据分析、可视化、方法论，一条龙服务！业务范围：Python、SQL、Excel、Tableau······](#)

## Excel中做数据透视表

① 选中整个数据源；

<img width="677" height="617" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__a30d0da993da47889.png"/>

img

② 依次点击“插入”—“数据透视表”

<img width="677" height="616" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__9416045e152a44578.png"/>

img

③ 选择在Excel中的哪个位置，插入数据透视表

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__1f68bb77bfab4af39.png)

img

④ 然后根据实际需求，从不同维度展示结果

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__2b3614a2d0b142f49.png)

img

⑤ 结果如下

<img width="677" height="391" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__11c4439c8bee4f6e9.png"/>

img

## pandas用pivot_table()做数据透视表

1）语法格式

```
`pd.pivot_table(data,index=None,columns=None,
               values=None,aggfunc='mean',
               margins=False,margins_name='All',
               dropna=True,fill_value=None)
`
```

2）对比excel，说明上述参数的具体含义

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__18f505f1a74a4cab9.png)

img

参数说明：

- data 相当于Excel中的"选中数据源"；
    
- index 相当于上述"数据透视表字段"中的行；
    
- columns 相当于上述"数据透视表字段"中的列；
    
- values 相当于上述"数据透视表字段"中的值；
    
- aggfunc 相当于上述"结果"中的计算类型；
    
- margins 相当于上述"结果"中的总计；
    
- margins_name 相当于修改"总计"名，为其它名称；
    

下面几个参数，用的较少，记住干嘛的，等以后需要就百度。

- dropna 表示是否删除缺失值，如果为True时，则把一整行全作为缺失值删除；
    
- fill_value 表示将缺失值，用某个指定值填充。
    

## 案例说明

1）求出不同品牌下，每个月份的销售数量之和

① 在Excel中的操作结果如下

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__0f3f580dc79a43c0b.png)

img

② 在pandas中的操作如下

```
`df = pd.read_excel(r"C:\Users\黄伟\Desktop\pivot_table.xlsx")
display(df.sample(5))
df.insert(1,"月份",df["销售日期"].apply(lambda x:x.month))
display(df.sample(5))
df1 = pd.pivot_table(df,index="品牌",columns="月份",
                     values="销售数量",aggfunc=np.sum)
display(df1)
`
```

结果如下：

<img width="677" height="706" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_bf2c5d9a4d8943349.jpg"/>

img

2）求出不同品牌下，每个地区、每个月份的销售数量之和

① 在Excel中的操作结果如下

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__72719616ced84ca99.png)

img

② 在pandas中的操作如下

```
`df = pd.read_excel(r"C:\Users\黄伟\Desktop\pivot_table.xlsx")
display(df.sample(5))
df.insert(1,"月份",df["销售日期"].apply(lambda x:x.month))
display(df.sample(5))
df1 = pd.pivot_table(df,index="品牌",columns=["销售区域","月份"],
                     values="销售数量",aggfunc=np.sum)
display(df1)
`
```

结果如下：

<img width="677" height="734" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_5c539bdd54d645feb.jpg"/>

img

3）求出不同品牌不同地区下，每个月份的销售数量之和

① 在Excel中的操作结果如下

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__1ae87d84bc684d33b.png)

img

② 在pandas中的操作如下

```
`df = pd.read_excel(r"C:\Users\黄伟\Desktop\pivot_table.xlsx")
display(df.sample(5))
df.insert(1,"月份",df["销售日期"].apply(lambda x:x.month))
display(df.sample(5))
df1 = pd.pivot_table(df,index=["品牌","销售区域"],columns="月份",
                     values="销售数量",aggfunc=np.sum)
display(df1)
`
```

结果如下：

<img width="677" height="955" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_c30b7a2f4b4246dc9.jpg"/>

img

4）求出不同品牌下的“销售数量之和”与“货号计数”

① 在Excel中的操作结果如下

<img width="677" height="703" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__88636a39d82b4d339.png"/>

img

② 在pandas中的操作如下

```
`df = pd.read_excel(r"C:\Users\黄伟\Desktop\pivot_table.xlsx")
display(df.sample(5))
df.insert(1,"月份",df["销售日期"].apply(lambda x:x.month))
display(df.sample(5))
df1 = pd.pivot_table(df,index="品牌",columns="月份",
                     values=["销售数量","货号"],
                     aggfunc={"销售数量":"sum","货号":"count"},
                     margins=True,margins_name="总计")
display(df1)
`
```

结果如下：

<img width="677" height="794" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_0646a5df1d75449e9.jpg"/>

\- EOF -

推荐阅读  点击标题可跳转

<ins>1、[用 pandas 高效清洗文本数据](http://mp.weixin.qq.com/s?__biz=MzA5ODM5MDU3MA==&mid=2650871641&idx=1&sn=56da0efab02f011fee7865fd72be3f11&chksm=8b67f01cbc10790a547fa835e4952c96ef68d3729785c9ce4b33d2b5a0db84a11f0078ea90d2&scene=21#wechat_redirect)</ins>

<ins>2、[用 pandas-profiling 做出更好的探索性数据分析](http://mp.weixin.qq.com/s?__biz=MzA5ODM5MDU3MA==&mid=2650866936&idx=2&sn=af894fc31c49cf35006c091638f9816f&chksm=8b67e3bdbc106aab0be79c89d3cb319bbb7038a48aa8b07dacd94a5e03ef8f2e61785b305dc6&scene=21#wechat_redirect)</ins>

<ins>3、[12 个 Numpy 和 Pandas 函数，提高效率](http://mp.weixin.qq.com/s?__biz=MzA5ODM5MDU3MA==&mid=2650865906&idx=1&sn=afb32717f2f095ca442ca9cd15cf828f&chksm=8b67e7b7bc106ea119d12a21f453f36318444479e562a8d5143c25035cd93b8f9b1822e7d4bc&scene=21#wechat_redirect)</ins>

看完本文有收获？请转发分享给更多人

**推荐关注「数据分析与开发」，提升数据技能**

![](../../../_resources/0_wx_fmt_png_22dd7a25b0dd4c83bf1b9ea2193b3141.png)

**数据分析与开发**

「数据分析与开发」分享数据分析与开发相关技术文章、教程、工具

<a id="js_profile_article"></a>55篇原创内容

Official Account

点赞和在看就是最大的支持❤️

People who liked this content also liked

我学习 Python 的三个神级网站

数据分析与开发

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___70d279530f3e4a4bb.bmp"/>

Scan to Follow