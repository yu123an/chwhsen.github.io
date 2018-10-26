---
layout: post
title: Jq--Linux环境下处理Json的得力助手
guid: urn:uuid:5e5a94ab-3392-41f6-9b69-7dcfa4b98a9c
---
大多数API请求的返回值是Json格式的，linux系统里用来处理Json最为强大的工具就是Jq了

以`Debian`系统为例首先安装安装Jq

```shell
sudo apt-get install jq
```

对于一段json数据

```
test.json
```

```json
{"HeWeather6":[{"basic":{"cid":"CN101181201","location":"鹤壁","parent_city":"鹤壁","admin_area":"河南","cnty":"中国","lat":"35.74823761","lon":"114.29544067","tz":"+8.00"},"update":{"loc":"2018-10-26 23:47","utc":"2018-10-26 15:47"},"status":"ok","now":{"cloud":"0","cond_code":"100","cond_txt":"晴","fl":"5","hum":"51","pcpn":"0.0","pres":"1023","tmp":"7","vis":"16","wind_deg":"342","wind_dir":"西北风","wind_sc":"1","wind_spd":"3"}}]}
```

是毫无逻辑性与组织性的，通过js进行初步处理后

```shell
cat test.json | jq '.'
```
或者
```shell
jq '.' test.json
```
```json
{
  "HeWeather6": [
    {
      "basic": {
        "cid": "CN101181201",
        "location": "鹤壁",
        "parent_city": "鹤壁",
        "admin_area": "河南",
        "cnty": "中国",
        "lat": "35.74823761",
        "lon": "114.29544067",
        "tz": "+8.00"
      },
      "update": {
        "loc": "2018-10-26 23:47",
        "utc": "2018-10-26 15:47"
      },
      "status": "ok",
      "now": {
        "cloud": "0",
        "cond_code": "100",
        "cond_txt": "晴",
        "fl": "4",
        "hum": "57",
        "pcpn": "0.0",
        "pres": "1023",
        "tmp": "6",
        "vis": "16",
        "wind_deg": "183",
        "wind_dir": "南风",
        "wind_sc": "1",
        "wind_spd": "5"
      }
    }
  ]
}

```

想要提取某一项的值，可以使用`[index]`比如获取今日天气`cond_txt`可以这样

```shell
cat test.json  | jq '.HeWeather6[0].now.cond_txt'
```

```
"晴"
```

如果想要去掉引号，可以添加`-r`参数

```shell
cat test.json  | jq -r '.HeWeather6[0].now.cond_txt'
```

```
晴
```

