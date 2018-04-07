# dataParserForKddCup2018

## 使用说明

三个Python脚本文件用于解析三种原始数据的类型，需要有pandas库。此外监测站的信息已经由london_station.json与beijing_station.json给出。<br>在使用时注意脚本应与原始数据表格位于同一目录下。数据可以到（链接:https://pan.baidu.com/s/1TQ6l426zpNrPzVKxyJkF8g  密码:qq0y）下载。

## 数据类型/格式/使用方法

### aq

aq表示空气质量（Air Quality），可以用如下方式解析：

```
python aqParser.py beijing
```

之后得到beijing_aq.json文件，可以使用python的json.loads解析：

```
python
>> import json
>> aq_file = open("beijing_aq.json", "r")
>> aq_data = json.loads(aq_file.read())
>> print(aq_data)
```

解析后的格式为list，其中的每一项为一个dict表示一条信息，一条信息的一个例子如下：

```
{"station_id": "aotizhongxin", "time": "2017-01-01 14:00:00", "PM2.5": 453.0, "PM10": 467.0, "NO2": 156.0, "CO": 7.2, "O3": 3.0, "SO2": 9.0}
```

对于London同理：

```
python aqParser.py london
```

其中一条信息的一个例子如下：

```
{"station_id": "CD1", "time": "2017/1/1 0:00", "PM2.5": 40.0, "PM10": 44.4, "NO2": 36.6}
```

需要注意的是，Beijing的监测指标比London多。<br>

如果某一条信息的某个属性为空（表示该属性缺失，比如某地点某时刻监测了其他指标却未监测PM10，则该条信息PM10项为空），则得到的是NaN，Python中判断一个值是否为NaN的方式为：

```
if val != val:
	...
```

即NaN不等于自身。（注意不只是监测指标会为空，监测地点或监测时间也可能为空）<br>

### meo

表示监测站的天气信息，目前只有Beijing有该项信息，可以用如下方式解析：

```
python meoParser.py beijing
```

一条信息的一个例子如下：

```
{"station_id": "shunyi", "longitude": 116.6152777777778, "latitude": 40.126666666666665, "time": "2017-01-30 16:00:00", "temperature": -1.7, "humidity": 15, "pressure": 1028.7, "wind_direction": 215.0, "wind_speed": 1.6, "weather": "Sunny/clear"}
```

### meo_grid

表示城市天气的历史信息，其位置不是监测站的位置，而是天气记录网格上的某点。可以用如下方式解析：

```
python meoGridParser.py beijing
```

一条信息的一个例子如下：

```
{"station_name": "beijing_grid_000", "longitude": 115.0, "latitude": 39.0, "time": "2017-01-01 00:00:00", "temperature": -5.47, "humidity": 76.6, "pressure": 984.73, "wind_direction": 53.71, "wind_speed": 3.53}
```

对于London同理：

```
python meoGridParser.py london
```

一条信息的一个例子如下：

```
{"station_name": "london_grid_000", "longitude": -2.0, "latitude": 50.5, "time": "2017-01-01 00:00:00", "temperature": 9.36, "humidity": 77.9, "pressure": 1024.81, "wind_direction": 250.88, "wind_speed": 23.74}
```

需要注意的是，此项数据十分巨大，可能需要较多的时间解析。

### station

由于题目给出的监测站的数据格式十分别扭，因此采用的是手动解析的方式，直接把结果放了上来。<br>

对于beijing_station.json，一条信息的一个例子如下：

```
{"station_id": "dongsi", "longitude": 116.417, "latitude": 39.929, "area": "city"}
```

其中area属性为beijing监测站特有的，包括：city（城区）、contryside（郊区）、control（对照点）、traffic（交通污染监测点）。<br>

对于london_station.json，一条信息的一个例子如下：

```
{"station_id": "BX9", "longitude": 0.184877127, "latitude": 51.46598327, "api_data": true, "historical_data": true, "need_prediction": false, "site_type": "Suburban", "site_name": "Bexley - Slade Green FDMS"}
```