# 关于Python的Selenium框架全解，一篇完整的说明书

<a id="profileBt"></a><a id="js_name"></a>Python丹卿 *2022-03-08 14:55*

收录于话题

<a id="js_article_tag_name__1951254269450387456"></a>#python <a id="js_article_tag_num__1951254269450387456"></a>130 <a id="js_article_tag_tips__1951254269450387456"></a>个

<a id="js_article_tag_name__2300644937517891587"></a>#元素 <a id="js_article_tag_num__2300644937517891587"></a>1 <a id="js_article_tag_tips__2300644937517891587"></a>个

<a id="js_article_tag_name__2290550538108796938"></a>#计算机专业 <a id="js_article_tag_num__2290550538108796938"></a>19 <a id="js_article_tag_tips__2290550538108796938"></a>个

<a id="js_article_tag_name__2063016782558265345"></a>#思维导图 <a id="js_article_tag_num__2063016782558265345"></a>16 <a id="js_article_tag_tips__2063016782558265345"></a>个

<a id="js_article_tag_name__1977373112081973248"></a>#人工智能 <a id="js_article_tag_num__1977373112081973248"></a>41 <a id="js_article_tag_tips__1977373112081973248"></a>个

目录

# selenium 基础语法

# 一、 环境配置

# 1、 安装环境

安装 selenium 第三方库

```
pip install selenium
```

下载浏览器驱动：

- Firefox浏览器驱动： geckodriver
    
- Chrome浏览器驱动： chromedriver , taobao备用地址
    
- IE浏览器驱动： IEDriverServer
    
- Edge浏览器驱动： MicrosoftWebDriver
    
- Opera浏览器驱动： operadriver
    
- PhantomJS浏览器驱动： phantomjs
    

需要把这些浏览器驱动放入 Python 应用目录里面的 Script 文件夹里面

# 干货主要有：

① 70 多本 Python 电子书（和经典的书籍）应该有

② Python标准库资料（最全中文版）

③ 项目源码（四五十个有趣且可靠的练手项目及源码）

④ Python基础入门、爬虫、分析方面的视频（适合小白学习）

⑤ Python学习路线图（告别不入流的学习）

私信小编01即可获取大量Python学习资源

# 2、 配置参数

每次当selenium启动chrome浏览器的时候，chrome浏览器很干净，没有插件、没有收藏、没有历史记录，这是因为selenium在启动chrome时为了保证最快的运行效率，启动了一个裸浏览器，这就是为什么需要配置参数的原因，但是有些时候我们需要的不仅是一个裸浏览器

selenium启动配置参数接收是ChromeOptions类，创建方式如下 ：

```
from selenium import webdriver
option = webdriver.ChromeOptions()
driver = webdriver.Chrome(chrome_options=option)
```

创建了ChromeOptions类之后就是添加参数，添加参数有几个特定的方法，分别对应添加不同类型的配置项目

```
from selenium import webdriver
option = webdriver.ChromeOptions()
# 添加启动参数
option.add_argument()
# 添加扩展应用 
option.add_extension()
option.add_encoded_extension()
# 添加实验性质的设置参数 
option.add_experimental_option()
# 设置调试器地址
option.debugger_address()
```

常用配置参数：

```
from selenium import webdriver
option = webdriver.ChromeOptions()
# 添加UA
options.add_argument('user-agent="MQQBrowser/26 Mozilla/5.0 (Linux; U; Android 2.3.7; zh-cn; MB200 Build/GRJ22; CyanogenMod-7) AppleWebKit/533.1 (KHTML, like Gecko) Version/4.0 Mobile Safari/533.1"')
# 指定浏览器分辨率
options.add_argument('window-size=1920x3000') 
# 谷歌文档提到需要加上这个属性来规避bug
chrome_options.add_argument('--disable-gpu') 
 # 隐藏滚动条, 应对一些特殊页面
options.add_argument('--hide-scrollbars')
# 不加载图片, 提升速度
options.add_argument('blink-settings=imagesEnabled=false') 
# 浏览器不提供可视化页面. linux下如果系统不支持可视化不加这条会启动失败
options.add_argument('--headless') 
# 以最高权限运行
options.add_argument('--no-sandbox')
# 手动指定使用的浏览器位置
options.binary_location = r"C:\Program Files (x86)\Google\Chrome\Application\chrome.exe" 
#添加crx插件
option.add_extension('d:\crx\AdBlock_v2.17.crx') 
# 禁用JavaScript
option.add_argument("--disable-javascript") 
# 设置开发者模式启动，该模式下webdriver属性为正常值
options.add_experimental_option('excludeSwitches', ['enable-automation']) 
# 禁用浏览器弹窗
prefs = {  
    'profile.default_content_setting_values' :  {  
        'notifications' : 2  
     }  
}  
options.add_experimental_option('prefs',prefs)
# 添加代理 ip
options.add_argument("--proxy-server=http://XXXXX.com:80")
driver = webdriver.Chrome(chrome_options=chrome_options)
```

