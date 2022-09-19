# 如何用 Python 发送通知到微信？

<a id="profileBt"></a><a id="js_name"></a>AI科技大本营 *2022-04-12 18:33*

<img width="661" height="117" src="../../../_resources/640_wx_fmt_gif_wxfrom_5_wx_lazy__ed9dfdb9700940698.gif"/>

来源丨CSDN博客

**<img width="661" height="70" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__c7ada1796c9e41168.png"/>**

## **通知方式有哪些？**

常见的通知方式有：邮件，电话，短信，微信。短信和电话：通常是收费的，较少使用；邮件：适合带文件类型的通知，较正式，存档使用；微信：适合告警类型通知，较方便。这里说的微信，是企业微信。

本文目的：通过企业微信应用给企业成员发消息。

**<img width="661" height="70" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__de5424312aad4d939.png"/>**

## **如何实现企业微信通知？**

### 

### **1、新建应用**

登陆网页版企业微信 (https://work.weixin.qq.com)，点击 应用管理 → 应用 → 创建应用

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

上传应用的 logo，输入应用名称（债券打新），再选择可见范围，成功创建一个告警应用

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### **2、获取Secret**

使用 Python 发送告警请求，其实就只使用到两个接口：

**获取 Token ：**

https://qyapi.weixin.qq.com/cgi-bin/gettoken?corpid={corpid}&corpsecret={secret}

**发送请求：**

https://qyapi.weixin.qq.com/cgi-bin/message/send?access_token={token}

可以看到，最重要的是 corpid 和 secret：

corpid：唯一标识你的企业

secret：应用级的密钥，有了它程序才知道你要发送该企业的哪个应用

corpid 可以通过 我的企业 → 企业信息 → 企业id 获取

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

secret 可以通过 点击 新创建的应用（债券打新） → 查看 secret → 发送 来获取

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

最后将 corpid 和 secret 填入下面的常量中。

### 3、代码实现

```
import json
import time
import requests
'''
本文件主要实现通过企业微信应用给企业成员发消息
'''
CORP_ID = "xxxx"
SECRET = "xxxx"
class WeChatPub:
    s = requests.session()
    def __init__(self):
        self.token = self.get_token()
    def get_token(self):
        url = f"https://qyapi.weixin.qq.com/cgi-bin/gettoken?corpid={CORP_ID}&corpsecret={SECRET}"
        rep = self.s.get(url)
        if rep.status_code != 200:
            print("request failed.")
            return
        return json.loads(rep.content)['access_token']
    def send_msg(self, content):
        url = "https://qyapi.weixin.qq.com/cgi-bin/message/send?access_token=" + self.token
        header = {
            "Content-Type": "application/json"
        }
        form_data = {
            "touser": "FengXianMei",#接收人
            "toparty": "1",#接收部门
            "totag": " TagID1 | TagID2 ",#通讯录标签id
            "msgtype": "textcard",
            "agentid": 1000002,#应用ID
            "textcard": {
                "title": "债券打新提醒",
                "description": content,
                "url": "URL",
                "btntxt": "更多"
            },
            "safe": 0
        }
        rep = self.s.post(url, data=json.dumps(form_data).encode('utf-8'), headers=header)
        if rep.status_code != 200:
            print("request failed.")
            return
        return json.loads(rep.content)
if __name__ == "__main__":
    wechat = WeChatPub()
    timenow = time.strftime("%Y-%m-%d %H:%M:%S",time.localtime())
    wechat.send_msg(f"<div class=\"gray\">{timenow}</div> <div class=\"normal\">注意！</div><div class=\"highlight\">今日有新债，坚持打新！</div>")
    print('消息已发送！')

```

### 4、实现效果：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

大家学会了吗？可以应用起来呦~

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![](../../../_resources/0_wx_fmt_png_211eea9f4f6b4e4a8fc8009889114c88.png)

**AI科技大本营**

为AI领域从业者提供人工智能领域热点报道和海量重磅访谈；面向技术人员，提供AI技术领域前沿研究进展和技术成长路线；面向垂直企业，实现行业应用与技术创新的对接。全方位触及人工智能时代，连接AI技术的创造者和使用者。

<a id="js_profile_article"></a>1427篇原创内容

Official Account

往

期

回

顾

技术

[YYDS！Python实现自动驾驶](http://mp.weixin.qq.com/s?__biz=Mzg4NDQwNTI0OQ==&mid=2247558656&idx=2&sn=fe88af86dfb2f3954d5f55df5da061fc&chksm=cfbb036ef8cc8a782e96d954f729ae0a730185f476a3f6d3ee450442f1afad04e1944ebd3ad4&scene=21#wechat_redirect)

资讯

[OpenAI发布DALL·E进化版，真香](http://mp.weixin.qq.com/s?__biz=Mzg4NDQwNTI0OQ==&mid=2247558625&idx=1&sn=4d37fc7ed4b08bb3df40f53cfb4fc33f&chksm=cfbb008ff8cc8999d5e6a426d9e3cf9783821ff932d5d701a3387b32994685f7f28c2e77e4b7&scene=21#wechat_redirect)

技术

[如何 “干掉” ERP?](http://mp.weixin.qq.com/s?__biz=Mzg4NDQwNTI0OQ==&mid=2247558687&idx=1&sn=7e3f688ddc0514de45e4418ec2e0fd86&chksm=cfbb0371f8cc8a6766396e3fefb32c50f2bdcc262a5c71cc0b7e006689b493c68feb02db96de&scene=21#wechat_redirect)

技术

[用Python打造一个语音合成系统](http://mp.weixin.qq.com/s?__biz=Mzg4NDQwNTI0OQ==&mid=2247558399&idx=1&sn=f7591eaea3f790e6fe7e06e5675f847f&chksm=cfbb0191f8cc88873e2ecdec59b33deb52555e4538fdea7be61ed3cac7c612ac5122428c3c74&scene=21#wechat_redirect)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

**分享**

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

**点收藏**

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

**点点赞**

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

**点在看**

People who liked this content also liked

windows下利用cmd开启3389端口

...

天驿安全

不看的原因

- 内容质量低
- 不看此公众号

Linux 新系统正式发布，易用性开始向 Windows 看齐

...

Python丹卿

不看的原因

- 内容质量低
- 不看此公众号

用 Python 爬了微信好友，原来他们是这样的人...

...

Python社区

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___c342db0e380049bc9.bmp"/>

Scan to Follow