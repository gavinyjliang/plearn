# Pandas高级函数transform使用指南

<a id="copyright_logo"></a>Original <a id="js_author_name"></a>尤而小屋 <a id="profileBt"></a><a id="js_name"></a>尤而小屋 *2022-01-15 00:56*

收录于话题

<a id="js_article_tag_name__1999223977696624640"></a>#机器学习 <a id="js_article_tag_num__1999223977696624640"></a>72 <a id="js_article_tag_tips__1999223977696624640"></a>个

<a id="js_article_tag_name__1999223975666581509"></a>#数据分析 <a id="js_article_tag_num__1999223975666581509"></a>104 <a id="js_article_tag_tips__1999223975666581509"></a>个

<a id="js_article_tag_name__2218642634804363265"></a>#数据科学 <a id="js_article_tag_num__2218642634804363265"></a>4 <a id="js_article_tag_tips__2218642634804363265"></a>个

<a id="js_article_tag_name__2019120411547860993"></a>#数据挖掘 <a id="js_article_tag_num__2019120411547860993"></a>37 <a id="js_article_tag_tips__2019120411547860993"></a>个

<a id="js_article_tag_name__1999223977226862596"></a>#pandas <a id="js_article_tag_num__1999223977226862596"></a>50 <a id="js_article_tag_tips__1999223977226862596"></a>个

> 公众号：尤而小屋
> 作者：Peter
> 编辑：Peter

大家好，我是Peter~

本文结合一个简单的案例来讲解Pandas中高级函数**transform**的使用。

官网的案例比较简单，具体地址：https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.transform.html

<img width="657" height="252" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_f205d7d3ea0d44458.jpg"/>

## 模拟数据

模拟了这样一份虚拟数据：姓名+科目+分数

<img width="657" height="664" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_d6a9fac6f4774d1c8.jpg"/>

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

```
df_copy = df.copy()  # 生成一个副本待用

```

**需求很简单：求出每个学生的各科成绩在个人总成绩中的占比**？

## 方法1：groupby+merge

### 步骤1：求出每个人的总成绩

<img width="657" height="170" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_e273d4b959d544fd8.jpg"/>

### 步骤2：使用merge拼接数据

Merge合并数据的时候使用全连接，保留两个DataFrame的全部字段

<img width="657" height="539" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_9b04a02a2ad849fca.jpg"/>

### 求出百分比

求出占比，并且转成百分比的形式：

<img width="657" height="324" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_ebc3e0604f9c41c58.jpg"/>

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 方法2：groupby+transform

### 步骤1：直接使用transform

一步到位，直接求出每个学生的总成绩👍这个就是tranform的神器之处。不用中间转换过程

<img width="657" height="296" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_2bdc4f79dca048efa.jpg"/>

### 步骤2：求出百分比

后面是同样的过程和方法求出百分比

<img width="657" height="359" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_bd8e35cfae2f49678.jpg"/>

## 活学活用transform

除了求和sum，transform的应用还很灵活：

### 使用其他内置聚合函数

```
df_copy["总成绩"] = df_copy.groupby("姓名")["分数"].transform(sum)
df_copy["平均成绩"] = df_copy.groupby("姓名")["分数"].transform(np.mean)  # 不能直接mean
df_copy["最高分"] = df_copy.groupby("姓名")["分数"].transform(max)
df_copy["最低分"] = df_copy.groupby("姓名")["分数"].transform(min)
df_copy.head()  # 显示5行

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

同时使用多样化的聚合函数求解多个指标

### 使用自定义函数

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 使用lambda函数

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 使用其他内置函数

除了和数据相关的函数，还可以是其他的函数：

1、将名字改成小写

<img width="657" height="324" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_e34871bc98944f909.jpg"/>

2、求名字的长度len

<img width="657" height="448" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_eb3c82d88b044c8c8.jpg"/>

### 传入多个函数

对不同的字段传入不同的函数

<img width="657" height="489" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_78e46acc385847c8b.jpg"/>

### 筛选过滤数据

除了新增或者改变数据，transform还能够筛选满足要求的数据：

- 总分大于350分
    
- 科目的最低分为90分
    

<img width="657" height="519" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_e724281e15374d20a.jpg"/>

## 备忘录

记住这张图，能够帮助你很好的理解transform函数的使用，建议收藏保存~

<img width="657" height="316" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_0063d918ccb64011b.jpg"/>

https://pbpython.com/pandas_transform.html

<img width="17" height="36" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__1210b23b56ad4c37a.png"/>

推荐阅读

<img width="17" height="36" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__99136aee55ea4d778.png"/>

[机器学习神器Scikit-Learn保姆级入门教程](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247503119&idx=1&sn=687622d2a1d1019b2996228e5893ce99&chksm=cf12ddd5f86554c3cab62f453f8bc8f2ae3ae390e7338563fd4e8ec8c0df406b90bfcdc112b4&scene=21#wechat_redirect)

[小而全的Pandas使用案例](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247503051&idx=1&sn=ec0d65bf91b1344cf0f4cd7b02bbabd6&chksm=cf12dc11f86555072560f1571b7e81fda646e88e509c455c9e4767c700274d1ed0dc246d0161&scene=21#wechat_redirect)

[Pandas行列转换的4大技巧！](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247501523&idx=1&sn=f4a011bec8abc547ad2ffd6fb8c06ef4&chksm=cf12d609f8655f1f0c781431e4d7f39ed202a1a799d372eb5db74da3a06e8e13c74248b4d218&scene=21#wechat_redirect)

[精选23个Pandas常用函数](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247502204&idx=1&sn=030b34c84657cfa6a04e2afee7836178&chksm=cf12d9a6f86550b085e6f5a0385d4346fa34038fd7aeecef6bf45d1b02dd6561bd2991c4956e&scene=21#wechat_redirect)

[Plotly+Seaborn+Folium可视化探索爱彼迎租房数据](http://mp.weixin.qq.com/s?__biz=Mzg3ODY2MDAyMQ==&mid=2247502834&idx=1&sn=a077cf1875b9601a9f0b8a964b0b0278&chksm=cf12db28f865523e2fc71e9a2da31736a0eda3b07de906378e852c253b4777a5b7bc3b5b74df&scene=21#wechat_redirect)

> 尤而小屋，一个温馨的小屋。小屋主人，一手代码谋求生存，一手掌勺享受生活，欢迎你的光临

![](../../../_resources/0_wx_fmt_png_21ff205e0f414c25adb1c048f8c31c92.png)

**尤而小屋**

尤而小屋，一个温馨且有爱的小屋🏡 小屋主人，一手代码谋求生存，一手掌勺享受生活，欢迎你的光临~

<a id="js_profile_article"></a>216篇原创内容

Official Account

收录于话题 #<a id="js_album_keep_read_title"></a>机器学习

 <a id="js_album_keep_read_size"></a>72个

<a id="js_album_prev"></a>上一篇 <a id="js_album_keep_read_pre_title"></a>No. 217，向组织报道！ <a id="js_album_next"></a>下一篇 <a id="js_album_keep_read_next_title"></a>酷！神经网络可视化12大利器

<a id="js_view_source"></a>Read more

People who liked this content also liked

【解题研究】动点与函数图象判断的解题策略（4）——黄于腾

盘龙区王学先名师工作室

不看的原因

- 内容质量低
- 不看此公众号

SQL基础入门（二）

啦啦的小天地

不看的原因

- 内容质量低
- 不看此公众号

SQL优化小技巧

SZMSJSZ

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___8eda4a46e3fa46609.bmp"/>

Scan to Follow