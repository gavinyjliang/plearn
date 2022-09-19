# 这 25 个 Pandas 实用技巧你都会吗

<a id="profileBt"></a><a id="js_name"></a>数据分析与开发 *2022-02-24 12:10*

↓推荐关注↓

![](../../../_resources/0_wx_fmt_png_d9189281aab64afdaa2a064bc4a11518.png)

**Python开发精选**

分享 Python 技术文章、资源、课程、资讯。

<a id="js_profile_article"></a>13篇原创内容

Official Account

**从剪贴板中创建DataFram****e**

假设你将一些数据储存在Excel或者Google Sheet中，你又想要尽快地将他们读取至DataFrame中。你需要选择这些数据并复制至剪贴板。然后，你可以使用**read_clipboard()函数**将他们读取至DataFrame中：

<img width="677" height="146" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__408951b7c0a9474b9.png"/>

和read\_csv()类似，read\_clipboard()会自动检测每一列的正确的数据类型：

<img width="677" height="88" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__af9350cb4a134db1b.png"/>

让我们再复制另外一个数据至剪贴板：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

神奇的是，pandas已经将第一列作为索引了：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

需要注意的是，**如果你想要你的工作在未来可复制，那么read_clipboard()并不值得推荐。**

**将DataFrame划分为两个随机的子集**

假设你想要将一个DataFrame划分为两部分，随机地将75%的行给一个DataFrame，剩下的25%的行给另一个DataFrame。

举例来说，我们的movie ratings这个DataFrame有979行：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

我们可以使用**sample()函数**来随机选取75%的行，并将它们赋值给"movies_1"DataFrame：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

接着我们使用drop()函数来舍弃“moive\_1”中出现过的行，将剩下的行赋值给"movies\_2"DataFrame：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

你可以发现总的行数是正确的：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

你还可以检查每部电影的索引，或者"moives_1":

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

或者"moives_2":

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

需要注意的是，**这个方法在索引值不唯一的情况下不起作用。**

> **注：**该方法在机器学习或者深度学习中很有用，因为在模型训练前，我们往往需要将全部数据集按某个比例划分成训练集和测试集。该方法既简单又高效，值得学习和尝试。

**多种类型过滤DataFrame**

### 

### 让我们先看一眼movies这个DataFrame：

```
In [60]:
movies.head()
Out[60]:

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

其中有一列是genre（类型）:

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

比如我们想要对该DataFrame进行过滤，我们只想显示genre为Action或者Drama或者Western的电影，我们可以使用多个条件，以"or"符号分隔：

```
In [62]:
movies[(movies.genre == 'Action') |     
       (movies.genre == 'Drama') |     
       (movies.genre == 
'Western')].head()
Out[62]:
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

但是，你实际上可以使用isin()函数将代码写得更加清晰，将genres列表传递给该函数：

```
In [63]:
movies[movies.genre.isin(['Action', 'Drama', 'Western'])].head()
Out[63]:

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

如果你想要进行相反的过滤，也就是你将吧刚才的三种类型的电影排除掉，那么你可以在过滤条件前加上破浪号：

```
In [64]:
movies[~movies.genre.isin(['Action',
'Drama', 'Western'])].head()
Out[64]:

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

这种方法能够起作用是**因为在Python中，波浪号表示“not”操作。**

**DataFrame筛选数量最多类别**

假设你想要对movies这个DataFrame通过genre进行过滤，但是只需要前3个数量最多的genre。

我们对genre使用**value_counts()函数**，并将它保存成counts（type为Series）:

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

该Series的nlargest()函数能够轻松地计算出Series中前3个最大值：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

事实上我们在该Series中需要的是索引：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

最后，我们将该索引传递给isin()函数，该函数会把它当成genre列表：

