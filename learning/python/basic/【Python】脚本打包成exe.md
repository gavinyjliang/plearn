# 【Python】脚本打包成exe

<a id="copyright_logo"></a>Original 曹锴 <a id="profileBt"></a><a id="js_name"></a>caokai001 *2022-03-07 08:00*

<a id="js_article-tag-card__left"></a>收录于话题 #python <a id="js_article-tag-card__right"></a>3个

## 1\. 背景：

当给其他部门完成了一个编程需求，你会考虑如何分享给对方使用呢~

比如：需求是批量修改文件名。

可选的方法：

- Shell脚本：一般非编程人员，没有此资源；并且windows环境无法使用；
    
- Windows bat脚本：不同的系统有些命令不一定通用；
    
- JS网页：出于安全考虑，网页无法修改本地文件名；
    
- R脚本：非编程人员没有此资源，R打包成exe不太常见；
    
- Python脚本：需要环境运行此脚本
    
- **打包成exe：可直接使用，需考虑压缩包大小；**
    

### 👳‍♂️之前的流程：

- 下载安装Python38
    

<img width="657" height="158" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__4c9d6801f0eb4fbc8.png"/>

img

- 配置notepad++运行py环境，并创建别名run_RA
    

<img width="657" height="142" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__17f6290558404a0f8.png"/>

img

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__fffedd5e3b8245658.png)

img

命令内容如下：

```
`cmd /k cd /d "$(CURRENT_DIRECTORY)" & C:\Users\kai.cao\Sync\Python38\python.exe "$(FULL_CURRENT_PATH)" & PAUSE & EXIT
`
```

- 将py代码拷贝到项目目录下，用notepad++打开并运行，run_RA，修改后产生一个excel文件；
    

<img width="657" height="190" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__35f393f8d3844b4fb.png"/>

img

### 👶更新后流程

- 将standard\_filename\_for\_RA\_KaiCao_20220302.exe文件放到项目根目录下面，
    
- 双击运行(实际项目大约需5-10s)，完成文件名修改（将不合规的符号，全部转为下划线_）；
    
- 最终会输出 standard\_filename\_for_RA.csv（一些基本统计信息），便于统计文件名长度，文件大小，路径长度是否合规；
    

<img width="657" height="269" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__93960caeef854654b.png"/>

img

## 2\. Python脚本打包exe流程

### 2.1 安装pipenv，减少不必要的package

- pip.exe install pipenv  https://www.cnblogs.com/hongdada/p/11014908.html
    

### 2.2 进入项目目录

- mkdir project->cd project
    

### 2.3 进入虚拟环境

- pipenv install; pipenv shell 进入虚拟环境；
    

### 2.4 安装必要的package ，并测试

- pip install pandas；pip install pyinstaller; pip install xlsxwriter
    

<img width="657" height="329" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__778b0292a59a40378.png"/>

img

- 测试所需的包，是否都安装好 pipenv run 1.py
    

<img width="657" height="219" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__d10add2ae679444da.png"/>

img

### 2.5安装UPX，进一步压缩体积

https://blog.csdn.net/chentianveiko/article/details/107083912

### 2.6 开始打包exe

pyinstaller.exe -F -w .\\standard\_filename\_for_RA.py

-F: 打包成单独exe模式

-w:没有黑色弹窗(用户可能以为没执行，所以最好不加-w)；

<img width="657" height="180" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__40fcc8241d8f40008.png"/>

img

<img width="657" height="475" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__f0e0d7ccce694c5a8.png"/>

img

### 2.7 打包结果

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__1e246b54740247e99.png)

img

![Image](../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__ec506901aea84d98a.png)

img

## 3.Notes

### UPX,Pipenv 相比直接运行Pyinstaller.exe 有什么变化：315M->21M;

pyinstaller.exe -F -w standard\_filename\_for_RA.py

<img width="657" height="94" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__3277cc0dcd3c42d59.png"/>

img

<img width="657" height="146" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__fcf41248b3484f0d8.png"/>

img

### pipenv虚拟环境中，下载package速度较慢，需要修改镜像加速下载

<img width="657" height="180" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__905b01e35a0041efb.png"/>

img

### 参考：

https://www.zhihu.com/question/281858271

https://www.cnblogs.com/zingp/p/8525138.html

https://www.cnblogs.com/hongdada/p/11014908.html

欢迎评论交流~

<a id="js_view_source"></a>Read more

People who liked this content also liked

python数据可视化--matplotlib绘制气泡图

AI小白笔记

不看的原因

- 内容质量低
- 不看此公众号

Python基础(下)

啊颜的学习场所

不看的原因

- 内容质量低
- 不看此公众号

9个好用的python技巧分享

AI算法之道

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___929ca4fc48f74081a.bmp"/>

Scan to Follow