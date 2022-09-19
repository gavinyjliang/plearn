# Python中JSON结构数据的高效增删改操作

<a id="profileBt"></a><a id="js_name"></a>Python数据之道 *2021-08-23 11:01*

The following article is from Python大数据分析 Author 费弗里

<a id="copyright_info"></a>[![](../../../_resources/0_24d996f152034679bdd83a50b9c7b211.jpg)<br>**Python大数据分析** .<br>分享python编程、可视化设计、大数据分析、机器学习等技术以及数据分析案例，包括但不限于pandas、numpy、spark、matplotlib、sklearn、tensorflow、keras、tableau等](#)

![](../../../_resources/0_wx_fmt_png_dfcf3803721149f4b7dcae40c64708af.png)

**Python数据之道**

点击领取《Python知识手册》高清电子版，回复数字 “600” 获取。「Python数据之道」秉承“让数据更有价值”的理念​，聚焦于 Python 数据分析、数据可视化、AI、机器学习、深度学习等领域。

<a id="js_profile_article"></a>240篇原创内容

Official Account

来源：Python大数据分析

1 简介

在今天的文章中，我就将带大家学习更加高级的`JSON`数据处理方式。

<img width="677" height="372" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_3f8dd7b781aa470fa.jpg"/>

# 2 基于jsonpath-ng的进阶JSON数据处理方法

`jsonpath-ng`是一个功能强大的`Python`库，它整合了`jsonpath-rw`、`jsonpath-rw-ext`等第三方`JSONPath`拓展库的实用功能，使得我们可以基于`JSONPath`语法，实现更多操纵`JSON`数据的功能，而不只是查询数据而已，使用`pip install jsonpath-ng`进行安装：

## 2.1 JSON数据的增删改

`jsonpath-ng`中设计了一些方法，可以帮助我们实现对现有`JSON`数据的增删改操作，首先我们来学习`jsonpath-ng`中如何定义`JSONPath`模式，并将其运用到对数据的匹配上，依然以上篇文章的数据为例：

```
`import json
from jsonpath_ng import parse
# 读入示例json数据
with open('json示例.json', encoding='utf-8') as j:
    demo_json = json.loads(j.read())
    
# 构造指定JSONPath模式对应的解析器
parser = parse('$..paths..steps[*].duration')
# 利用解析器的find方法找到目标数据中所有满足条件的节点
matches = parser.find(demo_json)
# 利用value属性取得对应匹配结果的值
matches[0].value
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

而基于上面产生的一些对象我们就可以实现对`JSON`数据的增删改：

### 2.1.1 对JSON数据进行增操作

在`jsonpath-ng`中对`JSON`数据添加节点，思想是先构造对**「原先不存在」**的节点进行匹配的解析器对象，利用`find_or_create`方法处理原始`JSON`数据：

```
`# 构造示例数据
demo_json = {
    'level1': [
        {
            'level2': {}
        },
        {
            'level2': {
                'level3': 12
            }
        }
    ]
}
# 构造规则解释器，所有除去最后一层节点规则外可以匹配到的节点
# 都属于合法匹配结果，会在匹配结果列表中出现
parser = parse('level1[*].level2.level3')
matches = parser.find_or_create(demo_json)
demo_json
`
```

在`find_or_create`操作之后，`demo_json`就被修改成下面的结果：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

接下来的事情就很简单了，只需要在`matches`结果中进行遍历，遇到`value`属性为`{}`的，就运用`full_path.update_or_create()`方法对原始`JSON`数据进行更新即可，比如这里我们填充999：

```
`for match in matches:
    if match.value == {}:
        # 更新原始输入的JSON数据
        match.full_path.update_or_create(demo_json, 999)
demo_json
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 2.1.2  对JSON数据进行删操作

当我们希望对`JSON`数据中指定`JSONPath`规则的节点予以删除时，可以使用到`parse`对象的`filter()`方法传入`lambda`函数，在`lambda`函数中进行条件判断，返回的即为删除指定节点之后的输入数据。

以上一步**「增」**操作后得到的`demo_json`为例，我们来对其`level1[*].level2.level3`值为999的予以过滤：

```
`parser = parse('level1[*].level2.level3')
# 过滤 level1[*].level2.level3 规则下值为 999 的节点
parser.filter(lambda x: x == 999, demo_json)
demo_json
`
```

可以看到结果正是我们所预期的：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 2.1.3 对JSON数据进行改操作

对`JSON`数据中的指定节点进行改操作非常的简单，只需要使用`parse`对象的`update`或`update_or_create`方法即可，使用效果的区别如下所示，轻轻松松就可以完成两种策略下的节点更新操作😋：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

`jsonpath-ng`中还有一些丰富的功能，这里就不再赘述，感兴趣的读者朋友可以前往`https://github.com/h2non/jsonpath-ng`查看。

* * *

以上就是本文的全部内容，欢迎在评论区与我进行讨论~

**---------End---------**

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 精选资料

回复关键词，获取对应的资料：

| 关键词 | 资料名称 |
| --- | --- |
| **600** | [《Python知识手册》](https://mp.weixin.qq.com/mp/appmsgalbum?__biz=MzI2NjY5NzI0NA==&action=getalbum&album_id=1370549534602133504#wechat_redirect) |
| **md** | [《Markdown速查表》](https://mp.weixin.qq.com/s?__biz=MzI2NjY5NzI0NA==&mid=2247495631&idx=1&sn=720c931b1f5cdb82ceccd9b34b182a95&scene=21#wechat_redirect) |
| **time** | [《Python时间使用指南》](https://mp.weixin.qq.com/s?__biz=MzI2NjY5NzI0NA==&mid=2247492370&idx=1&sn=1dc6b3edef0fcb241d07757fb9e2ae03&scene=21#wechat_redirect) |
| **str** | [《Python字符串速查表》](https://mp.weixin.qq.com/s?__biz=MzI2NjY5NzI0NA==&mid=2247496716&idx=1&sn=8ec7a6b373059fa49f04433990581aa6&scene=21#wechat_redirect) |
| **pip** | [《Python：Pip速查表》](https://mp.weixin.qq.com/s?__biz=MzI2NjY5NzI0NA==&mid=2247500405&idx=1&sn=c5a760279babd1075c6153858af84af8&scene=21#wechat_redirect) |
| **style** | [《Pandas表格样式配置指南》](https://mp.weixin.qq.com/s?__biz=MzI2NjY5NzI0NA==&mid=2247501026&idx=1&sn=378292e5435b7ef5eede36192812da3b&scene=21#wechat_redirect) |
| **mat** | [《Matplotlib入门100个案例》](https://mp.weixin.qq.com/s?__biz=MzI2NjY5NzI0NA==&mid=2247493274&idx=1&sn=323662b49f3b8e0d619d44315537a76a&scene=21#wechat_redirect) |
| **px** | [《Plotly Express可视化指南》](https://mp.weixin.qq.com/s?__biz=MzI2NjY5NzI0NA==&mid=2247501349&idx=1&sn=04502491758816e83d43525e911cce56&scene=21#wechat_redirect) |

### 精选内容

- [神器 VS Code，超详细Python配置使用指南](https://mp.weixin.qq.com/s?__biz=MzI2NjY5NzI0NA==&mid=2247495918&idx=1&sn=6b06eadc1604b693f48a127e7b5986a4&scene=21#wechat_redirect)
    
- [神器Tushare，财经数据必备工具！](https://mp.weixin.qq.com/s?__biz=MzI2NjY5NzI0NA==&mid=2247497358&idx=1&sn=e9574b69ca3c05142edd534e438e855e&scene=21#wechat_redirect)
    
- [Matplotlib 可视化最有价值的 50 个图表](https://mp.weixin.qq.com/s?__biz=MzI2NjY5NzI0NA==&mid=2247485238&idx=1&sn=f2d6b9136ff94697bdce9c9f48397e78&scene=21#wechat_redirect)
    
- [视频：Plotly 和 Dash 在投资领域的应用](https://mp.weixin.qq.com/s?__biz=MzI2NjY5NzI0NA==&mid=2247496637&idx=1&sn=91910ea5f9034fc6685d28ef0f00a70c&scene=21#wechat_redirect)
    

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

People who liked this content also liked

Matplotlib 可视化必备神书，附pdf下载

...

Python数据之道

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___2d5ea194d40b403fa.bmp"/>

Scan to Follow