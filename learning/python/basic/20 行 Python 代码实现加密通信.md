# 20 行 Python 代码实现加密通信

<a id="copyright_logo"></a>Original 巩庆奎 <a id="profileBt"></a><a id="js_name"></a>Python中文社区 *2022-03-26 22:06*

<img width="660" height="330" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_9d543f67eaf243b7a.jpg"/>

### 一、引言

网络上充满了窃听，我们的信息很容易被不怀好意的人获得，给我们造成不好的影响。如果你需要在网络上传输机密或者敏感的隐私信息，为了防备别有用心的人窃听，可能需要加密。而使用在线或者手机上的加密软件，可能不良软件更是泄露信息的温床。所以作为程序员的我们，完全可以自己来实现一个加密系统。

本文用 20 行 Python 代码来演示加密、解密、签名、验证的功能。大家依样画葫芦，不仅能理解加密技术，更能自己实现一套加密通信机制。

> 加密、解密建立在较高深的数学理论之上，不建议大家自己实现加密算法，直接调用相应库即可。

### 二、加密技术

加密技术我们这里演示两种，分别是对称加密和非对称加密。

讲解加密技术之前，我们需要假设下我们的使用场景，也是密码学常见的设定。

- Alice Bob是通信双方
    
- Eve是一个窃听者
    
- 传递的消息是PlainText
    
- 加密使用的秘钥key
    
- 加密后的密文是secret message
    

### 三、普通锁：简单的对称加密

对称加密：加密和解密双方使用同一个秘钥。比如这里， `key='1234567887654321'.encode('utf-8')`，这个 key 是 Alice 和 Bob 共同的密钥。当 Alice 发消息时，他需要如下操作完成加密。

1.  `from  Crypto.Cipher  import AES`
    
2.  `cryptor = AES.new(key, AES.MODE_ECB)`
    
3.  `secret = cryptor.encrypt(plain.encode('utf-8'))`
    
4.  `secret = b64encode(secret)`
    

- 第一行 导入了AES算法。AES 是对称加密的一种算法
    
- 第二行 新建加密器，key 是秘钥， `AES.MODE_ECB` 是信息填充模式
    
- 第三行 完成 encrypt 加密
    
- 第四行 加密后后的信息由 b64encode 编码后，发送给 Bob。
    

> HTTP 是文本协议，内容都是文本字符。想要对二进制文件进行传输，需要把它转化为文本，Base64代码就是用字符指代二进制的编码形式。

Bob 收到信息之后，进行如下解码、解密操作。

1.  `secret = b64decode(secret)`
    
2.  `plainText = cryptor.decrypt(secret).decode('utf-8')`
    

得到的 plainText 是 Alice 发来的明文信息。

注意，两个人用同一个秘钥来加密、解密。

现在我们先来解决一个小问题：网络经常丢包，导致 Alice 说话有时候缺头少尾，这该怎么办呢？

### 四、不可篡改的指纹：哈希函数

像人都有指纹一样，传递的消息也有自己的指纹。哈希函数用来找到消息的指纹。哈希函数也称为消息摘要函数，见名知意，是把一段内容提要出来，做成指纹。这个输出（指纹）很有特点：

- 不论输入多长，输出长度固定，输出看起来像乱码。
    
- 输入变一点，输出有很大不同。
    
- 消息可推出指纹，指纹推不出消息。
    

靠着以上特性，Alice 可以把消息哈希一下，把哈希值和消息都给 Bob。Bob 也把消息哈希一下，如果两个值一样，表明这句话内容完整，没有篡改和丢掉信息。

1.  `from hashlib import md5`
    
2.  `plainText =  'I love you!'`
    
3.  `hash_ = md5(plainText.encode('utf-8')).hexdigest()`
    

结果这样：`690a8cda8894e37a6fff4d1790d53b33`。如果 Bob 也对这条消息哈希，结果相同的话，说明这条信息完整。

现在我们再来解决一个大问题：对称加密如果秘钥遗失了，被坏人 Eve 获取之后，他完全可以窃听 Alice 和 Bob 之间的通信，甚至可以伪装成对方向另一方发送消息。

现在需要非对称加密登场了。

### 五、矛与盾：非对称加密

非对称加密，就是加密和解密秘钥不是一个，是一对。自己持有的称为私钥，交给对方的称为公钥。特点是：

- 公钥加密，私钥解密。
    
- 私钥加密，公钥解密。
    
- 私钥可推导出公钥，反之不行。
    

利用以上特点，我们可以实现安全的加密算法。首先 Bob 产生秘钥，并保存为文件。

