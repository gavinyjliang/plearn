# 如何利用 pandas 批量合并 Excel？

刘早起 <a id="profileBt"></a><a id="js_name"></a>早起Python *2022-05-30 11:21* *Posted on <a id="js_ip_wording"></a>安徽*

收录于合集

<a id="js_article_tag_name__2217965627481325571"></a>#pandas <a id="js_article_tag_num__2217965627481325571"></a>2 <a id="js_article_tag_tips__2217965627481325571"></a>个

<a id="js_article_tag_name__2052771479909367813"></a>#pandas进阶修炼 <a id="js_article_tag_num__2052771479909367813"></a>10 <a id="js_article_tag_tips__2052771479909367813"></a>个

大家好，我是早起。

今天分享一个利用`Pandas`进行数据分析的小技巧，也是之前有粉丝在后台进行提问的，即如何将多个`pandas.dataframe`保存到同一个`Excel`中。

其实只需要灵活使用`pandas`中的`pd.ExcelWriter()`方法即可，还是以[300题](https://mp.weixin.qq.com/s?__biz=Mzg5OTU3NjczMQ==&mid=2247519402&idx=1&sn=535dddd429bf383a1eda9f0316507ec4&scene=21#wechat_redirect)中的数据为例。

假设现在我们有`df1 df2 df3`三个`dataframe`，需要将它们保存到同一个`Excel`的不同`sheet`中，只需要先创建一个`ExcelWriter`对象，然后不停写入就行

```
df1 = pd.read_csv('东京奥运会奖牌数据.csv')
df2 = pd.read_excel("TOP250.xlsx")
df3 = pd.read_excel("2020年中国大学排名.xlsx")
writer = pd.ExcelWriter('test.xlsx')
df1.to_excel(writer,sheet_name="df1",index=False)
df2.to_excel(writer,sheet_name="df2",index=False)
df3.to_excel(writer,sheet_name="df3",index=False)
writer.save()

```

是不是和常见的文件读写`with`方法类似，我们也可以使用同样的方法

```
with pd.ExcelWriter("test1.xlsx") as xlsxwriter:
    df1.to_excel(xlsxwriter,sheet_name="df1",index=False)
    df2.to_excel(xlsxwriter,sheet_name="df2",index=False)
    df3.to_excel(xlsxwriter,sheet_name="df3",index=False)

```

得到的结果是一样的，可以将多个`df`保存到一个Excel中
<img width="675" height="461" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__9a506995f2f844dca.png"/>

这个方法虽然简单好用，但是如果要保存的 `df` 太多了，一个一个手动去读取再手动去保存就显得十分麻烦，另外我们希望`sheet`是文件名，如果手动复制粘贴，就更麻烦了。

这时，[办公自动化](https://mp.weixin.qq.com/s?__biz=Mzg5OTU3NjczMQ==&mid=2247511900&idx=1&sn=3c5008f0b24d2f263400517e801f105f&scene=21#wechat_redirect)系列的文章就发挥作用了，我们先简单拿来一个小脚本「获取指定目录下的全部Excel文件名」

```
import os
def getfile(dirpath):
    
    filelist = []
    for root,dirs,files in os.walk(dirpath):
        for file in files:
            if file.endswith("xlsx") or file.endswith("csv"):
                filelist.append(os.path.join(root,file)) 
    
    return filelist

```

执行一下，可以看到指定目录下的全部`Excel`文件名
![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

下面要做的，我想不用多说了「循环读取，自动保存」

```
filelist = getfile('/Users/liuzaoqi/Desktop/zaoqi/2022公众号文章/如何保存多个df')
writer = pd.ExcelWriter('test.xlsx')
for file in filelist:
    if file.endswith("xlsx"):
        df = pd.read_excel(file)
    else:
        df = pd.read_csv(file)
    df.to_excel(writer,sheet_name=file.split('/')[-1].split('.')[0],index=False)
writer.save()

```

现在，当前目录下的全部Excel就自动合并到一个`Excel`中的不同`sheet`中，并且sheet名是对应的文件名
![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

如果你对本文的内容感兴趣，不妨拿走代码试一下，如果你还有`pandas`相关问题，欢迎在评论区留言。

  文末赠书  

文末推荐一本**《人工智能数学基础》**，本书（1）没有高深理论，每章都以实例为主，读者参考书中源码运行，就能得到与书中一样的结果。（2）专注于Python数据分析与可视化操作中实际用到的技术。相比大而全的书籍资料，本书能让读者尽快上手，开始项目开发。（3）书中的“新手问答”和“小试牛刀”栏目能让读者巩固知识，举一反三，学以致用

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

最后还是老规矩，留言送 **3** 本，点赞前三将包邮获赠本书（时间截止6.7日，仅限2022年未获得过赠书的粉丝参与）

<a id="js_view_source"></a>Read more

People who liked this content also liked

【办公自动化】将excel自动的填入网站中

...

程序员哈利路

不看的原因

- 内容质量低
- 不看此公众号

如何用一个技巧提升pandas读取excel文件效率？

...

Python教程初学详解

不看的原因

- 内容质量低
- 不看此公众号

Excel与word交互（4）

...

VBA爱好者

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___b7d6745d10bc4aa6a.bmp"/>

Scan to Follow