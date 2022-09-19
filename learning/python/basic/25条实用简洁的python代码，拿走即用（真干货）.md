# 25条实用简洁的python代码，拿走即用（真干货）

<a id="profileBt"></a><a id="js_name"></a>python数据分析之禅 *2022-05-20 12:32* *Posted on <a id="js_ip_wording"></a>北京*

The following article is from Web图书馆 Author 九尾

<a id="copyright_info"></a>[![](../../../_resources/0_765f5242300042c78a444897e6c3d38b.jpg)<br>**Web图书馆** .<br>回复：【106本书】，领取106本Python电子书。 和你一起，探索好玩的编程世界。](#)

![](../../../_resources/0_wx_fmt_png_79fc529e44084a439afccb5756c30493.png)

**python数据分析之禅**

点击领取pandas高清速查表，后台回复“速查表”获取

<a id="js_profile_article"></a>85篇原创内容

Official Account

**经常写代码的大佬一定有自己私藏的非常好用的代码段对不对？**

虽然我不算什么大佬，但是我擅长搜集信息啊，通过我的努力，我从互联网上搜集整理了25个常用的Python代码段，分享给需要的人。

**你想问为什么是Python么？**

其实，选择Python也不是没有理由的。

**1，Python的前景非常好：**Guido龟叔表示：他打算在2022年10月发布3.11版本时将快CPython的速度提高1倍。在接下来的四年里，他的目标是将CPython的速度提高到原来的5倍。

**2，Python 的用法非常简洁、灵活：**它的扩展库也很丰富，可以满足非常多复杂场景的需求，能够替代非常多的手工操作。

**3，Python跨平台性非常好：**无论是在 macOS 和 Windows 间如何切换，不用修改任何一行代码，就可以让已经写好的程序直接在新的平台上运行。

好了，废话就不多说了。

总之，在职场总有人不需要加班就能完成老板布置的工作任务，那么**那个人为什么不能是你？**

所以，今天整理的25个常用的Python代码段请果断收藏起来，如果觉得足够好用记得分享给你身边的朋友和同事哟～

1交换两个变量的值

```
`num_1, num_2 = 666, 999``# 一行代码搞定交换两个变量的值``num_1, num_2 = num_2, num_1``print(num_1, num_2)``输出：``999 666``Process finished with exit code 0`
```

2查找对象使用的内存

```
`import sys``slogan = "今天你学python了么？"``size = sys.getsizeof(slogan)``print(size)``输出：``100``Process finished with exit code 0`
```

3反转字符串

```
`slogan = "今天你学习python了么？"``# 一行代码搞定字符串的反转``new_slogan = slogan[::-1]``print(new_slogan)``输出：``？么了nohtyp习学你天今``Process finished with exit code 0`
```

4检查字符串是否为回文

```
`# 定义一个判断字符串是否是回文的函数``def is_palindrome(string):` `return string == string[::-1]``示例：调用判断函数来进行判断slogan是否是回文字符串``slogan = "今天你学python了么？"``_ = is_palindrome(slogan)``print(_)``输出：``False``Process finished with exit code 0`
```

5将字符串列表合并为单个字符串

```
`slogan = ["今", "天", "你", "学", "python", "了", "么", "？"]``# 一行代码搞定将字符串列表合并为单个字符串``real_slogan = "".join(slogan)``print(real_slogan)``输出：``今天你学python了么？``Process finished with exit code 0`
```

6查找存在于两个列表中任一列表存在的元素

```
`# 定义一个函数用来查找存在于两个列表中任一列表存在的元素``def union(list1, list2):` `return list(set(list1 + list2))``示例：调用该函数用来查找存在于两个列表中任一列表存在的元素``list1, list2 = [5, 2, 0], [5, 2, 1]``new_list = union(list1, list2)``print(new_list)``输出：``[0, 1, 2, 5]``Process finished with exit code 0`
```

7打印N次字符串

```
`slogan = "今天你学python了么？"``new_slogan = 11*slogan``print(new_slogan)``输出：``今天你学python了么？今天你学python了么？今天你学python了么？今天你学python了么？今天你学python了么？今天你学python了么？今天你学python了么？今天你学python了么？今天你学python了么？今天你学python了么？今天你学python了么？``Process finished with exit code 0`
```

8链式比较

```
`number = 100``print(98<number<102)``输出：``True``Process finished with exit code 0``print(100==number<102)``输出：``True``Process finished with exit code 0`
```

9单词大小写

```
`slogan = "python happy"``# 一行代码搞定单词大小写转换``print(slogan.upper())``# 一行代码搞定单词首字母大写``print(slogan.capitalize())``# 一行代码搞定将每个单词的首字母转为大写，其余小写``print(slogan.title())``输出：``PYTHON HAPPY``Python happy``Python Happy``Process finished with exit code 0`
```

10统计列表中元素的频率

```
`from collections import Counter``numbers = [1, 1, 3, 2, 4, 4, 3, 6]``# 一行代码搞定求列表中每个元素出现的频率``count = Counter(numbers)``print(count)``输出：``Counter({1: 2, 3: 2, 4: 2, 2: 1, 6: 1})``Process finished with exit code 0`
```

11判断字符串所含元素是否相同

```
`from collections import Counter``course = "python"``new_course = "ypthon"``count_1, count_2 = Counter(course), Counter(new_course)``if count_1 == count_2:` `print("两个字符串所含元素相同！")``输出：``两个字符串所含元素相同！``Process finished with exit code 0`
```

12将数字字符串转化为数字列表

```
`string = "666888"``numbers = list(map(int, string))``print(numbers)``输出：``[6, 6, 6, 8, 8, 8]``Process finished with exit code 0`
```

13使用enumerate() 函数来获取索引-数值对

```
`string = "python"``for index, value in enumerate(string):` `print(index, value)``输出：``0 p``1 y``2 t``3 h``4 o``5 n``Process finished with exit code 0`
```

14代码执行消耗时间

```
`import time``start_time = time.time()``numbers = [i for i in range(10000)]``end_time = time.time()``time_consume = end_time - start_time``print("代码执行消耗的时间是：{}".format(time_consume))``输出示例：``代码执行消耗的时间是：0.002994537353515625``Process finished with exit code 0`
```

15比较集合和字典的查找效率

```
`import time``number = 999999``# 生成数字列表和数字集合``numbers = [i for i in range(1000000)]``digits = {i for i in range(1000000)}``start_time = time.time()``# 列表的查找``_ = number in numbers``end_time = time.time()``print("列表查找时间为：{}".format(end_time - start_time))``start_time = time.time()``# 集合的查找``_ = number in digits``end_time = time.time()``print("集合查找时间为：{}".format(end_time - start_time))``输出：``列表查找时间为：0.060904741287231445``集合查找时间为：0.0``Process finished with exit code 0`
```

16字典的合并

```
`info_1 = {"apple": 13, "orange": 22}``info_2 = {"爆款写作": 48, "跃迁": 49}``# 一行代码搞定合并两个字典``new_info = {**info_1, **info_2}``print(new_info)``输出：``{'apple': 13, 'orange': 22, '爆款写作': 48, '跃迁': 49}``Process finished with exit code 0`
```

17随机采样

```
`import random``books = ["爆款写作", "这个世界，偏爱会写作的人", "写作七堂课", "越书写越明白"]``# 随机取出2本书阅读``reading_book = random.sample(books, 2)``print(reading_book)``输出：``['这个世界，偏爱会写作的人', '越书写越明白']``Process finished with exit code 0`
```

18判断列表中元素的唯一性

```
`# 定义一个函数判断列表中元素的唯一性``def is_unique(list):` `if len(list) == len(set(list)):` `return True` `else:` `return False`  `# 调用该函数判断一个列表是否是唯一性的``numbers = [1, 2, 3, 3, 4, 5]``_ = is_unique(numbers)``print(_)``输出：``False``Process finished with exit code 0`
```

19计算阶乘 递归函数实现

```
`def fac(n):` `if n > 1:` `return n*fac(n-1)` `else:` `return 1``number = int(input("n="))``print("result = {}".format(fac(number)) )``输出：``n=5``result = 120``Process finished with exit code 0`
```

20列出当前目录下的所有文件和目录名

```
`import os``files = [file for file in os.listdir(".")]``print(files)``输出：``['.idea', '2048.py', 'access.log', 'beautiful_girls', 'beautiful_girls_photos', 'boy.py', 'cache.json', 'catoffice.py', 'cookie.json', 'data.csv', 'data.txt', 'diary', 'files', 'filtered_words.txt', 'geckodriver.log', 'get_movies_info2.py', 'girl.py', 'girl1.py', 'ha.conf', 'homework.py', 'homework3.py', 'index.html', 'info.ini', 'notepad.py', 'rent.csv', 'stock.txt', 'student.txt', 'student.xlsx', 'student_register_info.json', 'test.png', 'test_picture.txt', 'zhihu.html', '__pycache__', '九尾1997_200行代码实现2048小游戏.py', '九尾1997_python实现图片转字符画.py', '九尾1997_实现一个简单的计算器.py', '九尾1997_爬取优美图美女写真.py', '九尾1997_爬取北京58租房信息.py', '九尾1997_路飞学城注册页面', '校园管理系统.py', '爬虫模拟登录.py', '记事本.m4a']``Process finished with exit code 0`
```

21把原字典的键值对颠倒并生产新的字典

```
`dict_1 = {1: "python", 2: "java"}``new_dict = {value:key for key, value in dict_1.items()}``print(new_dict)``输出：``{'python': 1, 'java': 2}``Process finished with exit code 0`
```

22打印九九乘法表

```
`for i in range(1, 10):` `for j in range(1, i+1):` `print("{} * {} = {}".format(i, j, i*j), end="")` `print()``输出：``1 * 1 = 1` `2 * 1 = 2 2 * 2 = 4` `3 * 1 = 3 3 * 2 = 6 3 * 3 = 9` `4 * 1 = 4 4 * 2 = 8 4 * 3 = 12 4 * 4 = 16` `5 * 1 = 5 5 * 2 = 10 5 * 3 = 15 5 * 4 = 20 5 * 5 = 25` `6 * 1 = 6 6 * 2 = 12 6 * 3 = 18 6 * 4 = 24 6 * 5 = 30 6 * 6 = 36` `7 * 1 = 7 7 * 2 = 14 7 * 3 = 21 7 * 4 = 28 7 * 5 = 35 7 * 6 = 42 7 * 7 = 49` `8 * 1 = 8 8 * 2 = 16 8 * 3 = 24 8 * 4 = 32 8 * 5 = 40 8 * 6 = 48 8 * 7 = 56 8 * 8 = 64` `9 * 1 = 9 9 * 2 = 18 9 * 3 = 27 9 * 4 = 36 9 * 5 = 45 9 * 6 = 54 9 * 7 = 63 9 * 8 = 72 9 * 9 = 81` `Process finished with exit code 0`
```

23计算每个月天数

```
`import calendar``month_days = calendar.monthrange(2025,8)``print(month_days)``输出：``(4, 31)``Process finished with exit code 0`
```

24随机生成验证码，调用随机模块

```
`import random, string``str_1 = "0123456789"``# str_2 是包含所有字母的字符串``str_2 = string.ascii_letters``str_3 = str_1 + str_2``# 多个字符中选取特定数量的字符``verify_code = random.sample(str_3, 6)``# 使用join方法拼接转换为字符串``verify_code = "".join(verify_code)``print(verify_code)``输出：``Mk0L6Y``Process finished with exit code 0`
```

25判断闰年

```
`year  = input("请输入一个年份：")``year = int(year)``# 一行代码判断年份是否是闰年``if year % 4 == 0 and year % 100 != 0 or year % 400 == 0:` `print("{}是闰年！".format(year))``else:` `print("{}不是闰年！".format(year))``输出示例：``请输入一个年份：2000``2000是闰年！``Process finished with exit code 0`
```

996 一直是互联网老生常谈的话题了，但抛开其他只谈工作本身，你有没有想过，下班晚、加班，有时候可能是因为自己工作比较低效？

所以，平时多积累好用、常用、简洁的代码段真的非常有必要。

再次强调一下，如果觉得这些代码段有帮助，请一定要收藏起来，另外也不要忘了分享给你身边的朋友和同事哟～

People who liked this content also liked

支持300+常用功能的开源GO语言工具函数库

...

GoCN

不看的原因

- 内容质量低
- 不看此公众号

像Python一样玩C/C++

...

光城

不看的原因

- 内容质量低
- 不看此公众号

只需一个文件，Python 实现迷你 Web 框架！

...

Python猫

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___decf25d1be6545e78.bmp"/>

Scan to Follow