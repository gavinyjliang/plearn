# Python！要这样写！

<a id="profileBt"></a><a id="js_name"></a>数据管道 *2021-10-11 21:46*

The following article is from Datawhale Author Frank Andrade

<a id="copyright_info"></a>[![](../../../_resources/0_4f95b39236b243b1b826ec22ca452a28.jpg)<br>**Datawhale** .<br>一个专注于AI领域的开源组织，汇聚了众多优秀学习者，愿景-for the learner，和学习者一起成长。](#)

作者：Frank Andrade，译者：张峰

众所周知，编写Python代码在开始时十分容易，但随着你在工具包中添加更多的库，你的脚本可能会有不必要的代码行，变得冗长而混乱。可能短期内能够应付工作，但长期来看，麻烦不小。

在这篇文章中，我将与你分享7个技巧，使你在使用Python进行数据科学时更加简洁。这涵盖了我们日常所做的事情，例如修改Pandas数据框中的值，连接字符串，读取文件等操作！

## 1\. 使用Lambda来修改Pandas数据框中的值

假设我们有以下`df`数据框:

```
data = [[1,2,3], [4,5,6], [7,8,9]]
df = pd.DataFrame(data, columns=[0,1,2])
IN[1]: print (df)
OUT[1]:    0  1  2
        0  1  2  3
        1  4  5  6
        2  7  8  9

```

现在由于某种原因，你需要在第`0`列的数字上添加`01`的值。一个常见的方法是定义一个函数来完成这个任务，然后用 apply 函数来修改一列的值。

```
def add_numbers(x):
    return f'{x}01'
df[0] = df[0].apply(add_numbers)
IN[1]: print (df)
OUT[1]:     0   1   2
        0  101  2   3
        1  401  5   6
        2  701  8   9

```

这并不复杂，但是在数据框中对每一个改变创建一个函数是不切实际的。这时lambda就派上了用场。

lambda函数类似于普通的Python函数，但它可以不使用名称来定义，这使得它成为一个漂亮的单行代码。之前使用的代码可以用以下方式来减少。

```
df[0] = df[0].apply(lambda x:f'{x}01')

```

当你不知道是否可以访问一个系列的属性来修改数据时，Lambda变得非常有用。

例如，列`0`包含字母，我们想把它们大写。

```
# 如果你知道.str的存在，你可以这样做
df[0] = df[0].str.title()
# 如果你不知道.str，你仍然可以用lambda大写
df[0] = df[0].apply(lambda x: x.title())

```

## 2\. 使用f-string来连接字符串

字符串连接是Python中非常常见的操作，它可以用不同的方法来完成。最常见的方法是使用`+`运算符；然而，这个运算符的一个问题是我们不能在字符串之间添加任何分隔符。

当然，如果你想把 "Hello "和 "World "连接起来，一个典型的变通方法是添加一个空白分隔符（" "）。

```
print("Hello" + " " + "World")

```

这就完成了工作，但为了写出更可读的代码，我们可以用一个f-string来代替它。

```
IN[2]: print(f'{Hello} {World}')
OUT[2]: "Hello World"

```

在一个基本的例子中，这似乎是不必要的，但是当涉及到连接多个值时（正如你将在提示#3中看到的），f-string将使你免于书写多次`+ " " +`。我不知道过去有多少次不得不写`+`运算符，但现在不会了！

其他连接字符串的方法是使用`join()`方法或`format()`函数，然而f-string在字符串连接方面做得更好。

## 3\. 用Zip()函数对多个列表进行迭代

你是否曾经想在 Python 中循环遍历一个以上的列表？当你有两个列表时，你可以用 `enumerate` 来实现。

```
teams = ['Barcelona', 'Bayern Munich', 'Chelsea']
leagues = ['La Liga', 'Bundesliga', 'Premiere League']
for i, team in enumerate(teams):
    league = leagues[i]
    print(f'{team} plays in {league}')

```

然而，当你有两个或更多的列表时，这变得不切实际。一个更好的方法是使用`zip()`函数。`zip()`函数接收迭代数据，将它们聚集在一个元组中，并返回之。

让我们再增加一个列表，看看`zip()`的威力!

```
teams = ['Barcelona', 'Bayern Munich', 'Chelsea']
leagues = ['La Liga', 'Bundesliga', 'Premiere League']
countries = ['Spain', 'Germany', 'UK']
for team, league, country in zip(teams, leagues, countries):
    print(f'{team} plays in {league}. Country: {country}')

```

上述代码的输出结果为：

```
Barcelona plays in La Liga. Country: Spain
Bayern Munich plays in Bundesliga. Country: Germany
Chelsea plays in Premiere League. Country: UK

```

此处你注意到我们在这个例子中使用了f-string吗？代码变得更有可读性，不是吗？

## 4\. 使用列表理解法

清洗和处理数据的一个常见步骤是修改现有的列表。比如，我们有以下需要大写的列表：

```
words = ['california', 'florida', 'texas']

```

将words列表的每个元素大写的典型方法是创建一个新的大写列表，执行一次 for 循环，使用.title()，然后将每个修改的值附加到新的列表中。

```
capitalized = []
for word in words:
    capitalized.append(word.title())

```

然而，Pythonic的方法是使用列表理解来做到这一点。列表理解有一种优雅的方法来制作列表。

你可以用一行代码重写上面的`for`循环：

```
capitalized = [word.title() for word in words]

```

由此我们可以跳过第一个例子中的一些步骤，结果是一样的。

## 5\. 对文件对象使用with语句

当在一个项目上工作时，我们经常会对文件进行读写操作。最常见的方法是使用`open()`函数打开一个文件，它会创建一个我们可以操作的文件对象，然后作为一个习惯的做法，我们应该使用`close()`关闭该文件对象。

```
f = open('dataset.txt', 'w')
f.write('new_data')
f.close()

```

这很容易记住，但有时写了几个小时的代码，我们可能会忘记用`f.close()`关闭`f`文件。这时，`with`语句就派上了用场。`with`语句将自动关闭文件对象`f`，形式如下:

```
with open('dataset.txt', 'w') as f:
    f.write('new_data')

```

有了这个，我们可以保持代码的简短。

你不需要用它来读取CSV文件，因为你可以用pandas的 `pd.read_csv()`轻松地读取，但在读取其他类型的文件时，这仍然很有用。例如，从pickle文件中读取数据时经常使用它。

```
import pickle 
# 从pickle文件中读取数据集
with open(‘test’, ‘rb’) as input:
    data = pickle.load(input)

```

## 6\. 停止使用方括号来获取字典项, 利用.get()代替

比如，有以下一个字典:

```
person = {'name': 'John', 'age': 20}

```

我们可以通过`person[name]`和`person[age]`分别获得姓名和年龄。但是，由于某种原因，我们想获得一个不存在的键，如 "工资"，运行`person[salary]`会引发一个`KeyError'。

这时，get()方法就有用了。如果键在字典中，get()方法返回指定键的值，但是如果没有找到键，Python 将返回None。得益于此，你的代码不会中断。

```
person = {'name': 'John', 'age': 20}
print('Name: ', person.get('name'))
print('Age: ', person.get('age'))
print('Salary: ', person.get('salary'))

```

输出结果如下:

```
Name:  John
Age:  20
Salary:  None

```

## 7\. 多重赋值

你是否曾想减少用于创建多个变量、列表或字典的代码行数？那么，你可以用多重赋值轻松做到这一点。

```
# 原始操作
a = 1
b = 2
c = 3
# 替代操作
a, b, c = 1, 2, 3
# 代替在不同行中创建多个列表
data_1 = []
data_2 = []
data_3 = []
data_4 = []
# 可以在一行中创建它们的多重赋值
data_1, data_2, data_3, data_4 = [], [], [], []
# 或者使用列表理解法
data_1, data_2, data_3, data_4 = [[] for i in range(4)]

```

原文链接:

https://towardsdatascience.com/7-tips-to-level-up-your-python-code-for-data-science-4a64dbccd86d

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

·················END·················

## 推荐阅读

1.  [我在字节做了哪些事](http://mp.weixin.qq.com/s?__biz=MzU5NDgyMjc0OQ==&mid=2247503284&idx=1&sn=8e59089838605febce45a6713e68a4e7&chksm=fe79de86c90e579030f8b0565183d908e443be93bf35633709b917912d003403f14f2ee6443d&scene=21#wechat_redirect)
    
2.  [写给所有数据人。](http://mp.weixin.qq.com/s?__biz=MzU5NDgyMjc0OQ==&mid=2247493187&idx=1&sn=7fd8e6590ae406d7936a29eb5bd6e5ad&chksm=fe79f571c90e7c67df0fdeaee5d3544a0ef3095e773b9425a33fe6f153495565a65acf8a9a9f&scene=21#wechat_redirect)
    
3.  [从留存率业务案例谈0-1的数据指标体系](http://mp.weixin.qq.com/s?__biz=MzU5NDgyMjc0OQ==&mid=2247497334&idx=1&sn=60398d4ba0c4a77abda2961082a350fa&chksm=fe79e544c90e6c52aec043db113bf01703591d9d4b9f9ee5bc4f527bdd525a6de74fb6e24bcc&scene=21#wechat_redirect)
    
4.  [数据分析师的一周](http://mp.weixin.qq.com/s?__biz=MzU5NDgyMjc0OQ==&mid=2247501242&idx=1&sn=b1d3d24416d0a186cc728c221034596e&chksm=fe79d688c90e5f9ef1348344c79c30ed99d78e93dec9c495d6668781bb3b7559a1c214d0fc2f&scene=21#wechat_redirect)
    
5.  [超级菜鸟如何入门数据分析？](http://mp.weixin.qq.com/s?__biz=MzU5NDgyMjc0OQ==&mid=2247485269&idx=1&sn=88c370a387cbc8023ec7b0409c434d99&chksm=fe7a1467c90d9d71531bb7988b7b97bf97be0fab82a63604527ecedaad8f6bc1f9aba1b3409f&scene=21#wechat_redirect)
    

<img width="202" height="202" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_2ee37197170d44eda.jpg"/>

欢迎长按扫码关注「数据管道」

<a id="js_view_source"></a>Read more

People who liked this content also liked

MyHDL，体验一下“用python设计电路”

ExASIC

不看的原因

- 内容质量低
- 不看此公众号

【进阶】嫌弃Python慢，试试这几个方法？

深度学习算法与计算机视觉

不看的原因

- 内容质量低
- 不看此公众号

一文详解Python图形界面框架：PyQt5

Python大数据分析

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___049ac2f78e3349159.bmp"/>

Scan to Follow