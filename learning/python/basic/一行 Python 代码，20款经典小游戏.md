# 一行 Python 代码，20款经典小游戏

<a id="profileBt"></a><a id="js_name"></a>Python编程时光 *2022-02-25 09:02*

The following article is from 志斌的python笔记 Author 才哥

<a id="copyright_info"></a>[![](../../../_resources/0_2f7a8ab0667b4402a15c303375505b69.jpg)<br>**志斌的python笔记** .<br>爬虫、数据分析、自动化办公从小白到大神一条龙服务~](#)

今天分享一个有趣的`Python`游戏库`freegames`，它包含20余款经典小游戏，像贪吃蛇、吃豆人、乒乓、数字华容道等等，依托于标准库`Turtle`。

我们不仅可以通过1行代码进行重温这些童年小游戏，还可以查看源码自己学习下游戏编写，超赞！

## \# 1\. 安装

通过`pip`简单安装，目前最新版本是`2.3.2`

```
pip install freegames

```

<img width="600" height="213" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__8873e0c0a2e743489.png"/>

## \# 2\. 基础介绍

安装完成后，我们可以通过以下指令查看相关信息

```
# 查看已有游戏列表
!python -m freegames list # 在jupyter notebook
python -m freegames list # 在命令行界面

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg== "查看已有游戏列表")

查看已有游戏列表

```
# 查看帮助
help(freegames)
# 也可以用 ? 在jupyter notebook
import freegames
freegames?

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg== "查看帮助")

查看帮助

## \# 3\. 游戏演示

这里我们介绍几种大家熟知的小游戏，并演示

**Paint 涂鸦** 在屏幕上绘制线条和形状

1.  单击以标记形状的开始，然后再次单击以标记其结束；
    
2.  可以使用键盘选择不同的形状和颜色。
    

```
!python -m freegames.paint # 如果在命令行，则去掉前面的 感叹号 !

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg== "涂鸦")

涂鸦

**Snake 贪吃蛇** 经典的街机小游戏

1.  使用键盘的方向键导航并吃绿色食物，每吃一次食物，蛇就会长一段；
    
2.  避免吃到自己或越界。
    

```
!python -m freegames.snake

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg== "贪吃蛇")

贪吃蛇

**Pacman 吃豆人** 经典街机小游戏

1.  使用箭头键导航并吃掉所有的白色食物；
    
2.  当心漫游在迷宫的红色幽灵，碰到就跪了。
    

```
!python -m freegames.pacman

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg== "吃豆人")

吃豆人

**Cannon 大炮**

1.  点击屏幕发射你的大炮，炮弹在它的路径上炸掉蓝色气球；
    
2.  在它们穿过屏幕之前将所有气球炸掉。
    

```
!python -m freegames.cannon

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg== "大炮")

大炮

**Flappy Flappy-bird 类游戏**

1.  点击屏幕来扇动你的翅膀；
    
2.  当您飞过屏幕时，请注意不要碰到黑乌鸦。
    

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg== "Flappy Bird")

Flappy Bird

**Pong 乒乓** 经典街机小游戏

使用键盘上下移动球拍，第一个错过球的球员输了

```
!python -m freegames.pong

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg== "乒乓球")

乒乓球

**Tiles 数字华容道** 将数字滑动到指定位置的益智游戏

单击与空方块相邻的图块以交换位置，你能让数字从左到右从下到上成1到15吗？

```
!python -m freegames.tiles

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg== "数字华容道")

数字华容道

还有更多游戏，大家可以自行体验，如果感兴趣还可以研究源码学习怎么编写python小游戏哦！

## \# 4\. 源码查看

通过`copy`可以将相关源文件拷贝到本地，然后查看源码，我们可以根据源码学习学习！

```
!python -m freegames copy snake

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg== "snake源文件预览")

snake源文件预览

以上就是本次全部内容，感兴趣的小伙伴可以安装这个库玩玩，顺便学学自己写个小游戏！

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

[![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)](https://mp.weixin.qq.com/mp/appmsgalbum?__biz=MzkzMjMxMTg2NQ==&action=getalbum&album_id=2225391633284562944&scene=173&subscene=90&sessionid=1642653201&enterid=1642653281&from_msgid=2247483819&from_itemidx=1&count=3&nolastread=1#wechat_redirect)

[![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)](http://mp.weixin.qq.com/s?__biz=MzIzMzMzOTI3Nw==&mid=2247505072&idx=1&sn=3605fcc95b6c0c7b0b9ec5dd15480231&chksm=e885b452dff23d44649944d41a190d55955a9f8ea6987e1ed73363c10bf3711528f4f8524e8a&scene=21#wechat_redirect)

[![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)](http://mp.weixin.qq.com/s?__biz=MzIzMzMzOTI3Nw==&mid=2247505084&idx=1&sn=d3bc3a37cda2759c1a078ce9811fdec2&chksm=e885b45edff23d48744d42d9f6658cdddd2164af7042544815851a851dc710c70a1527a9b686&scene=21#wechat_redirect)

[![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)](http://mp.weixin.qq.com/s?__biz=MzIzMzMzOTI3Nw==&mid=2247505065&idx=1&sn=a8b98840693b8d57bc5422da6d68a25d&chksm=e885b44bdff23d5d93cd686803d091d4b280c6f8a33afcf9e38c3f92746a1c222d1b40a8ad9a&scene=21#wechat_redirect)

People who liked this content also liked

前端请装上这个 Chrome 插件

前端下午茶

不看的原因

- 内容质量低
- 不看此公众号

将 Kubernetes 扩展到超过 4k 个节点和 200k 个 Pod

InfoQ

不看的原因

- 内容质量低
- 不看此公众号

Linux 系统下 查找文件 命令总结，这个很哇塞！

Linux就该这么学

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___0b824dd4df73430f8.bmp"/>

Scan to Follow