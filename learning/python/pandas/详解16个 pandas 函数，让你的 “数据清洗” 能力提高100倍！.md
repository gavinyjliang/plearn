# 详解16个 pandas 函数，让你的 “数据清洗” 能力提高100倍！

<a id="profileBt"></a><a id="js_name"></a>数据分析与开发 *2021-04-10 12:00*

The following article is from 数据分析与统计学之美 Author 黄伟呢

<a id="copyright_info"></a>[![](../../../_resources/0_44507b3a28c84c8790325456b4cfd080.jpg)<br>**数据分析与统计学之美** .<br>免费领10w字"Python知识手册"，共400页，后台回复“十万”领取！](#)

## 本文介绍

你有没有这样一种感觉，为什么到自己手上的数据，总是乱七八糟？

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__e353e3539c804bcbb.png)

作为一个数据分析师来说，**数据清洗**是必不可少的环节。有时候由于数据太乱，往往需要花费我们很多时间去处理它。因此掌握更多的数据清洗方法，会让你的能力调高100倍。

本文基于此，**讲述pandas中超级好用的str矢量化字符串函数**，学了之后，瞬间感觉自己的数据清洗能力提高了。

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__f482520726ab498a8.png)

## 1个数据集，16个Pandas函数

**数据集是黄同学精心为大家编造**，只为了帮助大家学习到知识。数据集如下：

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

结果如下：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

观察上述数据，数据集是乱的。接下来，我们就用16个Pandas来对上述数据，进行数据清洗。

##### ① cat函数：用于字符串的拼接

```


df["姓名"].str.cat(df["家庭住址"],sep='-'*3)


```

结果如下：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

##### ② contains：判断某个字符串是否包含给定字符

```


df["家庭住址"].str.contains("广")


```

结果如下：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

##### ③ startswith/endswith：判断某个字符串是否以…开头/结尾

```


# 第一个行的“ 黄伟”是以空格开头的
df["姓名"].str.startswith("黄") 
df["英文名"].str.endswith("e")


```

结果如下：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

##### ④ count：计算给定字符在字符串中出现的次数

```


df["电话号码"].str.count("3")


```

结果如下：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

##### ⑤ get：获取指定位置的字符串

```


df["姓名"].str.get(-1)
df["身高"].str.split(":")
df["身高"].str.split(":").str.get(0)


```

结果如下：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

###### ⑥ len：计算字符串长度

```


df["性别"].str.len()


```

结果如下：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

##### ⑦ upper/lower：英文大小写转换

```


df["英文名"].str.upper()
df["英文名"].str.lower()


```

结果如下：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

##### ⑧ pad+side参数/center：在字符串的左边、右边或左右两边添加给定字符

```


df["家庭住址"].str.pad(10,fillchar="*")      # 相当于ljust()
df["家庭住址"].str.pad(10,side="right",fillchar="*")    # 相当于rjust()
df["家庭住址"].str.center(10,fillchar="*")


```

结果如下：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

##### ⑨ repeat：重复字符串几次

```


df["性别"].str.repeat(3)


```

结果如下：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

##### ⑩ slice_replace：使用给定的字符串，替换指定的位置的字符

```


df["电话号码"].str.slice_replace(4,8,"*"*4)


```

结果如下：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

##### ⑪ replace：将指定位置的字符，替换为给定的字符串

```


df["身高"].str.replace(":","-")


```

结果如下：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

##### ⑫ replace：将指定位置的字符，替换为给定的字符串(接受正则表达式)

- replace中传入正则表达式，才叫好用；
    
- 先不要管下面这个案例有没有用，你只需要知道，使用正则做数据清洗多好用；
    

```


df["收入"].str.replace("\d+\.\d+","正则")


```

结果如下：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

##### ⑬ split方法+expand参数：搭配join方法功能很强大

```


# 普通用法
df["身高"].str.split(":")
# split方法，搭配expand参数
df[["身高描述","final身高"]] = df["身高"].str.split(":",expand=True)
df
# split方法搭配join方法
df["身高"].str.split(":").str.join("?"*5)


```

结果如下：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

##### ⑭ strip/rstrip/lstrip：去除空白符、换行符

```


df["姓名"].str.len()
df["姓名"] = df["姓名"].str.strip()
df["姓名"].str.len()


```

结果如下：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

##### ⑮ findall：利用正则表达式，去字符串中匹配，返回查找结果的列表

- findall使用正则表达式，做数据清洗，真的很香！
    

```


df["身高"]
df["身高"].str.findall("[a-zA-Z]+")


```

结果如下：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

##### ⑯ extract/extractall：接受正则表达式，抽取匹配的字符串(一定要加上括号)

```


df["身高"].str.extract("([a-zA-Z]+)")
# extractall提取得到复合索引
df["身高"].str.extractall("([a-zA-Z]+)")
# extract搭配expand参数
df["身高"].str.extract("([a-zA-Z]+).*?([a-zA-Z]+)",expand=True)


```

结果如下：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

今天的文章，就讲述到这里，希望能够对你有所帮助。

\- EOF -

推荐阅读  点击标题可跳转

<ins>1、[一行 Pandas 代码搞定 Excel “条件格式”！](http://mp.weixin.qq.com/s?__biz=MzA5ODM5MDU3MA==&mid=2650870875&idx=1&sn=b28725c5f3d3c0a6331550f008994f59&chksm=8b67f31ebc107a0814ddd5c943b13369e69adf4b9e628bb1746b4844868c0ecb0339a7730a59&scene=21#wechat_redirect)</ins>

<ins>2、[Pandas 必知必会的使用技巧，值得收藏！](http://mp.weixin.qq.com/s?__biz=MzA5ODM5MDU3MA==&mid=2650869810&idx=2&sn=cea7c191f87235d091c30191eda28abc&chksm=8b67f777bc107e615e747b266937c032624708209d0339086d0edff9fe69fc843ee1aa696b4d&scene=21#wechat_redirect)</ins>

<ins>3、[12 个 Numpy 和 Pandas 函数，提高效率](http://mp.weixin.qq.com/s?__biz=MzA5ODM5MDU3MA==&mid=2650865906&idx=1&sn=afb32717f2f095ca442ca9cd15cf828f&chksm=8b67e7b7bc106ea119d12a21f453f36318444479e562a8d5143c25035cd93b8f9b1822e7d4bc&scene=21#wechat_redirect)</ins>

看完本文有收获？请转发分享给更多人

**推荐关注「数据分析与开发」，提升数据技能**

![](../../../_resources/0_wx_fmt_png_c081a95d86da445e9b38a350519b5c87.png)

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

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___650b97a5fd1345bc8.bmp"/>

Scan to Follow