# 如何用Python获取接口响应时间?elapsed方法来帮你！

<a id="copyright_logo"></a>Original <a id="js_author_name"></a>清菡 <a id="profileBt"></a><a id="js_name"></a>清菡软件测试 *2022-03-12 17:10*

<a id="js_article-tag-card__left"></a>收录于话题 #面经 <a id="js_article-tag-card__right"></a>8个

# 目录

- 1.查询A表中100条数据，查出其中性别是女，名字为张飞的人，根据工资做个倒序排序。
    
- 2.同时更新多条数据，怎么写sql
    
- 3.测试计划和测试方案是什么区别
    
- 4.如何用Python获取接口响应时间
    

- 1)获取响应时间（举个栗子）
    
- 2)timeout超时
    

- 5.如何搭建测试环境
    

## 1.查询A表中100条数据，查出其中性别是女，名字为张飞的人，根据工资做个倒序排序。

```
select * from a WHERE sex='女' AND name='张飞' ORDER BY salary DESC LIMIT 100

```

结果：

<img width="657" height="541" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__733594f3e3614fd7a.png"/>

## 2.同时更新多条数据，怎么写sql？

```
UPDATE A set m='读书',sex='女',salary=20000 ,age=27 ,name='张三',id=6  WHERE lik='英语'

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

运行成功

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

表中查看运行结果

参考链接：https://mbd.baidu.com/ma/s/kdHO1KQA

## 3.测试计划和测试方案是什么区别？

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

图片来自网络

## 4.如何用Python获取接口响应时间？

requests发请求时，接口的响应时间，也是我们需要关注的一个点，如果响应时间太长，显然是不合理的。

当然，如果服务端没及时响应，也不能一直等着，可以设置一个timeout超时的时间。

具体查看该博客：https://www.cnblogs.com/hls-code/p/14861813.html

`elapsed方法`：计算的是从发送请求到服务端响应回来的这段时间（也就是时间差），发送第一个数据到收到最后一个数据之间，这个时长不受响应内容的影响。

> elapsed方法：
> 
> total_seconds 总时长，单位秒
> 
> days 以天为单位
> 
> microseconds (>= 0 and less than 1 second) 获取微秒部分。大于或等于0，小于1秒
> 
> seconds Number of seconds (>= 0 and less than 1 day) 秒，大于或等于0，小于1天
> 
> max = datetime.timedelta(999999999, 86399, 999999) 最大时间
> 
> min = datetime.timedelta(-999999999) 最小时间
> 
> resolution = datetime.timedelta(0, 0, 1) 最小时间单位

**所以，获取响应时间是**：`r.elapsed.total_seconds()`  单位秒

### 1)获取响应时间（举个栗子）：

```
import requests
r = requests.get("http://www.baidu.com")
print(r.elapsed)
print(r.elapsed.total_seconds())
print(r.elapsed.microseconds)
print(r.elapsed.seconds)
print(r.elapsed.days)
print(r.elapsed.max)
print(r.elapsed.min)
print(r.elapsed.resolution)

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

运行结果

### 2)timeout超时

1、如果一个请求响应时间比较长，不能一直等着，可以设置一个超时时间，让它抛出异常。

2、如下请求，设置超时为1s，那么就会抛出这个异常：requests.exceptions.ReadTimeout: HTTPConnectionPool

3、如下请求，设置超时为0.15s，那么就会抛出这个异常：requests.exceptions.ConnectTimeout: HTTPConnectionPool

```
import requests
r = requests.get("http://cn.python-requests.org/zh_CN/latest/", timeout=1)
print(r.elapsed)
print(r.elapsed.total_seconds())
print(r.elapsed.microseconds)

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

运行结果

```
import requests
r = requests.get("http://www.baidu.com",timeout=0.15)
print(r.elapsed)
print(r.elapsed.total_seconds())

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

运行结果

参考链接：https://www.cnblogs.com/hls-code/p/15182011.html

## 5.如何搭建测试环境？

测试环境=硬件+软件+网络+数据准备+测试工具

硬件：指测试必需的服务器、客户端、网络连接等辅助设备。

软件：指测试软件运行时的操作系统、数据库及其他应用软件。

网络：指被测软件运行时的网络系统、网络结构以及其他网络设备构成的环境等。

数据准备：一般指测试数据的准备。测试数据会在测试用例设计的阶段设计好，然后软件运行的时候，作为软件输入去验证软件功能。如果是少量、正常的测试数据，可以直接通过手动方式模拟出来，如果是大量的用户数据的模拟，可以借助测试工具来构建。

测试工具：工具是辅助测试的好帮手，针对将要做的测试类型，可选择合适的工具让我们的测试事半功倍。比如接口测试，可以选择Jmeter或者postman；抓包工具，可以选择fiddler，wireshark等。

### 主要操作步骤有以下几项：

1）安装软件，如tomcat、jdk、mysql等；

2）上传项目包，如war包，放到tomcat的webapps目录下，解压war包的命令：unzip xxx.war；

3）修改配置，根据文档中说明修改tomcat、数据库等配置信息，项目的配置文件一般在项目名/WEB-INF/classes/这个目录下；

4）启动数据库，一般开发会给出初始化sql脚本；

5）重启tomcat服务。

6）查询相应的进程：ps -ef | grep tomcat7

7）杀掉进程：kill 进程编号。

8）重启tomcat：执行tomcat/bin下的./shutup.sh停止，再输入./startup.sh重新启动。

参考链接：https://mbd.baidu.com/ma/s/Rsjj7scx

晕晕乎乎，估计坑不少，到时候操作一遍就知道了，哈哈哈哈。

* * *

注：文章中的链接是本人整理过来的，皆来自网络。链接中的文章版权皆归原作者所有。除标明**“图片来自网络”**的图片，其它图片皆为小编本人所画。计算机知识都一样，文章是小编整理的。如有雷同，纯属巧合。

![](../../../_resources/0_wx_fmt_png_35e72e87847c426fb65633de6791ede7.png)

**清菡软件测试**

专注于自动化、开发等软件测试技术分享。

<a id="js_profile_article"></a>163篇原创内容

Official Account

公众号 清菡软件测试 首发，更多原创文章：**清菡软件测试 158+** 原创文章，欢迎关注、交流，禁止第三方擅自转载。如有转载，请标明出处。

收录于话题 #<a id="js_album_keep_read_title"></a>面经

 <a id="js_album_keep_read_size"></a>8个

<a id="js_album_next"></a>下一篇 <a id="js_album_keep_read_next_title"></a>小白必看：Python中json.load()和json.loads()方法有什么区别？傻傻分不清。

<a id="js_view_source"></a>Read more

People who liked this content also liked

嵌入式初学者，应该如何学好 C 语言？

工程师进阶笔记

不看的原因

- 内容质量低
- 不看此公众号

前端竟然用Golang 动态生成图片？

前端技术江湖

不看的原因

- 内容质量低
- 不看此公众号

C函数指针别再停留在语法，得上升到软件设计~

嵌入式资讯精选

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___0903c6a82e094874b.bmp"/>

Scan to Follow