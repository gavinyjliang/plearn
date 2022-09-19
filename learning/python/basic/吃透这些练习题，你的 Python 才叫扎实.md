# 吃透这些练习题，你的 Python 才叫扎实

<a id="profileBt"></a><a id="js_name"></a>Python编程时光 *2022-06-01 09:02* *Posted on <a id="js_ip_wording"></a>福建*

python入门简单，当我们学完基础知识后，最好的巩固方法就是拿一些练习题练手，综合所学内容，达到输入输出 纠正的完美学习路径。

这里给大家分享一份大佬整理的150道python练习题，涵盖Python基础内容的方方面面。非常经典。

内容包括：**数据类型、基础语法、内置函数、字符串方法、排序算法、简单算法、中等难度算法、地狱级难度算法。每道题均有详细的题目要求；示例代码；难能可贵的是附有详细的解题思路分析，以及所涉及可能会让新手困惑的知识点详解。**建议先不看答案，运用自己所学到的知识跑一遍代码，然后再来参照解析。达到最佳练习效果！

* * *

部分习题展示，完整版见文末：

- **【数据类型】**不运行程序，说出下方程序运行结果：
    

```
`4.0 == 4``"4.0" == 4``bool("1")``bool("0")``str(32)``int(6.26)``float(32)``float("3.21")``int("434")``int("3.42")``bool(-1)``bool("")``bool(0)``"wrqq" > "acd"``"ttt" == "ttt "``"sd"*3``"wer" + "2322"`
```

- **【字符串】**不用代码，口述下方代码执行结果
    

```
`string="Python is good"``string[1:20]``string[20]``string[3:-4]``string[-10:-3]``string.lower()``string.replace("o","0")``string.startswith("python")``string.split()``len(string)``string[30]``string.replace("",")`
```

- **【简单算法】**打印杨辉三角。给定一个正整数N，打印杨辉三角的前N行。杨辉三角形态如下：
    

```
 `1` `1 1` `1 2 1``   1 3 3 1` `1 4 6 4 1``1 5 10 10 5 1`
```

杨辉三角的每一行第一个和最后一个元素都是1；中间的元素，由上一行的两个元素相加得到；第N行的第index元素，是由第N-1行的第index-1元素和第index相加得到的。

* * *

- **【简单算法】**已知两个列表
    

```
`lst_1 = [1, 2, 3, 4]``lst_2 = ['a', 'b', 'c', 'd']`
```

请写算法，将两个列表交叉相乘，生成如下的矩阵

```
`['1a', '2a', '3a', '4a'],``['1b', '2b', '3b', '4b'],``['1c', '2c', '3c', '4c'],``['1d', '2d', '3d', '4d']`
```

- **【简单算法】**求三位数组合
    

这四个数字能组成多少个互不相同且无重复数字的三位数？请逐个输出

```
lst = [3, 6, 2, 7]
```

- **【排序】选择排序**
    

假设有一个序列，a\[0\] , a\[1\] , a\[2\]...a\[n\] ，现在对它进行排序。我们先从0这个位置找出最小值，然后将这个最小值与a\[0\] 交换，然后a\[1\]到a\[n\]就是我们接下来要排序的序列。

我们可以从1这个位置到n这个位置找出最小值，然后将这个最小值与a\[1\]交换，之后a\[2\]到a\[n\]就是我们接下来要排序的序列。每一次我们都从序列中找出一个最小值，然后把它与序列的第一个元素交换位置，这样下去，待排序的元素就会越来越少吗，直到最后一个

```
`def select_sort(lst):` `for i in range(len(lst)):``        min = i` `for j in range(min,len(lst)):` `# 寻找min 到len(lst)-1 这个范围内的最⼩小值` `if lst[min] > lst[j]:` `min = j``        lst[i], lst[min] = lst[min], lst[i]``lst = [2,6,1,8,2,4,9]``select_sort(lst)``print lst`
```

- **【中等难度算法】**将下方给定的字符串中的每个单词逐个翻转。翻转后，空格不能减少，单词之间的空格数量不能发生变化。
    

```
`输入: " the sky is  blue",``输出: "blue  is sky the "`
```

如果只是简单的翻转字符串，就过于简单了，因此要求翻转每一个单词，单词还是原来的样子，但是单词所在的位置却发生了翻转，第一个单词变成了倒数第一个单词。字符串是不可变对象，不能直接在字符串上进行翻转，要借助列表（list）进行翻转

- **【中等难度算法】**给定一个整数，请计算二进制中为1的位数
    

```
 13
```

如果一个数是奇数，那么它的二级制的最后一位一定是1，道理很简单，其他的位都表示2<sup>n </sup>只有最后一位表示2<sup>0</sup>。我们可以利用最后一位是否为1来统计为1的位数，这就需要最后一位是变化的，还好，我们可以利用用位运算符>>（右移位运算符）

13的二进制表示是1101，13>>1就表示二进制的每一位都向右移动一位，移动后为110，最右边的1舍弃。如果二进制最后一位是1，那么一定是奇数。

- **【地狱难度算法】**已知一个有序序列，请原地删除序列中重复出现的元素，返回删除重复元素后的序列长度。只能使用o(1)额外空间来完成这个任务，例如\[0,0,1,1,1,2,2,3,3,4,4,4,5\]，最终返回的长度是6，序列前6个元素是0 1 2 3 4 5
    

* * *

PDF文档预览：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

**篇幅有限，更多内容
**

**扫描下方二维码即可领取完整版**

**备注****“150练习题****”**

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

我会第一时间将资料发送给你

People who liked this content also liked

Python的可等待对象在Asyncio的作用

...

Python编程时光

不看的原因

- 内容质量低
- 不看此公众号

Python 是最受欢迎的语言？名不副实？

...

Python编程

不看的原因

- 内容质量低
- 不看此公众号

使用Python和YOLO检测车牌

...

小白学视觉

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___47687612e9cf4b529.bmp"/>

Scan to Follow