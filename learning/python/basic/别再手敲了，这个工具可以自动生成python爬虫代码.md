# 别再手敲了，这个工具可以自动生成python爬虫代码

<a id="copyright_logo"></a>Original <a id="js_author_name"></a>小dull鸟 <a id="profileBt"></a><a id="js_name"></a>python数据分析之禅 *2022-05-16 12:40* *Posted on <a id="js_ip_wording"></a>北京*

<a id="js_article-tag-card__left"></a>收录于合集 #爬虫 <a id="js_article-tag-card__right"></a>10个

点击上方“**Python数据分析之禅**”，后台分别回复“**福利1**”

可免费获取python数据分析视频教程

* * *

我们在写爬虫代码时，尝尝需要各种分析调试，而且每次直接用代码调试都很麻烦

所以今天给大家分享一个工具，不仅能方便模拟发送各种http请求，还能轻松调试，最重要的是，可以将调试最终结果自动转换成爬虫代码，它就是——***Postman***

postman以前是Chrome的插件，经过逐步演变，现在具备很好的夸平台性，完美支持MAC,Windows,Linux三大操作系统.不管你是哪种操作系统的用户,你都可以享受到Postman带来的便利

<img width="657" height="440" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_0057c511b63345778.jpg"/>

它还可以发送几乎所有类型的HTTP请求，可以在Postman界面里选择要发送的请求类型,接口地址,请求头信息以及向接口发送的入参.Postman完全是界面化的操作,非常直观.

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

当我们爬一些动态网页，或者测试一些接口时，只需勾选一些参数，就能测试出哪些参数是必须的，哪些参数是可以舍弃的

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

最后，我们可以将调试好的结果直接转换成我们需要的爬虫代码

测试完毕后，点击code

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

选择你需要的编程语言

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

以python为例，发现爬虫代码已自动生成，直接复制即可：

```
`import requests``url = "http://map.amap.com/service/subway"``querystring = {"_1599997789354":"","srhdata":"1100_drw_beijing.json"}``payload = ""``headers = {` `'cache-control': "no-cache",` `'Postman-Token': "74188fdc-2156-4fbf-a300-39c94c0b6a67"``    }``response = requests.request("GET", url, data=payload, headers=headers, params=querystring)``print(response.text)`
```

最后，**Postman安装包**已给大家准备好，请在后台回复**pos****t**获取。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

扫描二维码关注【python数据分析之禅】~

更多原创作品敬请期待！

```


万水千山总是情，点个 在看 行不行。




```

<a id="js_view_source"></a>Read more

People who liked this content also liked

像Python一样玩C/C++

...

光城

不看的原因

- 内容质量低
- 不看此公众号

我们公司使用了 6 年的Spring Boot 项目部署方案！打包 + Shell 脚本部署详解，稳的一批!

...

我是程序汪

不看的原因

- 内容质量低
- 不看此公众号

四行代码秒解微积分！Python这个模块神了！

...

快学Python

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___cb5d3072bdf145f8b.bmp"/>

Scan to Follow