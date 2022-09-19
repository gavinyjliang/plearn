# Pandas中的这3个函数，没想到竟成了我数据处理的主力

<a id="profileBt"></a><a id="js_name"></a>python数据分析之禅 *2022-05-10 12:38* *Posted on <a id="js_ip_wording"></a>北京*

The following article is from 小数志 Author luanhz

<a id="copyright_info"></a>[![](../../../_resources/0_6893480f7f8c4dc08cc067ac003badef.jpg)<br>**小数志** .<br>一个专注于数据科学的公众号！](#)

导读

学Pandas有一年多了，用Pandas做数据分析也快一年了，常常在总结梳理一些Pandas中好用的方法。例如[三个最爱函数](http://mp.weixin.qq.com/s?__biz=MzU0OTk5MDg3OQ==&mid=2247485034&idx=1&sn=cb00ac554728455b051556daecc9069a&chksm=fba63edaccd1b7cc1f4db60670bb33267bc0d4ab5638e74bf2856be9444b2b3f98c8afdb011a&scene=21#wechat_redirect)、[计数](http://mp.weixin.qq.com/s?__biz=MzU0OTk5MDg3OQ==&mid=2247484466&idx=1&sn=ac0e55949e2cc10267f6cf406d485fac&chksm=fba63c82ccd1b59456a0921d7c1c4a6afd1b2bd1870dbb1c1727edc288233b97fcdf0dca460d&scene=21#wechat_redirect)、[数据透视表](http://mp.weixin.qq.com/s?__biz=MzU0OTk5MDg3OQ==&mid=2247485261&idx=1&sn=0581d236e7ba83bf417af1988e8df2da&chksm=fba63ffdccd1b6eb0862786b77f4372957cde7e400baab9545c89a6c1dc53ff62028ce3e4552&scene=21#wechat_redirect)、[索引变换](http://mp.weixin.qq.com/s?__biz=MzU0OTk5MDg3OQ==&mid=2247485065&idx=1&sn=312b975a8e86d78019725d38f942f5c2&chksm=fba63e39ccd1b72fc7a434191c90cf065d48900d930012db519b6a25c68fd97561b973142931&scene=21#wechat_redirect)、[聚合统计](http://mp.weixin.qq.com/s?__biz=MzU0OTk5MDg3OQ==&mid=2247484703&idx=1&sn=72a7513ed98b0ca679635f7780af07fc&chksm=fba63dafccd1b4b92a11c9637780c91a3dec32cf601d1812c306d886d4fc31f0c337c689c1f1&scene=21#wechat_redirect)以及时间序列等等，每一个都称得上是**认知的升华、实践的结晶**。今天，延承这一系列，再分享三个函数，堪称是个人日常在数据处理环节中应用频率较高的3个函数：apply、map和applymap，其中apply是主角，map和applymap为赠送。

<img width="677" height="308" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__856a4ba68ee048bc8.png"/>

数据处理环节无非就是[各种数据清洗](http://mp.weixin.qq.com/s?__biz=MzU0OTk5MDg3OQ==&mid=2247485206&idx=1&sn=7872a661d8b0643ed2995d209ad0c210&chksm=fba63fa6ccd1b6b0a638247070bd3ff44617a4820f1a26ec9ffe4fdd474372c2c33b20115874&scene=21#wechat_redirect)，除了常规的缺失值和重复值处理逻辑相对较为简单，更为复杂的其实当属异常值处理以及各种数据变换：例如类型转换、简单数值计算等等。在这一过程中，如何既能保证数据处理效率而又不失优雅，Pandas中的这几个函数堪称理想的解决方案。

为展示应用这3个函数完成数据处理过程中的一些demo，这里以经典的泰坦尼克号数据集为例。需要下载该数据集和文中示例源码的可后台回复关键字apply获取下载方式。

01 apply的方法论

在学习apply具体应用之前，有必要首先阐释apply函数的方法论。apply英文原义是"应用"的意思，作为编程语言中的函数名，似乎在很多种语言都有体现，比如近日个人在学习Scala语言中apply被用作是伴生对象中自动创建对象的缺省实现，如此重要的角色也可见apply这个函数的重要性。那么apply应用在Pandas中，其核心功能其实可以概括为一句话：

> apply：我本身不处理数据，我们只是数据的搬运工。

说人话就是，apply自身是不带有任何数据处理功能的，但可以用作是对其他数据处理方法的调度器，至于调度什么又为谁而调度呢？这是理解apply的两个核心环节：

- 调度什么？调度的是apply函数接收的参数，即apply接收一个数据处理函数为主要参数，并将其应用到相应的数据上。所以调度什么取决于接收了什么样的数据处理函数；
    
- 为谁调度？也就是apply接收的数据处理函数，其作用对象是谁？或者说数据处理的粒度是什么？答案是数据处理的粒度包括了点线面三个层面：即可以是单个元素（标量，scalar），也可以是一行或一列（series），还可以是一个dataframe。
    

当然，这些文字描述肯定还比较抽象，那么不妨直接进入正题：talk is cheap，show me the code！

02 apply基本方法示例

前面提到，理解apply核心在于明确两个环节：调度函数和作用对象。调度函数就是apply接收的参数，既可以是Python内置的函数，也支持自定义函数，只要符合指定的作用对象（即是标量还是series亦或一个dataframe）即可。而作用对象则取决于调用apply的对象类型，具体来说：

- 一个Series对象调用apply时，数据处理函数作用于该Series的每个元素上，即作用对象是一个标量，实现从一个Series转换到另一个Series；
    
- 一个DataFrame对象调用apply时，数据处理函数作用于该DataFrame的每一行或者每一列上，即作用对象是一个Series，实现从一个DataFrame转换到一个Series上；
    
- 一个DataFrame对象经过groupby分组后调用apply时，数据处理函数作用于groupby后的每个子dataframe上，即作用对象还是一个DataFrame（行是每个分组对应的行；列字段少了groupby的相应列），实现从一个DataFrame转换到一个Series上。
    

以泰坦尼克号数据集为例，这里分别举几个小例子。原始数据集如下：

<img width="511" height="185" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__81b688b0d15a4fa28.png"/>

1\. 应用到Series的每个元素

①将性别sex列转化为0和1数值，其中female对应0，male对应1。应用apply函数实现这一功能非常简单：

<img width="495" height="188" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__466290b34191420e9.png"/>

其中，这里apply接收了一个lambda匿名函数，通过一个简单的if-else逻辑实现数据映射。该功能十分简单，接收的函数也不带任何其他参数。

②下面再来一个稍微复杂一点的案例，注意到年龄age列当前数据类型是小数，需要将其转换为整数，同时还有0.9167这种过小的年龄，所以要求接受一个函数，支持接受指定的最大和最小年龄限制，当数据中超出此年龄范围的统一用截断填充，同时由于原数据集中age列存在缺失值，还需首先进行缺失值填充。这里首先实现一个自定义函数用于实现指定的年龄处理功能：

```
`def get_age(age, max_age, min_age):` `age = int(age)  # 转换为整数` `if age > max_age:` `age = max_age` `if age < min_age:` `age = min_age` `return age`
```

然后，直接对age列调用该函数即可，其中除了第一个参数age由调用该函数的series进行向量化填充外，另两个参数需要指定，在apply中即通过args传入。具体而言，实现如下：

<img width="677" height="269" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__25186f901c034561b.png"/>

2\. 应用到DataFrame的每个Series

DataFrame是pandas中的核心数据结构，其每一行和每一列都是一个Series数据类型。那么应用apply到一个DataFrame的每个Series，自然存在一个问题是应用到行还是列的问题，所以一个DataFrame调用apply函数时需要指定一个axis参数，其中axis=0对应行方向的处理，即对每列应用apply接收函数；axis=1对应列方向处理，即对每行应用接收函数。默认为axis=0。这里仍然举两个小例子：

①取所有数值列的数据最大值。当然，这个处理其实可以直接调用max函数，但这里为了演示apply应用，所以不妨照此尝试：

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__b2ae934c081f461ca.png)

上述apply函数完成了对四个数值列求取最大值，其中缺省axis参数为0，对应行方向处理，即对每一列数据求最大值。

②然后来一个按行方向处理的例子，例如根据性别和年龄，区分4类人群：即女孩、成年女子、男孩、成年男子，其中年龄以18岁为界值进行区分。首先给出人群划分的函数实现：

```
`def cat_person(sr):` `if sr['sex_num'] == 0:` `if sr['age_num'] < 18:` `return '女孩'` `else:` `return '成年女子'` `else:` `if sr['age_num'] < 18:` `return '男孩'` `else:` `return '成年男子'`
```

基于此，用apply简单调用即可，其中axis=1设置apply的作用方向为按列方向，即对每行进行处理。其中每行都相当于一个带有age和sex等信息的Series，通过cat_person函数进行提取判断，即实现了人群的划分：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

3\. 应用到DataFrame groupby后的每个分组DataFrame

实际上，个人一直觉得这是一个非常有效的用法，相较于原生的groupby，通过配套使用goupby+apply两个函数，实现更为个性化的聚合统计功能。例如，这里我们希望统计不同舱位等级内的"生存年龄比"（仅为配合举例而随意定义的指标，无实际含义），定义为各舱位等级内生存人员的年龄之和与所有人员年龄之和的比值。为实现这一数据统计，则首先应以舱位等级作为分组字段进行分组，而后对每个分组内的数据进行聚合统计，示例代码如下：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

其中apply接收一个lambda匿名函数，该匿名函数接收一个dataframe为参数（该dataframe中不含pclass列），并提取survived列和age_num列参与计算。最后得到每个舱位等级的一个统计指标结果，返回类型是一个Series对象。

这里，再补充一个前期分享过的一片推文：[Pandas用的6不6，来试试这道题就能看出来](http://mp.weixin.qq.com/s?__biz=MzU0OTk5MDg3OQ==&mid=2247485509&idx=1&sn=1cfa56e733c78959253308c00cd9bae4&chksm=fba630f5ccd1b9e3c2cf5d6367c4dc34ed374893ad8eae2096bf960e7e85077af36d9565d43f&scene=21#wechat_redirect)，实际上也是实现了相同的分组聚合统计功能。

以上，可以梳理apply函数的执行流程：首先明确调用apply的数据结构类型，是Series还是DataFrame，如果是DataFrame还需进一步确定是直接调用apply还是经过groupby分组之后调用，其中前者对应apply的接收函数处理一行或一列，后者对应接收函数处理每个分组对应的子DataFrame，最后根据作用对象类型设计相应的接收函数，从而完成个性化的数据处理。

03 apply的两个兄弟

前面介绍了apply的三种应用场景，作用对象分别对应元素、Series以及DataFrame，可以说功能已经非常强大了。除了apply之外，pandas其实还提供了两个功能极为相近的函数：map和applymap，不过相较于功能强大的apply来说，二者功能则相对局限。具体而言，二者分别实现功能如下：

1.map。在Python中提到map关键词，个人首先联想到的是两个场景：①一种数据结构，即字典或者叫映射，通过键值对的方式组织数据，在Python中叫dict；②Python的一个内置函数叫map，实现数据按照一定规则完成映射的过程。而在Pandas框架中，这两种含义都有所体现：对一个Series对象的每个元素实现字典映射或者函数变换，其中后者与apply应用于Series的用法完全一致，而前者则仅仅是简单将函数参数替换为字典变量即可。仍以替换性别一列为0/1数值为例，应用map函数的实现方式为：

<img width="677" height="214" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__fe195a0a64ed46fd9.png"/>

虽然map对于Series元素级的变换提供了两种数据转换方式，但却仅能用于Series，而无法应用到DataFrame上。但与此同时，map相较于apply又在另一个方面具有独特应用，即对于索引列这种特殊的Series只能应用map，而无法应用apply。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

2.applymap。从名字上可以看出，这好像是个apply函数与map函数的混合体，实际上也确实有这方面的味道：即applymap综合了apply可以应用到DataFrame和map仅能应用到元素级进行变换的双重特性，所以applymap是将接收函数应用于DataFrame的每个元素，以实现相应的变换。

> 从某种角度来讲，这种变换得以实施的前提是该DataFrame的各列元素具有相同的数据类型和相近的业务含义，否则运用相同的数据变换很难保证实际效果。

假设需要获取DataFrame中各个元素的数据类型，则应用applymap实现如下：

<img width="562" height="213" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__181b9bcc233347c9b.png"/>

04 小结

- apply、map和applymap常用于实现Pandas中的数据变换，通过接收一个函数实现特定的变换规则；
    
- apply功能最为强大，可应用于Series、DataFrame以及DataFrame分组后的group DataFrame，分别实现元素级、Series级以及DataFrame级别的数据变换；
    
- map仅可作用于Series实现元素级的变换，既可以接收一个字典完成变化也可接收特定的函数，而且不仅可作用于普通的Series类型，也可用于索引列的变换，而索引列的变换是apply所不能应用的；
    
- applymap仅可用于DataFrame，接收一个函数实现对所有数据实现元素级的变换
    

<img width="578" height="21" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__78906f579eef4bf9a.png"/>

People who liked this content also liked

像Python一样玩C/C++

...

光城

不看的原因

- 内容质量低
- 不看此公众号

我承认tidyverse已经脱离了R语言的范畴

...

育种数据分析之放飞自我

不看的原因

- 内容质量低
- 不看此公众号

四行代码秒解微积分！Python这个模块神了！

...

快学Python

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___edd32fa655e24a2fa.bmp"/>

Scan to Follow