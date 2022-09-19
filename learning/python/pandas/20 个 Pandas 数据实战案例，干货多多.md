# 20 个 Pandas 数据实战案例，干货多多

<a id="profileBt"></a><a id="js_name"></a>AI科技大本营 *2022-03-03 18:15*

The following article is from 关于数据分析与可视化 Author 俊欣

<a id="copyright_info"></a>[![](../../../_resources/0_ff817eb197284807b287663c213e2a39.jpg)<br>**关于数据分析与可视化** .<br>本公众号定期分享数据分析与可视化干货文章，并有时结合热点话题进行深入讨论，希望您会喜欢，要是哪里写的不好，也渴望倾听您的想法和意见，感谢！❤️](#)

<img width="641" height="113" src="../../../_resources/640_wx_fmt_gif_wxfrom_5_wx_lazy__6a5a66017c1341519.gif"/>

作者 | 俊欣

来源 | 关于数据分析与可视化

今天我们讲一下`pandas`当中的数据过滤内容，小编之前也写过也一篇相类似的文章，但是是基于文本数据的过滤，大家有兴趣也可以去查阅一下。

下面小编会给出大概20个案例来详细说明数据过滤的方法，首先我们先建立要用到的数据集，代码如下

```


import pandas as pd
df = pd.DataFrame({
    "name": ["John","Jane","Emily","Lisa","Matt"],
    "note": [92,94,87,82,90],
    "profession":["Electrical engineer","Mechanical engineer",
                  "Data scientist","Accountant","Athlete"],
    "date_of_birth":["1998-11-01","2002-08-14","1996-01-12",
                     "2002-10-24","2004-04-05"],
    "group":["A","B","B","A","C"]
})



```

output

```


    name  note           profession date_of_birth group
0   John    92  Electrical engineer    1998-11-01     A
1   Jane    94  Mechanical engineer    2002-08-14     B
2  Emily    87       Data scientist    1996-01-12     B
3   Lisa    82           Accountant    2002-10-24     A
4   Matt    90              Athlete    2004-04-05     C



```

### 筛选表格中的若干列

代码如下

```


df[["name","note"]]



```

output

```


    name  note
0   John    92
1   Jane    94
2  Emily    87
3   Lisa    82
4   Matt    90



```

### 再筛选出若干行

我们基于上面搜索出的结果之上，再筛选出若干行，代码如下

```


df.loc[:3, ["name","note"]]



```

output

```


    name  note
0   John    92
1   Jane    94
2  Emily    87
3   Lisa    82



```

### 根据索引来过滤数据

这里我们用到的是`iloc`方法，代码如下

```


df.iloc[:3, 2]



```

output

```


0    Electrical engineer
1    Mechanical engineer
2         Data scientist



```

### 通过比较运算符来筛选数据

```


df[df.note > 90]



```

output

```


   name  note           profession date_of_birth group
0  John    92  Electrical engineer    1998-11-01     A
1  Jane    94  Mechanical engineer    2002-08-14     B



```

### `dt`属性接口

`dt`属性接口是用于处理时间类型的数据的，当然首先我们需要将字符串类型的数据，或者其他类型的数据转换成事件类型的数据，然后再处理，代码如下：

```


df.date_of_birth = df.date_of_birth.astype("datetime64[ns]")
df[df.date_of_birth.dt.month==11]



```

output

```


   name  note           profession date_of_birth group
0  John    92  Electrical engineer    1998-11-01     A



```

或者我们也可以

```


df[df.date_of_birth.dt.year > 2000]



```

output

```


   name  note           profession date_of_birth group
1  Jane    94  Mechanical engineer    2002-08-14     B
3  Lisa    82           Accountant    2002-10-24     A
4  Matt    90              Athlete    2004-04-05     C



```

### 多个条件交集过滤数据

当我们遇上多个条件，并且是交集的情况下过滤数据时，代码应该这么来写

```


df[(df.date_of_birth.dt.year > 2000) &  
   (df.profession.str.contains("engineer"))]



```

output

```


   name  note           profession date_of_birth group
1  Jane    94  Mechanical engineer    2002-08-14     B



```

### 多个条件并集筛选数据

当多个条件是以并集的方式来过滤数据的时候，代码如下

```


df[(df.note > 90) | (df.profession=="Data scientist")]



```

output

```


    name  note           profession date_of_birth group
0   John    92  Electrical engineer    1998-11-01     A
1   Jane    94  Mechanical engineer    2002-08-14     B
2  Emily    87       Data scientist    1996-01-12     B



```

### `Query`方法过滤数据

`Pandas`当中的`query`方法也可以对数据进行过滤，我们将过滤的条件输入

```


df.query("note > 90")



```

output

```


   name  note           profession date_of_birth group
0  John    92  Electrical engineer    1998-11-01     A
1  Jane    94  Mechanical engineer    2002-08-14     B



```

又或者是

```


df.query("group=='A' and note > 89")



```

output

```


   name  note           profession date_of_birth group
0  John    92  Electrical engineer    1998-11-01     A



```

### `nsmallest`方法过滤数据

`pandas`当中的`nsmallest`以及`nlargest`方法是用来找到数据集当中最大、最小的若干数据，代码如下

```


df.nsmallest(2, "note")



```

output

```


    name  note      profession date_of_birth group
3   Lisa    82      Accountant    2002-10-24     A
2  Emily    87  Data scientist    1996-01-12     B



```

```


df.nlargest(2, "note")



```

output

```


   name  note           profession date_of_birth group
1  Jane    94  Mechanical engineer    2002-08-14     B
0  John    92  Electrical engineer    1998-11-01     A



```

### `isna()`方法

`isna()`方法功能在于过滤出那些是空值的数据，首先我们将表格当中的某些数据设置成空值

```


df.loc[0, "profession"] = np.nan
df[df.profession.isna()]



```

output

```


   name  note profession date_of_birth group
0  John    92        NaN    1998-11-01     A



```

### `notna()`方法

`notna()`方法上面的`isna()`方法正好相反的功能在于过滤出那些不是空值的数据，代码如下

```


df[df.profession.notna()]



```

output

```


    name  note           profession date_of_birth group
1   Jane    94  Mechanical engineer    2002-08-14     B
2  Emily    87       Data scientist    1996-01-12     B
3   Lisa    82           Accountant    2002-10-24     A
4   Matt    90              Athlete    2004-04-05     C



```

### `assign`方法

`pandas`当中的`assign`方法作用是直接向数据集当中来添加一列

```


df_1 = df.assign(score=np.random.randint(0,100,size=5))
df_1



```

output

```


    name  note           profession date_of_birth group  score
0   John    92  Electrical engineer    1998-11-01     A     19
1   Jane    94  Mechanical engineer    2002-08-14     B     84
2  Emily    87       Data scientist    1996-01-12     B     68
3   Lisa    82           Accountant    2002-10-24     A     70
4   Matt    90              Athlete    2004-04-05     C     39



```

### `explode`方法

`explode()`方法直译的话，是爆炸的意思，我们经常会遇到这样的数据集

```


  Name            Hobby
0   吕布  [打篮球, 玩游戏, 喝奶茶]
1   貂蝉       [敲代码, 看电影]
2   赵云        [听音乐, 健身]



```

`Hobby`列当中的每行数据都以列表的形式集中到了一起，而`explode()`方法则是将这些集中到一起的数据拆开来，代码如下

```


 Name Hobby
0   吕布   打篮球
0   吕布   玩游戏
0   吕布   喝奶茶
1   貂蝉   敲代码
1   貂蝉   看电影
2   赵云   听音乐
2   赵云    健身



```

当然我们会展开来之后，数据会存在重复的情况，

```


df.explode('Hobby').drop_duplicates().reset_index(drop=True)



```

output

```


 Name Hobby
0   吕布   打篮球
1   吕布   玩游戏
2   吕布   喝奶茶
3   貂蝉   敲代码
4   貂蝉   看电影
5   赵云   听音乐
6   赵云    健身


```

<img width="661" height="132" src="../../../_resources/640_wx_fmt_gif_wxfrom_5_wx_lazy__02ac02df557b4e8e8.gif"/>

<img width="661" height="344" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__31af41f4d8934de4a.png"/>

往

期

回

顾

资讯

[Meta开发AI语音助手，助力元宇宙](http://mp.weixin.qq.com/s?__biz=Mzg4NDQwNTI0OQ==&mid=2247555928&idx=1&sn=16a79f8cd876e60bf9df9d4b824aea93&chksm=cfbafe36f8cd772060d1f72c5970755e9a181974d010e0ea49a305b4c9815d50ee65989e498d&scene=21#wechat_redirect)

技术

[霸占CSDN榜一的20个Python用例](http://mp.weixin.qq.com/s?__biz=Mzg4NDQwNTI0OQ==&mid=2247556389&idx=1&sn=5d05aeb261cda6303e76415e4f2c1935&chksm=cfbaf84bf8cd715dfee35b7fb5effcfd95759e685b1d066f220919a6153d800f1bc9fa7b0a6e&scene=21#wechat_redirect)

技术[](http://mp.weixin.qq.com/s?__biz=Mzg4NDQwNTI0OQ==&mid=2247556389&idx=2&sn=3b9c8526816873d7ed64f5f3a4f66bb4&chksm=cfbaf84bf8cd715dbccdf64872c12b7cfaa9f8401aed5daf7a355f70607c1716f4192dd79441&scene=21#wechat_redirect)

[一行代码搞定Python逐行内存消耗](http://mp.weixin.qq.com/s?__biz=Mzg4NDQwNTI0OQ==&mid=2247556389&idx=2&sn=3b9c8526816873d7ed64f5f3a4f66bb4&chksm=cfbaf84bf8cd715dbccdf64872c12b7cfaa9f8401aed5daf7a355f70607c1716f4192dd79441&scene=21#wechat_redirect)

资讯

[M2芯片终于要来了？全线换新](http://mp.weixin.qq.com/s?__biz=Mzg4NDQwNTI0OQ==&mid=2247555747&idx=1&sn=d78476bcb289c18a1e1a2e731b1af777&chksm=cfbaffcdf8cd76db4ce19ce4fea2844ad08092be0af481bc2558a2c447e696c70265b454ab9f&scene=21#wechat_redirect)

<img width="30" height="28" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__d9b58bb72ba5435e9.png"/>

**分享**

<img width="30" height="27" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__0c449355f74441c38.png"/>

**点收藏**

<img width="30" height="28" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__4884b7c6856a4e278.png"/>

**点点赞**

<img width="30" height="28" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__585075902fa74a23a.png"/>

**点在看**

People who liked this content also liked

Jetpack Compose 写一个聊天 App，附赠源码（下）

字节数组

不看的原因

- 内容质量低
- 不看此公众号

Apache 架构师总结的 30 条架构原则

架构文摘

不看的原因

- 内容质量低
- 不看此公众号

学Python 新手做到这7点，提升编程能力真不难！

Python编程大全

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___5ce96d8930e745dfa.bmp"/>

Scan to Follow