其他配置项目参数

```
–user-data-dir=”[PATH]” 
# 指定用户文件夹User Data路径，可以把书签这样的用户数据保存在系统分区以外的分区
　　–disk-cache-dir=”[PATH]“ 
# 指定缓存Cache路径
　　–disk-cache-size= 
# 指定Cache大小，单位Byte
　　–first run 
# 重置到初始状态，第一次运行
　　–incognito 
# 隐身模式启动
　　–disable-javascript 
# 禁用Javascript
　　--omnibox-popup-count="num" 
# 将地址栏弹出的提示菜单数量改为num个
　　--user-agent="xxxxxxxx" 
# 修改HTTP请求头部的Agent字符串，可以通过about:version页面查看修改效果
　　--disable-plugins 
# 禁止加载所有插件，可以增加速度。可以通过about:plugins页面查看效果
　　--disable-javascript 
# 禁用JavaScript，如果觉得速度慢在加上这个
　　--disable-java 
# 禁用java
　　--start-maximized 
# 启动就最大化
　　--no-sandbox 
# 取消沙盒模式
　　--single-process 
# 单进程运行
　　--process-per-tab 
# 每个标签使用单独进程
　　--process-per-site 
# 每个站点使用单独进程
　　--in-process-plugins 
# 插件不启用单独进程
　　--disable-popup-blocking 
# 禁用弹出拦截
　　--disable-plugins 
# 禁用插件
　　--disable-images 
# 禁用图像
　　--incognito 
# 启动进入隐身模式
　　--enable-udd-profiles 
# 启用账户切换菜单
　　--proxy-pac-url 
# 使用pac代理 [via 1/2]
　　--lang=zh-CN 
# 设置语言为简体中文
　　--disk-cache-dir 
# 自定义缓存目录
　　--disk-cache-size 
# 自定义缓存最大值（单位byte）
　　--media-cache-size 
# 自定义多媒体缓存最大值（单位byte）
　　--bookmark-menu 
# 在工具 栏增加一个书签按钮
　　--enable-sync 
# 启用书签同步
```

# 3、 常用参数搭配

制作无头浏览器

```
# 第一种写法
from selenium import webdriver
from selenium.webdriver.chrome.options import Options
chrome_options = Options()
chrome_options.add_argument('--headless')
chrome_options.add_argument('--disable-gpu')
driver = webdriver.Chrome(chrome_options=chrome_options)
# 第二种写法
from selenium import webdriver
options = webdriver.ChromeOptions()
options.add_argument('--headless')
options.add_argument('--disable-gpu')
driver = webdriver.Chrome(chrome_options=options)
```

规避检测

门户网站检测如果是selenium请求的，有可能会拒绝访问。这也是一种反爬机制

实现规避检测

```
from selenium import webdriver
from selenium.webdriver import ChromeOptions
options = ChromeOptions()
options.add_experimental_option('excludeSwitcher', ['enable-automation'])
driver = webdriver.Chrome(options=options)
```

注意：这里只能使用 options 添加

如果有其他的模块要添加，注意要分开添加

# 4、 分浏览器启动

```
from selenium import webdriver
driver = webdriver.Firefox()   # Firefox浏览器
# driver = webdriver.Firefox(executable_path="驱动路径")
driver = webdriver.Chrome()    # Chrome浏览器
driver = webdriver.Ie()        # Internet Explorer浏览器
driver = webdriver.Edge()      # Edge浏览器
driver = webdriver.Opera()     # Opera浏览器
driver = webdriver.PhantomJS()   # PhantomJS
```