```
In [68]:
movies[movies.genre.isin(counts.nlargest(3).index)].head()
Out[68]:

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

这样，在DataFrame中只剩下Drame, Comdey, Action这三种类型的电影了。

**处理缺失值**

让我们来看一看UFO sightings这个DataFrame:

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

你将会注意到有些值是**缺失的**。

为了找出每一列中有多少值是缺失的，你可以使用**isna()函数**，然后再使用**sum()**:

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

isna()会产生一个由True和False组成的DataFrame，sum()会将所有的True值转换为1，False转换为0并把它们加起来。

类似地，你可以通过mean()和isna()函数找出每一列中缺失值的百分比。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

如果你想要舍弃那些包含了缺失值的列，你可以使用dropna()函数：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

或者你想要舍弃那么缺失值占比超过10%的列，你可以给dropna()设置一个阈值：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

len(ufo)返回总行数，我们将它乘以0.9，以告诉pandas保留那些至少90%的值不是缺失值的列。

**一个字符串划分成多列**

我们先创建另一个新的示例DataFrame:

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

如果我们需要将“name”这一列划分为三个独立的列，用来表示first, middle, last name呢？我们将会使用**str.split()函数**，告诉它**以空格进行分隔**，并将结果扩展成一个DataFrame:

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

这三列实际上可以通过一行代码保存至原来的DataFrame:

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

如果我们想要划分一个字符串，但是仅保留其中一个结果列呢？比如说，让我们以", "来划分location这一列：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

如果我们只想保留第0列作为city name，我们仅需要选择那一列并保存至DataFrame:

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

**Series扩展成DataFrame**

让我们创建一个新的示例DataFrame:

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

这里有两列，第二列包含了Python中的由整数元素组成的列表。

如果我们想要将第二列扩展成DataFrame，我们可以对那一列使用**apply()函数并传递给Series constructor**:

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

通过使用concat()函数，我们可以将原来的DataFrame和新的DataFrame组合起来：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

**对多个函数进行聚合**

让我们来看一眼从Chipotle restaurant chain得到的orders这个DataFrame:

```
In [82]:
orders.head(10)
Out[82]:

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

每个订单（order）都有订单号（order\_id），包含一行或者多行。为了找出每个订单的总价格，你可以将那个订单号的价格（item\_price）加起来。比如，这里是订单号为1的总价格：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

如果你想要计算每个订单的总价格，你可以对order\_id使用groupby()，再对每个group的item\_price进行求和。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

但是，事实上你不可能在聚合时仅使用一个函数，比如sum()。**为了对多个函数进行聚合，你可以使用agg()函数，**传给它一个函数列表，比如sum()和count():

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

这将告诉我们没定订单的总价格和数量。

**聚合结果与DataFrame组合**

让我们再看一眼orders这个DataFrame:

```
In [86]:
orders.head(10)
Out[86]:

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

如果我们想要增加新的一列，用于展示每个订单的总价格呢？回忆一下，我们通过使用sum()函数得到了总价格：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

sum()是一个聚合函数，这表明它返回输入数据的精简版本（reduced version ）。

换句话说，sum()函数的输出：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

比这个函数的输入要小：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

解决的办法是**使用transform()函数****，它会执行相同的操作但是返回与输入数据相同的形状**：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

我们将这个结果存储至DataFrame中新的一列：

```
In [91]:
orders['total_price'] = 
total_price
orders.head(10)
Out[91]:

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

你可以看到，每个订单的总价格在每一行中显示出来了。

这样我们就能方便地甲酸每个订单的价格占该订单的总价格的百分比：

```
In [92]:
orders['percent_of_total'] = orders.item_price / orders.total_price
orders.head(10)
In [92]:

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

**选取行和列的切片**

让我们看一眼另一个数据集：

```
In [93]:
titanic.head()
Out[93]:

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

这就是著名的Titanic数据集，它保存了Titanic上乘客的信息以及他们是否存活。

如果你想要对这个数据集做一个数值方面的总结，你可以使用describe()函数：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

但是，这个DataFrame结果可能比你想要的信息显示得更多。

如果你想对这个结果进行过滤，只想显示“五数概括法”（five-number summary）的信息，你可以使用**loc函数并传递"min"到"max"的切片**:

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

如果你不是对所有列都感兴趣，你也可以传递列名的切片：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

**MultiIndexed Series重塑**

Titanic数据集的Survived列由1和0组成，因此你可以对这一列计算总的存活率：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

如果你想对某个类别，比如“Sex”，计算存活率，你可以使用groupby():

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

如果你想一次性对两个类别变量计算存活率，你可以对这些类别变量使用groupby()：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

该结果展示了由Sex和Passenger Class联合起来的存活率。它存储为一个MultiIndexed Series，也就是说它对实际数据有多个索引层级。

这使得该数据难以读取和交互，因此更为方便的是**通过unstack()函数将MultiIndexed Series重塑成一个DataFrame**:

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

该DataFrame包含了与MultiIndexed Series一样的数据，不同的是，现在你可以用熟悉的DataFrame的函数对它进行操作。

**创建数据透视表**

如果你经常使用上述的方法创建DataFrames，你也许会发现用**pivot_table()函数更为便捷**：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

想要使用数据透视表，你需要**指定索引**(index),** 列名**(columns), **值**(values)和**聚合函数**(aggregation function)。

数据透视表的另一个好处是，你可以**通过****设置margins=True轻松地将行和列都加起来**：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

这个结果既显示了总的存活率，也显示了Sex和Passenger Class的存活率。

最后，你可以创建交叉表（cross-tabulation），只需要将聚合函数由"mean"改为"count":

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

