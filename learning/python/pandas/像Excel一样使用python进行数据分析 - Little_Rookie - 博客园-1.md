<a id="top"></a>

- [<img width="76" height="28" src="../../../_resources/logo_c2f07136f1a846eb8c72643c2c8ebcd4.svg"/>](https://www.cnblogs.com/ "开发者的网上家园")
- [首页](https://www.cnblogs.com/)
- [新闻](https://news.cnblogs.com/)
- [博问](https://q.cnblogs.com/)
- <a id="nav_brandzone"></a>[专区](https://brands.cnblogs.com/huawei)
- [闪存](https://ing.cnblogs.com/)
- [班级](https://edu.cnblogs.com/)

- [注册](https://account.cnblogs.com/signup) 登录

<a id="Header1_HeaderTitle"></a>[Little_Rookie](https://www.cnblogs.com/nxld/)

- <a id="blog_nav_sitehome"></a>[博客园](https://www.cnblogs.com/)
- <a id="blog_nav_myhome"></a>[首页](https://www.cnblogs.com/nxld/)
- <a id="blog_nav_newpost"></a>[新随笔](https://i.cnblogs.com/EditPosts.aspx?opt=1)

- <a id="blog_nav_admin"></a>[管理](https://i.cnblogs.com/)

<a id="stats_post_count"></a>随笔 \- 114  <a id="stats_article_count"></a>文章 \- 0  <a id="stats_comment_count"></a>评论 \- 70  <a id="stats_total_view_count"></a>阅读 \- 334万

# <a id="cb_post_title_url"></a>[像Excel一样使用python进行数据分析](https://www.cnblogs.com/nxld/p/6756492.html)

Excel是数据分析中最常用的工具，本篇文章通过python与excel的功能对比介绍如何使用python通过函数式编程完成excel中的数据处理及分析工作。在Python中pandas库用于数据处理 ，我们从1787页的pandas官网文档中总结出最常用的36个函数，通过这些函数介绍如何通过python完成数据生成和导入，数据清洗，预处理，以及最常见的数据分类，数据筛选，分类 汇总，透视等最常见的操作。

文章内容共分为9个部分。这是第一篇，介绍前3部分内容，数据表生成，数据表查看，和数据清洗。以下是《像Excel一样使用python进行数据分析》系列文章的目录。

[![目录](http://bluewhale.cc/wp-content/uploads/2017/04/%E7%9B%AE%E5%BD%95-643x1024.png)](http://bluewhale.cc/wp-content/uploads/2017/04/%E7%9B%AE%E5%BD%95.png)

# 1， 生成数据表

第一部分是生成数据表，常见的生成方法有两种，第一种是导入外部数据，第二种是直接写入数据。 Excel中的文件菜单中提供了获取外部数据的功能，支持数据库和文本文件和页面的多种数据源导入。
[![获取外部数据](http://bluewhale.cc/wp-content/uploads/2017/04/%E8%8E%B7%E5%8F%96%E5%A4%96%E9%83%A8%E6%95%B0%E6%8D%AE.png)](http://bluewhale.cc/wp-content/uploads/2017/04/%E8%8E%B7%E5%8F%96%E5%A4%96%E9%83%A8%E6%95%B0%E6%8D%AE.png)
python支持从多种类型的数据导入。在开始使用python进行数据导入前需要先导入pandas库，为了方便起见，我们也同时导入numpy库。

|     |     |
| --- | --- |
| 1<br><br>2 | `import` `numpy as np`<br><br>`import` `pandas as pd` |

## 导入数据表

下面分别是从excel和csv格式文件导入数据并创建数据表的方法。代码是最简模式，里面有很多可选参数设置，例如列名称，索引列，数据格式等等。感兴趣的朋友可以参考pandas的
官方文档。

|     |     |
| --- | --- |
| 1<br><br>2 | `df``=``pd.DataFrame(pd.read_csv(``'name.csv'``,header``=``1``))`<br><br>`df``=``pd.DataFrame(pd.read_excel(``'name.xlsx'``))` |

## 创建数据表

另一种方法是通过直接写入数据来生成数据表，excel中直接在单元格中输入数据就可以，python中通过下面的代码来实现。生成数据表的函数是pandas库中的DateFrame函数，数据表一共有6行数据，每行有6个字段。在数据中我们特意设置了一些NA值和有问题的字段，例如包含空格等。后面将在数据清洗步骤进行处理。后面我们将统一以DataFrame的简称df来命名数据表。

|     |     |
| --- | --- |
| 1<br><br>2<br><br>3<br><br>4<br><br>5<br><br>6<br><br>7 | `df` `=` `pd.DataFrame({``"id"``:[``1001``,``1002``,``1003``,``1004``,``1005``,``1006``],`<br><br>`"date"``:pd.date_range(``'20130102'``, periods``=``6``),`<br><br>`"city"``:[``'Beijing '``,` `'SH'``,` `' guangzhou '``,` `'Shenzhen'``,` `'shanghai'``,` `'BEIJING '``],`<br><br>`"age"``:[``23``,``44``,``54``,``32``,``34``,``32``],`<br><br>`"category"``:[``'100-A'``,``'100-B'``,``'110-A'``,``'110-C'``,``'210-A'``,``'130-F'``],`<br><br>`"price"``:[``1200``,np.nan,``2133``,``5433``,np.nan,``4432``]},`<br><br>`columns` `=``[``'id'``,``'date'``,``'city'``,``'category'``,``'age'``,``'price'``])` |

这是刚刚创建的数据表，我们没有设置索引列，price字段中包含有NA值，city字段中还包含了一些脏数据。

# [![df](http://bluewhale.cc/wp-content/uploads/2017/04/df.png)](http://bluewhale.cc/wp-content/uploads/2017/04/df.png)
2，数据表检查

第二部分是对数据表进行检查，python中处理的数据量通常会比较大，比如我们之前的文章中介绍的纽约出租车数据和Citibike的骑行数据，数据量都在千万级，我们无法一目了然的 了解数据表的整体情况，必须要通过一些方法来获得数据表的关键信息。数据表检查的另一个目的是了解数据的概况，例如整个数据表的大小，所占空间，数据格式，是否有空值和重复项和具体的数据内容。为后面的清洗和预处理做好准备。

## 数据维度(行列)

Excel中可以通过CTRL+向下的光标键，和CTRL+向右的光标键来查看行号和列号。Python中使用shape函数来查看数据表的维度，也就是行数和列数，函数返回的结果(6,6)表示数据表有6行，6列。下面是具体的代码。

|     |     |
| --- | --- |
| 1<br><br>2<br><br>3 | `#查看数据表的维度`<br><br>`df.shape`<br><br>`(``6``,` `6``)` |

## 数据表信息

使用info函数查看数据表的整体信息，这里返回的信息比较多，包括数据维度，列名称，数据格式和所占空间等信息。

|     |     |
| --- | --- |
| 1<br><br>2<br><br>3<br><br>4<br><br>5<br><br>6<br><br>7<br><br>8<br><br>9<br><br>10<br><br>11<br><br>12<br><br>13<br><br>14 | `#数据表信息`<br><br>`df.info()`<br><br>`&lt;``class` `'pandas.core.frame.DataFrame'``&gt;`<br><br>`RangeIndex:` `6` `entries,` `0` `to` `5`<br><br>`Data columns (total` `6` `columns):`<br><br>`id`          `6` `non``-``null int64`<br><br>`date` `6` `non``-``null datetime64[ns]`<br><br>`city` `6` `non``-``null` `object`<br><br>`category` `6` `non``-``null` `object`<br><br>`age` `6` `non``-``null int64`<br><br>`price` `4` `non``-``null float64`<br><br>`dtypes: datetime64[ns](https://www.cnblogs.com/nxld/p/``1``), float64(``1``), int64(``2``),` `object``(``2``)`<br><br>`memory usage:` `368.0``+` `bytes` |

## 查看数据格式

Excel中通过选中单元格并查看开始菜单中的数值类型来判断数据的格式。Python中使用dtypes函数来返回数据格式。

[![查看数据格式](http://bluewhale.cc/wp-content/uploads/2017/04/%E6%9F%A5%E7%9C%8B%E6%95%B0%E6%8D%AE%E6%A0%BC%E5%BC%8F.png)](http://bluewhale.cc/wp-content/uploads/2017/04/%E6%9F%A5%E7%9C%8B%E6%95%B0%E6%8D%AE%E6%A0%BC%E5%BC%8F.png)

Dtypes是一个查看数据格式的函数，可以一次性查看数据表中所有数据的格式，也可以指定一列来单独查看。

|     |     |
| --- | --- |
| 1<br><br>2<br><br>3<br><br>4<br><br>5<br><br>6<br><br>7<br><br>8<br><br>9<br><br>10 | `#查看数据表各列格式`<br><br>`df.dtypes`<br><br>`id`                   `int64`<br><br>`date        datetime64[ns]`<br><br>`city` `object`<br><br>`category` `object`<br><br>`age                  int64`<br><br>`price              float64`<br><br>`dtype:` `object` |

|     |     |
| --- | --- |
| 1<br><br>2<br><br>3<br><br>4 | `#查看单列格式`<br><br>`df[``'B'``].dtype`<br><br>`dtype(``'int64'``)` |

## 查看空值

Excel中查看空值的方法是使用“定位条件”功能对数据表中的空值进行定位。“定位条件”在“开始”目录下的“查找和选择”目录中。
[![查看空值](http://bluewhale.cc/wp-content/uploads/2017/04/%E6%9F%A5%E7%9C%8B%E7%A9%BA%E5%80%BC.png)](http://bluewhale.cc/wp-content/uploads/2017/04/%E6%9F%A5%E7%9C%8B%E7%A9%BA%E5%80%BC.png)
Isnull是Python中检验空值的函数，返回的结果是逻辑值，包含空值返回True，不包含则返回False。可以对整个数据表进行检查，也可以单独对某一列进行空值检查。

|     |     |
| --- | --- |
| 1<br><br>2 | `#检查数据空值`<br><br>`df.isnull()` |

[![df_isnull](http://bluewhale.cc/wp-content/uploads/2017/04/df_isnull.png)](http://bluewhale.cc/wp-content/uploads/2017/04/df_isnull.png)[](http://bluewhale.cc/wp-content/uploads/2017/04/df_dropna.png)

|     |     |
| --- | --- |
| 1<br><br>2<br><br>3<br><br>4<br><br>5<br><br>6<br><br>7<br><br>8<br><br>9<br><br>10 | `#检查特定列空值`<br><br>`df[``'price'``].isnull()`<br><br>`0`    `False`<br><br>`1`     `True`<br><br>`2`    `False`<br><br>`3`    `False`<br><br>`4`     `True`<br><br>`5`    `False`<br><br>`Name: price, dtype:` `bool` |

## 查看唯一值

Excel中查看唯一值的方法是使用“条件格式”对唯一值进行颜色标记。Python中使用unique函数查看唯一值。

[![查看唯一值](http://bluewhale.cc/wp-content/uploads/2017/04/%E6%9F%A5%E7%9C%8B%E5%94%AF%E4%B8%80%E5%80%BC.png)](http://bluewhale.cc/wp-content/uploads/2017/04/%E6%9F%A5%E7%9C%8B%E5%94%AF%E4%B8%80%E5%80%BC.png)
Unique是查看唯一值的函数，只能对数据表中的特定列进行检查。下面是代码，返回的结果是该列中的唯一值。类似与Excel中删除重复项后的结果。

|     |     |
| --- | --- |
| 1<br><br>2<br><br>3<br><br>4 | `#查看city列中的唯一值`<br><br>`df[``'city'``].unique()`<br><br>`array([``'Beijing '``,` `'SH'``,` `' guangzhou '``,` `'Shenzhen'``,` `'shanghai'``,` `'BEIJING '``], dtype``=``object``)` |

## 查看数据表数值

Python中的Values函数用来查看数据表中的数值。以数组的形式返回，不包含表头信息。

|     |     |
| --- | --- |
| 1<br><br>2<br><br>3<br><br>4<br><br>5<br><br>6<br><br>7<br><br>8<br><br>9<br><br>10<br><br>11<br><br>12<br><br>13<br><br>14 | `#查看数据表的值`<br><br>`df.values`<br><br>`array([[``1001``, Timestamp(``'2013-01-02 00:00:00'``),` `'Beijing '``,` `'100-A'``,` `23``,`<br><br>`1200.0``],`<br><br>`[``1002``, Timestamp(``'2013-01-03 00:00:00'``),` `'SH'``,` `'100-B'``,` `44``, nan],`<br><br>`[``1003``, Timestamp(``'2013-01-04 00:00:00'``),` `' guangzhou '``,` `'110-A'``,` `54``,`<br><br>`2133.0``],`<br><br>`[``1004``, Timestamp(``'2013-01-05 00:00:00'``),` `'Shenzhen'``,` `'110-C'``,` `32``,`<br><br>`5433.0``],`<br><br>`[``1005``, Timestamp(``'2013-01-06 00:00:00'``),` `'shanghai'``,` `'210-A'``,` `34``,`<br><br>`nan],`<br><br>`[``1006``, Timestamp(``'2013-01-07 00:00:00'``),` `'BEIJING '``,` `'130-F'``,` `32``,`<br><br>`4432.0``]], dtype``=``object``)` |

## 查看列名称

Colums函数用来单独查看数据表中的列名称。

|     |     |
| --- | --- |
| 1<br><br>2<br><br>3<br><br>4 | `#查看列名称`<br><br>`df.columns`<br><br>`Index([``'id'``,` `'date'``,` `'city'``,` `'category'``,` `'age'``,` `'price'``], dtype``=``'object'``)` |

## 查看前10行数据

Head函数用来查看数据表中的前N行数据，默认head()显示前10行数据，可以自己设置参数值来确定查看的行数。下面的代码中设置查看前3行的数据。

|     |     |
| --- | --- |
| 1<br><br>2 | `#查看前3行数据`<br><br>`df.head(``3``)` |

[![df_head(3)](http://bluewhale.cc/wp-content/uploads/2017/04/df_head3.png)](http://bluewhale.cc/wp-content/uploads/2017/04/df_head3.png)

## 查看后10行数据

Tail行数与head函数相反，用来查看数据表中后N行的数据，默认tail()显示后10行数据，可以自己设置参数值来确定查看的行数。下面的代码中设置查看后3行的数据。

|     |     |
| --- | --- |
| 1<br><br>2 | `#查看最后3行`<br><br>`df.tail(``3``)` |

[![df_tail(3)](http://bluewhale.cc/wp-content/uploads/2017/04/df_tail3.png)](http://bluewhale.cc/wp-content/uploads/2017/04/df_tail3.png)

# 3，数据表清洗

第三部分是对数据表中的问题进行清洗。主要内容包括对空值，大小写问题，数据格式和重复值的处理。这里不包含对数据间的逻辑验证。

## 处理空值(删除或填充)

我们在创建数据表的时候在price字段中故意设置了几个NA值。对于空值的处理方式有很多种，可以直接删除包含空值的数据，也可以对空值进行填充，比如用0填充或者用均值填充。还可以根据不同字段的逻辑对空值进行推算。

Excel中可以通过“查找和替换”功能对空值进行处理，将空值统一替换为0或均值。也可以通过“定位”空值来实现。

[![查找和替换空值](http://bluewhale.cc/wp-content/uploads/2017/04/%E6%9F%A5%E6%89%BE%E5%92%8C%E6%9B%BF%E6%8D%A2%E7%A9%BA%E5%80%BC.png)](http://bluewhale.cc/wp-content/uploads/2017/04/%E6%9F%A5%E6%89%BE%E5%92%8C%E6%9B%BF%E6%8D%A2%E7%A9%BA%E5%80%BC.png)

Python中处理空值的方法比较灵活，可以使用 Dropna函数用来删除数据表中包含空值的数据，也可以使用fillna函数对空值进行填充。下面的代码和结果中可以看到使用dropna函数后，包含NA值的两个字段已经不见了。返回的是一个不包含空值的数据表。

|     |     |
| --- | --- |
| 1<br><br>2 | `#删除数据表中含有空值的行`<br><br>`df.dropna(how``=``'any'``)` |

[![df_dropna](http://bluewhale.cc/wp-content/uploads/2017/04/df_dropna.png)](http://bluewhale.cc/wp-content/uploads/2017/04/df_dropna.png)

除此之外也可以使用数字对空值进行填充，下面的代码使用fillna函数对空值字段填充数字0。

|     |     |
| --- | --- |
| 1<br><br>2 | `#使用数字0填充数据表中空值`<br><br>`df.fillna(value``=``0``)` |

我们选择填充的方式来处理空值，使用price列的均值来填充NA字段，同样使用fillna函数，在要填充的数值中使用mean函数先计算price列当前的均值，然后使用这个均值对NA进行填
充。可以看到两个空值字段显示为3299.5

|     |     |
| --- | --- |
| 1<br><br>2<br><br>3<br><br>4<br><br>5<br><br>6<br><br>7<br><br>8<br><br>9<br><br>10 | `#使用price均值对NA进行填充`<br><br>`df[``'price'``].fillna(df[``'price'``].mean())`<br><br>`0`    `1200.0`<br><br>`1`    `3299.5`<br><br>`2`    `2133.0`<br><br>`3`    `5433.0`<br><br>`4`    `3299.5`<br><br>`5`    `4432.0`<br><br>`Name: price, dtype: float64` |

[![df_nan](http://bluewhale.cc/wp-content/uploads/2017/04/df_nan.png)](http://bluewhale.cc/wp-content/uploads/2017/04/df_nan.png)

## 清理空格

除了空值，字符中的空格也是数据清洗中一个常见的问题，下面是清除字符中空格的代码。

|     |     |
| --- | --- |
| 1<br><br>2 | `#清除city字段中的字符空格`<br><br>`df[``'city'``]``=``df[``'city'``].``map``(``str``.strip)` |

## 大小写转换

在英文字段中，字母的大小写不统一也是一个常见的问题。Excel中有UPPER，LOWER等函数，python中也有同名函数用来解决大小写的问题。在数据表的city列中就存在这样的问题。我们将city列的所有字母转换为小写。下面是具体的代码和结果。

|     |     |
| --- | --- |
| 1<br><br>2<br><br>3 | `#city列大小写转换`<br><br>`df[``'city'``]``=``df[``'city'``].``str``.lower()` |

## ![lower](http://bluewhale.cc/wp-content/uploads/2017/04/lower.png)

## 更改数据格式

Excel中通过“设置单元格格式”功能可以修改数据格式。Python中通过astype函数用来修改数据格式。
[![设置单元格格式](http://bluewhale.cc/wp-content/uploads/2017/04/%E8%AE%BE%E7%BD%AE%E5%8D%95%E5%85%83%E6%A0%BC%E6%A0%BC%E5%BC%8F.png)](http://bluewhale.cc/wp-content/uploads/2017/04/%E8%AE%BE%E7%BD%AE%E5%8D%95%E5%85%83%E6%A0%BC%E6%A0%BC%E5%BC%8F.png)
Python中dtype是查看数据格式的函数，与之对应的是astype函数，用来更改数据格式。下面的代码中将price字段的值修改为int格式。

|     |     |
| --- | --- |
| 1<br><br>2<br><br>3<br><br>4<br><br>5<br><br>6<br><br>7<br><br>8<br><br>9<br><br>10 | `#更改数据格式`<br><br>`df[``'price'``].astype(``'int'``)`<br><br>`0`    `1200`<br><br>`1`    `3299`<br><br>`2`    `2133`<br><br>`3`    `5433`<br><br>`4`    `3299`<br><br>`5`    `4432`<br><br>`Name: price, dtype: int32` |

## 更改列名称

Rename是更改列名称的函数，我们将来数据表中的category列更改为category-size。下面是具体的代码和更改后的结果。

|     |     |
| --- | --- |
| 1<br><br>2 | `#更改列名称`<br><br>`df.rename(columns``=``{``'category'``:` `'category-size'``})` |

[![df_rename](http://bluewhale.cc/wp-content/uploads/2017/04/df_rename.png)](http://bluewhale.cc/wp-content/uploads/2017/04/df_rename.png)

## 删除重复值

很多数据表中还包含重复值的问题，Excel的数据目录下有“删除重复项”的功能，可以用来删除数据表中的重复值。默认Excel会保留最先出现的数据，删除后面重复出现的数据。
[![删除重复项](http://bluewhale.cc/wp-content/uploads/2017/04/%E5%88%A0%E9%99%A4%E9%87%8D%E5%A4%8D%E9%A1%B9.png)](http://bluewhale.cc/wp-content/uploads/2017/04/%E5%88%A0%E9%99%A4%E9%87%8D%E5%A4%8D%E9%A1%B9.png)
Python中使用drop\_duplicates函数删除重复值。我们以数据表中的city列为例，city字段中存在重复值。默认情况下drop\_duplicates()将删除后出现的重复值(与excel逻辑一致)。增加keep=’last’参数后将删除最先出现的重复值，保留最后的值。下面是具体的代码和比较结果。

原始的city列中beijing存在重复，分别在第一位和最后一位。

|     |     |
| --- | --- |
| 1<br><br>2<br><br>3<br><br>4<br><br>5<br><br>6<br><br>7<br><br>8 | `df[``'city'``]`<br><br>`0`      `beijing`<br><br>`1`           `sh`<br><br>`2`    `guangzhou`<br><br>`3`     `shenzhen`<br><br>`4`     `shanghai`<br><br>`5`      `beijing`<br><br>`Name: city, dtype:` `object` |

使用默认的drop_duplicates()函数删除重复值，从结果中可以看到第一位的beijing被保留，最后出现的beijing被删除。

|     |     |
| --- | --- |
| 1<br><br>2<br><br>3<br><br>4<br><br>5<br><br>6<br><br>7<br><br>8 | `#删除后出现的重复值`<br><br>`df[``'city'``].drop_duplicates()`<br><br>`0`      `beijing`<br><br>`1`           `sh`<br><br>`2`    `guangzhou`<br><br>`3`     `shenzhen`<br><br>`4`     `shanghai`<br><br>`Name: city, dtype:` `object` |

设置keep=’last‘’参数后，与之前删除重复值的结果相反，第一位出现的beijing被删除，保留了最后一位出现的beijing。

|     |     |
| --- | --- |
| 1<br><br>2<br><br>3<br><br>4<br><br>5<br><br>6<br><br>7<br><br>8 | `#删除先出现的重复值`<br><br>`df[``'city'``].drop_duplicates(keep``=``'last'``)`<br><br>`1`           `sh`<br><br>`2`    `guangzhou`<br><br>`3`     `shenzhen`<br><br>`4`     `shanghai`<br><br>`5`      `beijing`<br><br>`Name: city, dtype: objec` |

## 数值修改及替换

数据清洗中最后一个问题是数值修改或替换，Excel中使用“查找和替换”功能就可以实现数值的替换。

[![查找和替换空值](http://bluewhale.cc/wp-content/uploads/2017/04/%E6%9F%A5%E6%89%BE%E5%92%8C%E6%9B%BF%E6%8D%A2%E7%A9%BA%E5%80%BC1.png)](http://bluewhale.cc/wp-content/uploads/2017/04/%E6%9F%A5%E6%89%BE%E5%92%8C%E6%9B%BF%E6%8D%A2%E7%A9%BA%E5%80%BC1.png)

Python中使用replace函数实现数据替换。数据表中city字段上海存在两种写法，分别为shanghai和SH。我们使用replace函数对SH进行替换。

|     |     |
| --- | --- |
| 1<br><br>2<br><br>3<br><br>4<br><br>5<br><br>6<br><br>7<br><br>8<br><br>9 | `#数据替换`<br><br>`df[``'city'``].replace(``'sh'``,` `'shanghai'``)`<br><br>`0`      `beijing`<br><br>`1`     `shanghai`<br><br>`2`    `guangzhou`<br><br>`3`     `shenzhen`<br><br>`4`     `shanghai`<br><br>`5`      `beijing`<br><br>`Name: city, dtype:` `object` |

本篇文章这是系列的第二篇，介绍第4-6部分的内容，数据表生成，数据表查看，和数据清洗。

# [![4-6目录](http://bluewhale.cc/wp-content/uploads/2017/04/4-6%E7%9B%AE%E5%BD%95.png)](http://bluewhale.cc/wp-content/uploads/2017/04/4-6%E7%9B%AE%E5%BD%95.png)
**4，数据预处理**

第四部分是数据的预处理，对清洗完的数据进行整理以便后期的统计和分析工作。主要包括数据表的合并，排序，数值分列，数据分
组及标记等工作。

## 数据表合并

首先是对不同的数据表进行合并，我们这里创建一个新的数据表df1，并将df和df1两个数据表进行合并。在Excel中没有直接完成数据表合并的功能，可以通过VLOOKUP函数分步实现。在python中可以通过merge函数一次性实现。下面建立df1数据表，用于和df数据表进行合并。

|     |     |
| --- | --- |
| 1<br><br>2<br><br>3<br><br>4<br><br>5 | `#创建df1数据表`<br><br>`df1``=``pd.DataFrame({``"id"``:[``1001``,``1002``,``1003``,``1004``,``1005``,``1006``,``1007``,``1008``],`<br><br>`"gender"``:[``'male'``,``'female'``,``'male'``,``'female'``,``'male'``,``'female'``,``'male'``,``'female'``],`<br><br>`"pay"``:[``'Y'``,``'N'``,``'Y'``,``'Y'``,``'N'``,``'Y'``,``'N'``,``'Y'``,],`<br><br>`"m-point"``:[``10``,``12``,``20``,``40``,``40``,``40``,``30``,``20``]})` |

[![df1](http://bluewhale.cc/wp-content/uploads/2017/04/df1.png)](http://bluewhale.cc/wp-content/uploads/2017/04/df1.png)

使用merge函数对两个数据表进行合并，合并的方式为inner，将两个数据表中共有的数据匹配到一起生成新的数据表。并命名为df_inner。

|     |     |
| --- | --- |
| 1<br><br>2 | `#数据表匹配合并，inner模式`<br><br>`df_inner``=``pd.merge(df,df1,how``=``'inner'``)` |

[![df_inner](http://bluewhale.cc/wp-content/uploads/2017/04/df_inner.png)](http://bluewhale.cc/wp-content/uploads/2017/04/df_inner.png)

除了inner方式以外，合并的方式还有left，right和outer方式。这几种方式的差别在我其他的文章中有详细的说明和对比。

|     |     |
| --- | --- |
| 1<br><br>2<br><br>3<br><br>4 | `#其他数据表匹配模式`<br><br>`df_left``=``pd.merge(df,df1,how``=``'left'``)`<br><br>`df_right``=``pd.merge(df,df1,how``=``'right'``)`<br><br>`df_outer``=``pd.merge(df,df1,how``=``'outer'``)` |

## 设置索引列

完成数据表的合并后，我们对df_inner数据表设置索引列，索引列的功能很多，可以进行数据提取，汇总，也可以进行数据筛选等。
设置索引的函数为set_index。

|     |     |
| --- | --- |
| 1<br><br>2 | `#设置索引列`<br><br>`df_inner.set_index(``'id'``)` |

## [![df_inner_set_index](../../../_resources/df_inner_set_index_c96737b9693345c58583d5b24ccce02.png)](http://bluewhale.cc/wp-content/uploads/2017/04/df_inner_set_index.png)
排序(按索引，按数值)

Excel中可以通过数据目录下的排序按钮直接对数据表进行排序，比较简单。Python中需要使用ort\_values函数和sort\_index函数完成排序。

[![排序](http://bluewhale.cc/wp-content/uploads/2017/04/%E6%8E%92%E5%BA%8F.png)](http://bluewhale.cc/wp-content/uploads/2017/04/%E6%8E%92%E5%BA%8F.png)

在python中，既可以按索引对数据表进行排序，也可以看制定列的数值进行排序。首先我们按age列中用户的年龄对数据表进行排序。
使用的函数为sort_values。

|     |     |
| --- | --- |
| 1<br><br>2 | `#按特定列的值排序`<br><br>`df_inner.sort_values(by``=``[``'age'``])` |

[![sort_values](../../../_resources/sort_values_f059ae30150a4244a67b61d5555ca79a.png)](http://bluewhale.cc/wp-content/uploads/2017/04/sort_values.png)

Sort_index函数用来将数据表按索引列的值进行排序。

|     |     |
| --- | --- |
| 1<br><br>2 | `#按索引列排序`<br><br>`df_inner.sort_index()` |

## [![sort_index](../../../_resources/sort_index_1e072b06f56f4e1682f939a73daa08f7.png)](http://bluewhale.cc/wp-content/uploads/2017/04/sort_index.png)
数据分组

Excel中可以通过VLOOKUP函数进行近似匹配来完成对数值的分组，或者使用“数据透视表”来完成分组。相应的 python中使用where函数完成数据分组。

Where函数用来对数据进行判断和分组，下面的代码中我们对price列的值进行判断，将符合条件的分为一组，不符合条件的分为另一组，并使用group字段进行标记。

|     |     |
| --- | --- |
| 1<br><br>2 | `#如果price列的值>3000，group列显示high，否则显示low`<br><br>`df_inner[``'group'``]` `=` `np.where(df_inner[``'price'``] >` `3000``,``'high'``,``'low'``)` |

[![where](../../../_resources/where_57c81e7ee55243cd9b6133a3ce3bef01.png)](http://bluewhale.cc/wp-content/uploads/2017/04/where.png)

除了where函数以外，还可以对多个字段的值进行判断后对数据进行分组，下面的代码中对city列等于beijing并且price列大于等于4000的数据标记为1。

|     |     |
| --- | --- |
| 1<br><br>2 | `#对复合多个条件的数据进行分组标记`<br><br>`df_inner.loc[(df_inner[``'city'``]` `=``=` `'beijing'``) & (df_inner[``'price'``] >``=` `4000``),` `'sign'``]``=``1` |

## [![sign](../../../_resources/sign_552c90a18cc0480988621aba0f502f60.png)](http://bluewhale.cc/wp-content/uploads/2017/04/sign.png)
数据分列

与数据分组相反的是对数值进行分列，Excel中的数据目录下提供“分列”功能。在python中使用split函数实现分列。

[![数据分列](http://bluewhale.cc/wp-content/uploads/2017/04/%E6%95%B0%E6%8D%AE%E5%88%86%E5%88%97.png)](http://bluewhale.cc/wp-content/uploads/2017/04/%E6%95%B0%E6%8D%AE%E5%88%86%E5%88%97.png)
在数据表中category列中的数据包含有两个信息，前面的数字为类别id，后面的字母为size值。中间以连字符进行连接。我们使用split函数对这个字段进行拆分，并将拆分后的数据表匹配回原数据表中。

|     |     |
| --- | --- |
| 1<br><br>2 | `#对category字段的值依次进行分列，并创建数据表，索引值为df_inner的索引列，列名称为category和size`<br><br>`pd.DataFrame((x.split(``'-'``)` `for` `x` `in` `df_inner[``'category'``]),index``=``df_inner.index,columns``=``[``'category'``,``'size'``])` |

[![split](../../../_resources/split_08d0c600bd4c4003a15535be3c37171b.png)](http://bluewhale.cc/wp-content/uploads/2017/04/split.png)

|     |     |
| --- | --- |
| 1<br><br>2 | `#将完成分列后的数据表与原df_inner数据表进行匹配`<br><br>`df_inner``=``pd.merge(df_inner,split,right_index``=``True``, left_index``=``True``)` |

# [![merge_1](../../../_resources/merge_1-1024x298_526b78094b06404c95eaa17ad79bcef3.png)](http://bluewhale.cc/wp-content/uploads/2017/04/merge_1.png)
**5，数据提取**

第五部分是数据提取，也是数据分析中最常见的一个工作。这部分主要使用三个函数，loc，iloc和ix，loc函数按标签值进行提取，iloc按位置进行提取，ix可以同时按标签和位置进行提取。下面介绍每一种函数的使用方法。

## 按标签提取(loc)

Loc函数按数据表的索引标签进行提取，下面的代码中提取了索引列为3的单条数据。

|     |     |
| --- | --- |
| 1<br><br>2<br><br>3<br><br>4<br><br>5<br><br>6<br><br>7<br><br>8<br><br>9<br><br>10<br><br>11<br><br>12<br><br>13<br><br>14<br><br>15<br><br>16 | `#按索引提取单行的数值`<br><br>`df_inner.loc[``3``]`<br><br>`id` `1004`<br><br>`date` `2013``-``01``-``05` `00``:``00``:``00`<br><br>`city shenzhen`<br><br>`category` `110``-``C`<br><br>`age` `32`<br><br>`price` `5433`<br><br>`gender female`<br><br>`m``-``point` `40`<br><br>`pay Y`<br><br>`group high`<br><br>`sign NaN`<br><br>`category_1` `110`<br><br>`size C`<br><br>`Name:` `3``, dtype:` `object` |

使用冒号可以限定提取数据的范围，冒号前面为开始的标签值，后面为结束的标签值。下面提取了0到5的数据行。

|     |     |
| --- | --- |
| 1<br><br>2 | `#按索引提取区域行数值`<br><br>`df_inner.loc[``0``:``5``]` |

[![df_inner_loc1](../../../_resources/df_inner_loc1-1024x171_99a1f6c14eb447fe93d36e882bf.png)](http://bluewhale.cc/wp-content/uploads/2017/04/df_inner_loc1.png)

Reset_index函数用于恢复索引，这里我们重新将date字段的日期设置为数据表的索引，并按日期进行数据提取。

|     |     |
| --- | --- |
| 1<br><br>2 | `#重设索引`<br><br>`df_inner.reset_index()` |

[![reset_index](../../../_resources/reset_index-1024x274_e87c7580a74e468d96fcdb6b3d4b8.png)](http://bluewhale.cc/wp-content/uploads/2017/04/reset_index.png)

|     |     |
| --- | --- |
| 1<br><br>2 | `#设置日期为索引`<br><br>`df_inner``=``df_inner.set_index(``'date'``)` |

[![set_index_date](../../../_resources/set_index_date-1024x338_0b17ce80f6eb4267835038ed01.png)](http://bluewhale.cc/wp-content/uploads/2017/04/set_index_date.png)

使用冒号限定提取数据的范围，冒号前面为空表示从0开始。提取所有2013年1月4日以前的数据。

|     |     |
| --- | --- |
| 1<br><br>2 | `#提取4日之前的所有数据`<br><br>`df_inner[:``'2013-01-04'``]` |

[![按提起提取](http://bluewhale.cc/wp-content/uploads/2017/04/%E6%8C%89%E6%8F%90%E8%B5%B7%E6%8F%90%E5%8F%96-1024x217.png)](http://bluewhale.cc/wp-content/uploads/2017/04/%E6%8C%89%E6%8F%90%E8%B5%B7%E6%8F%90%E5%8F%96.png)

## 按位置提取(iloc)

使用iloc函数按位置对数据表中的数据进行提取，这里冒号前后的数字不再是索引的标签名称，而是数据所在的位置，从0开始。

|     |     |
| --- | --- |
| 1<br><br>2 | `#使用iloc按位置区域提取数据`<br><br>`df_inner.iloc[:``3``,:``2``]` |

[![iloc1](../../../_resources/iloc1_09a737124f834c308e6fa7f572a33fa5.png)](http://bluewhale.cc/wp-content/uploads/2017/04/iloc1.png)

iloc函数除了可以按区域提取数据，还可以按位置逐条提取，前面方括号中的0,2,5表示数据所在行的位置，后面方括号中的数表示所在列的位置。

|     |     |
| --- | --- |
| 1<br><br>2 | `#使用iloc按位置单独提取数据`<br><br>`df_inner.iloc[[``0``,``2``,``5``],[``4``,``5``]]` |

[![iloc2](../../../_resources/iloc2_19cd45c464a843dd8d86999959a2a2db.png)](http://bluewhale.cc/wp-content/uploads/2017/04/iloc2.png)

## 按标签和位置提取（ix）

ix是loc和iloc的混合，既能按索引标签提取，也能按位置进行数据提取。下面代码中行的位置按索引日期设置，列按位置设置。

|     |     |
| --- | --- |
| 1<br><br>2 | `#使用ix按索引标签和位置混合提取数据`<br><br>`df_inner.ix[:``'2013-01-03'``,:``4``]` |

## [![ix](../../../_resources/ix_c4cb49794f89479d89021c3c357df01f.png)](http://bluewhale.cc/wp-content/uploads/2017/04/ix.png)
按条件提取（区域和条件值）

除了按标签和位置提起数据以外，还可以按具体的条件进行数据。下面使用loc和isin两个函数配合使用，按指定条件对数据进行提取 。

使用isin函数对city中的值是否为beijing进行判断。

|     |     |
| --- | --- |
| 1<br><br>2<br><br>3<br><br>4<br><br>5<br><br>6<br><br>7<br><br>8<br><br>9<br><br>10<br><br>11 | `#判断city列的值是否为beijing`<br><br>`df_inner[``'city'``].isin([``'beijing'``])`<br><br>`date`<br><br>`2013``-``01``-``02` `True`<br><br>`2013``-``01``-``05` `False`<br><br>`2013``-``01``-``07` `True`<br><br>`2013``-``01``-``06` `False`<br><br>`2013``-``01``-``03` `False`<br><br>`2013``-``01``-``04` `False`<br><br>`Name: city, dtype:` `bool` |

将isin函数嵌套到loc的数据提取函数中，将判断结果为Ture数据提取出来。这里我们把判断条件改为city值是否为beijing和 shanghai。如果是就把这条数据提取出来。

|     |     |
| --- | --- |
| 1<br><br>2 | `#先判断city列里是否包含beijing和shanghai，然后将复合条件的数据提取出来。`<br><br>`df_inner.loc[df_inner[``'city'``].isin([``'beijing'``,``'shanghai'``])]` |

[![loc按筛选条件提取](http://bluewhale.cc/wp-content/uploads/2017/04/loc%E6%8C%89%E7%AD%9B%E9%80%89%E6%9D%A1%E4%BB%B6%E6%8F%90%E5%8F%96-1024x266.png)](http://bluewhale.cc/wp-content/uploads/2017/04/loc%E6%8C%89%E7%AD%9B%E9%80%89%E6%9D%A1%E4%BB%B6%E6%8F%90%E5%8F%96.png)

数值提取还可以完成类似数据分列的工作，从合并的数值中提取出制定的数值。

|     |     |
| --- | --- |
| 1<br><br>2<br><br>3<br><br>4<br><br>5<br><br>6<br><br>7<br><br>8 | `category``=``df_inner[``'category'``]`<br><br>`0` `100``-``A`<br><br>`3` `110``-``C`<br><br>`5` `130``-``F`<br><br>`4` `210``-``A`<br><br>`1` `100``-``B`<br><br>`2` `110``-``A`<br><br>`Name: category, dtype:` `object` |

|     |     |
| --- | --- |
| 1<br><br>2 | `#提取前三个字符，并生成数据表`<br><br>`pd.DataFrame(category.``str``[:``3``])` |

# [![category_str](../../../_resources/category_str_997e403e3f6a411bb225bb8c459434de.png)](http://bluewhale.cc/wp-content/uploads/2017/04/category_str.png)
**6，数据筛选**

第六部分为数据筛选，使用与，或，非三个条件配合大于，小于和等于对数据进行筛选，并进行计数和求和。与excel中的筛选功能和countifs和sumifs功能相似。

## 按条件筛选（与，或，非）

Excel数据目录下提供了“筛选”功能，用于对数据表按不同的条件进行筛选。Python中使用loc函数配合筛选条件来完成筛选功能。配合sum和count函数还能实现excel中sumif和countif函数的功能。

[![筛选](http://bluewhale.cc/wp-content/uploads/2017/04/%E7%AD%9B%E9%80%89.png)](http://bluewhale.cc/wp-content/uploads/2017/04/%E7%AD%9B%E9%80%89.png)
使用“与”条件进行筛选，条件是年龄大于25岁，并且城市为beijing。筛选后只有一条数据符合要求。

|     |     |
| --- | --- |
| 1<br><br>2 | `#使用“与”条件进行筛选`<br><br>`df_inner.loc[(df_inner[``'age'``] >` `25``) & (df_inner[``'city'``]` `=``=` `'beijing'``), [``'id'``,``'city'``,``'age'``,``'category'``,``'gender'``]]` |

[![与](http://bluewhale.cc/wp-content/uploads/2017/04/%E4%B8%8E.png)](http://bluewhale.cc/wp-content/uploads/2017/04/%E4%B8%8E.png)

使用“或”条件进行筛选，年龄大于25岁或城市为beijing。筛选后有6条数据符合要求。

|     |     |
| --- | --- |
| 1<br><br>2<br><br>3 | `#使用“或”条件筛选`<br><br>`df_inner.loc[(df_inner[``'age'``] >` `25``) \| (df_inner[``'city'``]` `=``=` `'beijing'``), [``'id'``,``'city'``,``'age'``,``'category'``,``'gender'``]].sort`<br><br>`([``'age'``])` |

[![或](http://bluewhale.cc/wp-content/uploads/2017/04/%E6%88%96.png)](http://bluewhale.cc/wp-content/uploads/2017/04/%E6%88%96.png)

在前面的代码后增加price字段以及sum函数，按筛选后的结果将price字段值进行求和，相当于excel中sumifs的功能。

|     |     |
| --- | --- |
| 1<br><br>2<br><br>3<br><br>4<br><br>5 | `#对筛选后的数据按price字段进行求和`<br><br>`df_inner.loc[(df_inner[``'age'``] >` `25``) \| (df_inner[``'city'``]` `=``=` `'beijing'``),`<br><br>`[``'id'``,``'city'``,``'age'``,``'category'``,``'gender'``,``'price'``]].sort([``'age'``]).price.``sum``()`<br><br>`19796` |

使用“非”条件进行筛选，城市不等于beijing。符合条件的数据有4条。将筛选结果按id列进行排序。

|     |     |
| --- | --- |
| 1<br><br>2 | `#使用“非”条件进行筛选`<br><br>`df_inner.loc[(df_inner[``'city'``] !``=` `'beijing'``), [``'id'``,``'city'``,``'age'``,``'category'``,``'gender'``]].sort([``'id'``])` |

[![非](http://bluewhale.cc/wp-content/uploads/2017/04/%E9%9D%9E.png)](http://bluewhale.cc/wp-content/uploads/2017/04/%E9%9D%9E.png)

在前面的代码后面增加city列，并使用count函数进行计数。相当于excel中的countifs函数的功能。

|     |     |
| --- | --- |
| 1<br><br>2<br><br>3 | `#对筛选后的数据按city列进行计数`<br><br>`df_inner.loc[(df_inner[``'city'``] !``=` `'beijing'``), [``'id'``,``'city'``,``'age'``,``'category'``,``'gender'``]].sort([``'id'``]).city.count()`<br><br>`4` |

还有一种筛选的方式是用query函数。下面是具体的代码和筛选结果。

|     |     |
| --- | --- |
| 1<br><br>2 | `#使用query函数进行筛选`<br><br>`df_inner.query(``'city == ["beijing", "shanghai"]'``)` |

[![query](../../../_resources/query-1024x263_edd0fe0a68574d2897b6eb435594069c.png)](http://bluewhale.cc/wp-content/uploads/2017/04/query.png)

在前面的代码后增加price字段和sum函数。对筛选后的price字段进行求和，相当于excel中的sumifs函数的功能。

|     |     |
| --- | --- |
| 1<br><br>2<br><br>3 | `#对筛选后的结果按price进行求和`<br><br>`df_inner.query(``'city == ["beijing", "shanghai"]'``).price.``sum``()`<br><br>`12230` |

这是第三篇，介绍第7-9部分的内容，数据汇总，数据统计，和数据输出。

[![7-9目录](http://bluewhale.cc/wp-content/uploads/2017/04/7-9%E7%9B%AE%E5%BD%95.png)](http://bluewhale.cc/wp-content/uploads/2017/04/7-9%E7%9B%AE%E5%BD%95.png)

# **7，数据汇总**

第七部分是对数据进行分类汇总，Excel中使用分类汇总和数据透视可以按特定维度对数据进行汇总，python中使用的主要函数是groupby和pivot_table。下面分别介绍这两个函数的使用方法。

## **分类汇总**

Excel的数据目录下提供了“分类汇总”功能，可以按指定的字段和汇总方式对数据表进行汇总。Python中通过Groupby函数完成相应的操作，并可以支持多级分类汇总。

[![分类汇总1](http://bluewhale.cc/wp-content/uploads/2017/04/%E5%88%86%E7%B1%BB%E6%B1%87%E6%80%BB1.png)](http://bluewhale.cc/wp-content/uploads/2017/04/%E5%88%86%E7%B1%BB%E6%B1%87%E6%80%BB1.png)
Groupby是进行分类汇总的函数，使用方法很简单，制定要分组的列名称就可以，也可以同时制定多个列名称，groupby按列名称出现的顺序进行分组。同时要制定分组后的汇总方式，常见的是计数和求和两种。

|     |     |
| --- | --- |
| 1<br><br>2 | `#对所有列进行计数汇总`<br><br>`df_inner.groupby(``'city'``).count()` |

[![groupby](../../../_resources/groupby_c51af09bc49f45268e0745766eea0096.png)](http://bluewhale.cc/wp-content/uploads/2017/04/groupby.png)

可以在groupby中设置列名称来对特定的列进行汇总。下面的代码中按城市对id字段进行汇总计数。

|     |     |
| --- | --- |
| 1<br><br>2<br><br>3<br><br>4<br><br>5<br><br>6<br><br>7<br><br>8 | `#对特定的ID列进行计数汇总`<br><br>`df_inner.groupby(``'city'``)[``'id'``].count()`<br><br>`city`<br><br>`beijing` `2`<br><br>`guangzhou` `1`<br><br>`shanghai` `2`<br><br>`shenzhen` `1`<br><br>`Name:` `id``, dtype: int64` |

在前面的基础上增加第二个列名称，分布对city和size两个字段进行计数汇总。

|     |     |
| --- | --- |
| 1<br><br>2<br><br>3<br><br>4<br><br>5<br><br>6<br><br>7<br><br>8<br><br>9<br><br>10 | `#对两个字段进行汇总计数`<br><br>`df_inner.groupby([``'city'``,``'size'``])[``'id'``].count()`<br><br>`city size`<br><br>`beijing A` `1`<br><br>`F` `1`<br><br>`guangzhou A` `1`<br><br>`shanghai A` `1`<br><br>`B` `1`<br><br>`shenzhen C` `1`<br><br>`Name:` `id``, dtype: int64` |

除了计数和求和外，还可以对汇总后的数据同时按多个维度进行计算，下面的代码中按城市对price字段进行汇总，并分别计算price的数量，总金额和平均金额。

|     |     |
| --- | --- |
| 1<br><br>2 | `#对city字段进行汇总并计算price的合计和均值。`<br><br>`df_inner.groupby(``'city'``)[``'price'``].agg([``len``,np.``sum``, np.mean])` |

## [![groupby1](../../../_resources/groupby1_8fdc66cadaca40579e2e5b837a351797.png)](http://bluewhale.cc/wp-content/uploads/2017/04/groupby1.png)
数据透视

Excel中的插入目录下提供“数据透视表”功能对数据表按特定维度进行汇总。Python中也提供了数据透视表功能。通过pivot_table函数实现同样的效果。
[![数据透视](http://bluewhale.cc/wp-content/uploads/2017/04/%E6%95%B0%E6%8D%AE%E9%80%8F%E8%A7%86.png)](http://bluewhale.cc/wp-content/uploads/2017/04/%E6%95%B0%E6%8D%AE%E9%80%8F%E8%A7%86.png)
数据透视表也是常用的一种数据分类汇总方式，并且功能上比groupby要强大一些。下面的代码中设定city为行字段，size为列字段，price为值字段。分别计算price的数量和金额并且按行与列进行汇总。

|     |     |
| --- | --- |
| 1<br><br>2 | `#数据透视表`<br><br>`pd.pivot_table(df_inner,index``=``[``"city"``],values``=``[``"price"``],columns``=``[``"size"``],aggfunc``=``[``len``,np.``sum``],fill_value``=``0``,margins``=``True``)` |

[![pivot_table](../../../_resources/pivot_table_197cf13350c947a886ebe633618f4d84.png)](http://bluewhale.cc/wp-content/uploads/2017/04/pivot_table.png)

# **8，数据统计**

第九部分为数据统计，这里主要介绍数据采样，标准差，协方差和相关系数的使用方法。

## 数据采样

Excel的数据分析功能中提供了数据抽样的功能，如下图所示。Python通过sample函数完成数据采样。

[![数据抽样](http://bluewhale.cc/wp-content/uploads/2017/04/%E6%95%B0%E6%8D%AE%E6%8A%BD%E6%A0%B7.png)](http://bluewhale.cc/wp-content/uploads/2017/04/%E6%95%B0%E6%8D%AE%E6%8A%BD%E6%A0%B7.png)

Sample是进行数据采样的函数，设置n的数量就可以了。函数自动返回参与的结果。

|     |     |
| --- | --- |
| 1<br><br>2 | `#简单的数据采样`<br><br>`df_inner.sample(n``=``3``)` |

[![简单随机采样](http://bluewhale.cc/wp-content/uploads/2017/04/%E7%AE%80%E5%8D%95%E9%9A%8F%E6%9C%BA%E9%87%87%E6%A0%B7-1024x218.png)](http://bluewhale.cc/wp-content/uploads/2017/04/%E7%AE%80%E5%8D%95%E9%9A%8F%E6%9C%BA%E9%87%87%E6%A0%B7.png)

Weights参数是采样的权重，通过设置不同的权重可以更改采样的结果，权重高的数据将更有希望被选中。这里手动设置6条数据的权重值。将前面4个设置为0，后面两个分别设置为0.5。

|     |     |
| --- | --- |
| 1<br><br>2<br><br>3 | `#手动设置采样权重`<br><br>`weights` `=` `[``0``,` `0``,` `0``,` `0``,` `0.5``,` `0.5``]`<br><br>`df_inner.sample(n``=``2``, weights``=``weights)` |

[![手动设置采样权重1](http://bluewhale.cc/wp-content/uploads/2017/04/%E6%89%8B%E5%8A%A8%E8%AE%BE%E7%BD%AE%E9%87%87%E6%A0%B7%E6%9D%83%E9%87%8D1-1024x341.png)](http://bluewhale.cc/wp-content/uploads/2017/04/%E6%89%8B%E5%8A%A8%E8%AE%BE%E7%BD%AE%E9%87%87%E6%A0%B7%E6%9D%83%E9%87%8D1.png)

从采样结果中可以看出，后两条权重高的数据被选中。

[![手动设置采样权重2](http://bluewhale.cc/wp-content/uploads/2017/04/%E6%89%8B%E5%8A%A8%E8%AE%BE%E7%BD%AE%E9%87%87%E6%A0%B7%E6%9D%83%E9%87%8D2-1024x176.png)](http://bluewhale.cc/wp-content/uploads/2017/04/%E6%89%8B%E5%8A%A8%E8%AE%BE%E7%BD%AE%E9%87%87%E6%A0%B7%E6%9D%83%E9%87%8D2.png)
Sample函数中还有一个参数replace，用来设置采样后是否放回。

|     |     |
| --- | --- |
| 1<br><br>2 | `#采样后不放回`<br><br>`df_inner.sample(n``=``6``, replace``=``False``)` |

[![采样后不放回](http://bluewhale.cc/wp-content/uploads/2017/04/%E9%87%87%E6%A0%B7%E5%90%8E%E4%B8%8D%E6%94%BE%E5%9B%9E-1024x343.png)](http://bluewhale.cc/wp-content/uploads/2017/04/%E9%87%87%E6%A0%B7%E5%90%8E%E4%B8%8D%E6%94%BE%E5%9B%9E.png)

|     |     |
| --- | --- |
| 1<br><br>2 | `#采样后放回`<br><br>`df_inner.sample(n``=``6``, replace``=``True``)` |

## [![采样后放回](http://bluewhale.cc/wp-content/uploads/2017/04/%E9%87%87%E6%A0%B7%E5%90%8E%E6%94%BE%E5%9B%9E-1024x348.png)](http://bluewhale.cc/wp-content/uploads/2017/04/%E9%87%87%E6%A0%B7%E5%90%8E%E6%94%BE%E5%9B%9E.png)
描述统计

Excel中的数据分析中提供了描述统计的功能。Python中可以通过Describe对数据进行描述统计。

[![描述统计](http://bluewhale.cc/wp-content/uploads/2017/04/%E6%8F%8F%E8%BF%B0%E7%BB%9F%E8%AE%A1.png)](http://bluewhale.cc/wp-content/uploads/2017/04/%E6%8F%8F%E8%BF%B0%E7%BB%9F%E8%AE%A1.png)

Describe函数是进行描述统计的函数，自动生成数据的数量，均值，标准差等数据。下面的代码中对数据表进行描述统计，并使用round函数设置结果显示的小数位。并对结果数据进行转置。

|     |     |
| --- | --- |
| 1<br><br>2 | `#数据表描述性统计`<br><br>`df_inner.describe().``round``(``2``).T` |

[![describe](http://bluewhale.cc/wp-content/uploads/2017/04/describe.png)](http://bluewhale.cc/wp-content/uploads/2017/04/describe.png)
**标准差**
Python中的Std函数用来接算特定数据列的标准差。

|     |     |
| --- | --- |
| 1<br><br>2<br><br>3 | `#标准差`<br><br>`df_inner[``'price'``].std()`<br><br>`1523.3516556155596` |

**协方差**
Excel中的数据分析功能中提供协方差的计算，python中通过cov函数计算两个字段或数据表中各字段间的协方差。

[![协方差](http://bluewhale.cc/wp-content/uploads/2017/04/%E5%8D%8F%E6%96%B9%E5%B7%AE.png)](http://bluewhale.cc/wp-content/uploads/2017/04/%E5%8D%8F%E6%96%B9%E5%B7%AE.png)

Cov函数用来计算两个字段间的协方差，可以只对特定字段进行计算，也可以对整个数据表中各个列之间进行计算。

|     |     |
| --- | --- |
| 1<br><br>2<br><br>3 | `#两个字段间的协方差`<br><br>`df_inner[``'price'``].cov(df_inner[``'m-point'``])`<br><br>`17263.200000000001` |

|     |     |
| --- | --- |
| 1<br><br>2 | `#数据表中所有字段间的协方差`<br><br>`df_inner.cov()` |

[![cov](http://bluewhale.cc/wp-content/uploads/2017/04/cov.png)](http://bluewhale.cc/wp-content/uploads/2017/04/cov.png)

**相关分析**
Excel的数据分析功能中提供了相关系数的计算功能，python中则通过corr函数完成相关分析的操作，并返回相关系数。

[![相关系数](http://bluewhale.cc/wp-content/uploads/2017/04/%E7%9B%B8%E5%85%B3%E7%B3%BB%E6%95%B0.png)](http://bluewhale.cc/wp-content/uploads/2017/04/%E7%9B%B8%E5%85%B3%E7%B3%BB%E6%95%B0.png)

Corr函数用来计算数据间的相关系数，可以单独对特定数据进行计算，也可以对整个数据表中各个列进行计算。相关系数在-1到1之间，接近1为正相关，接近-1为负相关，0为不相关。

|     |     |
| --- | --- |
| 1<br><br>2<br><br>3 | `#相关性分析`<br><br>`df_inner[``'price'``].corr(df_inner[``'m-point'``])`<br><br>`0.77466555617085264` |

|     |     |
| --- | --- |
| 1<br><br>2 | `#数据表相关性分析`<br><br>`df_inner.corr()` |

[![corr](http://bluewhale.cc/wp-content/uploads/2017/04/corr.png)](http://bluewhale.cc/wp-content/uploads/2017/04/corr.png)

# **9，数据输出**

第九部分是数据输出，处理和分析完的数据可以输出为xlsx格式和csv格式。

**写入excel**

|     |     |
| --- | --- |
| 1<br><br>2 | `#输出到excel格式`<br><br>`df_inner.to_excel(``'excel_to_python.xlsx'``, sheet_name``=``'bluewhale_cc'``)` |

[![excel](http://bluewhale.cc/wp-content/uploads/2017/04/excel-1024x156.png)](http://bluewhale.cc/wp-content/uploads/2017/04/excel.png)
**写入csv**

|     |     |
| --- | --- |
| 1<br><br>2 | `#输出到CSV格式`<br><br>`df_inner.to_csv(``'excel_to_python.csv'``)` |

在数据处理的过程中，大部分基础工作是重复和机械的，对于这部分基础工作，我们可以使用自定义函数进行自动化。以下简单介绍对数据表信息获取自动化处理。

|     |     |
| --- | --- |
| 1<br><br>2<br><br>3<br><br>4<br><br>5<br><br>6<br><br>7<br><br>8 | `#创建数据表`<br><br>`df` `=` `pd.DataFrame({``"id"``:[``1001``,``1002``,``1003``,``1004``,``1005``,``1006``],`<br><br>`"date"``:pd.date_range(``'20130102'``, periods``=``6``),`<br><br>`"city"``:[``'Beijing '``,` `'SH'``,` `' guangzhou '``,` `'Shenzhen'``,` `'shanghai'``,` `'BEIJING '``],`<br><br>`"age"``:[``23``,``44``,``54``,``32``,``34``,``32``],`<br><br>`"category"``:[``'100-A'``,``'100-B'``,``'110-A'``,``'110-C'``,``'210-A'``,``'130-F'``],`<br><br>`"price"``:[``1200``,np.nan,``2133``,``5433``,np.nan,``4432``]},`<br><br>`columns` `=``[``'id'``,``'date'``,``'city'``,``'category'``,``'age'``,``'price'``])` |

|     |     |
| --- | --- |
| 1<br><br>2<br><br>3<br><br>4<br><br>5<br><br>6<br><br>7<br><br>8 | `#创建自定义函数`<br><br>`def` `table_info(x):`<br><br>`shape``=``x.shape`<br><br>`types``=``x.dtypes`<br><br>`colums``=``x.columns`<br><br>`print``(``"数据维度(行，列):\n"``,shape)`<br><br>`print``(``"数据格式:\n"``,types)`<br><br>`print``(``"列名称:\n"``,colums)` |

|     |     |
| --- | --- |
| 1<br><br>2<br><br>3<br><br>4<br><br>5<br><br>6<br><br>7<br><br>8<br><br>9<br><br>10<br><br>11<br><br>12<br><br>13<br><br>14<br><br>15 | `#调用自定义函数获取df数据表信息并输出结果`<br><br>`table_info(df)`<br><br>`数据维度(行，列):`<br><br>`(``6``,` `6``)`<br><br>`数据格式:`<br><br>`id` `int64`<br><br>`date datetime64[ns]`<br><br>`city` `object`<br><br>`category` `object`<br><br>`age int64`<br><br>`price float64`<br><br>`dtype:` `object`<br><br>`列名称:`<br><br>`Index([``'id'``,` `'date'``,` `'city'``,` `'category'``,` `'age'``,` `'price'``], dtype``=``'object'``)` |

转载来源于：

http://bluewhale.cc/2017-04-21/use-python-for-data-analysis-like-excel-1.html

http://bluewhale.cc/2017-04-21/use-python-for-data-analysis-like-excel-2.html

http://bluewhale.cc/2017-04-21/use-python-for-data-analysis-like-excel-3.html

即使只是凡世中一颗小小的尘埃，命运也要由自己主宰，像向日葵般，迎向阳光、勇敢盛开

分类: [R](https://www.cnblogs.com/nxld/category/908520.html)

标签: [r](https://www.cnblogs.com/nxld/tag/r/), [excel](https://www.cnblogs.com/nxld/tag/excel/)

<a id="green_channel_digg"></a>好文要顶 <a id="green_channel_follow"></a>关注我 <a id="green_channel_favorite"></a>收藏该文 <a id="green_channel_weibo"></a>![](../../../_resources/icon_weibo_24_acd7a2ba4fb9469983aab97b1365d2e6.png)<a id="green_channel_wechat"></a><img width="24" height="24" src="../../../_resources/wechat_7ca1016ca29646f6871d81bb0f27936f.png"/>

[![](../../../_resources/20161112155140_ba20bf7d1a6a495ea64d33209f4e3f82.png)](https://home.cnblogs.com/u/nxld/)

[Little_Rookie](https://home.cnblogs.com/u/nxld/)
[关注 \- 1](https://home.cnblogs.com/u/nxld/followees/)
[粉丝 \- 435](https://home.cnblogs.com/u/nxld/followers/)

+加关注

<a id="digg_count"></a>12

<a id="bury_count"></a>0

[«](https://www.cnblogs.com/nxld/p/6687253.html) 上一篇： [python数据分析实用小抄](https://www.cnblogs.com/nxld/p/6687253.html "发布于 2017-04-10 01:55")
[»](https://www.cnblogs.com/nxld/p/7087704.html) 下一篇： [一张色环图教你搞定配色！](https://www.cnblogs.com/nxld/p/7087704.html "发布于 2017-06-27 23:47")

posted @ <a id="post-date"></a>2017-04-24 13:44  [Little_Rookie](https://www.cnblogs.com/nxld/)  阅读(<a id="post_view_count"></a>76358)  评论(<a id="post_comment_count"></a>2)  [编辑](https://i.cnblogs.com/EditPosts.aspx?postid=6756492)  收藏  举报

<a id="!comments"></a>

<a id="commentform"></a>

<a id="lnk_RefreshComments"></a>刷新评论[刷新页面](#)[返回顶部](#top)

登录后才能查看或发表评论，立即 登录 或者 [逛逛](https://www.cnblogs.com/) 博客园首页

[【推荐】百度智能云开发者赋能计划，云服务器4元起，域名1元起](https://cloud.baidu.com/campaign/2022developer/index.html?track=18708feb6fc4c4db36171b5d7d99f1509444b5c535f8dfc7#person)
[【推荐】华为开发者专区，与开发者一起构建万物互联的智能世界](https://brands.cnblogs.com/huawei)

**编辑推荐：**
· [戏说领域驱动设计（十九）——外验](https://www.cnblogs.com/skevin/p/16035785.html)
· [ASP.NET Core 在 IIS 下的两种部署模式](https://www.cnblogs.com/artech/p/inside-asp-net-core-6-32.html)
· [记我第一次做线下技术分享的那些事](https://www.cnblogs.com/skychen1218/p/16068503.html)
· [换个数据结构，一不小心节约了 591 台机器！](https://www.cnblogs.com/thisiswhy/p/16066548.html)
· [\[ASP.NET Core\] MVC 模型绑定：非规范正文内容的处理](https://www.cnblogs.com/tcjiaan/p/16058491.html)

**最新新闻**：
· [知乎的商业化迷途](https://news.cnblogs.com/n/718066/)
· [清华博士提出「Chiplet精算师」登顶会！越接近摩尔极限，多芯片集成越划算](https://news.cnblogs.com/n/718041/)
· [这届年轻人靠陪伴赚钱：有人陪逛、陪考月入过万，有人忙活一天还捐钱](https://news.cnblogs.com/n/718067/)
· [起底小米生态链：8年投资500+，近30家成功上市，多家排队IPO](https://news.cnblogs.com/n/718065/)
· [董小姐的日子不太好过](https://news.cnblogs.com/n/718064/)
» [更多新闻...](https://news.cnblogs.com/ "IT 新闻")

|     |     |     |
| --- | --- | --- |
| <   | 2022年4月 | >   |

日一二三四五六27282930311234567891011121314151617181920212223242526272829301234567

### 我的标签

- [r(50)](https://www.cnblogs.com/nxld/tag/r/)
- [excel(16)](https://www.cnblogs.com/nxld/tag/excel/)
- [mysql(13)](https://www.cnblogs.com/nxld/tag/mysql/)
- [python(13)](https://www.cnblogs.com/nxld/tag/python/)
- [信用知识(12)](https://www.cnblogs.com/nxld/tag/%E4%BF%A1%E7%94%A8%E7%9F%A5%E8%AF%86/)
- [机器学习(8)](https://www.cnblogs.com/nxld/tag/%E6%9C%BA%E5%99%A8%E5%AD%A6%E4%B9%A0/)
- [tableau(3)](https://www.cnblogs.com/nxld/tag/tableau/)
- [pandas(2)](https://www.cnblogs.com/nxld/tag/pandas/)
- [分类(1)](https://www.cnblogs.com/nxld/tag/%E5%88%86%E7%B1%BB/)
- [回归(1)](https://www.cnblogs.com/nxld/tag/%E5%9B%9E%E5%BD%92/)
- [更多](https://www.cnblogs.com/nxld/tag/)

### 积分与排名

- 积分 \- 310907
- 排名 \- 2359

### 随笔档案

- [2018年10月(1)](https://www.cnblogs.com/nxld/archive/2018/10.html)
- [2017年8月(2)](https://www.cnblogs.com/nxld/archive/2017/08.html)
- [2017年6月(1)](https://www.cnblogs.com/nxld/archive/2017/06.html)
- [2017年4月(2)](https://www.cnblogs.com/nxld/archive/2017/04.html)
- [2017年3月(5)](https://www.cnblogs.com/nxld/archive/2017/03.html)
- [2017年2月(28)](https://www.cnblogs.com/nxld/archive/2017/02.html)
- [2017年1月(2)](https://www.cnblogs.com/nxld/archive/2017/01.html)
- [2016年12月(25)](https://www.cnblogs.com/nxld/archive/2016/12.html)
- [2016年11月(48)](https://www.cnblogs.com/nxld/archive/2016/11.html)

### 最新评论

- [1\. Re:ROC曲线-阈值评价标准](https://www.cnblogs.com/nxld/p/6365637.html)
- 请问“AUC小于0.5的把结果取反就行,”意思是把变量值取倒数吗？
    
- --Kukulcan
- [2\. Re:python－－数据清洗](https://www.cnblogs.com/nxld/p/6085605.html)
- 图都挂了。。
    
- --Mr_Guanghui
- [3\. Re:自己动手写Logistic回归算法](https://www.cnblogs.com/nxld/p/6125809.html)
- 太强了
    
- --不奋斗不青春
- [4\. Re:Mysql中的定时任务](https://www.cnblogs.com/nxld/p/6624966.html)
- 最喜欢这种代码+解释的博客了，感谢分享
    
- --哒哒哒哒崽丶
- [5\. Re:如何在R语言中使用Logistic回归模型](https://www.cnblogs.com/nxld/p/6170690.html)
- 求博主有空更新一下博文，好多图片都失效了，博文真的很有用哎！！！看了好多遍。
    
- --V米兰小铁匠

Copyright © 2022 Little_Rookie
<a id="poweredby"></a>Powered by .NET 6 on Kubernetes