# 二、 基本语法

# 1、 元素定位

元素定位语法

常用语法：

```
find_element_by_id()
find_element_by_name()
find_element_by_class_name()
find_element_by_tag_name()
find_element_by_link_text()
find_element_by_partial_link_text()
find_element_by_xpath()
find_element_by_css_selector()
```

在 element 变成 elements 时，返回符合条件的所有元素组成的数组

# 2、 控制浏览器操作

控制浏览器大小

- driver.set\_window\_size(480, 800)
    

浏览器后退，前进

```
driver.forward()
driver.back()

```

刷新

- driver.refresh()
    

# 3、 操作元素的方法

# 3.1 点击和输入

```
driver.find_element_by_id("kw").clear() # 清空文本 
driver.find_element_by_id("kw").send_keys("selenium") # 模拟按键输入 
driver.find_element_by_id("su").click() # 单击元素
```

# 3.2 提交

在搜索框模拟回车操作

```
search_text = driver.find_element_by_id('kw') search_text.send_keys('selenium') search_text.submit()  # 模拟回车操作
```

# 3.3 其他

```
drive.size  # 返回元素的尺寸
drive.text  # 获取元素的文本
drive.get_attribute(name)  # 获得属性值
drive.is_displayed()  # 设置该元素是否用户可见
drive.page_source  # 获取网页源代码
```

# 4、 鼠标操作

在 WebDriver 中， 将这些关于鼠标操作的方法封装在 ActionChains 类提供

ActionChains 类提供了鼠标操作的常用方法：

```
click(on_element=None) ——单击鼠标左键
click_and_hold(on_element=None) ——点击鼠标左键，不松开
context_click(on_element=None) ——点击鼠标右键
double_click(on_element=None) ——双击鼠标左键
drag_and_drop(source, target) ——拖拽到某个元素然后松开
drag_and_drop_by_offset(source, xoffset, yoffset) ——拖拽到某个坐标然后松开
key_down(value, element=None) ——按下某个键盘上的键
key_up(value, element=None) ——松开某个键
move_by_offset(xoffset, yoffset) ——鼠标从当前位置移动到某个坐标
move_to_element(to_element) ——鼠标移动到某个元素
move_to_element_with_offset(to_element, xoffset, yoffset) ——移动到距某个元素（左上角坐标）多少距离的位置
perform() ——执行链中的所有动作
release(on_element=None) ——在某个元素位置松开鼠标左键
send_keys(*keys_to_send) ——发送某个键到当前焦点的元素
send_keys_to_element(element, *keys_to_send) ——发送某个键到指定元素
```

语法：

```
from selenium.webdriver.common.action_chains import ActionChains
# 获取元素
menu = driver.find_element_by_css_selector(".nav")
hidden_submenu = driver.find_element_by_css_selector(".nav #submenu1")
# 链式写法
ActionChains(driver).move_to_element(menu).click(hidden_submenu).perform()
# 分步写法
actions = ActionChains(driver)
actions.move_to_element(menu)
actions.click(hidden_submenu)
actions.perform()
```

# 5、 键盘操作

想使用selenium中的键盘事件，首先我们必须导入Keys包，需要注意的是包名称Keys首字母需要大写。Keys类中提供了几乎所有的键盘事件包括组合按键如 Ctrl+A、 Ctrl+C 等

使用语法：

```
from selenium.webdriver.common.keys import Keys
element.send_keys(键盘事件)
# 常用键盘事件
Keys.BACK_SPACE 	# 回退键(BackSpace)
Keys.TAB	# 制表键(Tab)
Keys.ENTER		# 回车键(Enter)
Keys.SHIFT		# 大小写转换键(Shift)
Keys.CONTROL	# Control键(Ctrl)
Keys.ALT	# ALT键(Alt)
Keys.ESCAPE 	# 返回键(Esc)
Keys.SPACE 		# 空格键(Space)
Keys.PAGE_UP		# 翻页键上(Page Up)
Keys.PAGE_DOWN 		# 翻页键下(Page Down)
Keys.END		# 行尾键(End)
Keys.HOME		# 行首键(Home)
Keys.LEFT		# 方向键左(Left)
Keys.UP		# 方向键上(Up)
Keys.RIGHT		# 方向键右(Right)
Keys.DOWN		# 方向键下(Down)
Keys.INSERT		# 插入键(Insert)
DELETE		# 删除键(Delete)
NUMPAD0 ~ NUMPAD9		# 数字键1-9
Keys.F5		# 刷新键
F1 ~ F12		# F1 - F12键
(Keys.CONTROL, 'a')		# 组合键Control+a，全选
(Keys.CONTROL, 'c')		# 组合键Control+c，复制
(Keys.CONTROL, 'x')		# 组合键Control+x，剪切
(Keys.CONTROL, 'v')		# 组合键Control+v，粘贴
```

