# GitHub 7.5k star！各种视觉Transformer的PyTorch实现合集整理好了

<a id="profileBt"></a><a id="js_name"></a>机器学习实验室 *2022-01-02 19:43*

 ViT 

**转自：机器之心**

> 这个项目登上了今天的GitHub Trending。

近一两年，Transformer 跨界 CV 任务不再是什么新鲜事了。

自 2020 年 10 月谷歌提出 Vision Transformer (ViT) 以来，各式各样视觉 Transformer 开始在图像合成、点云处理、视觉 - 语言建模等领域大显身手。

之后，在 PyTorch 中实现 Vision Transformer 成为了研究热点。GitHub 中也出现了很多优秀的项目，今天要介绍的就是其中之一。

该项目名为「vit-pytorch」，**它是一个 Vision Transformer 实现，展示了一种在 PyTorch 中仅使用单个 transformer 编码器来实现视觉分类 SOTA 结果的简单方法。**

项目当前的 star 量已经达到了 7.5k，创建者为 Phil Wang，ta 在 GitHub 上有 147 个资源库。

<img width="677" height="181" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__d7645a7a768d4880a.png"/>

项目地址：https://github.com/lucidrains/vit-pytorch

项目作者还提供了一段动图展示：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

**项目介绍**

首先来看 Vision Transformer-PyTorch 的安装、使用、参数、蒸馏等步骤。

第一步是安装：

```
$ pip install vit-pytorch

```

第二步是使用：

```
import torch
from vit_pytorch import ViT
v = ViT(
    image_size = 256,
    patch_size = 32,
    num_classes = 1000,
    dim = 1024,
    depth = 6,
    heads = 16,
    mlp_dim = 2048,
    dropout = 0.1,
    emb_dropout = 0.1
)
img = torch.randn(1, 3, 256, 256)
preds = v(img) # (1, 1000)

```

第三步是所需参数，包括如下：

- image_size：图像大小
    
- patch_size：patch 数量
    
- num_classes：分类类别的数量
    
- dim：线性变换 nn.Linear(..., dim) 后输出张量的最后维
    
- depth：Transformer 块的数量
    
- heads：多头注意力层中头的数量
    
- mlp_dim：MLP（前馈）层的维数
    
- channels：图像通道的数量
    
- dropout：Dropout rate
    
- emb_dropout：嵌入 dropout rate
    
- ……
    

最后是蒸馏，采用的流程出自 Facebook AI 和索邦大学的论文《Training data-efficient image transformers & distillation through attention》。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

*论文地址：https://arxiv.org/pdf/2012.12877.pdf*

从 ResNet50（或任何教师网络）蒸馏到 vision transformer 的代码如下：

```
import torchfrom torchvision.models import resnet50from vit_pytorch.distill import DistillableViT, DistillWrapperteacher = resnet50(pretrained = True)
v = DistillableViT(
    image_size = 256,
    patch_size = 32,
    num_classes = 1000,
    dim = 1024,
    depth = 6,
    heads = 8,
    mlp_dim = 2048,
    dropout = 0.1,
    emb_dropout = 0.1
)
distiller = DistillWrapper(
    student = v,
    teacher = teacher,
    temperature = 3,           # temperature of distillationalpha = 0.5,               # trade between main loss and distillation losshard = False               # whether to use soft or hard distillation
)
img = torch.randn(2, 3, 256, 256)labels = torch.randint(0, 1000, (2,))
loss = distiller(img, labels)loss.backward()
# after lots of training above ...pred = v(img) # (2, 1000)

```

除了 Vision Transformer 之外，该项目还提供了 Deep ViT、CaiT、Token-to-Token ViT、PiT 等其他 ViT 变体模型的 PyTorch 实现。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

对 ViT 模型 PyTorch 实现感兴趣的读者可以参阅原项目。

```


往期精彩：

```


 [时隔一年！深度学习语义分割理论与代码实践指南.pdf第二版来了！](http://mp.weixin.qq.com/s?__biz=MzI4ODY2NjYzMQ==&mid=2247495133&idx=1&sn=f685ca12cc5a708b968bb3252808d202&chksm=ec3848b5db4fc1a36a6b8ed89f119d8f428d135d2ca6dfc5bcdb8a4210ab989bafe57964b8de&scene=21#wechat_redirect)

 [基于 docker 和 Flask 的深度学习模型部署！](http://mp.weixin.qq.com/s?__biz=MzI4ODY2NjYzMQ==&mid=2247496083&idx=1&sn=9774201d1ddf26ed0d1c7c36a5846906&chksm=ec3854fbdb4fdded2b31b21af460eb8afb9793b272d325fcecaf6612d63471e063e33f3385e5&scene=21#wechat_redirect)

 [新书预告 | 《机器学习公式推导与代码实现》出版在即！](http://mp.weixin.qq.com/s?__biz=MzI4ODY2NjYzMQ==&mid=2247495873&idx=1&sn=4f0f41409228108f46afc919d6b1ae3b&chksm=ec3855a9db4fdcbfd90f22df3dc533f7ff0f5f20bca360200e9f07c40daf9269588cec82747d&scene=21#wechat_redirect)


```




```

People who liked this content also liked

让移动设备用上轻量级、低延迟的视觉Transformer，苹果搞了个MobileViT

...

机器之心

不看的原因

- 内容质量低
- 不看此公众号

大白话用Transformer做Object Detection

...

PaperWeekly

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___851919c817904fefa.bmp"/>

Scan to Follow