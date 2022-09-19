# Python 字符串深度总结

<a id="profileBt"></a><a id="js_name"></a>AI科技大本营 *2022-05-12 18:00* *Posted on <a id="js_ip_wording"></a>北京*

The following article is from 萝卜大杂烩 Author 周萝卜

<a id="copyright_info"></a>[![](../../../_resources/0_462a752f78dd457b96ec873658abf60a.jpg)<br>**萝卜大杂烩** .<br>Do it by yourself！爱好Python，测试，NLP，数据分析，小程序，k8s等技术，期待与你一同成长](#)

<img width="661" height="117" src="../../../_resources/640_wx_fmt_gif_wxfrom_5_wx_lazy__355d9301cb0147bf8.gif"/>

作者 | 周萝卜

来源 | 萝卜大杂烩

今天我们来学习字符串数据类型相关知识，将讨论如何声明字符串数据类型，字符串数据类型与 ASCII 表的关系，字符串数据类型的属性，以及一些重要的字符串方法和操作，超级干货，不容错过！

## 什么是 Python 字符串

字符串是包含一系列字符的对象。字符是长度为 1 的字符串。在 Python 中，单个字符也是字符串。但是比较有意思的是，Python 编程语言中是没有字符数据类型的，不过在 C、Kotlin 和 Java 等其他编程语言中是存在字符数据类型的

我们可以使用单引号、双引号、三引号或 str() 函数来声明 Python 字符串。下面的代码片段展示了如何在 Python 中声明一个字符串：

```


# A single quote string
single_quote = 'a'  # This is an example of a character in other programming languages. It is a string in Python
# Another single quote string
another_single_quote = 'Programming teaches you patience.'
# A double quote string
double_quote = "aa"
# Another double-quote string
another_double_quote = "It is impossible until it is done!"
# A triple quote string
triple_quote = '''aaa'''
# Also a triple quote string
another_triple_quote = """Welcome to the Python programming language. Ready, 1, 2, 3, Go!"""
# Using the str() function
string_function = str(123.45)  # str() converts float data type to string data type
# Another str() function
another_string_function = str(True)  # str() converts a boolean data type to string data type
# An empty string
empty_string = ''
# Also an empty string
second_empty_string = ""
# We are not done yet
third_empty_string = """"""  # This is also an empty string: ''''''



```

在 Python 中获取字符串的另一种方法是使用 input() 函数。input() 函数允许我们使用键盘将输入的值插入到程序中。插入的值被读取为字符串，但我们可以将它们转换为其他数据类型：

```


# Inputs into a Python program
input_float = input()  # Type in: 3.142
input_boolean = input() # Type in: True
# Convert inputs into other data types
convert_float = float(input_float)  # converts the string data type to a float
convert_boolean = bool(input_boolean) # converts the string data type to a bool



```

我们使用 type() 函数来确定 Python 中对象的数据类型，它返回对象的类。当对象是字符串时，它返回 str 类。同样，当对象是字典、整数、浮点数、元组或布尔值时，它分别返回 dict、int、float、tuple、bool 类。现在让我们使用 type() 函数来确定前面代码片段中声明的变量的数据类型：

```


# Data types/ classes with type()
print(type(single_quote))
print(type(another_triple_quote))
print(type(empty_string))
print(type(input_float))
print(type(input_boolean))
print(type(convert_float))
print(type(convert_boolean))



```

## ASCII 表与 Python 字符串字符

美国信息交换标准代码 (ASCII) 旨在帮助我们将字符或文本映射到数字，因为数字集比文本更容易存储在计算机内存中。ASCII 主要用英语对 128 个字符进行编码，用于处理计算机和编程中的信息。ASCII 编码的英文字符包括小写字母（a-z）、大写字母（A-Z）、数字（0-9）以及标点符号等符号

ord() 函数将长度为 1（一个字符）的 Python 字符串转换为其在 ASCII 表上的十进制表示，而 chr() 函数将十进制表示转换回字符串。例如：

```


import string
# Convert uppercase characters to their ASCII decimal numbers
ascii_upper_case = string.ascii_uppercase  # Output: ABCDEFGHIJKLMNOPQRSTUVWXYZ
for one_letter in ascii_upper_case[:5]:  # Loop through ABCDE
    print(ord(one_letter))



```

Output：

```


65
66
67
68
69



```

```


# Convert digit characters to their ASCII decimal numbers
ascii_digits = string.digits  # Output: 0123456789
for one_digit in ascii_digits[:5]:  # Loop through 01234
    print(ord(one_digit))



```

Output：

```


48
49
50
51
52



```

在上面的代码片段中，我们遍历字符串 ABCDE 和 01234，并将每个字符转换为它们在 ASCII 表中的十进制表示。我们还可以使用 chr() 函数执行反向操作，从而将 ASCII 表上的十进制数字转换为它们的 Python 字符串字符。例如：

```


decimal_rep_ascii = [37, 44, 63, 82, 100]
for one_decimal in decimal_rep_ascii:
    print(chr(one_decimal))



```

Output：

```


%
,
?
R
d



```

在 ASCII 表中，上述程序输出中的字符串字符映射到它们各自的十进制数

## 字符串属性

**零索引：** 字符串中的第一个元素的索引为零，而最后一个元素的索引为 len(string) - 1。例如：

```


immutable_string = "Accountability"
print(len(immutable_string))
print(immutable_string.index('A'))
print(immutable_string.index('y'))



```

Output：

```


14
0
13



```

**不变性：** 这意味着我们不能更新字符串中的字符。例如我们不能从字符串中删除一个元素或尝试在其任何索引位置分配一个新元素。如果我们尝试更新字符串，它会抛出 TypeError：

```


immutable_string = "Accountability"
# Assign a new element at index 0
immutable_string[0] = 'B'



```

Output：

```


---------------------------------------------------------------------------
TypeError                                 Traceback (most recent call last)
~\AppData\Local\Temp/ipykernel_11336/2351953155.py in 
      2 
      3 # Assign a new element at index 0
----> 4 immutable_string[0] = 'B'
TypeError: 'str' object does not support item assignment



```

但是我们可以将字符串重新分配给 immutable_string 变量，不过我们应该注意它们不是同一个字符串，因为它们不指向内存中的同一个对象。Python 不会更新旧的字符串对象；它创建了一个新的，正如我们通过 ids 看到的那样：

```


immutable_string = "Accountability"
print(id(immutable_string))
immutable_string = "Bccountability"
print(id(immutable_string)
test_immutable = immutable_string
print(id(test_immutable))



```

Output：

```


2693751670576
2693751671024
2693751671024



```

上述两个 id 在同一台计算机上也不相同，这意味着两个 immutable\_string 变量都指向内存中的不同地址。我们将最后一个 immutable\_string 变量分配给 test\_immutable 变量。可以看到 test\_immutable 变量和最后一个 immutable_string 变量指向同一个地址

**连接：** 将两个或多个字符串连接在一起以获得带有 \+ 符号的新字符串。例如：

```


first_string = "Zhou"
second_string = "luobo"
third_string = "Learn Python"
fourth_string = first_string + second_string
print(fourth_string)
fifth_string = fourth_string + " " + third_string
print(fifth_string)



```

Output：

```


Zhouluobo
Zhouluobo Learn Python



```

**重复：** 字符串可以用 \* 符号重复。例如：

```


print("Ha" * 3)



```

Output：

```


HaHaHa



```

**索引和切片：** 我们已经确定字符串是从零开始索引的，我们可以使用其索引值访问字符串中的任何元素。我们还可以通过在两个索引值之间进行切片来获取字符串的子集。例如：

```


main_string = "I learned English and Python with ZHouluobo. You can do it too!"
# Index 0
print(main_string[0])
# Index 1
print(main_string[1])
# Check if Index 1 is whitespace
print(main_string[1].isspace())
# Slicing 1
print(main_string[0:11])
# Slicing 2:
print(main_string[-18:])
# Slicing and concatenation
print(main_string[0:11] + ". " + main_string[-18:])



```

Output：

```


I
True
I learned English
You can do it too!
I learned English. You can do it too!



```

## 字符串方法

**str.split(sep=None, maxsplit=-1)：** 字符串拆分方法包含两个属性：sep 和 maxsplit。当使用其默认值调用此方法时，它会在任何有空格的地方拆分字符串。此方法返回字符串列表：

```


string = "Apple, Banana, Orange, Blueberry"
print(string.split())



```

Output:

```


['Apple,', 'Banana,', 'Orange,', 'Blueberry']



```

我们可以看到字符串没有很好地拆分，因为拆分的字符串包含`,`。我们可以使用 `sep=','` 在有 `,`的地方进行拆分：

```


print(string.split(sep=','))



```

Output:

```


['Apple', ' Banana', ' Orange', ' Blueberry']



```

这比之前的拆分要好，但是我们可以在一些拆分字符串之前看到空格。可以使用 (sep=', ') 删除它：

```


# Notice the whitespace after the comma
print(string.split(sep=', '))



```

Output:

```


['Apple', 'Banana', 'Orange', 'Blueberry']



```

现在字符串被很好地分割了。有时我们不想分割最大次数，我们可以使用 maxsplit 属性来指定我们打算拆分的次数：

```


print(string.split(sep=', ', maxsplit=1))
print(string.split(sep=', ', maxsplit=2))



```

Output:

```


['Apple', 'Banana, Orange, Blueberry']
['Apple', 'Banana', 'Orange, Blueberry']



```

**str.splitlines(keepends=False)：** 有时我们想处理一个在边界处具有不同换行符（'\\n'、\\n\\n'、'\\r'、'\\r\\n'）的语料库。我们要拆分成句子，而不是单个单词。可以使用 splitline 方法来执行此操作。当 keepends=True 时，文本中包含换行符；否则它们被排除在外

```


import nltk  # You may have to `pip install nltk` to use this library.
macbeth = nltk.corpus.gutenberg.raw('shakespeare-macbeth.txt')
print(macbeth.splitlines(keepends=True)[:5])



```

Output:

```


['[The Tragedie of Macbeth by William Shakespeare 1603]\n', '\n', '\n', 'Actus Primus. Scoena Prima.\n', '\n']



```

**str.strip(\[chars\])：** 我们使用 strip 方法从字符串的两侧删除尾随空格或字符。例如：

```


string = "    Apple Apple Apple no apple in the box apple apple             "
stripped_string = string.strip()
print(stripped_string)
left_stripped_string = (
    stripped_string
    .lstrip('Apple')
    .lstrip()
    .lstrip('Apple')
    .lstrip()
    .lstrip('Apple')
    .lstrip()
)
print(left_stripped_string)
capitalized_string = left_stripped_string.capitalize()
print(capitalized_string)
right_stripped_string = (
    capitalized_string
    .rstrip('apple')
    .rstrip()
    .rstrip('apple')
    .rstrip()
)
print(right_stripped_string)



```

Output:

```


Apple Apple Apple no apple in the box apple apple
no apple in the box apple apple
No apple in the box apple apple
No apple in the box



```

在上面的代码片段中，我们使用了 lstrip 和 rstrip 方法，它们分别从字符串的左侧和右侧删除尾随空格或字符。我们还使用了 capitalize 方法，它将字符串转换为句子大小写**str.zfill(width)：** zfill 方法用 0 前缀填充字符串以获得指定的宽度。例如：

```


example = "0.8"  # len(example) is 3
example_zfill = example.zfill(5) # len(example_zfill) is 5
print(example_zfill)



```

Output:

```


000.8



```

**str.isalpha()：** 如果字符串中的所有字符都是字母，该方法返回True；否则返回 False：

```


# Alphabet string
alphabet_one = "Learning"
print(alphabet_one.isalpha())
# Contains whitspace
alphabet_two = "Learning Python"
print(alphabet_two.isalpha())
# Contains comma symbols
alphabet_three = "Learning,"
print(alphabet_three.isalpha())



```

Output:

```


True
False
False



```

如果字符串字符是字母数字，str.isalnum() 返回 True；如果字符串字符是十进制，str.isdecimal() 返回 True；如果字符串字符是数字，str.isdigit() 返回 True；如果字符串字符是数字，则 str.isnumeric() 返回 True

如果字符串中的所有字符都是小写，**str.islower()** 返回 True；如果字符串中的所有字符都是大写，**str.isupper()** 返回 True；如果每个单词的首字母大写，**str.istitle()** 返回 True：

```


# islower() example
string_one = "Artificial Neural Network"
print(string_one.islower())
string_two = string_one.lower()  # converts string to lowercase
print(string_two.islower())
# isupper() example
string_three = string_one.upper() # converts string to uppercase
print(string_three.isupper())
# istitle() example
print(string_one.istitle())



```

Output:

```


False
True
True
True



```

**str.endswith(suffix)** 返回 True 是以指定后缀结尾的字符串。如果字符串以指定的前缀开头，**str.startswith(prefix)** 返回 True：

```


sentences = ['Time to master data science', 'I love statistical computing', 'Eat, sleep, code']
# endswith() example
for one_sentence in sentences:
    print(one_sentence.endswith(('science', 'computing', 'Code')))



```

Output:

```


True
True
False



```

```


# startswith() example
for one_sentence in sentences:
    print(one_sentence.startswith(('Time', 'I ', 'Ea')))



```

Output:

```


True
True
True



```

**str.find(substring)** 如果子字符串存在于字符串中，则返回最低索引；否则它返回 -1。**str.rfind(substring)** 返回最高索引。如果找到，**str.index(substring)** 和 **str.rindex(substring)** 也分别返回子字符串的最低和最高索引。如果字符串中不存在子字符串，则会引发 ValueError

```


string = "programming"
# find() and rfind() examples
print(string.find('m'))
print(string.find('pro'))
print(string.rfind('m'))
print(string.rfind('game'))
# index() and rindex() examples
print(string.index('m'))
print(string.index('pro'))
print(string.rindex('m'))
print(string.rindex('game'))



```

Output:

```


6
0
7
-1
6
0
7
---------------------------------------------------------------------------
ValueError                                Traceback (most recent call last)
~\AppData\Local\Temp/ipykernel_11336/3954098241.py in 
     11 print(string.index('pro'))  # Output: 0
     12 print(string.rindex('m'))  # Output: 7
---> 13 print(string.rindex('game'))  # Output: ValueError: substring not found
ValueError: substring not found



```

**str.maketrans(dict_map)** 从字典映射创建一个翻译表，str.translate(maketrans) 用它们的新值替换翻译中的元素。例如：

```


example = "abcde"
mapped = {'a':'1', 'b':'2', 'c':'3', 'd':'4', 'e':'5'}
print(example.translate(example.maketrans(mapped)))



```

Output:

```


12345



```

## 字符串操作

**循环遍历一个字符串**

字符串是可迭代的，因此它们支持使用 for 循环和枚举的循环操作：

```


# For-loop example
word = "bank"
for letter in word:
    print(letter)



```

Output:

```


b
a
n
k



```

```


# Enumerate example
for idx, value in enumerate(word):
    print(idx, value)



```

Output:

```


0 b
1 a
2 n
3 k



```

**字符串和关系运算符**

当使用关系运算符（>、<、== 等）比较两个字符串时，两个字符串的元素按其 ASCII 十进制数字逐个索引进行比较。例如：

```


print('a' > 'b')
print('abc' > 'b')



```

Output：

```


False
False



```

在这两种情况下，输出都是 False。关系运算符首先比较两个字符串的索引 0 上元素的 ASCII 十进制数。由于 b 大于 a，因此返回 False；在这种情况下，其他元素的 ASCII 十进制数字和字符串的长度无关紧要

当字符串长度相同时，它比较从索引 0 开始的每个元素的 ASCII 十进制数，直到找到具有不同 ASCII 十进制数的元素。例如：

```


print('abd' > 'abc')



```

Output:

```


True



```

**检查字符串的成员资格**

in 运算符用于检查子字符串是否是字符串的成员：

```


print('data' in 'dataquest')
print('gram' in 'programming')



```

Output:

```


True
True



```

检查字符串成员资格、替换子字符串或匹配模式的另一种方法是使用正则表达式

```


import re
substring = 'gram'
string = 'programming'
replacement = '1234'
# Check membership
print(re.search(substring, string))
# Replace string
print(re.sub(substring, replacement, string))



```

Output:

```


pro1234ming



```

**字符串格式**

f-string 和 str.format() 方法用于格式化字符串。两者都使用大括号 {} 占位符。例如：

```


monday, tuesday, wednesday = "Monday", "Tuesday", "Wednesday"
format_string_one = "{} {} {}".format(monday, tuesday, wednesday)
print(format_string_one)
format_string_two = "{2} {1} {0}".format(monday, tuesday, wednesday)
print(format_string_two)
format_string_three = "{one} {two} {three}".format(one=tuesday, two=wednesday, three=monday)
print(format_string_three)
format_string_four = f"{monday} {tuesday} {wednesday}"
print(format_string_four)



```

Output:

```


Monday Tuesday Wednesday
Wednesday Tuesday Monday
Tuesday Wednesday Monday
Monday Tuesday Wednesday



```

f-strings 更具可读性，并且它们比 str.format() 方法实现得更快。因此，f-string 是字符串格式化的首选方法

**处理引号和撇号**

撇号 (') 在 Python 中表示一个字符串。为了让 Python 知道我们不是在处理字符串，我们必须使用 Python 转义字符 ()。因此撇号在 Python 中表示为 '。与处理撇号不同，Python 中有很多处理引号的方法。它们包括以下内容：

```


# 1. Represent string with single quote (`""`) and quoted statement with double quote (`""`)
quotes_one =  '"Friends don\'t let friends use minibatches larger than 32" - Yann LeCun'
print(quotes_one)
# 2. Represent string with double quote `("")` and quoted statement with escape and double quote `(\"statement\")`
quotes_two =  "\"Friends don\'t let friends use minibatches larger than 32\" - Yann LeCun"
print(quotes_two)
# 3. Represent string with triple quote `("""""")` and quoted statment with double quote ("")
quote_three = """"Friends don\'t let friends use minibatches larger than 32" - Yann LeCun"""
print(quote_three)



```

Output:

```


"Friends don't let friends use minibatches larger than 32" - Yann LeCun
"Friends don't let friends use minibatches larger than 32" - Yann LeCun
"Friends don't let friends use minibatches larger than 32" - Yann LeCun



```

## 写在最后

字符串作为编程语言当中最为常见的数据类型，熟练而灵活的掌握其各种属性和方法，实在是太重要了，小伙伴们千万要实时温习，处处留心哦！

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

```


往

期

回

顾

资讯

[变身「毒」苹果？全球首个DMP漏洞](http://mp.weixin.qq.com/s?__biz=Mzg4NDQwNTI0OQ==&mid=2247559704&idx=1&sn=114f55595b74bf59aaa2e2ba08264390&chksm=cfbb0f76f8cc8660458b8e75bb1855d82570f8488168dadaccab4454dafbc1c3d0c1df5b71b6&scene=21#wechat_redirect)

资讯

[程序员化身“侦探”识破AI律所骗局](http://mp.weixin.qq.com/s?__biz=Mzg4NDQwNTI0OQ==&mid=2247559602&idx=1&sn=0385fa77b6822c878d80b47a37183787&chksm=cfbb0cdcf8cc85ca6fdd88045d4756ba17234e2181d49f2bf8e42cce2dbf986c4c9c719cafb4&scene=21#wechat_redirect)

技术

[浅谈Python中的字符串格式化输出](http://mp.weixin.qq.com/s?__biz=Mzg4NDQwNTI0OQ==&mid=2247559826&idx=1&sn=9a82d9e1b9b45140dc485852af8cd2e5&chksm=cfbb0ffcf8cc86ea09a1e8e08b94fc17cbb7548275c22154a09929e5d1ab56240025b8a17643&scene=21#wechat_redirect)

技术

[10个有趣的Python高级脚本！](http://mp.weixin.qq.com/s?__biz=Mzg4NDQwNTI0OQ==&mid=2247559235&idx=3&sn=d160e0ea521b785bcc19b5e6544318b8&chksm=cfbb0d2df8cc843b3c74cdb8cc4fe2bc641104243a8af5471c1730a591a7eb120b50e93e9267&scene=21#wechat_redirect)

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

真香！详解 Python 好用的内置函数

...

AI科技大本营

不看的原因

- 内容质量低
- 不看此公众号

一文弄懂Python中的 if \_\_name\_\_==\_\_main\_\_

...

AI算法之道

不看的原因

- 内容质量低
- 不看此公众号

浅谈 Python 中的字符串格式化输出

...

AI科技大本营

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___1e1f7f31ceb4481fa.bmp"/>

Scan to Follow