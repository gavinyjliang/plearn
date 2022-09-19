# 建议收藏，22个Python迷你项目(附源码)

<a id="profileBt"></a><a id="js_name"></a>python数据分析之禅 *2022-04-27 12:42* *Posted on <a id="js_ip_wording"></a>北京*

The following article is from 法纳斯特 Author 小F

<a id="copyright_info"></a>[![](../../../_resources/0_e8f55550a9b34979a1d3c10b60c2c817.jpg)<br>**法纳斯特** .<br>分享学习Python爬虫、数据分析、数据挖掘的点滴。](#)

在使用Python的过程中，我最喜欢的就是Python的各种第三方库，能够完成很多操作。

下面就给大家介绍22个通过Python构建的项目，以此来学习Python编程。

大家也可根据项目的目的及提示，自己构建解决方法，提高编程水平。

**①** **骰子模拟器**

目的：创建一个程序来模拟掷骰子。

提示：当用户询问时，使用random模块生成一个1到6之间的数字。

<img width="677" height="172" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__71b3a3444eea4f649.png"/>

********②****** ****石头剪刀布游戏******

目标：创建一个命令行游戏，游戏者可以在石头、剪刀和布之间进行选择，与计算机PK。如果游戏者赢了，得分就会添加，直到结束游戏时，最终的分数会展示给游戏者。

提示：接收游戏者的选择，并且与计算机的选择进行比较。计算机的选择是从选择列表中随机选取的。如果游戏者获胜，则增加1分。

```


import random
choices = ["Rock", "Paper", "Scissors"]
computer = random.choice(choices)
player = False
cpu_score = 0
player_score = 0
while True:
    player = input("Rock, Paper or  Scissors?").capitalize()
    # 判断游戏者和电脑的选择
    if player == computer:
        print("Tie!")
    elif player == "Rock":
        if computer == "Paper":
            print("You lose!", computer, "covers", player)
            cpu_score+=1
        else:
            print("You win!", player, "smashes", computer)
            player_score+=1
    elif player == "Paper":
        if computer == "Scissors":
            print("You lose!", computer, "cut", player)
            cpu_score+=1
        else:
            print("You win!", player, "covers", computer)
            player_score+=1
    elif player == "Scissors":
        if computer == "Rock":
            print("You lose...", computer, "smashes", player)
            cpu_score+=1
        else:
            print("You win!", player, "cut", computer)
            player_score+=1
    elif player=='E':
        print("Final Scores:")
        print(f"CPU:{cpu_score}")
        print(f"Plaer:{player_score}")
        break
    else:
        print("That's not a valid play. Check your spelling!")
    computer = random.choice(choices)



```

****************③**************** **随机密码生成器**

目标：创建一个程序，可指定密码长度，生成一串随机密码。

提示：创建一个数字+大写字母+小写字母+特殊字符的字符串。根据设定的密码长度随机生成一串密码。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

**********************④********************** **句子生成器**

目的：通过用户提供的输入，来生成随机且唯一的句子。

提示：以用户输入的名词、代词、形容词等作为输入，然后将所有数据添加到句子中，并将其组合返回。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

****************************⑤**************************** **猜数字游戏**

目的：在这个游戏中，任务是创建一个脚本，能够在一个范围内生成一个随机数。如果用户在三次机会中猜对了数字，那么用户赢得游戏，否则用户输。

提示：生成一个随机数，然后使用循环给用户三次猜测机会，根据用户的猜测打印最终的结果。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

************⑥******** **故事生成器******

目的：每次用户运行程序时，都会生成一个随机的故事。

提示：random模块可以用来选择故事的随机部分，内容来自每个列表里。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

************⑦******** **邮件地址切片器******

目的：编写一个Python脚本，可以从邮件地址中获取用户名和域名。

提示：使用@作为分隔符，将地址分为分为两个字符串。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

************************⑧************************ **自动发送邮件**

目的：编写一个Python脚本，可以使用这个脚本发送电子邮件。

提示：email库可用于发送电子邮件。

```


import smtplib 
from email.message import EmailMessage
email = EmailMessage() ## Creating a object for EmailMessage
email['from'] = 'xyz name'   ## Person who is sending
email['to'] = 'xyz id'       ## Whom we are sending
email['subject'] = 'xyz subject'  ## Subject of email
email.set_content("Xyz content of email") ## content of email
with smtlib.SMTP(host='smtp.gmail.com',port=587)as smtp:     
## sending request to server 
    smtp.ehlo()          ## server object
smtp.starttls()      ## used to send data between server and client
smtp.login("email_id","Password") ## login id and password of gmail
smtp.send_message(email)   ## Sending email
print("email send")    ## Printing success message



```

************⑨******** **缩写词******

目的：编写一个Python脚本，从给定的句子生成一个缩写词。

提示：你可以通过拆分和索引来获取第一个单词，然后将其组合。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

************⑩******** **文字冒险游戏******

目的：编写一个有趣的Python脚本，通过为路径选择不同的选项让用户进行有趣的冒险。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

************⑪******** Hangman****

目的：创建一个简单的命令行hangman游戏。

提示：创建一个密码词的列表并随机选择一个单词。现在将每个单词用下划线“_”表示，给用户提供猜单词的机会，如果用户猜对了单词，则将“_”用单词替换。

```


import time
import random
name = input("What is your name? ")
print ("Hello, " + name, "Time to play hangman!")
time.sleep(1)
print ("Start guessing...\n")
time.sleep(0.5)
## A List Of Secret Words
words = ['python','programming','treasure','creative','medium','horror']
word = random.choice(words)
guesses = ''
turns = 5
while turns > 0:         
    failed = 0             
    for char in word:      
        if char in guesses:    
            print (char,end="")    
        else:
            print ("_",end=""),     
            failed += 1    
    if failed == 0:        
        print ("\nYou won") 
        break              
    guess = input("\nguess a character:") 
    guesses += guess                    
    if guess not in word:  
        turns -= 1        
        print("\nWrong")    
        print("\nYou have", + turns, 'more guesses') 
        if turns == 0:           
            print ("\nYou Lose") 



```

************⑫******** 闹钟****

目的：编写一个创建闹钟的Python脚本。

提示：你可以使用date-time模块创建闹钟，以及playsound库播放声音。

```


from datetime import datetime   
from playsound import playsound
alarm_time = input("Enter the time of alarm to be set:HH:MM:SS\n")
alarm_hour=alarm_time[0:2]
alarm_minute=alarm_time[3:5]
alarm_seconds=alarm_time[6:8]
alarm_period = alarm_time[9:11].upper()
print("Setting up alarm..")
while True:
    now = datetime.now()
    current_hour = now.strftime("%I")
    current_minute = now.strftime("%M")
    current_seconds = now.strftime("%S")
    current_period = now.strftime("%p")
    if(alarm_period==current_period):
        if(alarm_hour==current_hour):
            if(alarm_minute==current_minute):
                if(alarm_seconds==current_seconds):
                    print("Wake Up!")
                    playsound('audio.mp3') ## download the alarm sound from link
                    break


```

************⑬******** 有声读物****

目的：编写一个Python脚本，用于将Pdf文件转换为有声读物。

提示：借助pyttsx3库将文本转换为语音。

安装：pyttsx3，PyPDF2

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

************⑭******** 天气应用****

目的：编写一个Python脚本，接收城市名称并使用爬虫获取该城市的天气信息。

提示：你可以使用Beautifulsoup和requests库直接从谷歌主页爬取数据。

安装：requests，BeautifulSoup

```


from bs4 import BeautifulSoup
import requests
headers = {'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.3'}
def weather(city):
    city=city.replace(" ","+")
    res = requests.get(f'https://www.google.com/search?q={city}&oq={city}&aqs=chrome.0.35i39l2j0l4j46j69i60.6128j1j7&sourceid=chrome&ie=UTF-8',headers=headers)
    print("Searching in google......\n")
    soup = BeautifulSoup(res.text,'html.parser')   
    location = soup.select('#wob_loc')[0].getText().strip()  
    time = soup.select('#wob_dts')[0].getText().strip()       
    info = soup.select('#wob_dc')[0].getText().strip() 
    weather = soup.select('#wob_tm')[0].getText().strip()
    print(location)
    print(time)
    print(info)
    print(weather+"°C") 
print("enter the city name")
city=input()
city=city+" weather"
weather(city)



```

************⑮******** 人脸检测****

目的：编写一个Python脚本，可以检测图像中的人脸，并将所有的人脸保存在一个文件夹中。

提示：可以使用haar级联分类器对人脸进行检测。它返回的人脸坐标信息，可以保存在一个文件中。

安装：OpenCV。

下载：haarcascade\_frontalface\_default.xml

https://raw.githubusercontent.com/opencv/opencv/master/data/haarcascades/haarcascade\_frontalface\_default.xml

```


import cv2
# Load the cascade
face_cascade = cv2.CascadeClassifier('haarcascade_frontalface_default.xml')
# Read the input image
img = cv2.imread('images/img0.jpg')
# Convert into grayscale
gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
# Detect faces
faces = face_cascade.detectMultiScale(gray, 1.3, 4)
# Draw rectangle around the faces
for (x, y, w, h) in faces:
    cv2.rectangle(img, (x, y), (x+w, y+h), (255, 0, 0), 2)
    crop_face = img[y:y + h, x:x + w]  
    cv2.imwrite(str(w) + str(h) + '_faces.jpg', crop_face)
# Display the output
cv2.imshow('img', img)
cv2.imshow("imgcropped",crop_face)
cv2.waitKey()



```

************⑯******** 提醒应用****

目的：创建一个提醒应用程序，在特定的时间提醒你做一些事情(桌面通知)。

提示：Time模块可以用来跟踪提醒时间，toastnotifier库可以用来显示桌面通知。

安装：win10toast

```


from win10toast import ToastNotifier
import time
toaster = ToastNotifier()
try:
    print("Title of reminder")
    header = input()
    print("Message of reminder")
    text = input()
    print("In how many minutes?")
    time_min = input()
    time_min=float(time_min)
except:
    header = input("Title of reminder\n")
    text = input("Message of remindar\n")
    time_min=float(input("In how many minutes?\n"))
time_min = time_min * 60
print("Setting up reminder..")
time.sleep(2)
print("all set!")
time.sleep(time_min)
toaster.show_toast(f"{header}",
f"{text}",
duration=10,
threaded=True)
while toaster.notification_active(): time.sleep(0.005)     



```

************⑰******** 维基百科文章摘要****

目的：使用一种简单的方法从用户提供的文章链接中生成摘要。

提示：你可以使用爬虫获取文章数据，通过提取生成摘要。

```


from bs4 import BeautifulSoup
import re
import requests
import heapq
from nltk.tokenize import sent_tokenize,word_tokenize
from nltk.corpus import stopwords
url = str(input("Paste the url"\n"))
num = int(input("Enter the Number of Sentence you want in the summary"))
num = int(num)
headers = {'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.3'}
#url = str(input("Paste the url......."))
res = requests.get(url,headers=headers)
summary = ""
soup = BeautifulSoup(res.text,'html.parser') 
content = soup.findAll("p")
for text in content:
    summary +=text.text 
def clean(text):
    text = re.sub(r"\[[0-9]*\]"," ",text)
    text = text.lower()
    text = re.sub(r'\s+'," ",text)
    text = re.sub(r","," ",text)
    return text
summary = clean(summary)
print("Getting the data......\n")
##Tokenixing
sent_tokens = sent_tokenize(summary)
summary = re.sub(r"[^a-zA-z]"," ",summary)
word_tokens = word_tokenize(summary)
## Removing Stop words
word_frequency = {}
stopwords =  set(stopwords.words("english"))
for word in word_tokens:
    if word not in stopwords:
        if word not in word_frequency.keys():
            word_frequency[word]=1
        else:
            word_frequency[word] +=1
maximum_frequency = max(word_frequency.values())
print(maximum_frequency)          
for word in word_frequency.keys():
    word_frequency[word] = (word_frequency[word]/maximum_frequency)
print(word_frequency)
sentences_score = {}
for sentence in sent_tokens:
    for word in word_tokenize(sentence):
        if word in word_frequency.keys():
            if (len(sentence.split(" "))) <30:
                if sentence not in sentences_score.keys():
                    sentences_score[sentence] = word_frequency[word]
                else:
                    sentences_score[sentence] += word_frequency[word]
print(max(sentences_score.values()))
def get_key(val): 
    for key, value in sentences_score.items(): 
        if val == value: 
            return key 
key = get_key(max(sentences_score.values()))
print(key+"\n")
print(sentences_score)
summary = heapq.nlargest(num,sentences_score,key=sentences_score.get)
print(" ".join(summary))
summary = " ".join(summary)



```

************⑱******** 获取谷歌搜索结果****

目的：创建一个脚本，可以根据查询条件从谷歌搜索获取数据。

```


from bs4 import BeautifulSoup 
import requests
headers = {'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.3'}
def google(query):
    query = query.replace(" ","+")
    try:
        url = f'https://www.google.com/search?q={query}&oq={query}&aqs=chrome..69i57j46j69i59j35i39j0j46j0l2.4948j0j7&sourceid=chrome&ie=UTF-8'
        res = requests.get(url,headers=headers)
        soup = BeautifulSoup(res.text,'html.parser')
    except:
        print("Make sure you have a internet connection")
    try:
        try:
            ans = soup.select('.RqBzHd')[0].getText().strip()
        except:
            try:
                title=soup.select('.AZCkJd')[0].getText().strip()
                try:
                    ans=soup.select('.e24Kjd')[0].getText().strip()
                except:
                    ans=""
                ans=f'{title}\n{ans}'
            except:
                try:
                    ans=soup.select('.hgKElc')[0].getText().strip()
                except:
                    ans=soup.select('.kno-rdesc span')[0].getText().strip()
    except:
        ans = "can't find on google"
    return ans
result = google(str(input("Query\n")))
print(result)



```

获取结果如下。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

************⑲******** 货币换算器****

目的：编写一个Python脚本，可以将一种货币转换为其他用户选择的货币。

提示：使用Python中的API，或者通过forex-python模块来获取实时的货币汇率。

安装：forex-python

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

************⑳******** 键盘记录器****

目的：编写一个Python脚本，将用户按下的所有键保存在一个文本文件中。

提示：pynput是Python中的一个库，用于控制键盘和鼠标的移动，它也可以用于制作键盘记录器。简单地读取用户按下的键，并在一定数量的键后将它们保存在一个文本文件中。

```


from pynput.keyboard import Key, Controller,Listener
import time
keyboard = Controller()
keys=[]
def on_press(key):
    global keys
    #keys.append(str(key).replace("'",""))
    string = str(key).replace("'","")
    keys.append(string)
    main_string = "".join(keys)
    print(main_string)
    if len(main_string)>15:
      with open('keys.txt', 'a') as f:
          f.write(main_string)   
          keys= []     
def on_release(key):
    if key == Key.esc:
        return False
with listener(on_press=on_press,on_release=on_release) as listener:
    listener.join()



```

************㉑******** 文章朗读器****

目的：编写一个Python脚本，自动从提供的链接读取文章。

```


import pyttsx3
import requests
from bs4 import BeautifulSoup
url = str(input("Paste article url\n"))
def content(url):
  res = requests.get(url)
  soup = BeautifulSoup(res.text,'html.parser')
  articles = []
  for i in range(len(soup.select('.p'))):
    article = soup.select('.p')[i].getText().strip()
    articles.append(article)
    contents = " ".join(articles)
  return contents
engine = pyttsx3.init('sapi5')
voices = engine.getProperty('voices')
engine.setProperty('voice', voices[0].id)
def speak(audio):
  engine.say(audio)
  engine.runAndWait()
contents = content(url)
## print(contents)      ## In case you want to see the content
#engine.save_to_file
#engine.runAndWait() ## In case if you want to save the article as a audio file



```

************㉒******** 短网址生成器****

目的：编写一个Python脚本，使用API缩短给定的URL。

```


from __future__ import with_statement
import contextlib
try:
    from urllib.parse import urlencode
except ImportError:
    from urllib import urlencode
try:
    from urllib.request import urlopen
except ImportError:
    from urllib2 import urlopen
import sys
def make_tiny(url):
    request_url = ('http://tinyurl.com/api-create.php?' + 
    urlencode({'url':url}))
    with contextlib.closing(urlopen(request_url)) as response:
        return response.read().decode('utf-8')
def main():
    for tinyurl in map(make_tiny, sys.argv[1:]):
        print(tinyurl)
if __name__ == '__main__':
    main()
-----------------------------OUTPUT------------------------
python url_shortener.py https://www.wikipedia.org/
https://tinyurl.com/buf3qt3



```

以上就是今天分享的内容，针对上面这些项目，有的可以适当调整。

比如自动发送邮件，可以选择使用自己的QQ邮箱。

天气信息也可使用国内一些免费的API，维基百科可以对应百度百科，谷歌搜索可以对应百度搜索等等。

这些都是大伙可以思考的～

****万水千山总是情，点个 👍 行不行**。**

People who liked this content also liked

分享10个超级实用事半功倍的Python自动化脚本

...

简说Python

不看的原因

- 内容质量低
- 不看此公众号

C++那些事之json解析

...

光城

不看的原因

- 内容质量低
- 不看此公众号

python免杀---进化史

...

广软NSDA安全团队

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___97def1a1540a43549.bmp"/>

Scan to Follow