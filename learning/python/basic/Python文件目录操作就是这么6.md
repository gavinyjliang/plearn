# Python文件目录操作就是这么6

<a id="profileBt"></a><a id="js_name"></a>Python程序员 *2022-04-08 08:30*

The following article is from Python爬虫与数据挖掘 Author Python进阶者

<a id="copyright_info"></a>[![](../../../_resources/0_d1b9a4c8f58d4da880bf5e078a1d1a99.jpg)<br>**Python爬虫与数据挖掘** .<br>人生苦短，我用Python。该公众号专注于分享Python网络爬虫、数据挖掘、数据分析、数据处理、数据可视化、自动化测试、运维、大数据、人工智能、云计算、机器学习等工具资源、热点资讯、相关技术文章、学习视频和学习资料等，期待您的加入~~~](#)

```


### 

```


```


```



```


```


```


























```

**/前言/**

对于学Python的人来说想必大家os这个模块不会陌生，还有一个与之紧密相关的便是sys，但是sys一般很少用，所以今天我们就来说说这两个模块到底有哪些神器的功能。

**/sys/**

sys模块包括了一组非常实用的服务，内含很多函数方法和变量，

用来处理Python运行时配置以及资源，从而可以与前当程序之外的系统环境交互。

也就是说他是一个很有用的模块，只是我们平时很少用到而已。

```
`#命令行参数List，第一个元素是程序本身路径``sys.argv` `#返回系统导入的模块字段，key是模块名，value是模块``sys.modules` `#退出程序，正常退出时exit(0)``sys.exit(n)` `#返回模块的搜索路径，初始化时使用PYTHONPATH环境变量的值``sys.path` `#标准输出``sys.stdout.write('output')`  `#标准输入``sys.stdin.readline()[:-1]``#返回所有已经导入的模块名``sys.modules.keys()` `#返回所有已经导入的模块``sys.modules.values()`  `#获取当前正在处理的异常类,exc_type、exc_value、exc_traceback当前处理的异常详细信息``sys.exc_info()` `#获取Python解释程序的版本值，16进制格式如：0x020403F0``sys.hexversion` `#获取Python解释程序的版本``sys.version`  `#解释器的C的API版本``sys.api_version` `#解释器版本信息 返回major=3, minor=6, micro=6, releaselevel='final', serial=0``#‘final’表示最终,也有’candidate’表示候选，serial表示版本级别，是否有后继的发行``sys.version_info``#如果value非空，这个函数会把他输出到sys.stdout，并且将他保存进__builtin__._.指在python的交互式解释器里，’_’ 代表上次你输入得到的结果，hook是钩子的意思，将上次的结果钩过来``sys.displayhook(value)` `#打印出给定的回溯和异常sys.stderr。``sys.excepthook（type，value,callback）` `#返回当前你所用的默认的字符编码格式``sys.getdefaultencoding()` `#返回将Unicode文件名转换成系统文件名的编码的名字``sys.getfilesystemencoding()` `#用来设置当前默认的字符编码，如果name和任何一个可用的编码都不匹配，抛出 LookupError，这个函数只会被site模块的sitecustomize使用，一旦别site模块使用了，他会从sys模块移除``sys.setdefaultencoding(name)``#Python解释器导入的模块列表``sys.builtin_module_names` `#Python解释程序路径``sys.executable` `#获取Windows的版本``sys.getwindowsversion()` `#记录python版权相关的东西``sys.copyright` `#本地字节规则的指示器，big-endian平台的值是’big’,little-endian平台的值是’little’``sys.byteorder` `#用来清除当前线程所出现的当前的或最近的错误信息``sys.exc_clear()` `#返回平台独立的python文件安装的位置``sys.exec_prefix` `#错误输出``sys.stderr` `#标准输入``sys.stdin` `#标准输出``sys.stdout` `#返回操作系统平台名称``sys.platform` `#最大的Unicode值``sys.maxunicode` `#最大的Int值``sys.maxint` `#呼叫func(*args)，同时启用跟踪。跟踪状态被保存，然后恢复。``#这是从调试器从检查点调用，以递归调试其他一些代码。``sys.call_tracing（func，args )` `#清除内部类型缓存。类型缓存用于加速属性和方法查找``sys._clear_type_cache（）` `#返回一个字典，将每个线程的标识符映射到调用该函数时该线程中当前活动的最顶层堆栈帧。``sys._current_frames（）` `#指定Python DLL句柄的整数。``sys.dllhandle`  `#如果这是真的，Python将不会尝试在源模块的导入上编写.pyc或.pyo文件。``#此值最初设置为True或 False取决于-B命令行选项``#和 PYTHONDONTWRITEBYTECODE 环境变量，但您可以自己设置它来控制字节码文件的生成。``sys.dont_write_bytecode` `#获取检查间隔``sys.getcheckinterval()``#返回用于dlopen()调用的标志的当前值。标志常量在dl和DLFCN模块中定义。可用性：Unix。``sys.getdlopenflags（）` `#返回对象的引用计数。返回的计数通常比您预期的高一个，因为它包含（临时）引用作为参数getrefcount()。``sys.getrefcount（对象）``#以字节为单位返回对象的大小。对象可以是任何类型的对象。所有内置对象都将返回正确的结果，但这不一定适用于第三方扩展，因为它是特定于实现的。``#如果给定，则如果对象未提供检索大小的方法，则将返回default。否则TypeError将被提出。``#如果对象由垃圾收集器管理，则调用该对象的方法并添加额外的垃圾收集器开销。``sys.getsizeof（对象[，默认] ）` `#从调用堆栈返回一个框架对象。如果给出了可选的整数深度，``#则返回堆栈顶部下方多次调用的帧对象。如果它比调用堆栈更深，``#ValueError则引发。深度的默认值为零，返回调用堆栈顶部的帧。``sys._getframe（[ 深度] ）` `#获取设置的探查器功能setprofile()。``sys.getprofile（）` `#获取设置的跟踪功能settrace()。``sys.gettrace（）` `#返回一个描述当前正在运行的Windows版本的命名元组``sys.getwindowsversion（）`
```

看到这么多方法大家是不是已经哭晕在厕所里了了，不着急，

小编在这里先给大家一个练手的好案例让大家开开荤：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

那么这个案例实现的是什么功能了，大致就是每隔一秒打印一个#，好像进度条一样的说。

在这里就用到标准输出流和输出流刷新了，也算是超级简单的进度条了。

sys里面有几个可以表示脚本当前路径的函数，可以说是非常强大了，比如说：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

大家可以看到，后面两个结果相同，说明后面两个效果一样吗，那可不一定。

不信，你可以看看我接下来的操作，让你怀疑人生：

创建两个py文件，你懂的，然后分别键入代码：

```
`#cg.py``import sys``def ff():` `print(sys.path[0],':',sys.argv[0],':',__file__)` `#sys.argv[0] 一般得到的是相对路径``ff()``#fd.py``import cg``cg.ff()`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

可以看到，最后的结果就是显然不一样了，那么这是什么原因产生的了，哦，

原来，在fd中导入cg执行的sys.argv\[0\]还是指的是运行的主文件fd.py,而file却输出的是cg.py.

顺便跟大家科普以下查看模块下有哪些方法的一个函数：dir()

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

所有的函数都打印出来了，当然，如果你想快速了解每个方法的用法，则可以使用help()函数：

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

可以说是很强大了。

接下来我们再来说说另一个非常牛逼的东西---------os。

**/OS/**

一、文件操作（os直属常用方法）

```
`# 关闭文件描述符 hw``os.close(hw)` `# 关闭所有文件描述符，从 hw1(包含) 到 hw2(不包含), 错误会忽略``os.closerange(hw1,hw2)` `# 通过文件描述符改变当前工作目录``os.fchdir(hw)` `# 改变一个文件的访问权限，该文件由参数hw指定，参数mode是Unix下的文件访问权限。``os.fchmod(hw, mode)` `# 修改一个文件的所有权，这个函数修改一个文件的用户ID和用户组ID，该文件由文件描述符hw指定。``os.fchown(hw, id1, id2)` `# 强制将文件写入磁盘，该文件由文件描述符hw指定，但是不强制更新文件的状态信息。``os.fdatasync(hw)` `#通过文件描述符hw 创建一个文件对象，并返回这个文件对象``os.fdopen(hw[, mode[, bufsize]])``#返回一个打开的文件的系统配置信息。name为检索的系统配置的值，``#它也许是一个定义系统值的字符串，这些名字在很多标准中指定（POSIX.1, Unix 95, Unix 98, 和其它）。``os.fpathconf(hw, name)` `#返回文件描述符hw的状态，像stat()。``os.fstat(hw)` `#返回包含文件描述符hw的文件的文件系统的信息，像 statvfs()``os.fstatvfs(hw)` `#强制将文件描述符为hw的文件写入硬盘。``os.fsync(hw)` `#裁剪文件描述符hw对应的文件, 所以它最大不能超过文件大小。``os.ftruncate(hw,length)` `#复制文件描述符hw``os.dup(hw)` `# 将一个文件描述符hw复制到另一个hw1``os.dup2(hw,hw1)` `# 返回与终端hw（一个由os.open()返回的打开的文件描述符）关联的进程组``os.tcgetpgrp(hw)` `# 设置与终端hw（一个由os.open()返回的打开的文件描述符）关联的进程组为hw1。``os.tcsetpgrp(hw, hw1)` `# 返回唯一的路径名用于创建临时文件。``os.tempnam([dir[, prefix]])` `# 返回一个打开的模式为(w+b)的文件对象 .这文件对象没有文件夹入口，没有文件描述符，将会自动删除。``os.tmpfile()` `# 为创建一个临时文件返回一个唯一的路径``os.tmpnam()` `# 返回一个字符串，它表示与文件描述符hw 关联的终端设备。如果hw 没有与终端设备关联，则引发一个异常。``os.ttyname(hw)` `# 删除文件路径``os.unlink(path)` `# 返回指定的path文件的访问和修改的时间。``os.utime(path, times)` `# 输出在文件夹中的文件名通过在树中游走，向上或者向下。``os.walk(top[, topdown=True[, onerror=None[, followlinks=False]]])` `# 写入字符串到文件描述符hw中. 返回实际写入的字符串长度``os.write(hw, str)` `# 获取指定路径的文件系统统计信息``os.statvfs(path)` `# 返回当前工作目录``os.getcwd()` `# 返回一个当前工作目录的Unicode对象``os.getcwdu()` `# 如果文件描述符hw是打开的，同时与tty(-like)设备相连，则返回true, 否则False。``os.isatty(hw)` `# 设置路径的标记为数字标记，类似 chflags()，但是没有软链接``os.lchflags(path, flags)` `# 修改连接文件权限``os.lchmod(path, mode)` `# 更改文件所有者，类似 chown，但是不追踪链接。``os.lchown(path, uid, gid)` `# 返回path指定的文件夹包含的文件或文件夹的名字的列表。``os.listdir(path)` `# 设置文件描述符 fd当前位置为pos, how方式修改: SEEK_SET 或者 0` `#设置从文件开始的计算的pos; SEEK_CUR或者 1 则从当前位置计算;` `#os.SEEK_END或者2则从文件尾部开始. 在unix，Windows中有效``os.lseek(fd, pos, how)` `# 打开一个文件，并且设置需要的打开选项，mode参数是可选的``os.open(file, flags[, mode])` `# 打开一个新的伪终端对。返回 pty 和 tty的文件描述符。``os.openpty()` `# 返回相关文件的系统配置信息。``os.pathconf(path, name)` `# 用于分割文件路径的字符串``os.pathsep` `# 获取当前目录的父目录字符串名：('..')``os.pardir` `# 创建一个管道. 返回一对文件描述符(r, w) 分别为读和写``os.pipe()` `# 从一个 command 打开一个管道``os.popen(command[, mode[, bufsize]])` `# 字符串指示当前使用平台。win->'nt'; Linux->'posix'``os.name` `# 从文件描述符hw中读取最多n个字节，返回包含读取字节的字符串，``#文件描述符hw对应文件已达到结尾, 返回一个空字符串。``os.read(hw, n)` `#返回路径所在的根目录，主目录主文件``os.walk(path)``#返回路径下的目录和文件``os.listdir(path)``#切换当前程序操作的路径``os.chdir(path)` `#重命名文件``os.rename(oldfile,newfile)`
```

二、路径操作

```
`#将文件路径和文件名分割(会将最后一个目录作为文件名而分离)``os.path.split(filename)``#将文件路径和文件扩展名分割成一个元组``os.path.splitext(filename)` `#返回文件路径的目录部分``os.path.dirname(filename)` `#返回文件路径的文件名部分``os.path.basename(filename)` `#将文件路径和文件名凑成完整文件路径``os.path.join(dirname,basename)` `#获得绝对路径``os.path.abspath(name)` `#把路径分割为挂载点和文件名``os.path.splitunc(path)` `#规范path字符串形式``os.path.normpath(path)` `#判断文件或目录是否存在``os.path.exists()` `#如果path是绝对路径，返回True``os.path.isabs()` `#返回path的真实路径``os.path.realpath(path)``#从start开始计算相对路径``os.path.relpath(path[, start])` `#转换path的大小写和斜杠``os.path.normcase(path)` `#判断name是不是一个目录，name不是目录就返回false``os.path.isdir()` `#判断name是不是一个文件，不存在返回false``os.path.isfile()` `#判断文件是否连接文件,返回boolean``os.path.islink()` `#指定路径是否存在且为一个挂载点，返回boolean``os.path.ismount()` `#是否相同路径的文件，返回boolean``os.path.samefile()` `#返回最近访问时间 浮点型``os.path.getatime()` `#返回上一次修改时间 浮点型``os.path.getmtime()` `#返回文件创建时间 浮点型``os.path.getctime()` `#返回文件大小 字节单位``os.path.getsize()` `#返回list(多个路径)中，所有path共有的最长的路径``os.path.commonprefix(list)` `#路径存在则返回True,路径损坏也返回True``os.path.lexists` `#把path中包含的”~”和”~user”转换成用户目录``os.path.expanduser(path)` `#根据环境变量的值替换path中包含的”$name”和”${name}”``os.path.expandvars(path)` `#判断fp1和fp2是否指向同一文件``os.path.sameopenfile(fp1, fp2)` `#判断stat tuple stat1和stat2是否指向同一个文件``os.path.samestat(stat1, stat2)``#一般用在windows下，返回驱动器名和路径组成的元组``os.path.splitdrive(path)` `#遍历path，给每个path执行一个函数``os.path.walk(path, visit, arg)` `#设置是否支持unicode路径名``os.path.supports_unicode_filenames()`
```

```


```

三、进程管理

```
`# 创建一个软链接``os.symlink(src, dst)` `# 创建硬链接，名为参数 dst，指向参数 src``os.link(src, dst)` `# 像stat(),但是没有软链接``os.lstat(path)` `# 运行shell命令，直接显示``os.system("bash command")` `# 改变当前进程的根目录``os.chroot(path)` `#shell命令，不返回结果，但是可以进行读取``os.popen(command).read()``# 当前平台使用的行终止符，win下为"\t\n",Linux下为"\n"``os.linesep` `#路径连接符``os.sep``#从原始的设备号中提取设备major号码 (使用stat中的st_dev或者st_rdev field)。``os.major(device)` `# 以major和minor设备号组成一个原始设备号``os.makedev(major, minor)` `# 递归文件夹创建函数。像mkdir()，但创建的所有intermediate-level文件夹需要包含子文件夹。``os.makedirs(path[, mode])` `# 从原始的设备号中提取设备minor号码(使用stat中的st_dev或者st_rdev field )。``os.minor(device)``# 以数字mode的mode创建一个名为path的文件夹.默认的 mode 是 0777 (八进制)。``os.mkdir(path[, mode])` `# 创建命名管道，mode 为数字，默认为 0666 (八进制)``os.mkfifo(path[, mode])` `# 创建一个名为filename文件系统节点（文件，设备特别文件或者命名pipe）。``os.mknod(filename[, mode=0600, device])`
```

```


```

四、环境参数

```
`#检验权限模式` `os.access(path, mode)` `# 设置路径的标记为数字标记。``os.chflags(path, flags)` `# 更改权限``os.chmod(path, mode)` `# 更改文件所有者``os.chown(path, uid, gid)` `#获得当前系统登陆用户名``os.getlogin()` `#获得当前系统的CPU数量``os.cpu_count()` `# 获得n个字节长度的随机字符串，通常用于加解密运算``os.urandom(n)` `# 获取系统所有环境变量``os.environ` `#获取某个特定的环境变量的值``os.getenv('CYGWIN')``os.environ.get('CYGWIN')``#设置环境变量``os.environ.setdefault(key,value)``#获取某个特定的环境变量的值的byte形式``os.getenvb('CYGWIN')`
```

五、stat子库

```
`#获取到的文件属性列表``fileStats = os.stat(path)` `#获取文件的模式``fileStats[stat.ST_MODE]` `#文件大小``fileStats[stat.ST_SIZE]` `#文件最后修改时间``fileStats[stat.ST_MTIME]` `#文件最后访问时间``fileStats[stat.ST_ATIME]` `#文件创建时间``fileStats[stat.ST_CTIME]` `#是否目录``stat.S_ISDIR(fileStats[stat.ST_MODE])` `#是否一般文件``stat.S_ISREG(fileStats[stat.ST_MODE])` `#是否连接文件``stat.S_ISLNK(fileStats[stat.ST_MODE])` `#是否COCK文件``stat.S_ISSOCK(fileStats[stat.ST_MODE])` `#是否命名管道``stat.S_ISFIFO(fileStats[stat.ST_MODE])` `#是否块设备``stat.S_ISBLK(fileStats[stat.ST_MODE])``#是否字符设置``stat.S_ISCHR(fileStats[stat.ST_MODE])`
```

6、一键死机

为什么要说这个呢？完全是为了好玩，哈哈哈。

我们都知道os 能很轻松打开一个程序，只要你知道这个源程序根目录的位置。

所以我们一般都会：

```
`os.system('notepad')  #打开记事本``os.system('start notepad')#打开记事本`
```

这样可以很轻松打开一个记事本，那么我们要想让它无限打开。我给它整个死循环，给它个无限递归不就完了。是不是很简单了？

还有一种比较优雅的打开程序的方法，那就是popen方法：

```
pp=os.popen('notebook')
```

不仅如此，我们还能看到它执行结果后的返回值。

```
print(pp.read())
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

哈哈，也是很666了。

**/小结/**

1、本文基于Python编程语言，主要介绍了Python操作文件目录的两个大库sys和os，在实际应用中十分的常用，希望大家了解。本文内容十分丰富，欢迎大家积极学习。

2、结合部分实际案例，图文并茂，帮助大家更好的理解文件操作。

People who liked this content also liked

又一款python开发神器

...

基因学苑

不看的原因

- 内容质量低
- 不看此公众号

如何使用Python的lambda、map和filter函数

...

完美Excel

不看的原因

- 内容质量低
- 不看此公众号

用Python去除图片背景：​Rembg库

...

Python大数据分析

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___6cc7b7bd163e4142a.bmp"/>

Scan to Follow