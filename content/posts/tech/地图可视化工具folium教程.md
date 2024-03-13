---
title: "地图可视化工具folium教程"
date: 2021-08-13T18:34:25+08:00
lastmod: 2021-08-24T22:54:22+08:00
draft: false
categories:
- 技术
- 地图可视化
tags:
- python-folium
- Getting Started
---

#  地图可视化工具Folium教程

[文档资料](http://python-visualization.github.io/folium/)

[Python 如何画出漂亮的地图？](https://www.zhihu.com/question/33783546)

[Python绘制地图神器，上手直接开大！](https://jishuin.proginn.com/p/763bfbd57572)

[使用 Folium 进行地理空间分析的入门指南（包含多个案例研究）](https://www.analyticsvidhya.com/blog/2020/06/guide-geospatial-analysis-folium-python/)

安装`sudo pip3 install folium`

```txt
Installing collected packages: branca, folium
Successfully installed branca-0.4.2 folium-0.12.1
```

作为 Python 的一个可视化工具包 Folium，它通过 [Leaflet.js](https://leafletjs.com/) 的地图服务，可以在 Jupyter Notebook 上实现可视化的地理位置作图，制作各种各样精美的地图信息。它不仅可以针对某个经纬度进行地理位置的可视化操作，还能够根据实时的人群地理位置信息来构建静态与动态热力图，甚至还能够针对经纬度的数量来进行必要的聚类可视化。

Folium是建立在 Python 生态系统的数据整理 Datawrangling 能力和 Leaflet.js 库的映射能力之上的开源库。用 Python 处理数据，然后用 Folium 将它在 Leaflet 地图上进行可视化。Folium能够将通过 Python 处理后的数据轻松地在**交互式**的 Leaflet 地图上进行可视化展示。它不单单可以在地图上展示数据的分布图，还可以使用 Vincent/Vega 在地图上加以标记。这个开源库中有许多来自 OpenStreetMap、MapQuest Open、MapQuestOpen Aerial、Mapbox和Stamen 的内建地图元件，而且支持使用 Mapbox 或 Cloudmade 的 API 密钥来定制个性化的地图元件。Folium支持 GeoJSON 和 TopoJSON 两种文件格式的叠加，也可以将数据连接到这两种文件格式的叠加层，最后可使用 color-brewer 配色方案创建分布图。Folium可以让你用 Python 强大生态系统来处理数据，然后用 Leaflet 地图来展示。Folium内置一些来自 OpenStreetMap、MapQuest Open、MapQuest Open Aerial、Mapbox和Stamen 的地图元件(tilesets)，并且支持用 Mapbox 或者 Cloudmade API keys 来自定义地图元件。Folium支持 GeoJSON 和 TopJSON 叠加(overlays)，绑定数据来创造一个分级统计图(Choropleth map)。但是，Folium库绘制热点图的时候，需要联网才可显示。



```python
class folium.Map(location=None, width='100%', height='100%', left='0%', top='0%', position='relative', tiles='OpenStreetMap', attr=None, min_zoom=0, max_zoom=18, zoom_start=10, min_lat=-90, max_lat=90, min_lon=-180, max_lon=180, max_bounds=False, crs='EPSG3857', control_scale=False, prefer_canvas=False, no_touch=False, disable_3d=False, png_enabled=False, zoom_control=True, **kwargs)

```

几个重要的参数：

- location：经纬度，list 或者 tuple 格式，顺序为 latitude纬度, longitude经度
- zoom_start：缩放值，默认为 10，值越大比例尺越小，地图放大级别越大
- control_scale：Bool型，控制是否在地图上添加比例尺，默认为 False 即不添加
- tiles：显示样式，默认 "OpenStreetMap"，也就是开启街道显示，剩下的样式如'Stamen Terrain'，'Stamen Toner'。
- crs：地理坐标参考系统，默认为 "EPSG3857"

## 1.地图绘制



### 绘制中国地图

```python
import folium

chungkuo_map = folium.Map(location=[35,110])
chungkuo_map.save('index.html')
```





### 绘制世界地图

```python
import folium

#print(folium.__version__)

# define the world map
world_map = folium.Map()
# save world map
world_map.save('test_01.html')
```



### 绘制市级地图

改变地图显示也就是改变显示的经纬度和缩放比例，省级、市级、县级用法相似，这里举一个市级的例子为例，如北京市：

```python
import folium
import webbrowser                                                      

# 定义南京地图
city_map = folium.Map(location=[32, 119], zoom_start=10)  
# save map
city_map.save('test1.html')
webbrowser.open('test1.html')         #直接调用浏览器打开
```





## 2. 在地图上标记

#### 普通标记

添加普通标记用 Marker，可以选择标记的图案。

```python
import folium

bj_map = folium.Map(location=[39.93, 115.40], zoom_start=12, tiles='Stamen Terrain')

folium.Marker(
    location=[39.95, 115.33],
    popup='Mt. Hood Meadows',
    icon=folium.Icon(icon='cloud')
).add_to(bj_map)

folium.Marker(
    location=[39.96, 115.32],
    popup='Timberline Lodge',
    icon=folium.Icon(color='green')
).add_to(bj_map)

folium.Marker(
    location=[39.93, 115.34],
    popup='Some Other Location',
    icon=folium.Icon(color='red', icon='info-sign')    # 标记颜色  图标
).add_to(bj_map)

bj_map.save('test_04.html')
```

结果如下：

![img](https://filescdn.proginn.com/c869ced42b14c788039eb2f1112db955/98de75cc82a269b14e2d7dd05e6fc5dd.webp)

#### 圆形标记

添加圆形标记用 Circle 以及 CircleMarker

```python
import folium

bj_map = folium.Map(location=[39.93, 116.40], zoom_start=12, tiles='Stamen Toner')

folium.Circle(
    radius=200,
    location=(39.92, 116.43),
    popup='The Waterfront',
    color='#00FFFF',   # 颜色
    fill=False,        # 填充
).add_to(bj_map)

folium.CircleMarker(
    location=(39.93, 116.38),
    radius=50,   # 圆的半径
    popup='Laurelhurst Park',
    color='#FF1493',
    fill=True,
    fill_color='#FFD700'
).add_to(bj_map)

bj_map.save('test_05.html')
```

结果如下：

![img](https://filescdn.proginn.com/f01b44362ec7180a33c50159f601d093/ae82aa24784f91be788bfe75c2980cc8.webp)

#### 动态放置标记

```python
import folium

dynamic_tagging = folium.Map(
    location=[46.8527, -121.7649],
    tiles='Stamen Terrain',
    zoom_start=13
)

folium.Marker(
    [46.8354, -121.7325],
    popup='Camp Muir'
).add_to(dynamic_tagging)

dynamic_tagging.add_child(folium.ClickForMarker(popup='Waypoint'))
dynamic_tagging.save('test_06.html')
```

结果如下：

![img](https://filescdn.proginn.com/ab5d6d37a094ab203ff037f7ad9ddb35/20f6c4cb7b0c85bef909baee5dc24a58.webp)

更多详细使用可以参考官方文档：**http://python-visualization.github.io/folium/quickstart.html**[2]

#### 显示经纬度

```python
import folium

coordinate_orchard_road = [1.304247, 103.833264]                       

city_map = folium.Map(location=coordinate_orchard_road, zoom_start=11) 
                          
city_map.add_child(folium.LatLngPopup())                               

city_map.save('图片/test1.html') 
```



#### 显示特定几何形状





## 3、实战案例

以将停车场地理位置数据可视化在地图上示例，熟悉 folium 地图可视化的使用。

### 1. 获取经纬度数据

停车场地理位置数据来源于网络，数据真实可靠，下面先利用 Python 爬虫获取数据

```python
#数据来源：http://219.136.133.163:8000/Pages/Commonpage/login.aspx

import requests
import csv
import json
import logging

headers = {
    'X-Requested-With': 'XMLHttpRequest',
    'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/65.0.3325.181 Safari/537.36'
}
logging.basicConfig(level=logging.INFO, format='%(asctime)s - %(levelname)s: %(message)s')
url = 'http://219.136.133.163:8000/Pages/Commonpage/AsyGetData.asmx/GetParkList'
s = requests.session()
s.get(url, headers=headers)
for i in range(1, 318):
    data = {
        'cp': str(i),
        'ps': '10',
        'kw': '',
        'lon': 'undefined',
        'lat': 'undefined',
        'type': 'undefined'
    }
    url = 'http://219.136.133.163:8000/Pages/Commonpage/AsyGetData.asmx/GetParkList'
    # post提交表单数据
    res = s.post(url, data=data, headers=headers)
    # 重新设置编码
    res.encoding = 'utf-8'
    # str转json  便于提取数据
    result = json.loads(res.text)['Result']
    for j in result:
        park_name = j['ParkName']
        Lon = j['Longitude']
        Lat = j['Latitude']
        with open('parkings.csv', 'a+', newline='', encoding='gb18030') as f:
            f_csv = csv.writer(f)
            f_csv.writerow([park_name, Lon, Lat])
            logging.info([park_name, Lon, Lat])
```

结果如下：

![img](https://filescdn.proginn.com/4632f39ad503065844b907735ffb4a4f/33f81d67018de6a7b09fc887633fdb4e.webp)

![img](https://filescdn.proginn.com/e645c2d2c4111350c3e42982723bfff8/5b7b029b84ce05832fd799eebbd5f841.webp)

共有 3170 个停车场地理位置数据

### 2. folium地图可视化

```python
import pandas as pd
import folium

# 读取csv数据
data = pd.read_csv('parkings.csv', encoding='gbk')
# 传入纬度和经度数据
park_map = folium.Map(location=[data['latitude'].mean(), data['longitude'].mean()], zoom_start=10, control_scale=True,)
# 实例化 folium.map.FeatureGroup 对象
incidents = folium.map.FeatureGroup()
for name,row in data.iterrows():
    incidents.add_child(
        folium.CircleMarker(            # CircleMarker表示花圆
            (row["latitude"], row["longitude"]),   # 每个停车场的经纬度坐标
            radius=7,                   # 圆圈半径
            color='#FF1493',            # 标志的外圈颜色
            fill=True,                  # 是否填充
            fill_color='#00FF00',       # 填充颜色
            fill_opacity=0.4            # 填充透明度
        )
    )

park_map.add_child(incidents)
park_map.save('park_map1.html')
```

效果如下：

![img](https://filescdn.proginn.com/0aec257994b264a32eca027d592099d6/f299e14e3576e91a46b87b601ec80d8c.webp)

这样看起来有点乱，下面我们来统计一下各个局域的停车场数量

```python
import pandas as pd
import folium
from folium import plugins

data = pd.read_csv('parkings.csv', encoding='gbk')
park_map = folium.Map(location=[data['latitude'].mean(), data['longitude'].mean()], zoom_start=10, control_scale=True,)
marker_cluster = plugins.MarkerCluster().add_to(park_map)

for name,row in data.iterrows():
    folium.Marker(location=[row["latitude"], row["longitude"]]).add_to(marker_cluster)
park_map.save('park_map2.html')
```

效果如下：

![img](https://filescdn.proginn.com/a8be63c4c1418791800fb44c64b0b86c/ffb2006419dd0aed147403ca8cd10a27.webp)

这样能对各个局域停车场的数量在地图上进行统计，将图不断放大以后，还可以显示每个停车场的具体位置，非常方便。

[1]Python绘制地图神器folium入门: *https://blog.csdn.net/weixin_38169413/article/details/104806257*

[2]http://python-visualization.github.io/folium/quickstart.html



### 热力图

在实际使用经纬度信息的时候，通常来说会针对某个 APP 或者多款 APP 的实时经纬度信息来获取路况的拥挤程度，景区的人流量等信息，在这种情况下，就可以做出热力图。通过实时的热力图信息，我们可以获得相应的人流量信息进行必要的数据分析工作。

```python
import folium
import numpy as np 
from folium.plugins import HeatMap 

coordinate_orchard_road = [1.304247, 103.833264]  
# define the city map 
city_map = folium.Map(location=coordinate_orchard_road, zoom_start=11) 

# 构建随机数据 
data = (np.random.normal(size=(10, 3)) * 0.03 * np.array([[1, 1,1]])+np.array([[coordinate_orchard_road[0],coordinate_orchard_road[1], 1]]) ).tolist() 

city_map.add_child(HeatMap(data=data)) 

city_map.save('test1.html') 
```



![img](https://pica.zhimg.com/80/v2-b0ce7250ad926de532ca4851f4cfa5fa_720w.jpg?source=1940ef5c)



除了单张图的热力图之外，Folium 还能够计算一段时间的连续热力图信息。

```python
import folium
import numpy as np 
from folium.plugins import HeatMapWithTime

coordinate_orchard_road = [1.304247, 103.833264]  
# define the city map 
city_map = folium.Map(location=coordinate_orchard_road, zoom_start=11) 

# 使用numpy建立初始数据 
initial_data = (np.random.normal(size=(200, 2)) * np.array([[0.02, 0.02]])+np.array([coordinate_orchard_road]))
# 建立连续的数据
data = [initial_data.tolist()] 
for i in range(20):
	data.append((data[i] + np.random.normal(size=(200, 2))*0.001).tolist())

#显示连续的热力图

city_map.add_child(HeatMapWithTime(data=data)) 

city_map.save('test1.html') 
```



**经纬度点的聚类**

除了热力图能反映人流量的信息，基于地理位置的聚类算法同样能够反映一个地区的拥挤程度，Folium 的 MarkerCluster() 函数可以对一个区域中的点来做聚类，在地图中可以放大和缩小，从而知道局部最拥挤的点在哪里了，最稀疏的区域是哪里。

```python
import folium
import numpy as np 
from folium.plugins import MarkerCluster

coordinate_nus = [1.296202,103.776899]
coordinate_orchard_road = [1.304247, 103.833264] 
# define the city map 
city_map = folium.Map(location=coordinate_orchard_road, zoom_start=11) 

# 经纬度的聚类
#在NUS经纬度附近随机生成100个点
data = (np.random.normal(size=(100, 2))  * np.array([[0.001, 0.001]]) +np.array([coordinate_nus]))

# create a mark cluster object

marker_cluster = MarkerCluster().add_to(city_map)

#将这些经纬度加入聚类

for element in data:
	folium.Marker(location=[element[0], element[1]],icon=None).add_to(marker_cluster)

# add marker_cluster to map
city_map.add_child(marker_cluster) 

city_map.save('test1.html') 
```











