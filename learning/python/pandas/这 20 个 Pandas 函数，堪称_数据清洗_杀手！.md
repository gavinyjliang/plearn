# 这 20 个 Pandas 函数，堪称"数据清洗"杀手！

<a id="profileBt"></a><a id="js_name"></a>数据分析与开发 *2021-09-15 12:15*

The following article is from 数据分析与统计学之美 Author 黄伟呢

<a id="copyright_info"></a>[![](../../../_resources/0_791b666043254a9287e297416bd81089.jpg)<br>**数据分析与统计学之美** .<br>免费领10w字"Python知识手册"，共400页，后台回复“十万”领取！](#)

今天准备介绍一篇`超级肝货`！

**Pandas** 是基于NumPy 的一种工具，该工具是为解决数据分析任务而创建的。它提供了大量能使我们快速便捷地处理数据的函数和方法。

本文介绍的这20个**【被分成了15组】**函数，绝对是`数据处理`杀手，用了你会爱不释手。

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__9c556b85a7604293a.png)

## 构造数据集

这里为大家先`构造一个数据集`，用于为大家演示这20个函数。

```


import pandas as pd
df ={'姓名':[' 黄同学','黄至尊','黄老邪 ','陈大美','孙尚香'],
     '英文名':['Huang tong_xue','huang zhi_zun','Huang Lao_xie','Chen Da_mei','sun shang_xiang'],
     '性别':['男','women','men','女','男'],
     '身份证':['463895200003128433','429475199912122345','420934199110102311','431085200005230122','420953199509082345'],
     '身高':['mid:175_good','low:165_bad','low:159_bad','high:180_verygood','low:172_bad'],
     '家庭住址':['湖北广水','河南信阳','广西桂林','湖北孝感','广东广州'],
     '电话号码':['13434813546','19748672895','16728613064','14561586431','19384683910'],
     '收入':['1.1万','8.5千','0.9万','6.5千','2.0万']}
df = pd.DataFrame(df)
df


```

效果图：

<img width="677" height="168" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__91563720e69049119.png"/>

## 1\. cat函数

这个函数主要用于`字符串的拼接`；

```


df["姓名"].str.cat(df["家庭住址"],sep='-'*3)


```

效果图：

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__47cdfdfa2a9a4f2e8.png)

## 2\. contains函数

这个函数主要用于`判断某个字符串是否包含给定字符`；

```


df["家庭住址"].str.contains("广")


```

效果图：

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__d523d9716bd340268.png)

## 3\. startswith、endswith函数

这个函数主要用于`判断某个字符串是否以...开头/结尾`；

```


# 第一个行的“ 黄伟”是以空格开头的
df["姓名"].str.startswith("黄") 
df["英文名"].str.endswith("e")


```

效果图：

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__cd5d0f2584cd480c8.png)

## 4\. count函数

这个函数主要用于`计算给定字符在字符串中出现的次数`；

```


df["电话号码"].str.count("3")


```

效果图：

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__65a70fc1a8c24f4da.png)

## 5\. get函数

这个函数主要用于`获取指定位置的字符串`；

```


df["姓名"].str.get(-1)
df["身高"].str.split(":")
df["身高"].str.split(":").str.get(0)


```

效果图：

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__c3e6d853ff3b4318b.png)

## 6\. len函数

这个函数主要用于`计算字符串长度`；

```


df["性别"].str.len()


```

效果图：

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__2257ab11a29a490a8.png)

## 7\. upper、lower函数

这个函数主要用于`英文大小写转换`；

```


df["英文名"].str.upper()
df["英文名"].str.lower()


```

效果图：

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__87cd76ccb161441a8.png)

## 8\. pad+side参数/center函数

这个函数主要用于`在字符串的左边、右边或左右两边添加给定字符`；

```


df["家庭住址"].str.pad(10,fillchar="*")      # 相当于ljust()
df["家庭住址"].str.pad(10,side="right",fillchar="*")    # 相当于rjust()
df["家庭住址"].str.center(10,fillchar="*")


```

效果图：

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__e745e099aedb441e9.png)