其他事件可以通过查看源码获取

# 6、 获取断言信息

```
title = driver.title # 打印当前页面title
now_url = driver.current_url # 打印当前页面URL
user = driver.find_element_by_class_name('nums').text # # 获取结果数目
```

# 7、 等待页面加载完成

# 7.1 显示等待

显式等待使WebdDriver等待某个条件成立时继续执行，否则在达到最大时长时抛出超时异常

实例：

```
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions 
driver = webdriver.Firefox()
driver.get("http://www.baidu.com")
element = WebDriverWait(driver, 5, 0.5).until(
          expected_conditions.presence_of_element_located((By.ID, "kw"))
                      )  # expected_conditions.presence_of_element_located()方法判断元素是否存在
element.send_keys('selenium')
driver.quit()
```

WebDriverWait类是由WebDirver 提供的等待方法。在设置时间内，默认每隔一段时间检测一次当前页面元素是否存在，如果超过设置时间检测不到则抛出异常

语法：

```
WebDriverWait(driver, timeout, poll_frequency=0.5, ignored_exceptions=None)
```

参数：

- driver ：浏览器驱动
    
- timeout ：最长超时时间，默认以秒为单位
    
- poll_frequency ：检测的间隔（步长）时间，默认为0.5S
    
- ignored_exceptions ：超时后的异常信息，默认情况下抛NoSuchElementException异常
    
- WebDriverWait()一般由until()或until\_not()方法配合使用until(method, message=‘’) ：调用该方法提供的驱动程序作为一个参数，直到返回值为Trueuntil\_not(method, message=‘’)： 调用该方法提供的驱动程序作为一个参数，直到返回值为False
    

# 7.2 隐式等待

如果某些元素不是立即可用的，隐式等待是告诉WebDriver去等待一定的时间后去查找元素。 默认等待时间是0秒，一旦设置该值，隐式等待是设置该WebDriver的实例的生命周期

```
from selenium import webdriver
driver = webdriver.Firefox()    
driver.implicitly_wait(10) # 隐式等待 10 s 
driver.get("http://www.baidu.com")    
myDynamicElement = driver.find_element_by_id("myDynamicElement")
```

# 8、 页面切换

```
driver.switch_to_window("windowName")  # 切换窗口
driver.switch_to_frame("frameName")  # 切换进框架里面
driver.switch_to_default_content()  # 退出框架
```

案例

```
#先通过xpth定位到iframe
xf = driver.find_element_by_xpath('//*[@id="x-URS-iframe"]')
#再将定位对象传给switch_to_frame()方法
driver.switch_to_frame(xf)
driver.switch_to_default_content()  # 退出框架
```

# 9、 框处理

# 9.1 警告框处理

语法：

```
alert = driver.switch_to_alert()
```

alert 里面的方法

- text：返回 alert/confirm/prompt 中的文字信息
    
- accept()：接受现有警告框
    
- dismiss()：解散现有警告框
    
- send_keys(keysToSend)：发送文本至警告框。keysToSend：将文本发送至警告框
    

# 9.2 下拉框选择

# 9.2.1 Select类的方法

# 9.2.1.1 选中方法

```
from selenium import webdriver
from selenium.webdriver.support.select import Select
driver = webdriver.Chrome()
driver.implicitly_wait(10)  # 隐式等待
driver.get('http://www.baidu.com')
sel = driver.find_element_by_xpath("//select[@id='nr']")
"""
有三种方式选择下拉框
select_by_value(value)  通过value属性值进行选择
select_by_index(index)  通过索引查找,index从0开始
select_by_visible_text(text)  通过标签显示的text进行选择
"""
Select(sel).select_by_value(value)
```

