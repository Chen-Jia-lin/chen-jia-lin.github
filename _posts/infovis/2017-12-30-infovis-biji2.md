---
title: 如何用python结合高德key抓取数据？
layout: article
thread: 180
date: 2017-12-30
author: 陈嘉琳
categories: post infovis
excerpt: 这篇文章主要来自老师提供的代码。
image:
  teaser: post_infovis2.jpg
  feature: post_infovis2.jpg
---

---
### 第一步
- 你需要去[高德开放平台](http://lbs.amap.com/)注册一个账号，才能获取到key，你可以根据自己的需要选择不同使用地方的key，但是这里你需要选择web服务类型的key。

### 第二步
- 你需要打开一个能跑python代码的工具，例如：jupter。

### 第三步
- 根据以下的代码一步一步的跑
#### 获取你的api_key（例子里是我的！！）
```
my_api_key = 'ced346dd9eedc2d6fd9ee3382edb0835'
```
#### 这一步直接执行即可
```
from pprint import pprint
import requests #用於API HTTP requests
import json     #用於输入出json

url_api = "http://restapi.amap.com/v3/config/district"
parameters = {'keywords': '中国', 
              'subdistrict': 1,
              'key': my_api_key}
r = requests.get (url_api, params=parameters)
data = r.json()

```
#### 列出各省的地理编码
```
#  dict comprehension code --> int
print( { str(x['adcode']):x['name'] for x in data['districts'][0]['districts'] } )
```
#### 根据自身需要输入所要查找的关键词
```
places = { int(x['adcode']):x['name'] for x in data['districts'][0]['districts'] }
keywords = ['XXXX','XXXX','XXXX','XXXX']
```
#### 直接复制跑
```
import requests #用於API HTTP requests
import pandas as pd


def amap_api_search_by_keyword_city_code(kw, cc, api_key):

    parameters = {'key': api_key}  # 高德地图API 获取Key
    parameters.update( {'keywords': kw,
                        'city':     cc,
                        'citylimit': True
                       } )

    pois = []   # 建构最终结果列表pois
    pg_no = 1   # 建构pg_no，每次给不一样的可选参数page值

    while True: # 不断迭代，直到 break跳出
        parameters.update({'page':pg_no})  #建构可选参数page值 为 pg_no

        r = requests.get ("http://restapi.amap.com/v3/place/text", params=parameters)
        data = r.json()
        #print(data)

        no_pois = int(data['count'])      # 计算 应有的实质数量

        if no_pois>0:
            pois.extend(data.get('pois', []))         # 使用Python列表之extend方法，把返回数据中的结果data['pois']存入pois
            no_pois_this_search = len(pois)   # 计算 累积实质结果数
            if (no_pois_this_search >= no_pois):  # 若 累积实质结果数 >= 应有的实质数量
                break                                   # 则结束迭代跳出
            else:
                pg_no +=1                               # 否则找下一页数据，把 pg_no增1

        else:
            break                                    # empty

    if len(pois)>0:
        # 使用pandas 模块处理数据
        df_input = pd.DataFrame(pois)

        # 选择部份栏位准备输出
        select_fields = ['location', 'address', 'adname', 'cityname', 'id', 'importance', 'name', 'pname', 'tel', 'type', 'typecode']
        df = df_input[select_fields]

        # 经纬度 增加新栏位字段 places 及 keywords
        df = df.assign( 经 = [x.split(',')[0] for x in df['location']])
        df = df.assign( 纬 = [x.split(',')[1] for x in df['location']])
        
        df = df.assign( places_code = [cc]*len(df['location']))
        df = df.assign( places = [places[cc]]*len(df['location']))
        df = df.assign( keywords = [kw]*len(df['location']))
        
    else:
        df = pd.DataFrame([])
    
    # 使用 return返回
    return(df)

```
#### 同上(以例子为例，你抓取的数据会保存在文档里的data_outcome_20171124a.tsv)
```
import os.path
fn_output = "data_outcome_20171124a.tsv"

for k in keywords:
    for p in places:
        d = amap_api_search_by_keyword_city_code(k, p, my_api_key)
        print ("读取 {地点} 中的 {关键字} 搜索结果...共{几个}个".format(地点=places[p], 关键字=k, 几个=len(d)))

        if os.path.exists(fn_output): 
            header_needed = False
        else:
            header_needed = True
        
        # header  若数据档不存在则输出时包括栏位标题，若已存在则不包括
        d.to_csv(fn_output, encoding='utf8', sep='\t', mode='a',
                                index = False,  index_label=False,
                                header = header_needed)
        
```
#### 读取 所有数据
```
df_all = pd.read_csv(fn_output, encoding='utf8', sep="\t")
```
#### 数据长度
```
len(df_all)
```
#### 列出你抓取数据的横标题栏
```
df_all.columns
```
#### 显示table
```
df_all.pivot_table(values='name', index='places', columns='keywords', 
                         aggfunc=lambda x: len(x.unique()))
```
#### [更多的在这里](http://localhost:8888/notebooks/research_amap_search_NewMedia_cities_aggregate.ipynb)



