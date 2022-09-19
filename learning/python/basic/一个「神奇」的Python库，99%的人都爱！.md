# 一个「神奇」的Python库，99%的人都爱！

<a id="profileBt"></a><a id="js_name"></a>python数据分析之禅 *2022-04-07 12:34*

The following article is from 数据分析与统计学之美 Author 黄伟呢

<a id="copyright_info"></a>[![](../../../_resources/0_02f0cb145f3246c6a7f6b4b389ebd595.jpg)<br>**数据分析与统计学之美** .<br>免费领10w字"Python知识手册"，共400页，后台回复“十万”领取！](#)

![](../../../_resources/0_wx_fmt_png_9c8fecc141a44f6797ed2e0d898ea61c.png)

**python数据分析之禅**

点击领取pandas高清速查表，后台回复“速查表”获取

<a id="js_profile_article"></a>85篇原创内容

Official Account

大家好，我是鸟哥~

今天介绍Python中一个超级神奇的库，**99%人用过都喜欢它，剩下的1%没用过！**

在如今的大数据时代，数据的价值可想而知。有时候为了做测试，需要模拟真实的环境，但是又不能直接使用真实数据，就需要我们人为制造一些数据出来。

对比Excel，我还是觉得Python制造这样的 "虚拟"数据，更省时、省力。

周末，突然想到了曾今做过的这个问题，这里为大家做个复盘吧！

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__f396768b9c29404f9.png)

**需求：** 老板让模拟一批数据，用于项目实验，由于一些真实数据不能展示出来，我需要模拟一些数据，字段包括：`姓名`、`所在省份`、`详细地址`、`手机号`、`身份证号`、`出生年月`、`邮箱`等。

当然，这批数据肯定是需要你最终写入到Excel中，一次性交给老板的。那么，这样的需求，你会做吗？

## 模拟1w条数据写入Excel

在讲述基础之前，直接上实战，让大家体会一下，如何将生成的模拟数据，最终写入到Excel文件中。

```
from faker import Faker
import pandas as pd
 
fake = Faker(["zh_CN"])
Faker.seed(0)
def get_data():
    key_list = ["姓名","详细地址","所在省份","手机号","身份证号","出生年月","邮箱"]
    name = fake.name()
    address = fake.address()
    province = address[:3]
    number = fake.phone_number()
    id_card = fake.ssn()
    birth_date = id_card[6:14]
    email = fake.email()
    info_list = [name,address,province,number,id_card,birth_date,email]
    person_info = dict(zip(key_list,info_list))
    return person_info
df = pd.DataFrame(columns=["姓名","详细地址","所在省份","手机号","身份证号","出生年月","邮箱"])
for i in range(10000):
    person_info = [get_data()]
    df1 = pd.DataFrame(person_info)
    df = pd.concat([df,df1])
df.to_excel("模拟数据.xlsx",index=None)

```

结果如下：

<img width="677" height="330" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__e47e2210897446258.png"/>

> **以上数据为模拟生成，如果雷同，纯属巧合！**

## Python库讲解

这么好用的Python库，究竟应该**怎么使用呢？**

我们直接使用下面的代码，可以完成这个库的安装。

```
pip install Faker -i https://pypi.tuna.tsinghua.edu.cn/simple/

```

使用之前，使用如下代码，导入这个库。

```
from faker import Faker

```

在讲述写入到Excel之前，我们先分布讲述一下，每个函数的用法。

#### 1\. 生成姓名

```
fake = Faker(locale='zh_CN')
name = fake.name()
name

```

结果如下：

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__ee9c83c5d6e34b68a.png)

#### 2\. 生成详细地址

```
address = fake.address()
address

```

结果如下：

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__2679542eaab34fb69.png)

#### 3\. 生成所在省份

```
province = address[:3]
province

```

结果如下：

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__bc4bf8a6064e4fedb.png)

由于这个函数每次运行结果都不一样，所以我才用切片方式，生成省份。当然这里也有特定函数，生成省份。

```
fake.province()

```

结果如下：

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__d17b7825db684dd1b.png)

#### 4\. 生成手机号

```
number = fake.phone_number()
number

```

结果如下：

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__eab5a4059ef543f59.png)

#### 5\. 生成身份证号

```
id_card = fake.ssn()
id_card

```

结果如下：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

#### 6\. 生成出生年月

```
birth_date = id_card[6:14]
birth_date

```

结果如下：

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__1a0026a0413242bba.png)

#### 7\. 生成邮箱

```
email = fake.email()
email

```

结果如下：

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__dc61e06f4939408eb.png)

## 补充

当然，`faker`库不仅可以帮助我们生成上述信息，还有很多其它方法可用，这些方法分为以下几类：

- address 地址
    
- person 人物类：性别、姓名等
    
- barcode 条码类
    
- color 颜色类
    
- company 公司类：公司名、email、公司名前缀等
    
- credit_card 银行卡类：卡号、有效期、类型等
    
- currency 货币
    
- date_time 时间日期类：日期、年、月等
    
- file 文件类：文件名、文件类型、文件扩展名等
    
- internet 互联网类
    
- job 工作
    
- lorem 乱数假文
    
- misc 杂项类
    
- phone_number 手机号码类：手机号、运营商号段
    
- python python数据
    
- profile 人物描述信息：姓名、性别、地址、公司等
    
- ssn 社会安全码(身份证号码)
    
- user_agent 用户代理
    