## 9\.  repeat函数

这个函数主要用于`重复字符串几次`；

```


df["性别"].str.repeat(3)


```

效果图：

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__e1f9d3afd36f49078.png)

## 10\.  slice_replace函数

这个函数主要用于`使用给定的字符串，替换指定的位置的字符`；

```


df["电话号码"].str.slice_replace(4,8,"*"*4)


```

效果图：

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__519472b1c86a4a8ca.png)

## 11\. replace函数

这个函数主要用于`将指定位置的字符，替换为给定的字符串`；

```


df["身高"].str.replace(":","-")


```

效果图：

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__ae99c01c30f8454ba.png)

这个函数还`接受正则表达式`，将指定位置的字符，替换为给定的字符串。

```


df["收入"].str.replace("\d+\.\d+","正则")


```

效果图：

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__6af1e8c7c1c04744a.png)

## 12\.  split方法+expand参数

这个函数主要用于`将一列扩展为好几列`；

```


# 普通用法
df["身高"].str.split(":")
# split方法，搭配expand参数
df[["身高描述","final身高"]] = df["身高"].str.split(":",expand=True)
df
# split方法搭配join方法
df["身高"].str.split(":").str.join("?"*5)


```

效果图：

<img width="677" height="538" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__92bd19d4770f4be38.png"/>

## 13\. strip、rstrip、lstrip函数

这个函数主要用于`去除空白符、换行符`；

```


df["姓名"].str.len()
df["姓名"] = df["姓名"].str.strip()
df["姓名"].str.len()


```

效果图：

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__1f4a6b057c964fa59.png)

## 14\. findall函数

这个函数主要用于`利用正则表达式，去字符串中匹配，返回查找结果的列表`；

```


df["身高"]
df["身高"].str.findall("[a-zA-Z]+")


```

效果图：

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__5db15090a0d24374b.png)

## 15\. extract、extractall函数

这个函数主要用于`接受正则表达式，抽取匹配的字符串(一定要加上括号)`；

```


df["身高"].str.extract("([a-zA-Z]+)")
# extractall提取得到复合索引
df["身高"].str.extractall("([a-zA-Z]+)")
# extract搭配expand参数
df["身高"].str.extract("([a-zA-Z]+).*?([a-zA-Z]+)",expand=True)


```

效果图：

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__825ec7cdddde438da.png)

\- EOF -

推荐阅读  点击标题可跳转

<ins>1、[pandas 筛选数据的 8 个骚操作](http://mp.weixin.qq.com/s?__biz=MzA5ODM5MDU3MA==&mid=2650874603&idx=2&sn=9f842a5e30a328180104fadd12767d81&chksm=8b67c5aebc104cb8ed4767a001cbdde06553f242e1baff84b4c873812f074b61c3533ff5029e&scene=21#wechat_redirect)</ins>

<ins>2、[简单实用的 pandas 技巧：如何将内存占用降低90%](http://mp.weixin.qq.com/s?__biz=MzA5ODM5MDU3MA==&mid=2650873961&idx=1&sn=34233ae4367cfd78a385a367698cc84c&chksm=8b67c72cbc104e3a6eeb581e05b1dc40cae61b93e5ae468cb6a9f88a31925c983be642f77cef&scene=21#wechat_redirect)</ins>

<ins>3、[数据分析打工人的 Pandas 75 个高频操作（收藏查询用~）](http://mp.weixin.qq.com/s?__biz=MzA5ODM5MDU3MA==&mid=2650873957&idx=2&sn=9315785842178161f0ea8febc9ba756d&chksm=8b67c720bc104e369dd9c4b787cbc3c53bdfb0bed33921d00873c90ef5f899e7aa97161e3c0e&scene=21#wechat_redirect)</ins>

看完本文有收获？请转发分享给更多人

**推荐关注「数据分析与开发」，提升数据技能**

![](../../../_resources/0_wx_fmt_png_e58e115d2ec3404795f6a913563a5288.png)

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

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___05afbbb04cda4e969.bmp"/>

Scan to Follow