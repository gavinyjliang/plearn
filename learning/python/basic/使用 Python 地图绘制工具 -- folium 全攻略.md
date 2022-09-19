# 使用 Python 地图绘制工具 -- folium 全攻略

<a id="profileBt"></a><a id="js_name"></a>Python编程时光 *2021-12-13 09:02*

The following article is from 可以叫我才哥 Author 道才

<a id="copyright_info"></a>[![](../../../_resources/0_8eb0cf183ae240a591ea7357ba9ded10.jpg)<br>**可以叫我才哥** .<br>学python，玩转数据分析、网络爬虫以及可视化等等](#)

今天借朋友的一篇文章，跟大家介绍一一下如何使用`folium`**更换地图底图样式。**

**1\. 准备工作**

有朋友可能没用过`folium`，它其实就是`python`的一个**专业绘制地图**的第三方库，所以在使用之前需要先安装它。

```
`pip install folium
`
```

在安装完成之后，我们可以在`jupyterlab`进行演示如下：

```
`import folium
m = folium.Map()
m
`
```

<img width="657" height="395" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__329934cce53947498.png"/>

默认

对于上面的输出，其实是一个`可交互`的地图，支持放大缩写拖拽等等。

如果你想将输出存在本地，可以这样来：

```
`m.save('map.html')
`
```

可以看到本地就存了这个一个文件，浏览器打开就可以进行交互式操作了。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

map文件

以上就是一个平平淡淡的过程......

**2\. 关于folium.Map()**

在上一部分我们可以看到这个`map`玩意直接就是一个地图啦，这里我们就介绍一下它常用的几个参数。

```
`folium.Map(
    location=None,
    width='100%',
    height='100%',
    left='0%',
    top='0%',
    position='relative',
    tiles='OpenStreetMap',
    attr=None,
    min_zoom=0,
    max_zoom=18,
    zoom_start=10,
    min_lat=-90,
    max_lat=90,
    min_lon=-180,
    max_lon=180,
    max_bounds=False,
    crs='EPSG3857',
    control_scale=False,
    prefer_canvas=False,
    no_touch=False,
    disable_3d=False,
    png_enabled=False,
    zoom_control=True,
    **kwargs,
)
`
```

参数可真多啊！！

> 没有参数的`folium.Map()`将得到一张世界地图。
> 
> - **location**：地图中心，\[40.002694, 116.322373\]是清华大学校区；
>     
> - **zoom_start**：比例尺，默认为10级，大约是一个城市的范围；
>     
> 
> 其他常用参数包括：
> 
> - `width`和`height`：地图的长宽，如果是int则表示像素值，如果是str则表示百分比；
>     
> - `max_zoom`：地图可以手动调节的最大比例，默认为18级；
>     
> - `control_scale`：是否在地图上添加比例尺，默认为False；
>     
> - `no_touch`：是否禁止手动操作，默认为False；
>     
> - **tiles**：地图样式，默认为OpenStreetMap
>     
> - `attr`：如果设置非内建地图样式，则需要传入这个值，可以理解为你选择的地图样式名称
>     

以上是常用的一些参数，而最常用的莫过于 `location`、`zoom_start`和`tiles`等。

**内建地图样式**还有一下几种：

```
`- "OpenStreetMap"
- "Mapbox Bright" (Limited levels of zoom for free tiles)
- "Mapbox Control Room" (Limited levels of zoom for free tiles)
- "Stamen" (Terrain, Toner, and Watercolor)
- "Cloudmade" (Must pass API key)
- "Mapbox" (Must pass API key)
- "CartoDB" (positron and dark_matter)
`
```

我们简单试下`location`和`zoo_start`参数：

```
`import folium
m = folium.Map([40.002694, 116.322373],
               zoom_start=15,
               control_scale=True
              )
m
`
```

可以看到**清华大学校区**

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

清华大学校区

以上对`Map`的参数进行了简单的介绍，接下来，我们就来看看地图底图样式的选取情况吧~

**3\. 内建地图底图样式**

我们看到`folium`其实有好几种内建地图底图样式，其中部分需要去申请`key`，由于我这边没有申请成功就不做演示了。

- "OpenStreetMap"
    
- "Mapbox Bright" (Limited levels of zoom for free tiles)
    
- "Mapbox Control Room" (Limited levels of zoom for free tiles)
    
- "Stamen" (Terrain, Toner, and Watercolor)
    
- "Cloudmade" (Must pass API key)
    
- "Mapbox" (Must pass API key)
    
- "CartoDB" (positron and dark_matter)
    

**地势地形底图**

```
`m = folium.Map([40.002694, 116.322373],
               tiles='Stamen Terrain',
               zoom_start=15,
               control_scale=True
              )
m
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

**黑白无标记底图**

```
`m = folium.Map([40.002694, 116.322373],
               tiles='Stamen Toner',
               zoom_start=15,
               control_scale=True
              )
m
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

**水墨画底图**

```
`m = folium.Map([40.002694, 116.322373],
               tiles='Stamen Watercolor',
               zoom_start=15,
               control_scale=True
              )
m
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

以上就是内建地图底图样式的一些展示，部分需要key的大家可以去这个网站申请：

> http://openwhatevermap.xyz/（可惜我上不去）

另外，在这里也可以找到一些地图底图

> http://leaflet-extras.github.io/leaflet-providers/preview/

我后续也会去研究这些地图底图样式，试着分享更多有趣的地图分享给大家。

当然了，国内咱们用的较多的地图是高德、百度和腾讯地图等，接下来我们就来玩玩！

**4\. 多种第三方地图底图样式**

这里我将演示高德地图、智图GeoQ和腾讯地图等

### 4.1. 高德地图

高德地图的 中英文地图、卫星影像图、街道图与常规图

**中英文地图**

```
`folium.Map([40.002694, 116.322373],
           tiles='https://webrd02.is.autonavi.com/appmaptile?lang=zh_en&size=1&scale=1&style=8&x={x}&y={y}&z={z}',
           attr='高德-中英文对照',
           zoom_start=15,
          )
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

**纯英文地图**

```
`folium.Map([40.002694, 116.322373],
           tiles='https://webrd02.is.autonavi.com/appmaptile?lang=en&size=1&scale=1&style=8&x={x}&y={y}&z={z}',
           attr='高德-纯英文对照',
           zoom_start=15,
          )
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

**卫星影像图**

```
`tiles = 'https://webst02.is.autonavi.com/appmaptile?style=6&x={x}&y={y}&z={z}'
folium.Map([40.002694, 116.322373],
           tiles= tiles,
           attr='高德-卫星影像图',
           zoom_start=15,
          )
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

**街道图**

```
`folium.Map([40.002694, 116.322373],
           tiles= 'https://wprd01.is.autonavi.com/appmaptile?x={x}&y={y}&z={z}&lang=zh_cn&size=1&scl=1&style=8&ltype=11',
           attr='高德-街道路网图',
           zoom_start=10,
          )
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

**常规图**

```
`folium.Map([40.002694, 116.322373],
           tiles= 'https://wprd01.is.autonavi.com/appmaptile?x={x}&y={y}&z={z}&lang=zh_cn&size=1&scl=1&style=7',
           attr='高德-常规图',
           zoom_start=15,
          )
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 4.2. 智图GeoQ

反正我觉得这个蛮好的，用起来简单

多种风格地图，即拿即用

**彩色版**

```
`m = folium.Map([40.002694, 116.322373],
               tiles='http://map.geoq.cn/ArcGIS/rest/services/ChinaOnlineCommunity/MapServer/tile/{z}/{y}/{x}',
               attr='彩色版',
               zoom_start=15,
              )
m
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

**暖色版**

```
`m = folium.Map([40.002694, 116.322373],
               tiles='http://map.geoq.cn/ArcGIS/rest/services/ChinaOnlineStreetWarm/MapServer/tile/{z}/{y}/{x}',
               attr='暖色版',
               zoom_start=15,
              )
m
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

**灰色版**

```
`m = folium.Map([40.002694, 116.322373],
               tiles='http://map.geoq.cn/ArcGIS/rest/services/ChinaOnlineStreetGray/MapServer/tile/{z}/{y}/{x}',
               attr='灰色版',
               zoom_start=15,
              )
m
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

**蓝黑版**

```
`m = folium.Map([40.002694, 116.322373],
               tiles='http://map.geoq.cn/ArcGIS/rest/services/ChinaOnlineStreetPurplishBlue/MapServer/tile/{z}/{y}/{x}',
               attr='蓝黑版',
               zoom_start=15,
              )
m
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

**英文版**

```
`m = folium.Map([40.002694, 116.322373],
               tiles='http://map.geoq.cn/ArcGIS/rest/services/ChinaOnlineCommunityENG/MapServer/tile/{z}/{y}/{x}',
               attr='英文版',
               zoom_start=15,
              )
m
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

**中国行政区划边界**

```
`m = folium.Map([40.002694, 116.322373],
               tiles='http://thematic.geoq.cn/arcgis/rest/services/ThematicMaps/administrative_division_boundaryandlabel/MapServer/tile/{z}/{y}/{x}',
               attr='中国行政区划边界',
              )
m
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

**水系专题**

```
`m = folium.Map([40.002694, 116.322373],
               tiles='http://thematic.geoq.cn/arcgis/rest/services/ThematicMaps/WorldHydroMap/MapServer/tile/{z}/{y}/{x}',
               attr='水系专题',
              )
m
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

**街道网图**

```
`m = folium.Map([40.002694, 116.322373],
               tiles='http://thematic.geoq.cn/arcgis/rest/services/StreetThematicMaps/Gray_OnlySymbol/MapServer/tile/{z}/{y}/{x}',
               attr='街道网图',
              )
m
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

**暖色街道网图**

```
`m = folium.Map([40.002694, 116.322373],
               tiles='http://thematic.geoq.cn/arcgis/rest/services/StreetThematicMaps/Warm_OnlySymbol/MapServer/tile/{z}/{y}/{x}',
               attr='暖色-街道网图',
              )
m
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 4.3. 腾讯地图

```
`tiles =  'https://rt0.map.gtimg.com/tile?z={z}&x={x}&y={-y}'
folium.Map([39.904989, 116.405285],
           tiles= tiles,
           attr='腾讯地图'          
          )
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 4.4. 天地图

> https://www.tianditu.gov.cn/

需要注册一个key

**天地图影像**

```
`m = folium.Map([40.002694, 116.322373],
               tiles='http://t7.tianditu.gov.cn/img_w/wmts?SERVICE=WMTS&REQUEST=GetTile&VERSION=1.0.0&LAYER=img&STYLE=default&TILEMATRIXSET=w&FORMAT=tiles&TILEMATRIX={z}&TILEROW={y}&TILECOL={x}&tk=',
               attr='天地图-影像'
              )
m
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

**天地图影像注记**

```
`m = folium.Map([40.002694, 116.322373],
               tiles='http://t7.tianditu.gov.cn/cia_w/wmts?SERVICE=WMTS&REQUEST=GetTile&VERSION=1.0.0&LAYER=cia&STYLE=default&TILEMATRIXSET=w&FORMAT=tiles&TILEMATRIX={z}&TILEROW={y}&TILECOL={x}&tk=',
               attr='天地图-影像标注'
              )
m
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

**天地图矢量**

```
`m = folium.Map([40.002694, 116.322373],
               tiles='http://t7.tianditu.gov.cn/vec_w/wmts?SERVICE=WMTS&REQUEST=GetTile&VERSION=1.0.0&LAYER=vec&STYLE=default&TILEMATRIXSET=w&FORMAT=tiles&TILEMATRIX={z}&TILEROW={y}&TILECOL={x}&tk=',
               attr='天地图-矢量',
               zoom_start=10,
              )
m
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

**天地图矢量注记**

```
`m = folium.Map([40.002694, 116.322373],
               tiles='http://t7.tianditu.gov.cn/cva_w/wmts?SERVICE=WMTS&REQUEST=GetTile&VERSION=1.0.0&LAYER=cva&STYLE=default&TILEMATRIXSET=w&FORMAT=tiles&TILEMATRIX={z}&TILEROW={y}&TILECOL={x}&tk=',
               attr='天地图-矢量注记'
              )
m
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

**天地图地形**

```
`m = folium.Map([40.002694, 116.322373],
               tiles='http://t7.tianditu.gov.cn/ter_w/wmts?SERVICE=WMTS&REQUEST=GetTile&VERSION=1.0.0&LAYER=ter&STYLE=default&TILEMATRIXSET=w&FORMAT=tiles&TILEMATRIX={z}&TILEROW={y}&TILECOL={x}&tk=',
               attr='天地图-地形',
               zoom_start=3,
              )
m
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

**天地图地形注记**

```
`m = folium.Map([40.002694, 116.322373],
               tiles='http://t7.tianditu.gov.cn/cta_w/wmts?SERVICE=WMTS&REQUEST=GetTile&VERSION=1.0.0&LAYER=cta&STYLE=default&TILEMATRIXSET=w&FORMAT=tiles&TILEMATRIX={z}&TILEROW={y}&TILECOL={x}&tk=',
               attr='天地图-地形标记',
               zoom_start=3,
              )
m
`
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

百度地图我这边测试失败了，暂时没有找到合适的替换方案。

**5\. 补充**

其实，我们还可以找更多的**地图底图瓦片URL**来进行替换，多样化我们的地图绘制。

另外，大家在用经纬度坐标点进行地图绘制的时候，比如标记点、绘制区域、热力图绘制等等，**需要考虑经纬度坐标是哪个地图系下面的，然后再用对应地图系的相关底图进行绘制才准确！**

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

[![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)](http://mp.weixin.qq.com/s?__biz=MzIzMzMzOTI3Nw==&mid=2247501630&idx=1&sn=d9995a6d275ba89597c067632d7179ec&chksm=e885a7dcdff22eca04579bbce5a16cfb50656729e016167d7a710b16f4e611d62c4940da3f3d&scene=21#wechat_redirect)

[![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)](http://mp.weixin.qq.com/s?__biz=MzIzMzMzOTI3Nw==&mid=2247499305&idx=1&sn=353e2297120352fc93a7a3ff9e8cc53a&chksm=e8859ecbdff217dd586b39767e5b195dd53fe4aabf887b42415b6079fed7cc0ef1d1db02ec19&scene=21#wechat_redirect)

[![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)](http://mp.weixin.qq.com/s?__biz=MzIzMzMzOTI3Nw==&mid=2247502689&idx=1&sn=83fe83679511a5ad0711c33748e89f51&chksm=e885ab83dff222956a622612fae2bdfb6ecd2578e0a173704cdee72d3be72df9cdfc88ed94b3&scene=21#wechat_redirect)

People who liked this content also liked

Python 的 \_\_name\_\_ 变量，到底是个什么东西？

小白学视觉

不看的原因

- 内容质量低
- 不看此公众号

会写代码的AI开源了！C语言写得比Codex还要好，掌握12种编程语言丨CMU

量子位

不看的原因

- 内容质量低
- 不看此公众号

python asyncio 异步 I/O - 实现并发http请求(asyncio + aiohttp)

从零开始学自动化测试

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___dc1bb48456974a9cb.bmp"/>

Scan to Follow