这个结果展示了每一对类别变量组合后的记录总数。

**连续数据转类别数据**

让我们来看一下Titanic数据集中的Age那一列：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

它现在是连续性数据，但是如果我们想要将它转变成类别数据呢？

一个解决办法是对年龄范围打标签，比如"adult", "young adult", "child"。实现该功能的最好方式是使用**cut()函数**：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

这会对每个值打上标签。0到18岁的打上标签"child"，18-25岁的打上标签"young adult"，25到99岁的打上标签“adult”。

注意到，该数据类型为**类别变量**，该类别变量**自动排好序**了（有序的类别变量）。

**Style a DataFrame**

上一个技巧在你想要修改整个jupyter notebook中的显示会很有用。但是，一个更灵活和有用的方法是定义特定DataFrame中的格式化（style）。

让我们回到stocks这个DataFrame:

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

我们可以**创建一个格式化字符串的字典，用于对每一列进行格式化**。然后将其传递给DataFrame的style.format()函数：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

注意到，Date列是month-day-year的格式，Close列包含一个$符号，Volume列包含逗号。

我们可以通过**链式调用函数**来应用更多的格式化：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

我们现在隐藏了索引，将Close列中的最小值高亮成红色，将Close列中的最大值高亮成浅绿色。

这里有另一个DataFrame格式化的例子：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

Volume列现在有一个渐变的背景色，你可以轻松地识别出大的和小的数值。

最后一个例子：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

现在，Volumn列上有一个条形图，DataFrame上有一个标题。

请注意，还有许多其他的选项你可以用来格式化DataFrame。

**额外技巧**

### **Profile a DataFrame**

假设你拿到一个新的数据集，你不想要花费太多力气，只是想快速地探索下。那么你可以**使用****pandas-profiling这个模块**。

在你的系统上安装好该模块，然后**使用ProfileReport()函数**，传递的参数为任何一个DataFrame。它会返回一个互动的HTML报告：

- 第一部分为该数据集的总览，以及该数据集可能出现的问题列表
    
- 第二部分为每一列的总结。你可以点击"toggle details"获取更多信息
    
- 第三部分显示列之间的关联热力图
    
- 第四部分为缺失值情况报告
    
- 第五部分显示该数据及的前几行
    

**使用示例如下**（只显示第一部分的报告）：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

> 原文链接：
> https://nbviewer.jupyter.org/github/justmarkham/pandas-videos/blob/master/top\_25\_pandas_tricks.ipynb

\- EOF -

推荐阅读  点击标题可跳转

<ins>1、[pandas 与 lambda 完美结合指南](http://mp.weixin.qq.com/s?__biz=MzA5ODM5MDU3MA==&mid=2650881097&idx=2&sn=5369b5f82c7b05f971f961a0e453ac3a&chksm=8b67db0cbc10521a6fdf9013241fc7cca377b5414632dc8a3177e3b2394162607639bc47e547&scene=21#wechat_redirect)</ins>

<ins>2、[Pandas 缺失数据处理大全（附代码）](http://mp.weixin.qq.com/s?__biz=MzA5ODM5MDU3MA==&mid=2650881301&idx=2&sn=9a15270ef6ee99bdfbc8ce0a68feaacd&chksm=8b67da50bc105346a162d0611c3a9b84cd5073859e76828f14aa9ddc402856173beca96b49e4&scene=21#wechat_redirect)</ins>

<ins>3、[简单好用，分享 4 款 Pandas 自动数据分析神器！](http://mp.weixin.qq.com/s?__biz=MzA5ODM5MDU3MA==&mid=2650881904&idx=2&sn=e09088143c317a00b684cc606f459095&chksm=8b67d835bc105123be15f363b9a3f133aac31c35dde87f3142db4f73495ad7281696bc9579b4&scene=21#wechat_redirect)</ins>

看完本文有收获？请转发分享给更多人

**推荐关注「数据分析与开发」，提升数据技能**

![](../../../_resources/0_wx_fmt_png_dda9e35975ae48699cba30c6aa48fde5.png)

**数据分析与开发**

「数据分析与开发」分享数据分析与开发相关技术文章、教程、工具

<a id="js_profile_article"></a>55篇原创内容

Official Account

点赞和在看就是最大的支持❤️

People who liked this content also liked

Elasticsearch如何做到亿级数据查询毫秒级返回？

互联网后端架构

不看的原因

- 内容质量低
- 不看此公众号

当eBPF遇上Linux内核网络

Linux内核之旅

不看的原因

- 内容质量低
- 不看此公众号

Linux 性能优化的全景指南，可能都在这里了，建议收藏~

高效运维

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___be8436249ee5484e9.bmp"/>

Scan to Follow