# 9.2.1.2 取消选择方法

```
"""
deselect_all()  取消全选
deselect_by_value(value)  通过value属性取消选择
deselect_by_index(index)  通过index取消选择
deselect_by_visible_text(text)  通过text取消选择
"""
# 使用方法
Select(sel).deselect_by_value(value)
```

# 9.2.2 先定位select 然后在定位option

```
# 定位到下拉选择框
selector = driver.find_element_by_id("selectdemo")
# selector = driver.find_element_by_xpath(".//*[@id='selectdemo']")
 
# 选择"篮球运动员"
selector.find_element_by_xpath("//option[@value='210103']").click()
# selector.find_elements_by_tag_name("option")[2].click()
```

# 9.2.3 直接通过xpath层级标签定位

```
# 直接通过xpath定位并选择"篮球运动员"
driver.find_element_by_xpath(".//*[@id='selectdemo']/option[3]").click()
```

# 10、 文件上传

```
driver.find_element_by_name("file").send_keys('D:\\upload_file.txt')  # 定位上传按钮，添加本地文件
```

# 11、 cookie操作

WebDriver操作cookie的方法：

```
get_cookies()
get_cookie(name)
add_cookie(cookie_dict)
delete_cookie(name,optionsString)
delete_all_cookies()

```

# 11.1 cookie 登录方法

参考链接：
https://www.jianshu.com/p/773c58406bdb

1.  手动获取网页的cookie，将其序列化并存储在本地
    
2.  写入代码
    

```
for item in cookies:
    driver.add_cookie(item)
```

与普通的在headers里添加 {'Cookies':' '} 不一样的是，此方法需要按照cookie的name,value,path,domain格式逐个cookie添加

# 12、 调用JS代码

```
js="window.scrollTo(100,450);"
driver.execute_script(js) # 通过javascript设置浏览器窗口的滚动条位置
```

通过execute_script()方法执行JavaScripts代码来移动滚动条的位置

# 13、 窗口截图

```
driver.get_screenshot_as_file("D:\\baidu_img.jpg") # 截取当前窗口，并指定截图图片的保存位置
```

# 13.1 截取验证码图片案例

```
# encoding:utf-8
from PIL import Image
from selenium import webdriver
 
url = 'https://weixin.sogou.com/antispider/?from=http%3A%2F%2Fweixin.sogou.com%2Fweixin%3Ftype%3D2%26query%3Dpython'
driver = webdriver.Chrome()
driver.maximize_window()  # 将浏览器最大化
driver.get(url)
# 截取当前网页并放到D盘下命名为printscreen，该网页有我们需要的验证码
driver.save_screenshot('D:\\python371\\python_wordspace\\img\\printscreen.png')
imgelement = driver.find_element_by_id('seccodeImage')  # 定位验证码
location = imgelement.location  # 获取验证码x,y轴坐标
print(location)
size = imgelement.size  # 获取验证码的长宽
print(size)
rangle = (int(location['x']+110), int(location['y']+60), int(location['x'] + size['width']+165),
          int(location['y'] + size['height']+90))  # 写成我们需要截取的位置坐标
i = Image.open("D:\\python371\\python_wordspace\\img\\printscreen.png")  # 打开截图
frame4 = i.crop(rangle)  # 使用Image的crop函数，从截图中再次截取我们需要的区域
frame4 = frame4.convert('RGB')
frame4.save('D:\\python371\\python_wordspace\\img\\save.jpg') # 保存我们接下来的验证码图片 进行打码
 
driver.close()
```

# 14、 关闭浏览器

```
driver.close() 
driver.quit() 
```

People who liked this content also liked

go语言 实现 http服务端与客户端 通讯

Go语言圈

不看的原因

- 内容质量低
- 不看此公众号

python极客项目编程 中文pdf完整版入门到精通（视频400集

Python丹卿

不看的原因

- 内容质量低
- 不看此公众号

Fortran基础编程（入门简介篇）

易木木响叮当

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___cdbc4da2d7fa46eba.bmp"/>

Scan to Follow