关于这些方法的使用，我们直接参考faker的官网，用起来超方便。

*faker.readthedocs.io/en/master/providers.html*

#### 1\. address 地址

```
fake.country()  # 国家
fake.city()  # 城市
fake.city_suffix()  # 城市的后缀,中文是：市或县
fake.address()  # 地址
fake.street_address()  # 街道
fake.street_name()  # 街道名
fake.postcode()  # 邮编
fake.latitude()  # 维度
fake.longitude()  # 经度

```

#### 2\. person 人物

```
fake.name() # 姓名
fake.last_name() # 姓
fake.first_name() # 名
fake.name_male() # 男性姓名
fake.last_name_male() # 男性姓
fake.first_name_male() # 男性名
fake.name_female() # 女性姓名

```

#### 3\. color 颜色

```
fake.hex_color() # 16进制表示的颜色
fake.rgb_css_color() # css用的rgb色
fake.rgb_color()  # 表示rgb色的字符串
fake.color_name() # 颜色名字
fake.safe_hex_color()  #安全16进制色
fake.safe_color_name() # 安全颜色名字

```

#### 4\. company 公司

```
fake.company() # 公司名
fake.company_suffix() # 公司名后缀

```

#### 5\. credit_card 银行信用卡

```
fake.credit_card_number(card_type=None) # 卡号
fake.credit_card_provider(card_type=None) # 卡的提供者
fake.credit_card_security_code(card_type=None)# 卡的安全密码
fake.credit_card_expire() # 卡的有效期
fake.credit_card_full(card_type=None) # 完整卡信息

```

#### 6\. date_time 时间日期

```
fake.date_time(tzinfo=None) # 随机日期时间
fake.iso8601(tzinfo=None) # 以iso8601标准输出的日期
fake.date_time_this_month(before_now=True, after_now=False, tzinfo=None) # 本月的某个日期
fake.date_time_this_year(before_now=True, after_now=False, tzinfo=None) # 本年的某个日期
fake.date_time_this_decade(before_now=True, after_now=False, tzinfo=None)  # 本年代内的一个日期
fake.date_time_this_century(before_now=True, after_now=False, tzinfo=None)  # 本世纪一个日期
fake.date_time_between(start_date="-30y", end_date="now", tzinfo=None)  # 两个时间间的一个随机时间
fake.timezone() # 时区
fake.time(pattern="%H:%M:%S") # 时间（可自定义格式）
fake.am_pm() # 随机上午下午
fake.month() # 随机月份
fake.month_name() # 随机月份名字
fake.year() # 随机年
fake.day_of_week() # 随机星期几
fake.day_of_month() # 随机月中某一天
fake.time_delta() # 随机时间延迟
fake.date_object()  # 随机日期对象
fake.time_object() # 随机时间对象
fake.unix_time() # 随机unix时间（时间戳）
fake.date(pattern="%Y-%m-%d") # 随机日期（可自定义格式）
fake.date_time_ad(tzinfo=None)  # 公元后随机日期

```

#### 7\. file 文件

```
fake.file_name(category="image", extension="png") # 文件名（指定文件类型和后缀名）
fake.file_name() # 随机生成各类型文件
fake.file_extension(category=None) # 文件后缀
fake.mime_type(category=None) # mime-type

```

#### 8\. internet 互联网

```
fake.ipv4(network=False)  # ipv4地址
fake.ipv6(network=False)  # ipv6地址
fake.uri_path(deep=None) # uri路径
fake.uri_extension() # uri扩展名
fake.uri() # uri
fake.url() # url
fake.image_url(width=None, height=None)  # 图片url
fake.domain_word() # 域名主体
fake.domain_name() # 域名
fake.tld() # 域名后缀
fake.user_name() # 用户名
fake.user_agent() # UA
fake.mac_address() # MAC地址
fake.safe_email() # 安全邮箱
fake.free_email() # 免费邮箱
fake.company_email()  # 公司邮箱
fake.email() # 邮箱

```

#### 9\. job 工作

```
fake.job()#工作职位

```

#### 10\. lorem 乱数假文

```
fake.text(max_nb_chars=200) # 随机生成一篇文章
fake.word() # 随机单词
fake.words(nb=3)  # 随机生成几个字
fake.sentence(nb_words=6, variable_nb_words=True)  # 随机生成一个句子
fake.sentences(nb=3) # 随机生成几个句子
fake.paragraph(nb_sentences=3, variable_nb_sentences=True)  # 随机生成一段文字(字符串)
fake.paragraphs(nb=3)  # 随机生成成几段文字(列表)

```

#### 11\. phone_number 电话号码

```
fake.phone_number() # 手机号码
fake.phonenumber_prefix() # 运营商号段，手机号码前三位

```

#### 12\. ssn 社会安全码(身份证)

```
fake.ssn() # 随机生成身份证号(18位)

```

#### 13\. user_agent 用户代理

```
fake.user_agent()

```

****万水千山总是情，点个 👍 行不行**。**

People who liked this content also liked

Python 一行代码算出每个省面积的神器—Geopandas

...

Python实用宝典

不看的原因

- 内容质量低
- 不看此公众号

python 显示地图

...

叶子陪你玩编程

不看的原因

- 内容质量低
- 不看此公众号

Python 强大的模式匹配工具—Pampy

...

快学Python

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___034d20ddd654463a8.bmp"/>

Scan to Follow