# Pandas 重置索引深度总结

<a id="profileBt"></a><a id="js_name"></a>AI科技大本营 *2022-06-01 18:00* *Posted on <a id="js_ip_wording"></a>北京*

The following article is from 萝卜大杂烩 Author 周萝卜

<a id="copyright_info"></a>[![](../../../_resources/0_c31e5f8befcf4d80b13daf5b26277e4a.jpg)<br>**萝卜大杂烩** .<br>Do it by yourself！爱好Python，测试，NLP，数据分析，小程序，k8s等技术，期待与你一同成长](#)

<img width="661" height="117" src="../../../_resources/640_wx_fmt_gif_wxfrom_5_wx_lazy__11fbe2eec78448dc9.gif"/>

作者 | 周萝卜

来源 | 萝卜大杂烩

今天我们来讨论 Pandas 中的 `reset_index()` 方法，包括为什么我们需要在 Pandas 中重置 DataFrame 的索引，以及我们应该如何应用该方法~

在本文我们将使用 Kaggle 上的数据集样本 Animal Shelter Analytics 来作为我们的测试数据

## Pandas 中的 Reset_Index() 是什么？

如果我们使用 Pandas 的 `read_csv()` 方法读取 csv 文件而不指定任何索引，则生成的 DataFrame 将具有默认的基于整数的索引，第一行从 0 开始，随后每行增加 1：

```


import pandas as pd
import numpy as np
df = pd.read_csv('Austin_Animal_Center_Intakes.csv').head()
df



```

Output：

```


 Animal ID	Name	DateTime	MonthYear	Found Location	Intake Type	Intake Condition	Animal Type	Sex upon Intake	Age upon Intake	Breed	Color
0	A786884	*Brock	01/03/2019 04:19:00 PM	01/03/2019 04:19:00 PM	2501 Magin Meadow Dr in Austin (TX)	Stray	Normal	Dog	Neutered Male	2 years	Beagle Mix	Tricolor
1	A706918	Belle	07/05/2015 12:59:00 PM	07/05/2015 12:59:00 PM	9409 Bluegrass Dr in Austin (TX)	Stray	Normal	Dog	Spayed Female	8 years	English Springer Spaniel	White/Liver
2	A724273	Runster	04/14/2016 06:43:00 PM	04/14/2016 06:43:00 PM	2818 Palomino Trail in Austin (TX)	Stray	Normal	Dog	Intact Male	11 months	Basenji Mix	Sable/White
3	A665644	NaN	10/21/2013 07:59:00 AM	10/21/2013 07:59:00 AM	Austin (TX)	Stray	Sick	Cat	Intact Female	4 weeks	Domestic Shorthair Mix	Calico
4	A682524	Rio	06/29/2014 10:38:00 AM	06/29/2014 10:38:00 AM	800 Grove Blvd in Austin (TX)	Stray	Normal	Dog	Neutered Male	4 years	Doberman Pinsch/Australian Cattle Dog	Tan/Gray


```

在某些情况下，我们可能希望拥有更有意义的行标签，因此我们将选择 DataFrame 的其中一列作为 DataFrame 索引。我们可以使用 `read_csv()` 方法的 index_col 参数直接执行此操作：

```


df = pd.read_csv('Austin_Animal_Center_Intakes.csv', index_col='Animal ID').head()
df



```

Output：

```


Animal ID	Name	DateTime	MonthYear	Found Location	Intake Type	Intake Condition	Animal Type	Sex upon Intake	Age upon Intake	Breed	Color
A786884	*Brock	01/03/2019 04:19:00 PM	01/03/2019 04:19:00 PM	2501 Magin Meadow Dr in Austin (TX)	Stray	Normal	Dog	Neutered Male	2 years	Beagle Mix	Tricolor
A706918	Belle	07/05/2015 12:59:00 PM	07/05/2015 12:59:00 PM	9409 Bluegrass Dr in Austin (TX)	Stray	Normal	Dog	Spayed Female	8 years	English Springer Spaniel	White/Liver
A724273	Runster	04/14/2016 06:43:00 PM	04/14/2016 06:43:00 PM	2818 Palomino Trail in Austin (TX)	Stray	Normal	Dog	Intact Male	11 months	Basenji Mix	Sable/White
A665644	NaN	10/21/2013 07:59:00 AM	10/21/2013 07:59:00 AM	Austin (TX)	Stray	Sick	Cat	Intact Female	4 weeks	Domestic Shorthair Mix	Calico
A682524	Rio	06/29/2014 10:38:00 AM	06/29/2014 10:38:00 AM	800 Grove Blvd in Austin (TX)	Stray	Normal	Dog	Neutered Male	4 years	Doberman Pinsch/Australian Cattle Dog	Tan/Gray



```

或者我们还可以使用 set_index() 方法将 DataFrame 的任何列设置为 DataFrame 索引：

```


df = pd.read_csv('Austin_Animal_Center_Intakes.csv').head()
df.set_index('Animal ID', inplace=True)
df



```

Output：

```


Animal ID	Name	DateTime	MonthYear	Found Location	Intake Type	Intake Condition	Animal Type	Sex upon Intake	Age upon Intake	Breed	Color
A786884	*Brock	01/03/2019 04:19:00 PM	01/03/2019 04:19:00 PM	2501 Magin Meadow Dr in Austin (TX)	Stray	Normal	Dog	Neutered Male	2 years	Beagle Mix	Tricolor
A706918	Belle	07/05/2015 12:59:00 PM	07/05/2015 12:59:00 PM	9409 Bluegrass Dr in Austin (TX)	Stray	Normal	Dog	Spayed Female	8 years	English Springer Spaniel	White/Liver
A724273	Runster	04/14/2016 06:43:00 PM	04/14/2016 06:43:00 PM	2818 Palomino Trail in Austin (TX)	Stray	Normal	Dog	Intact Male	11 months	Basenji Mix	Sable/White
A665644	NaN	10/21/2013 07:59:00 AM	10/21/2013 07:59:00 AM	Austin (TX)	Stray	Sick	Cat	Intact Female	4 weeks	Domestic Shorthair Mix	Calico
A682524	Rio	06/29/2014 10:38:00 AM	06/29/2014 10:38:00 AM	800 Grove Blvd in Austin (TX)	Stray	Normal	Dog	Neutered Male	4 years	Doberman Pinsch/Australian Cattle Dog	Tan/Gray



```

如果在某个时候我们需要恢复默认的数字索引呢，这时就可以使用 reset_index()函数了

```


df.reset_index()



```

Output：

```


 Animal ID	Name	DateTime	MonthYear	Found Location	Intake Type	Intake Condition	Animal Type	Sex upon Intake	Age upon Intake	Breed	Color
0	A786884	*Brock	01/03/2019 04:19:00 PM	01/03/2019 04:19:00 PM	2501 Magin Meadow Dr in Austin (TX)	Stray	Normal	Dog	Neutered Male	2 years	Beagle Mix	Tricolor
1	A706918	Belle	07/05/2015 12:59:00 PM	07/05/2015 12:59:00 PM	9409 Bluegrass Dr in Austin (TX)	Stray	Normal	Dog	Spayed Female	8 years	English Springer Spaniel	White/Liver
2	A724273	Runster	04/14/2016 06:43:00 PM	04/14/2016 06:43:00 PM	2818 Palomino Trail in Austin (TX)	Stray	Normal	Dog	Intact Male	11 months	Basenji Mix	Sable/White
3	A665644	NaN	10/21/2013 07:59:00 AM	10/21/2013 07:59:00 AM	Austin (TX)	Stray	Sick	Cat	Intact Female	4 weeks	Domestic Shorthair Mix	Calico
4	A682524	Rio	06/29/2014 10:38:00 AM	06/29/2014 10:38:00 AM	800 Grove Blvd in Austin (TX)	Stray	Normal	Dog	Neutered Male	4 years	Doberman Pinsch/Australian Cattle Dog	Tan/Gray


```

此方法的默认行为包括用默认的基于整数的索引替换现有的 DataFrame 索引，并将旧索引转换为与旧索引同名的新列（或名称索引）。此外，默认情况下，reset_index() 方法会从 MultiIndex 中删除所有级别并且不会影响原始 DataFrame 数据，而是创建一个新的

## 何时使用 Reset_Index() 方法

reset_index() 方法将 DataFrame 索引重置为默认数字索引，在以下情况下特别有用：

- 执行数据整理时——尤其是过滤数据或删除缺失值等预处理操作，会导致较小的 DataFrame 具有不再连续的数字索引
    
- 当索引应该被视为一个常见的 DataFrame 列时
    
- 当索引标签没有提供有关数据的任何有价值的信息时
    

## 如何调整 Reset_Index() 方法

前面的讨论中，我们看到了当我们不向它传递任何参数时，reset_index() 方法是如何工作的，当然如果有需要，我们可以通过调整方法的各种参数来更改此默认行为。让我们看看最有用的三种参数：level、drop 和 inplace

### level

此参数采用整数、字符串、元组或列表作为可能的数据类型，并且仅适用于具有 MultiIndex 的 DataFrame，如下所示：

```


df_multiindex = pd.read_csv('Austin_Animal_Center_Intakes.csv', index_col=['Animal ID', 'Name']).head()
df_multiindex



```

Output：

```


Animal ID	Name	DateTime	MonthYear	Found Location	Intake Type	Intake Condition	Animal Type	Sex upon Intake	Age upon Intake	Breed	Color
A786884	*Brock	01/03/2019 04:19:00 PM	01/03/2019 04:19:00 PM	2501 Magin Meadow Dr in Austin (TX)	Stray	Normal	Dog	Neutered Male	2 years	Beagle Mix	Tricolor
A706918	Belle	07/05/2015 12:59:00 PM	07/05/2015 12:59:00 PM	9409 Bluegrass Dr in Austin (TX)	Stray	Normal	Dog	Spayed Female	8 years	English Springer Spaniel	White/Liver
A724273	Runster	04/14/2016 06:43:00 PM	04/14/2016 06:43:00 PM	2818 Palomino Trail in Austin (TX)	Stray	Normal	Dog	Intact Male	11 months	Basenji Mix	Sable/White
A665644	NaN	10/21/2013 07:59:00 AM	10/21/2013 07:59:00 AM	Austin (TX)	Stray	Sick	Cat	Intact Female	4 weeks	Domestic Shorthair Mix	Calico
A682524	Rio	06/29/2014 10:38:00 AM	06/29/2014 10:38:00 AM	800 Grove Blvd in Austin (TX)	Stray	Normal	Dog	Neutered Male	4 years	Doberman Pinsch/Australian Cattle Dog	Tan/Gray



```

事实上，如果现在我们检查上面 DataFrame 的索引，我们会发现它不是一个常见的 DataFrame 索引，而是一个 MultiIndex 对象：

```


df_multiindex.index



```

Output:

```


MultiIndex([('A786884',  '*Brock'),
            ('A706918',   'Belle'),
            ('A724273', 'Runster'),
            ('A665644',       nan),
            ('A682524',     'Rio')],
           names=['Animal ID', 'Name'])



```

默认情况下，reset_index() 方法参数 level (level=None) 会移除 MultiIndex 的所有级别：

```


df_multiindex.reset_index()



```

Output:

```


 Animal ID	Name	DateTime	MonthYear	Found Location	Intake Type	Intake Condition	Animal Type	Sex upon Intake	Age upon Intake	Breed	Color
0	A786884	*Brock	01/03/2019 04:19:00 PM	01/03/2019 04:19:00 PM	2501 Magin Meadow Dr in Austin (TX)	Stray	Normal	Dog	Neutered Male	2 years	Beagle Mix	Tricolor
1	A706918	Belle	07/05/2015 12:59:00 PM	07/05/2015 12:59:00 PM	9409 Bluegrass Dr in Austin (TX)	Stray	Normal	Dog	Spayed Female	8 years	English Springer Spaniel	White/Liver
2	A724273	Runster	04/14/2016 06:43:00 PM	04/14/2016 06:43:00 PM	2818 Palomino Trail in Austin (TX)	Stray	Normal	Dog	Intact Male	11 months	Basenji Mix	Sable/White
3	A665644	NaN	10/21/2013 07:59:00 AM	10/21/2013 07:59:00 AM	Austin (TX)	Stray	Sick	Cat	Intact Female	4 weeks	Domestic Shorthair Mix	Calico
4	A682524	Rio	06/29/2014 10:38:00 AM	06/29/2014 10:38:00 AM	800 Grove Blvd in Austin (TX)	Stray	Normal	Dog	Neutered Male	4 years	Doberman Pinsch/Australian Cattle Dog	Tan/Gray


```

我们看到 DataFrame 的两个索引都被转换为通用 DataFrame 列，而索引被重置为默认的基于整数的索引

相反，如果我们显式传递 level 的值，则此参数会从 DataFrame 索引中删除选定的级别，并将它们作为常见的 DataFrame 列返回（除非我们选择使用 drop 参数从 DataFrame 中完全删除此信息）。比较以下操作：

```


df_multiindex.reset_index(level='Animal ID')



```

Output:

```


Name	Animal ID	DateTime	MonthYear	Found Location	Intake Type	Intake Condition	Animal Type	Sex upon Intake	Age upon Intake	Breed	Color
*Brock	A786884	01/03/2019 04:19:00 PM	01/03/2019 04:19:00 PM	2501 Magin Meadow Dr in Austin (TX)	Stray	Normal	Dog	Neutered Male	2 years	Beagle Mix	Tricolor
Belle	A706918	07/05/2015 12:59:00 PM	07/05/2015 12:59:00 PM	9409 Bluegrass Dr in Austin (TX)	Stray	Normal	Dog	Spayed Female	8 years	English Springer Spaniel	White/Liver
Runster	A724273	04/14/2016 06:43:00 PM	04/14/2016 06:43:00 PM	2818 Palomino Trail in Austin (TX)	Stray	Normal	Dog	Intact Male	11 months	Basenji Mix	Sable/White
NaN	A665644	10/21/2013 07:59:00 AM	10/21/2013 07:59:00 AM	Austin (TX)	Stray	Sick	Cat	Intact Female	4 weeks	Domestic Shorthair Mix	Calico
Rio	A682524	06/29/2014 10:38:00 AM	06/29/2014 10:38:00 AM	800 Grove Blvd in Austin (TX)	Stray	Normal	Dog	Neutered Male	4 years	Doberman Pinsch/Australian Cattle Dog	Tan/Gray



```

最开始 Animal ID 是 DataFrame 的索引之一，当我们设置 level 参数后，将其从索引中删除并作为称为 Animal ID 的公共列插入到 DataFrame 中

```


df_multiindex.reset_index(level='Name')



```

Output:

```


Animal ID	Name	DateTime	MonthYear	Found Location	Intake Type	Intake Condition	Animal Type	Sex upon Intake	Age upon Intake	Breed	Color
A786884	*Brock	01/03/2019 04:19:00 PM	01/03/2019 04:19:00 PM	2501 Magin Meadow Dr in Austin (TX)	Stray	Normal	Dog	Neutered Male	2 years	Beagle Mix	Tricolor
A706918	Belle	07/05/2015 12:59:00 PM	07/05/2015 12:59:00 PM	9409 Bluegrass Dr in Austin (TX)	Stray	Normal	Dog	Spayed Female	8 years	English Springer Spaniel	White/Liver
A724273	Runster	04/14/2016 06:43:00 PM	04/14/2016 06:43:00 PM	2818 Palomino Trail in Austin (TX)	Stray	Normal	Dog	Intact Male	11 months	Basenji Mix	Sable/White
A665644	NaN	10/21/2013 07:59:00 AM	10/21/2013 07:59:00 AM	Austin (TX)	Stray	Sick	Cat	Intact Female	4 weeks	Domestic Shorthair Mix	Calico
A682524	Rio	06/29/2014 10:38:00 AM	06/29/2014 10:38:00 AM	800 Grove Blvd in Austin (TX)	Stray	Normal	Dog	Neutered Male	4 years	Doberman Pinsch/Australian Cattle Dog	Tan/Gray



```

在这里，Name 最初是 DataFrame 的索引之一，设置完level参数后，就变成了一个常用的列，叫做Name

### drop

此参数决定在索引重置后是否将旧索引保留为通用 DataFrame 列，或者将其从 DataFrame 中完全删除。默认情况下 (drop=False) 是进行保留的，正如我们在前面的所有示例中看到的那样。否则，如果我们不想将旧索引保留为列，我们可以在索引重置后将其从 DataFrame 中完全删除（drop=True）：

```


df



```

Output:

```


Animal ID	Name	DateTime	MonthYear	Found Location	Intake Type	Intake Condition	Animal Type	Sex upon Intake	Age upon Intake	Breed	Color
A786884	*Brock	01/03/2019 04:19:00 PM	01/03/2019 04:19:00 PM	2501 Magin Meadow Dr in Austin (TX)	Stray	Normal	Dog	Neutered Male	2 years	Beagle Mix	Tricolor
A706918	Belle	07/05/2015 12:59:00 PM	07/05/2015 12:59:00 PM	9409 Bluegrass Dr in Austin (TX)	Stray	Normal	Dog	Spayed Female	8 years	English Springer Spaniel	White/Liver
A724273	Runster	04/14/2016 06:43:00 PM	04/14/2016 06:43:00 PM	2818 Palomino Trail in Austin (TX)	Stray	Normal	Dog	Intact Male	11 months	Basenji Mix	Sable/White
A665644	NaN	10/21/2013 07:59:00 AM	10/21/2013 07:59:00 AM	Austin (TX)	Stray	Sick	Cat	Intact Female	4 weeks	Domestic Shorthair Mix	Calico
A682524	Rio	06/29/2014 10:38:00 AM	06/29/2014 10:38:00 AM	800 Grove Blvd in Austin (TX)	Stray	Normal	Dog	Neutered Male	4 years	Doberman Pinsch/Australian Cattle Dog	Tan/Gray



```

增加 drop 参数

```


df.reset_index(drop=True)



```

Output:

```


 Name	DateTime	MonthYear	Found Location	Intake Type	Intake Condition	Animal Type	Sex upon Intake	Age upon Intake	Breed	Color
0	*Brock	01/03/2019 04:19:00 PM	01/03/2019 04:19:00 PM	2501 Magin Meadow Dr in Austin (TX)	Stray	Normal	Dog	Neutered Male	2 years	Beagle Mix	Tricolor
1	Belle	07/05/2015 12:59:00 PM	07/05/2015 12:59:00 PM	9409 Bluegrass Dr in Austin (TX)	Stray	Normal	Dog	Spayed Female	8 years	English Springer Spaniel	White/Liver
2	Runster	04/14/2016 06:43:00 PM	04/14/2016 06:43:00 PM	2818 Palomino Trail in Austin (TX)	Stray	Normal	Dog	Intact Male	11 months	Basenji Mix	Sable/White
3	NaN	10/21/2013 07:59:00 AM	10/21/2013 07:59:00 AM	Austin (TX)	Stray	Sick	Cat	Intact Female	4 weeks	Domestic Shorthair Mix	Calico
4	Rio	06/29/2014 10:38:00 AM	06/29/2014 10:38:00 AM	800 Grove Blvd in Austin (TX)	Stray	Normal	Dog	Neutered Male	4 years	Doberman Pinsch/Australian Cattle Dog	Tan/Gray


```

在上面的 DataFrame 中，旧索引中包含的信息已完全从 DataFrame 中删除了

drop 参数也适用于具有 MultiIndex 的 DataFrame，就像我们之前创建的那样：

```


df_multiindex



```

Output:

```


Animal ID	Name	DateTime	MonthYear	Found Location	Intake Type	Intake Condition	Animal Type	Sex upon Intake	Age upon Intake	Breed	Color
A786884	*Brock	01/03/2019 04:19:00 PM	01/03/2019 04:19:00 PM	2501 Magin Meadow Dr in Austin (TX)	Stray	Normal	Dog	Neutered Male	2 years	Beagle Mix	Tricolor
A706918	Belle	07/05/2015 12:59:00 PM	07/05/2015 12:59:00 PM	9409 Bluegrass Dr in Austin (TX)	Stray	Normal	Dog	Spayed Female	8 years	English Springer Spaniel	White/Liver
A724273	Runster	04/14/2016 06:43:00 PM	04/14/2016 06:43:00 PM	2818 Palomino Trail in Austin (TX)	Stray	Normal	Dog	Intact Male	11 months	Basenji Mix	Sable/White
A665644	NaN	10/21/2013 07:59:00 AM	10/21/2013 07:59:00 AM	Austin (TX)	Stray	Sick	Cat	Intact Female	4 weeks	Domestic Shorthair Mix	Calico
A682524	Rio	06/29/2014 10:38:00 AM	06/29/2014 10:38:00 AM	800 Grove Blvd in Austin (TX)	Stray	Normal	Dog	Neutered Male	4 years	Doberman Pinsch/Australian Cattle Dog	Tan/Gray



```

增加 drop 参数

```


df_multiindex.reset_index(drop=True)



```

Output:

```


 DateTime	MonthYear	Found Location	Intake Type	Intake Condition	Animal Type	Sex upon Intake	Age upon Intake	Breed	Color
0	01/03/2019 04:19:00 PM	01/03/2019 04:19:00 PM	2501 Magin Meadow Dr in Austin (TX)	Stray	Normal	Dog	Neutered Male	2 years	Beagle Mix	Tricolor
1	07/05/2015 12:59:00 PM	07/05/2015 12:59:00 PM	9409 Bluegrass Dr in Austin (TX)	Stray	Normal	Dog	Spayed Female	8 years	English Springer Spaniel	White/Liver
2	04/14/2016 06:43:00 PM	04/14/2016 06:43:00 PM	2818 Palomino Trail in Austin (TX)	Stray	Normal	Dog	Intact Male	11 months	Basenji Mix	Sable/White
3	10/21/2013 07:59:00 AM	10/21/2013 07:59:00 AM	Austin (TX)	Stray	Sick	Cat	Intact Female	4 weeks	Domestic Shorthair Mix	Calico
4	06/29/2014 10:38:00 AM	06/29/2014 10:38:00 AM	800 Grove Blvd in Austin (TX)	Stray	Normal	Dog	Neutered Male	4 years	Doberman Pinsch/Australian Cattle Dog	Tan/Gray


```

两个旧索引都已从 Dataframe 中完全删除，并且索引已重置为默认值

当然，我们可以结合 drop 和 level 参数，指定要从 DataFrame 中完全删除哪些旧索引：

```


df_multiindex.reset_index(level='Animal ID', drop=True)



```

Output:

```


 DateTime	MonthYear	Found Location	Intake Type	Intake Condition	Animal Type	Sex upon Intake	Age upon Intake	Breed	Color
Name										
*Brock	01/03/2019 04:19:00 PM	01/03/2019 04:19:00 PM	2501 Magin Meadow Dr in Austin (TX)	Stray	Normal	Dog	Neutered Male	2 years	Beagle Mix	Tricolor
Belle	07/05/2015 12:59:00 PM	07/05/2015 12:59:00 PM	9409 Bluegrass Dr in Austin (TX)	Stray	Normal	Dog	Spayed Female	8 years	English Springer Spaniel	White/Liver
Runster	04/14/2016 06:43:00 PM	04/14/2016 06:43:00 PM	2818 Palomino Trail in Austin (TX)	Stray	Normal	Dog	Intact Male	11 months	Basenji Mix	Sable/White
NaN	10/21/2013 07:59:00 AM	10/21/2013 07:59:00 AM	Austin (TX)	Stray	Sick	Cat	Intact Female	4 weeks	Domestic Shorthair Mix	Calico
Rio	06/29/2014 10:38:00 AM	06/29/2014 10:38:00 AM	800 Grove Blvd in Austin (TX)	Stray	Normal	Dog	Neutered Male	4 years	Doberman Pinsch/Australian Cattle Dog	Tan/Gray


```

旧索引 Animal ID 已从索引和 DataFrame 本身中删除，另一个索引 Name 被保留为 DataFrame 的当前索引

### inplace

该参数决定是直接修改原来的 DataFrame 还是新建一个 DataFrame 对象。默认情况下，它会使用新索引 (inplace=False) 创建一个新的 DataFrame，并保持原始 DataFrame 不变。让我们使用默认参数再次运行 reset_index() 方法，然后将结果与原始 DataFrame 进行比较：

```


df.reset_index()



```

Output:

```


 Animal ID	Name	DateTime	MonthYear	Found Location	Intake Type	Intake Condition	Animal Type	Sex upon Intake	Age upon Intake	Breed	Color
0	A786884	*Brock	01/03/2019 04:19:00 PM	01/03/2019 04:19:00 PM	2501 Magin Meadow Dr in Austin (TX)	Stray	Normal	Dog	Neutered Male	2 years	Beagle Mix	Tricolor
1	A706918	Belle	07/05/2015 12:59:00 PM	07/05/2015 12:59:00 PM	9409 Bluegrass Dr in Austin (TX)	Stray	Normal	Dog	Spayed Female	8 years	English Springer Spaniel	White/Liver
2	A724273	Runster	04/14/2016 06:43:00 PM	04/14/2016 06:43:00 PM	2818 Palomino Trail in Austin (TX)	Stray	Normal	Dog	Intact Male	11 months	Basenji Mix	Sable/White
3	A665644	NaN	10/21/2013 07:59:00 AM	10/21/2013 07:59:00 AM	Austin (TX)	Stray	Sick	Cat	Intact Female	4 weeks	Domestic Shorthair Mix	Calico
4	A682524	Rio	06/29/2014 10:38:00 AM	06/29/2014 10:38:00 AM	800 Grove Blvd in Austin (TX)	Stray	Normal	Dog	Neutered Male	4 years	Doberman Pinsch/Australian Cattle Dog	Tan/Gray


```

```


df



```

Output:

```


Animal ID	Name	DateTime	MonthYear	Found Location	Intake Type	Intake Condition	Animal Type	Sex upon Intake	Age upon Intake	Breed	Color
A786884	*Brock	01/03/2019 04:19:00 PM	01/03/2019 04:19:00 PM	2501 Magin Meadow Dr in Austin (TX)	Stray	Normal	Dog	Neutered Male	2 years	Beagle Mix	Tricolor
A706918	Belle	07/05/2015 12:59:00 PM	07/05/2015 12:59:00 PM	9409 Bluegrass Dr in Austin (TX)	Stray	Normal	Dog	Spayed Female	8 years	English Springer Spaniel	White/Liver
A724273	Runster	04/14/2016 06:43:00 PM	04/14/2016 06:43:00 PM	2818 Palomino Trail in Austin (TX)	Stray	Normal	Dog	Intact Male	11 months	Basenji Mix	Sable/White
A665644	NaN	10/21/2013 07:59:00 AM	10/21/2013 07:59:00 AM	Austin (TX)	Stray	Sick	Cat	Intact Female	4 weeks	Domestic Shorthair Mix	Calico
A682524	Rio	06/29/2014 10:38:00 AM	06/29/2014 10:38:00 AM	800 Grove Blvd in Austin (TX)	Stray	Normal	Dog	Neutered Male	4 years	Doberman Pinsch/Australian Cattle Dog	Tan/Gray



```

即使我们将索引重置为运行第一段代码的默认数字，原始 DataFrame 仍然保持不变。如果我们需要将原始 DataFrame 重新分配给对其应用 reset\_index() 方法的结果，我们可以直接重新分配它（df = df.reset\_index()）或将参数 inplace=True 传递给该方法：

```


df.reset_index(inplace=True)
df



```

Output:

```


 Animal ID	Name	DateTime	MonthYear	Found Location	Intake Type	Intake Condition	Animal Type	Sex upon Intake	Age upon Intake	Breed	Color
0	A786884	*Brock	01/03/2019 04:19:00 PM	01/03/2019 04:19:00 PM	2501 Magin Meadow Dr in Austin (TX)	Stray	Normal	Dog	Neutered Male	2 years	Beagle Mix	Tricolor
1	A706918	Belle	07/05/2015 12:59:00 PM	07/05/2015 12:59:00 PM	9409 Bluegrass Dr in Austin (TX)	Stray	Normal	Dog	Spayed Female	8 years	English Springer Spaniel	White/Liver
2	A724273	Runster	04/14/2016 06:43:00 PM	04/14/2016 06:43:00 PM	2818 Palomino Trail in Austin (TX)	Stray	Normal	Dog	Intact Male	11 months	Basenji Mix	Sable/White
3	A665644	NaN	10/21/2013 07:59:00 AM	10/21/2013 07:59:00 AM	Austin (TX)	Stray	Sick	Cat	Intact Female	4 weeks	Domestic Shorthair Mix	Calico
4	A682524	Rio	06/29/2014 10:38:00 AM	06/29/2014 10:38:00 AM	800 Grove Blvd in Austin (TX)	Stray	Normal	Dog	Neutered Male	4 years	Doberman Pinsch/Australian Cattle Dog	Tan/Gray


```

我们看到现在更改已直接应用于原始 DataFrame 之上了

## 应用实例：删除缺失值后重置索引

让我们将到目前为止讨论的所有内容付诸实践，看看当我们从 DataFrame 中删除缺失值时，重置 DataFrame 索引是如何有用的

首先，让我们恢复我们最开始时创建的第一个 DataFrame，它具有默认数字索引：

```


df = pd.read_csv('Austin_Animal_Center_Intakes.csv').head()
df



```

Output:

```


 Animal ID	Name	DateTime	MonthYear	Found Location	Intake Type	Intake Condition	Animal Type	Sex upon Intake	Age upon Intake	Breed	Color
0	A786884	*Brock	01/03/2019 04:19:00 PM	01/03/2019 04:19:00 PM	2501 Magin Meadow Dr in Austin (TX)	Stray	Normal	Dog	Neutered Male	2 years	Beagle Mix	Tricolor
1	A706918	Belle	07/05/2015 12:59:00 PM	07/05/2015 12:59:00 PM	9409 Bluegrass Dr in Austin (TX)	Stray	Normal	Dog	Spayed Female	8 years	English Springer Spaniel	White/Liver
2	A724273	Runster	04/14/2016 06:43:00 PM	04/14/2016 06:43:00 PM	2818 Palomino Trail in Austin (TX)	Stray	Normal	Dog	Intact Male	11 months	Basenji Mix	Sable/White
3	A665644	NaN	10/21/2013 07:59:00 AM	10/21/2013 07:59:00 AM	Austin (TX)	Stray	Sick	Cat	Intact Female	4 weeks	Domestic Shorthair Mix	Calico
4	A682524	Rio	06/29/2014 10:38:00 AM	06/29/2014 10:38:00 AM	800 Grove Blvd in Austin (TX)	Stray	Normal	Dog	Neutered Male	4 years	Doberman Pinsch/Australian Cattle Dog	Tan/Gray


```

我们看到 DataFrame 中有一个缺失值，让我们使用 dropna() 方法删除具有缺失值的整行

```


df.dropna(inplace=True)
df



```

Output:

```


 Animal ID	Name	DateTime	MonthYear	Found Location	Intake Type	Intake Condition	Animal Type	Sex upon Intake	Age upon Intake	Breed	Color
0	A786884	*Brock	01/03/2019 04:19:00 PM	01/03/2019 04:19:00 PM	2501 Magin Meadow Dr in Austin (TX)	Stray	Normal	Dog	Neutered Male	2 years	Beagle Mix	Tricolor
1	A706918	Belle	07/05/2015 12:59:00 PM	07/05/2015 12:59:00 PM	9409 Bluegrass Dr in Austin (TX)	Stray	Normal	Dog	Spayed Female	8 years	English Springer Spaniel	White/Liver
2	A724273	Runster	04/14/2016 06:43:00 PM	04/14/2016 06:43:00 PM	2818 Palomino Trail in Austin (TX)	Stray	Normal	Dog	Intact Male	11 months	Basenji Mix	Sable/White
4	A682524	Rio	06/29/2014 10:38:00 AM	06/29/2014 10:38:00 AM	800 Grove Blvd in Austin (TX)	Stray	Normal	Dog	Neutered Male	4 years	Doberman Pinsch/Australian Cattle Dog	Tan/Gray


```

该行已从 DataFrame 中删除，但是索引不再是连续的：0、1、2、4。让我们重新设置它：

```


df.reset_index()



```

Output:

```


 index	Animal ID	Name	DateTime	MonthYear	Found Location	Intake Type	Intake Condition	Animal Type	Sex upon Intake	Age upon Intake	Breed	Color
0	0	A786884	*Brock	01/03/2019 04:19:00 PM	01/03/2019 04:19:00 PM	2501 Magin Meadow Dr in Austin (TX)	Stray	Normal	Dog	Neutered Male	2 years	Beagle Mix	Tricolor
1	1	A706918	Belle	07/05/2015 12:59:00 PM	07/05/2015 12:59:00 PM	9409 Bluegrass Dr in Austin (TX)	Stray	Normal	Dog	Spayed Female	8 years	English Springer Spaniel	White/Liver
2	2	A724273	Runster	04/14/2016 06:43:00 PM	04/14/2016 06:43:00 PM	2818 Palomino Trail in Austin (TX)	Stray	Normal	Dog	Intact Male	11 months	Basenji Mix	Sable/White
3	4	A682524	Rio	06/29/2014 10:38:00 AM	06/29/2014 10:38:00 AM	800 Grove Blvd in Austin (TX)	Stray	Normal	Dog	Neutered Male	4 years	Doberman Pinsch/Australian Cattle Dog	Tan/Gray


```

现在索引是连续的了，但是由于我们没有显式传递 drop 参数，旧索引被转换为列，具有默认名称 index，下面让我们从 DataFrame 中完全删除旧索引：

```


df.reset_index(drop=True)



```

Output:

```


 Animal ID	Name	DateTime	MonthYear	Found Location	Intake Type	Intake Condition	Animal Type	Sex upon Intake	Age upon Intake	Breed	Color
0	A786884	*Brock	01/03/2019 04:19:00 PM	01/03/2019 04:19:00 PM	2501 Magin Meadow Dr in Austin (TX)	Stray	Normal	Dog	Neutered Male	2 years	Beagle Mix	Tricolor
1	A706918	Belle	07/05/2015 12:59:00 PM	07/05/2015 12:59:00 PM	9409 Bluegrass Dr in Austin (TX)	Stray	Normal	Dog	Spayed Female	8 years	English Springer Spaniel	White/Liver
2	A724273	Runster	04/14/2016 06:43:00 PM	04/14/2016 06:43:00 PM	2818 Palomino Trail in Austin (TX)	Stray	Normal	Dog	Intact Male	11 months	Basenji Mix	Sable/White
3	A682524	Rio	06/29/2014 10:38:00 AM	06/29/2014 10:38:00 AM	800 Grove Blvd in Austin (TX)	Stray	Normal	Dog	Neutered Male	4 years	Doberman Pinsch/Australian Cattle Dog	Tan/Gray


```

现在我们彻底摆脱了无意义的旧索引，当前索引是连续的。最后一步是使用 inplace 参数将这些修改保存到我们的原始 DataFrame 中：

```


df.reset_index(drop=True, inplace=True)
df



```

Output:

```


 Animal ID	Name	DateTime	MonthYear	Found Location	Intake Type	Intake Condition	Animal Type	Sex upon Intake	Age upon Intake	Breed	Color
0	A786884	*Brock	01/03/2019 04:19:00 PM	01/03/2019 04:19:00 PM	2501 Magin Meadow Dr in Austin (TX)	Stray	Normal	Dog	Neutered Male	2 years	Beagle Mix	Tricolor
1	A706918	Belle	07/05/2015 12:59:00 PM	07/05/2015 12:59:00 PM	9409 Bluegrass Dr in Austin (TX)	Stray	Normal	Dog	Spayed Female	8 years	English Springer Spaniel	White/Liver
2	A724273	Runster	04/14/2016 06:43:00 PM	04/14/2016 06:43:00 PM	2818 Palomino Trail in Austin (TX)	Stray	Normal	Dog	Intact Male	11 months	Basenji Mix	Sable/White
3	A682524	Rio	06/29/2014 10:38:00 AM	06/29/2014 10:38:00 AM	800 Grove Blvd in Austin (TX)	Stray	Normal	Dog	Neutered Male	4 years	Doberman Pinsch/Australian Cattle Dog	Tan/Gray


```

## 总结

今天我们从多个方面讨论了 reset_index() 方法

- reset_index() 方法的默认行为
    
- 如何恢复 DataFrame 的默认数字索引
    
- 何时使用 reset_index() 方法
    
- 该方法最重要的几个参数
    
- 如何使用 MultiIndex
    
- 如何从 DataFrame 中完全删除旧索引
    
- 如何将修改直接保存到原始 DataFrame 中
    

最后我们又完整的完成了一个在删除缺失值后重置 DataFrame 索引的实战案例~

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

**往期回顾**

[上云避坑指南100篇|云淘金时代，安全为王！](http://mp.weixin.qq.com/s?__biz=Mzg4NDQwNTI0OQ==&mid=2247560628&idx=2&sn=c0f0a4d7203bfcff0ddd7772da01962d&chksm=cfbb08daf8cc81cc9a58bc953e9b365e9d7d9b1b05c12c224b5e41016207a4fbe8c2e737813f&scene=21#wechat_redirect)

[用Python+Excel制作一个视频下载器~](http://mp.weixin.qq.com/s?__biz=Mzg4NDQwNTI0OQ==&mid=2247560560&idx=2&sn=8a0bd49afbabd75442d06641ea602bd5&chksm=cfbb081ef8cc8108edeb66adc804d553431c5f293047d231bd22ee47669e378e442ca4e330e5&scene=21#wechat_redirect)

[Pandas实用技能，将列排序的几种方法](http://mp.weixin.qq.com/s?__biz=Mzg4NDQwNTI0OQ==&mid=2247560628&idx=3&sn=6ca3cb2ac543d07795346be51d189d9b&chksm=cfbb08daf8cc81cc757e50a9655d21257f52fbd55d7185f9c4e0ebb2e083007380383dde8f9e&scene=21#wechat_redirect)

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

技术分析 | 浅析MySQL与ElasticSearch的组合使用

...

GreatSQL社区

不看的原因

- 内容质量低
- 不看此公众号

pandas 分类数据处理大全

...

AI科技大本营

不看的原因

- 内容质量低
- 不看此公众号

感谢ggblanket开发者，使得ggplot2绘图变得更简单

...

R语言与SPSS学习笔记

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___dd51602582ba43359.bmp"/>

Scan to Follow