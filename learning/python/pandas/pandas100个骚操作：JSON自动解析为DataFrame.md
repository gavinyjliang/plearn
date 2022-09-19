# pandas100个骚操作：JSON自动解析为DataFrame

<a id="copyright_logo"></a>Original 东哥起飞 <a id="profileBt"></a><a id="js_name"></a>Python数据科学 *2021-01-18 13:55*

<a id="js_article-tag-card__left"></a>收录于合集 #pandas骚操作 <a id="js_article-tag-card__right"></a>25个

关注上方“Python数据科学”，选择星标，

关键时间，第一时间送达！

<img width="562" height="285" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__7d499ab45603439bb.png"/>

来源：Python数据科学

作者：东哥起飞

大家好，我是你们的东哥。

本篇是pandas100个骚操作的第2篇：**JSON自动解析为DataFrame**

全部内容请点文章头部的「**pandas100个骚操作**」系列查看。

* * *

调用API和文档数据库会返回嵌套的JSON对象，当我们使用Python尝试将嵌套结构中的键转换为列时，数据加载到pandas中往往会得到如下结果：

```


```


df = pd.DataFrame.from_records（results [“ issues”]，columns = [“ key”，“ fields”]）


```


```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

说明：这里results是一个大的字典，issues是results其中的一个键，issues的值为一个嵌套JSON对象字典的列表，后面会看到JSON嵌套结构。

问题在于API返回了嵌套的JSON结构，而我们关心的键在对象中确处于不同级别。

嵌套的JSON结构张成这样的。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

而我们想要的是下面这样的。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

下面以一个API返回的数据为例，API通常包含有关字段的元数据。假设下面这些是我们想要的字段。

- key：JSON密钥，在第一级的位置。
    
- summary：第二级的“字段”对象。
    
- status name：第三级位置。
    
- statusCategory name：位于第4个嵌套级别。
    

如上，我们选择要提取的字段在issues列表内的JSON结构中分别处于4个不同的嵌套级别，一环扣一环。

```


{
  "expand": "schema,names",
  "issues": [
    {
      "fields": {
        "issuetype": {
          "avatarId": 10300,
          "description": "",
          "id": "10005",
          "name": "New Feature",
          "subtask": False
        },
        "status": {
          "description": "A resolution has been taken, and it is awaiting verification by reporter. From here issues are either reopened, or are closed.",
          "id": "5",
          "name": "Resolved",
          "statusCategory": {
            "colorName": "green",
            "id": 3,
            "key": "done",
            "name": "Done",
          }
        },
        "summary": "Recovered data collection Defraglar $MFT problem"
      },
      "id": "11861",
      "key": "CAE-160",
    },
    {
      "fields": { 
... more issues],
  "maxResults": 5,
  "startAt": 0,
  "total": 160
}


```

# 

# **一个不太好的解决方案**

一种选择是直接撸码，写一个查找特定字段的函数，但问题是必须对每个嵌套字段调用此函数，然后再调用.apply到DataFrame中的新列。

为获取我们想要的几个字段，首先我们提取fields键内的对象至列：

```


df = (
    df["fields"]
    .apply(pd.Series)
    .merge(df, left_index=True, right_index = True)
)


```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

从上表看出，只有summary是可用的，issuetype、status等仍然埋在嵌套对象中。

下面是提取issuetype中的name的一种方法。

```


# 提取issue type的name到一个新列叫"issue_type"
df_issue_type = (
    df["issuetype"]
    .apply(pd.Series)
    .rename(columns={"name": "issue_type_name"})["issue_type_name"]
)
df = df.assign(issue_type_name = df_issue_type)


```

```


![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)



```

像上面这样，如果嵌套层级特别多，就需要自己手撸一个递归来实现了，因为每层嵌套都需要调用一个像上面解析并添加到新列的方法。

对于编程基础薄弱的朋友，手撸一个其实还挺麻烦的，尤其是对于数据分析师，着急想用数据的时候，希望可以快速拿到结构化的数据进行分析。

下面东哥分享一个pandas的内置解决方案。

# **内置的解决方案**

pandas中有一个牛逼的内置功能叫 .json_normalize。

pandas的文档中提到：**将半结构化JSON数据规范化为平面表。**

前面方案的所有代码，用这个内置功能仅需要3行就可搞定。步骤很简单，懂了下面几个用法即可。

- 确定我们要想的字段，使用 . 符号连接嵌套对象。
    
- 将想要处理的嵌套列表（这里是results\["issues"\]）作为参数放进 .json_normalize 中。
    
- 过滤我们定义的FIELDS列表。
    

```


FIELDS = ["key", "fields.summary", "fields.issuetype.name", "fields.status.name", "fields.status.statusCategory.name"]
df = pd.json_normalize(results["issues"])
df[FIELDS]


```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

# 没错，就这么简单。

# **其它操作**

## **记录路径**

除了像上面那样传递results\["issues"\]列表之外，我们还使用record_path参数在JSON对象中指定列表的路径。

```


```


# 使用路径而不是直接用results["issues"]
pd.json_normalize(results, record_path="issues")[FIELDS]


```


```

## **自定义分隔符**

还可以使用sep参数自定义嵌套结构连接的分隔符，比如下面将默认的“.”替换“-”。

```


# 用 "-" 替换默认的 "."
FIELDS = ["key", "fields-summary", "fields-issuetype-name", "fields-status-name", "fields-status-statusCategory-name"]
pd.json_normalize(results["issues"], sep = "-")[FIELDS]


```

## **控制递归**

如果不想递归到每个子对象，可以使用max_level参数控制深度。在这种情况下，由于statusCategory.name字段位于JSON对象的第4级，因此不会包含在结果DataFrame中。

```


```


# 只深入到嵌套第二级
pd.json_normalize(results, record_path="issues", max_level = 2)


```


```

下面是.json_normalize的pandas官方文档说明，如有不明白可自行学习，本次东哥就介绍到这里。

参考：

\[1\] pandas官方文档：https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.json_normalize.html

\[2\] https://medium.com/swlh/converting-nested-json-structures-to-pandas-dataframes-e8106c59976e

以上所有代码已上传至我的GitHub：

**项目链接：**https://github.com/xiaoyusmd/PythonDataScience

**原创不易，如果觉得有帮助，支持下给个Star****![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)**

```


![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg== "音符")

我是东哥，最后给大家分享《100本Python电子书》，包括Python编程技巧、数据分析、爬虫、Web开发、机器学习、深度学习。

现在免费分享出来，有需要的读者可以下载学习，在下面的公众号「GitHuboy」里回复关键字：Python，就行。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

```


```


# 

```


爱点赞的人，运气都不会太差![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)


```




```


```




```

收录于合集 #<a id="js_album_keep_read_title"></a>pandas骚操作

 <a id="js_album_keep_read_size"></a>25个

<a id="js_album_prev"></a>上一篇 <a id="js_album_keep_read_pre_title"></a>pandas100个骚操作：再见 for 循环！速度提升315倍！ <a id="js_album_next"></a>下一篇 <a id="js_album_keep_read_next_title"></a>pandas100个骚操作：用 pandas 快速爬数据

<a id="js_view_source"></a>Read more

People who liked this content also liked

超强图解pandas，建议收藏(文末赠书)

...

Python数据科学

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___ec5e6071ba1743bbb.bmp"/>

Scan to Follow