1.  `import rsa`
    
2.  `Bob_pubkey,  Bob_privkey  = rsa.newkeys(512)`
    
3.  
4.  `with open('Bob-pri.pem',  'wb')as prif, open('Bob-pub.pem',  'wb')as pubf:`
    
5.   `prif.write(Bob_privkey.save_pkcs1())`
    
6.   `pubf.write(Bob_pubkey.save_pkcs1())`
    

其中

- `Bob_prikey` 是 Bob 的私钥，自己存放。
    
- `Bob_pubkey` 是 Bob 的公钥，交给 Bob。
    

Alice 发送信息给 Bob 时

- 使用 Bob 的公钥加密： `secret=rsa.encrypt(plain_byte,Bob_pubkey)`。
    

Bob 接收到消息后

- Bob 使用自己的私钥，来对 Alice 发来的信息进行解密： `plain=rsa.decrypt(secret,Bob_prikey).decode('utf-8')`。
    

Bob 的公钥可以让 Alice 发消息给 Bob，Bob 用自己的私钥揭秘。同样，Alice 的密钥对可以让对方发消息给自己。至此，Alice 和 Bob 实现了安全的通信，他们用对方公钥加密，用自己的私钥解密发给自己的信息。

Alice 发给 Bob 的信息，即使被 Eve 截获了，他也没有 Bob 的私钥，解不开密文。

但是，存在一个问题，如果 Eve 用 Bob 的公钥加密信息，伪装成 Alice 发个 Bob，这样怎么办呢？怎么确定 Alice 是 Alice 而不是 Eve 呢？问题的关键，在于 Alice 持有 Alice 私钥，而 Eve 没有私钥，这是数字签名技术的基础。

### 六、真言：数字签名

Eve 伪装成 Alice，如同假唐僧伪装成唐僧，言行举止看起来很像，让人怎么区分呢？很简单，真唐僧有一个核心科技，那就是紧箍咒。

非对称加密时，通常用公钥加密，私钥解密。如果用私钥加密，其实相当与签名了。因为只有私钥持有者才能加密，且被公钥解密。所以私钥加密相当于私钥持有者确认签名——该消息来自私钥持有人。

私钥就相当于真唐僧的紧箍咒。

因为效率，一般不对原始信息进行加密，而是对其哈希之后的值进行加密。根据上文哈希的特性，这依然可以保证原始信息唯一、未篡改。

对消息摘要进行私钥加密，称为数字签名。

验证步骤如下：

- Alice 准备发送信息 PlainText
    
- 首先计算其 MD5 哈希值 Hash_a
    
- 再对哈希值进行私钥加密（数字签名）
    
- 发送 Alice 的公钥，数字签名，消息给 Bob
    
- Bob 收到信息后
    
- 使用 Alice 的公钥解密数字签名，产生一个待验证哈希值 Hash_a
    
- 然后计算消息哈希值 Hash_b
    
- 如果Hasha == Hashb，说明发送者必然是持有私钥的 Alice ，且消息未修改
    
- 否则，说明信息不是 Alice 发送的
    

1.  `signature = rsa.sign(plain_byte,  Alice_prikey,  'MD5')`
    
2.  `status = rsa.verify(plain_byte, signature,  Alice_pubkey)`
    

注意上例 sign 方法中签名的是 Alice 的私钥，而检查时则使用 Alice 的公钥。Alice 无法抵赖他签名的信息，因为只有他持有自己的私钥，别人无法签名（私钥加密）一个这样的信息。

如同真唐僧会念紧箍咒，这就是他的私钥。假唐僧看起来很像样，但是他并不掌握紧箍咒，所以无法念动真言。

### 七、总结

本文用 20 行 Python 代码来演示如何实现安全通信的功能。

- 哈希函数，是可以提取消息数字指纹的工具，他可以验证数据完整性。
    
- 对称加密简单实用。
    
- 借助非对称加密，我们实现了安全通信，而数字签名使得对方无法伪装或抵赖。
    

作者：巩庆奎，大奎，对计算机、电子信息工程感兴趣。

**赞 赏 作 者**

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

People who liked this content also liked

当eBPF遇上Linux内核网络

Linux内核之旅

不看的原因

- 内容质量低
- 不看此公众号

厉害了，用Python破个世界纪录！

Jack Cui

不看的原因

- 内容质量低
- 不看此公众号

一文读懂层次聚类（Python代码）

Python数据科学

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___dd5b6650f3af49b29.bmp"/>

Scan to Follow