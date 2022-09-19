# Python 一个快速视频剪辑编辑神器 — Moviepy

<a id="copyright_logo"></a>Original <a id="js_author_name"></a>Ckend <a id="profileBt"></a><a id="js_name"></a>Python实用宝典 *2022-03-03 20:20*

收录于话题

<a id="js_article_tag_name__1502593514189144064"></a>#Python <a id="js_article_tag_num__1502593514189144064"></a>119 <a id="js_article_tag_tips__1502593514189144064"></a>个

<a id="js_article_tag_name__1882395701842591745"></a>#办公 <a id="js_article_tag_num__1882395701842591745"></a>4 <a id="js_article_tag_tips__1882395701842591745"></a>个

<a id="js_article_tag_name__1888181387946409984"></a>#视频 <a id="js_article_tag_num__1888181387946409984"></a>2 <a id="js_article_tag_tips__1888181387946409984"></a>个

你知道吗，用moviepy一行代码就能够快速剪辑视频中某个区间的片段：

```
clip = VideoFileClip("videoplayback.mp4").subclip(50,60)
```

这一段代码，能够在3秒内将videoplayback.mp4的50秒-60秒的视频片段提取出来，非常方便。

不仅如此，moviepy还支持添加字幕、调整音量、片段链接等功能。下面看看详细的操作方法。

***1.准备***

开始之前，你要确保Python和pip已经成功安装在电脑上，如果没有，可以访问这篇文章：[超详细Python安装指南](http://mp.weixin.qq.com/s?__biz=MzI3MzM0ODU4Mg==&mid=2247485004&idx=1&sn=6f89120cf926e71c7eb4788744ff625f&chksm=eb25e4c5dc526dd31f216f56b963179a0bc301a5654644ef98f436aa4740caa6f2774046296f&scene=21#wechat_redirect) 进行安装。

(可选1) 如果你用Python的目的是数据分析，可以直接安装Anaconda：[Python数据分析与挖掘好帮手—Anaconda](http://mp.weixin.qq.com/s?__biz=MzI3MzM0ODU4Mg==&mid=2247486014&idx=1&sn=4519422fbd83b5feffcbb21552226bc3&chksm=eb25e8b7dc5261a1aef2fa400ca7bcaa8c06394ea1f9a5860ab02bcf95d4664f41903b12bbd8&scene=21#wechat_redirect)，它内置了Python和pip.

(可选2) 此外，推荐大家用VSCode编辑器，它有许多的优点：[Python 编程的最好搭档—VSCode 详细指南](http://mp.weixin.qq.com/s?__biz=MzI3MzM0ODU4Mg==&mid=2247485849&idx=1&sn=ec098cf67a55bd1d61d4513397434c94&chksm=eb25eb10dc52620682db716d206c18b00bd53c01743729a9dea381e1791566a04a06f1fabca5&scene=21#wechat_redirect)。

**请选择以下任一种方式输入命令安装依赖**：
1\. Windows 环境 打开 Cmd (开始-运行-CMD)。
2\. MacOS 环境 打开 Terminal (command+空格输入Terminal)。
3\. 如果你用的是 VSCode编辑器 或 Pycharm，可以直接使用界面下方的Terminal.

```
pip install moviepy
```

***2.视频剪辑***

剪辑个视频，多大点事，比起下载PR，用Python 写3行代码，3秒剪辑不香吗？

```
from moviepy.editor import*
# 剪辑50-60秒的音乐 00:00:50 - 00:00:60
video =CompositeVideoClip([VideoFileClip("videoplayback.mp4").subclip(50,60)])
# 写入剪辑完成的音乐
video.write_videofile("done.mp4")
```

***3.视频拼接***

“哦？Python？哼，那你肯定很难进行拼接工作吧，PR多方便，拖拽即可完成拼接。”

那你可真是太小看Python了，moviepy几行代码随随便便就能拼接许多片段：

```
from moviepy.editor importVideoFileClip, concatenate_videoclips
clip1 =VideoFileClip("myvideo.mp4")
# 结合剪辑，你甚至能够完全自动化剪辑拼接视频的操作
clip2 =VideoFileClip("myvideo2.mp4").subclip(50,60)
clip3 =VideoFileClip("myvideo3.mp4")
final_clip = concatenate_videoclips([clip1,clip2,clip3])
final_clip.write_videofile("my_concatenation.mp4")
```

结合剪辑，你甚至能够完全自动化剪辑拼接视频的操作。

***4.逐帧变化***

“那你能完成针对每一帧图像的快速图像处理吗？PR可是做得到的哦”

那当然可以，教你如何反转视频每一帧的绿色和蓝色通道：

```
from moviepy.editor importVideoFileClip
my_clip =VideoFileClip("videoplayback.mp4")
def scroll(get_frame, t):
    """
    处理每一帧图像
    """
    frame = get_frame(t)
    frame_region = frame[:,:,[0,2,1]]
    return frame_region
modifiedClip = my_clip.fl(scroll)
modifiedClip.write_videofile("test.mp4")
```

***5.导出GIF***

哇，听起来好像挺牛逼的，那用来导出到GIF吗？

当然可以：

```
from moviepy.editor import*
# 剪辑50-60秒的音乐 00:00:50 - 00:00:60
video = CompositeVideoClip([VideoFileClip("videoplayback.mp4").subclip(50,60)])
my_clip.write_gif('test.gif', fps=12)
```

怎么样，moviepy的这些技巧你学会了吗？它还有更多的功能和技巧，详情请见官方文档哦：
https://zulko.github.io/moviepy/

原创不易，希望你能在下面点个赞和在看支持我继续创作，谢谢！

**点击下方阅读原文可获得更好的阅读体验**

Python实用宝典 (pythondict.com)
不只是一个宝典
欢迎关注公众号：Python实用宝典

![Image](../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_94233492a6774674a.jpg)

<a id="js_view_source"></a>Read more

People who liked this content also liked

NNI — 微软开源的一个自动帮你做机器学习调参的神器

Python实用宝典

不看的原因

- 内容质量低
- 不看此公众号

有了这篇 Docker 网络原理，彻底爱了~

高效运维

不看的原因

- 内容质量低
- 不看此公众号

Python Delorean 优秀的时间格式智能转换工具

Python实用宝典

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___6825101eb8af423b9.bmp"/>

Scan to Follow