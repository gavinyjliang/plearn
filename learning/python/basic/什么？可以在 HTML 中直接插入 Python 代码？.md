# 什么？可以在 HTML 中直接插入 Python 代码？

<a id="profileBt"></a><a id="js_name"></a>k8s技术圈 *2022-05-07 21:22*

The following article is from Github爱好者 Author PyScript

<a id="copyright_info"></a>[![](../../../_resources/0_116ce7a104d6459fa3d206a98d7882ca.jpg)<br>**Github爱好者** .<br>我们是一群 Github 爱好者，专注分享有价值、有趣的开源项目和学习资料，包括 Python、Golang、Java、Rust、AI、前端、运维、数据分析、大数据、云计算、Kubernetes、Service Mesh 等领域资源。](#)

PyScript 由来自 Anaconda 的团队开发，是一个用于在 HTML 中插入 Python 代码的工具，这意味着你可以在 HTML 中编写和运行 Python 代码，在 PyScript 中调用 Javascript 库，并在 Python 中进行所有的 Web 开发，是不是很神奇！

<img width="657" height="335" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__05b5454cb1804ab7b.png"/>

有了 PyScript，我们现在可以在 HTML 中编写 Python 代码并构建 Web 应用了，PyScript 可以让更多的前端开发者接触到 Python 的强大功能。有了 PyScript，我们不再需要担心部署问题，因为一切都将在你的浏览器中执行，比如作为数据科学家，你可以在一个 html 文件中分享模型，只要其他人在浏览器中打开该文件，就会运行代码了。

<img width="657" height="251" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_af33b7eb830043569.jpg"/>

PyScript 是基于 Pyodide(https://pyodide.org/) 开发的，它是 **CPython 到 WebAssembly/Emscripten 的入口**，PyScript 支持在浏览器中编写和运行 Python 代码，未来还将提供对其他语言的支持。

![Image](../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_1169394f72c141b58.jpg)

## 什么是 WebAssembly？

使得用 Python 编写网站成为可能的基础技术是 **WebAssembly**。最初开发 WebAssembly 时，Web 浏览器仅支持 Javascript。WebAssembly 于 2017 年首次发布，到 2019 年迅速成为了 W3C 的官方标准，它包括一种人类可读的 `.wat` 文本格式语言，然后将其转换为浏览器可以运行的二进制 `.wasm` 格式，这就使得我们可以用任何语言编写代码，将其编译为 WebAssembly，然后在网络浏览器中运行。

## 如何使用 PyScript？

PyScript 的 alpha 版本可以在 pyscript.net 上找到，代码可在 https://github.com/pyscript 获得。PyScript 允许你使用以下三个主要组件在 html 中编写 Python：

- 定义了运行 Python 代码所需的 Python 包。
    
- 是你编写在网页中执行的 Python 代码的地方。
    
- 创建一个 REPL 组件，用于评估用户输入的代码并显示结果。
    

我们先创建一个最简单的示例，代码如下所示：

```
`<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width,initial-scale=1" />
    <title>PyScript Hello World</title>
    <link rel="icon" type="image/png" href="favicon.png" />
    <link rel="stylesheet" href="https://pyscript.net/alpha/pyscript.css" />
    <script defer src="https://pyscript.net/alpha/pyscript.js"></script>
  </head>
  <body>
    Hello world! <br />
    This is the current date and time, as computed by Python:
    <py-script>
from datetime import datetime
now = datetime.now()
now.strftime("%m/%d/%Y, %H:%M:%S")
    </py-script>
  </body>
</html>
`
```

只需要这页面上显示当前时间即可，在浏览器中打开后就可以看到结果了，如下所示：

<img width="657" height="283" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__e67fa6fec8bc44b4b.png"/>

又比如我们创建一个具有流式数据的 Panel 仪表盘，代码如下所示，先通过 `<py-env>` 引入需要的包，然后在 `<py-script>` 中编写 Python 代码，如果你不喜欢直接在下编写 Python 代码，也可以使用 Python 文件作为源代码，例如。

```
``<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <link rel="icon" type="image/x-icon" href="./favicon.png">
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="default">
    <meta name="theme-color" content="#000000">
    <meta name="name" content="PyScript/Panel Streaming Demo">
    <title>PyScript/Panel Streaming Demo</title>
    <link rel="icon" type="image/x-icon" href="./favicon.ico">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.15.3/css/all.min.css" type="text/css" />
    <link rel="stylesheet" href="https://unpkg.com/@holoviz/panel@0.13.0/dist/css/widgets.css" type="text/css" />
    <link rel="stylesheet" href="https://unpkg.com/@holoviz/panel@0.13.0/dist/css/markdown.css" type="text/css" />
    <script type="text/javascript" src="https://unpkg.com/tabulator-tables@4.9.3/dist/js/tabulator.js"></script>
    <script type="text/javascript" src="https://cdn.bokeh.org/bokeh/release/bokeh-2.4.2.js"></script>
    <script type="text/javascript" src="https://cdn.bokeh.org/bokeh/release/bokeh-widgets-2.4.2.min.js"></script>
    <script type="text/javascript" src="https://cdn.bokeh.org/bokeh/release/bokeh-tables-2.4.2.min.js"></script>
    <script type="text/javascript" src="https://unpkg.com/@holoviz/panel@0.13.0/dist/panel.min.js"></script>
    <script type="text/javascript">
      Bokeh.set_log_level("info");
    </script>
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@4.6.1/dist/css/bootstrap.min.css">
    <link rel="stylesheet" href="https://unpkg.com/@holoviz/panel@0.13.0/dist/bundled/bootstraptemplate/bootstrap.css">
    <link rel="stylesheet" href="https://unpkg.com/@holoviz/panel@0.13.0/dist/bundled/defaulttheme/default.css">
    <script src="https://cdn.jsdelivr.net/npm/jquery@3.5.1/dist/jquery.slim.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@4.6.1/dist/js/bootstrap.bundle.min.js"></script>
    <link rel="stylesheet" href="https://pyscript.net/alpha/pyscript.css" />
    <script defer src="https://pyscript.net/alpha/pyscript.js"></script>
  </head>
  <py-env>
  - bokeh
  - numpy
  - pandas
  - panel
  </py-env>
  <body>
    <div class="container-fluid d-flex flex-column vh-100 overflow-hidden" id="container">
      <nav class="navbar navbar-expand-md navbar-dark sticky-top shadow" id="header" style="background-color: #000000;">
        <button type="button" class="navbar-toggle collapsed" id="sidebarCollapse">
   <span class="navbar-toggler-icon"></span>
 </button>
 <div class="app-header">
   <a class="navbar-brand app-logo" href="/">
     <img src="./logo.png" class="app-logo">
   </a>
   <a class="title" href="" style="color: #f0ab3c;">Panel Streaming Demo</a>
 </div>
      </nav>
      <div class="row overflow-hidden" id="content">
        <div class="col mh-100 float-left" id="main">
          <div class="bk-root" id="controls"></div>
          <div class="row">
            <div class="bk-root" id="table"></div>
            <div class="bk-root" id="plot"></div>
          </div>
        </div>
      </div>
    </div>
    <py-script>
import asyncio
import panel as pn
import numpy as np
import pandas as pd
from bokeh.models import ColumnDataSource
from bokeh.plotting import figure
from panel.io.pyodide import show
df = pd.DataFrame(np.random.randn(10, 4), columns=list('ABCD')).cumsum()
rollover = pn.widgets.IntInput(name='Rollover', value=15)
follow = pn.widgets.Checkbox(name='Follow', value=True, align='end')
tabulator = pn.widgets.Tabulator(df, height=450, width=400)
def color_negative_red(val):
    """
    Takes a scalar and returns a string with
    the css property `'color: red'` for negative
    strings, black otherwise.
    """
    color = 'red' if val < 0 else 'green'
    return 'color: %s' % color
tabulator.style.applymap(color_negative_red)
p = figure(height=450, width=600)
cds = ColumnDataSource(data=ColumnDataSource.from_df(df))
p.line('index', 'A', source=cds, line_color='red')
p.line('index', 'B', source=cds, line_color='green')
p.line('index', 'C', source=cds, line_color='blue')
p.line('index', 'D', source=cds, line_color='purple')
def stream():
    data = df.iloc[-1] + np.random.randn(4)
    tabulator.stream(data, rollover=rollover.value, follow=follow.value)
    value = {k: [v] for k, v in tabulator.value.iloc[-1].to_dict().items()}
    value['index'] = [tabulator.value.index[-1]]
    cds.stream(value)
cb = pn.state.add_periodic_callback(stream, 200)
controls = pn.Row(cb.param.period, rollover, follow, width=400)
await show(controls, 'controls')
await show(tabulator, 'table')
await show(p, 'plot')
    </py-script>
  </body>
</html>
``
```

上面的代码在浏览器中打开后就可以直接显示对应的效果了：

<img width="657" height="392" src="../../../_resources/640_wx_fmt_gif_wxfrom_5_wx_lazy__c0cb727f23ce4d379.gif"/>

关于 PyScript 的更多使用方法可以查看 Git 仓库示例 https://github.com/pyscript/pyscript/tree/main/pyscriptjs/examples 了解更多。

> Git 仓库：https://github.com/pyscript/pyscript

![](../../../_resources/0_wx_fmt_png_09ac46bd95144998b1477cf214e4f4f2.png)

**Github爱好者**

我们是一群 Github 爱好者，专注分享有价值、有趣的开源项目和学习资料，包括 Python、Golang、Java、Rust、AI、前端、运维、数据分析、大数据、云计算、Kubernetes、Service Mesh 等领域资源。

<a id="js_profile_article"></a>38篇原创内容

Official Account

Posted on 四川

<a id="js_view_source"></a>Read more

People who liked this content also liked

针对企业的信息侦察 \- Base on ATT&CK

...

关注安全技术

不看的原因

- 内容质量低
- 不看此公众号

Julia开源新框架SimpleChain：小型神经网络速度比PyTorch快5倍！

...

新智元

不看的原因

- 内容质量低
- 不看此公众号

py-libterraform 的使用和实现：一个 Terraform 的 Python 绑定

...

微软中国MSDN

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___ada395cdc0e548c8a.bmp"/>

Scan to Follow