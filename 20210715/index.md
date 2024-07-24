# 2020新型冠状病毒（COVID-19/2019-nCoV）疫情分析(补档)

# 新型冠状病毒（COVID-19/2019-nCoV）疫情分析

**源文档详见：[博客相关资源-新冠疫情数据分析文件](https://www.spiritlhl.top/ziyuan/)**

## <font color='red'>重要说明</font>


分析文档：完成度：代码质量 3:5:2

其中分析文档是指你数据分析的过程中，对各问题分析的思路、对结果的解释、说明(要求言简意赅，不要为写而写)

ps:<font color='red'>你自己写的代码胜过一切的代笔，无关美丑，只问今日比昨日更长进！加油！</font>

由于数据过多，查看数据尽量使用head()或tail()，以免程序长时间无响应

=======================

本项目数据来源于丁香园。本项目主要目的是**通过对疫情历史数据的分析研究，以更好的了解疫情与疫情的发展态势，为抗击疫情之决策提供数据支持。**

关于本章使用的数据集，欢迎点击——>[我的B站视频](https://www.bilibili.com/video/BV1rC4y187Ek/)            在评论区获取。
## 一. 提出问题

从全国范围，你所在省市，国外疫情等三个方面主要研究以下几个问题：

（一）全国累计确诊/疑似/治愈/死亡情况随时间变化趋势如何？

（二）全国新增确诊/疑似/治愈/死亡情况随时间变化趋势如何？

（三）全国新增境外输入随时间变化趋势如何？

（四）你所在的省市情况如何？

（五）国外疫情态势如何？

（六）结合你的分析结果，对个人和社会在抗击疫情方面有何建议？


## 二. 理解数据

原始数据集：AreaInfo.csv，导入相关包及读取数据：


```python
r_hex = '#dc2624'     # red,       RGB = 220,38,36
dt_hex = '#2b4750'    # dark teal, RGB = 43,71,80
tl_hex = '#45a0a2'    # teal,      RGB = 69,160,162
r1_hex = '#e87a59'    # red,       RGB = 232,122,89
tl1_hex = '#7dcaa9'   # teal,      RGB = 125,202,169
g_hex = '#649E7D'     # green,     RGB = 100,158,125
o_hex = '#dc8018'     # orange,    RGB = 220,128,24
tn_hex = '#C89F91'    # tan,       RGB = 200,159,145
g50_hex = '#6c6d6c'   # grey-50,   RGB = 108,109,108
bg_hex = '#4f6268'    # blue grey, RGB = 79,98,104
g25_hex = '#c7cccf'   # grey-25,   RGB = 199,204,207
```


```python
import numpy as np
import pandas as pd
import matplotlib,re
import matplotlib.pyplot as plt
from matplotlib.pyplot import MultipleLocator


data = pd.read_csv(r'data/AreaInfo.csv')
```

**查看与统计数据，以对数据有一个大致了解**


```python
data.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>continentName</th>
      <th>continentEnglishName</th>
      <th>countryName</th>
      <th>countryEnglishName</th>
      <th>provinceName</th>
      <th>provinceEnglishName</th>
      <th>province_zipCode</th>
      <th>province_confirmedCount</th>
      <th>province_suspectedCount</th>
      <th>province_curedCount</th>
      <th>province_deadCount</th>
      <th>updateTime</th>
      <th>cityName</th>
      <th>cityEnglishName</th>
      <th>city_zipCode</th>
      <th>city_confirmedCount</th>
      <th>city_suspectedCount</th>
      <th>city_curedCount</th>
      <th>city_deadCount</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>北美洲</td>
      <td>North America</td>
      <td>美国</td>
      <td>United States of America</td>
      <td>美国</td>
      <td>United States of America</td>
      <td>971002</td>
      <td>2306247</td>
      <td>0.0</td>
      <td>640198</td>
      <td>120351</td>
      <td>2020-06-23 10:01:45</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>南美洲</td>
      <td>South America</td>
      <td>巴西</td>
      <td>Brazil</td>
      <td>巴西</td>
      <td>Brazil</td>
      <td>973003</td>
      <td>1106470</td>
      <td>0.0</td>
      <td>549386</td>
      <td>51271</td>
      <td>2020-06-23 10:01:45</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>欧洲</td>
      <td>Europe</td>
      <td>英国</td>
      <td>United Kingdom</td>
      <td>英国</td>
      <td>United Kingdom</td>
      <td>961007</td>
      <td>305289</td>
      <td>0.0</td>
      <td>539</td>
      <td>42647</td>
      <td>2020-06-23 10:01:45</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>欧洲</td>
      <td>Europe</td>
      <td>俄罗斯</td>
      <td>Russia</td>
      <td>俄罗斯</td>
      <td>Russia</td>
      <td>964006</td>
      <td>592280</td>
      <td>0.0</td>
      <td>344416</td>
      <td>8206</td>
      <td>2020-06-23 10:01:45</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>南美洲</td>
      <td>South America</td>
      <td>智利</td>
      <td>Chile</td>
      <td>智利</td>
      <td>Chile</td>
      <td>973004</td>
      <td>246963</td>
      <td>0.0</td>
      <td>44946</td>
      <td>4502</td>
      <td>2020-06-23 10:01:45</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



## 三. 数据清洗

### （一）基本数据处理

数据清洗主要包括：**选取子集，缺失数据处理、数据格式转换、异常值数据处理**等。

#### 国内疫情数据选取（最终选取的数据命名为china）

1. 选取国内疫情数据

2. 对于更新时间(updateTime)列，需将其转换为日期类型并提取出年-月-日，并查看处理结果。(提示：dt.date)

3. 因数据每天按小时更新，一天之内有很多重复数据，请去重并只保留一天之内最新的数据。

> 提示：df.drop_duplicates(subset=['provinceName', 'updateTime'], keep='first', inplace=False)

> 其中df是你选择的国内疫情数据的DataFrame

分析：选取countryName一列中值为中国的行组成CHINA。


```python
CHINA = data.loc[data['countryName'] == '中国']
CHINA.dropna(subset=['cityName'], how='any', inplace=True)
#CHINA
```

    D:\Anaconda\envs\python32\lib\site-packages\ipykernel_launcher.py:2: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      
    

分析：取出含所有中国城市的列表


```python
cities = list(set(CHINA['cityName']))
```

分析：遍历取出每一个城市的子dataframe，然后用sort对updateTime进行时间排序


```python
for city in cities:
    CHINA.loc[data['cityName'] == city].sort_values(by = 'updateTime')
```

分析：去除空值所在行


```python
CHINA.dropna(subset=['cityName'],inplace=True)
#CHINA.loc[CHINA['cityName'] == '秦皇岛'].tail(20)
```

    D:\Anaconda\envs\python32\lib\site-packages\ipykernel_launcher.py:1: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      """Entry point for launching an IPython kernel.
    

 分析：将CHINA中的updateTime列进行格式化处理


```python
CHINA.updateTime = pd.to_datetime(CHINA.updateTime,format="%Y-%m-%d",errors='coerce').dt.date
#CHINA.loc[data['cityName'] == '秦皇岛'].tail(15)
```

    D:\Anaconda\envs\python32\lib\site-packages\pandas\core\generic.py:5303: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      self[name] = value
    


```python
CHINA.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>continentName</th>
      <th>continentEnglishName</th>
      <th>countryName</th>
      <th>countryEnglishName</th>
      <th>provinceName</th>
      <th>provinceEnglishName</th>
      <th>province_zipCode</th>
      <th>province_confirmedCount</th>
      <th>province_suspectedCount</th>
      <th>province_curedCount</th>
      <th>province_deadCount</th>
      <th>updateTime</th>
      <th>cityName</th>
      <th>cityEnglishName</th>
      <th>city_zipCode</th>
      <th>city_confirmedCount</th>
      <th>city_suspectedCount</th>
      <th>city_curedCount</th>
      <th>city_deadCount</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>136</th>
      <td>亚洲</td>
      <td>Asia</td>
      <td>中国</td>
      <td>China</td>
      <td>陕西省</td>
      <td>Shaanxi</td>
      <td>610000</td>
      <td>317</td>
      <td>1.0</td>
      <td>307</td>
      <td>3</td>
      <td>2020-06-23</td>
      <td>境外输入</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>72.0</td>
      <td>0.0</td>
      <td>65.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>137</th>
      <td>亚洲</td>
      <td>Asia</td>
      <td>中国</td>
      <td>China</td>
      <td>陕西省</td>
      <td>Shaanxi</td>
      <td>610000</td>
      <td>317</td>
      <td>1.0</td>
      <td>307</td>
      <td>3</td>
      <td>2020-06-23</td>
      <td>西安</td>
      <td>Xi'an</td>
      <td>610100.0</td>
      <td>120.0</td>
      <td>0.0</td>
      <td>117.0</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>138</th>
      <td>亚洲</td>
      <td>Asia</td>
      <td>中国</td>
      <td>China</td>
      <td>陕西省</td>
      <td>Shaanxi</td>
      <td>610000</td>
      <td>317</td>
      <td>1.0</td>
      <td>307</td>
      <td>3</td>
      <td>2020-06-23</td>
      <td>安康</td>
      <td>Ankang</td>
      <td>610900.0</td>
      <td>26.0</td>
      <td>0.0</td>
      <td>26.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>139</th>
      <td>亚洲</td>
      <td>Asia</td>
      <td>中国</td>
      <td>China</td>
      <td>陕西省</td>
      <td>Shaanxi</td>
      <td>610000</td>
      <td>317</td>
      <td>1.0</td>
      <td>307</td>
      <td>3</td>
      <td>2020-06-23</td>
      <td>汉中</td>
      <td>Hanzhong</td>
      <td>610700.0</td>
      <td>26.0</td>
      <td>0.0</td>
      <td>26.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>140</th>
      <td>亚洲</td>
      <td>Asia</td>
      <td>中国</td>
      <td>China</td>
      <td>陕西省</td>
      <td>Shaanxi</td>
      <td>610000</td>
      <td>317</td>
      <td>1.0</td>
      <td>307</td>
      <td>3</td>
      <td>2020-06-23</td>
      <td>咸阳</td>
      <td>Xianyang</td>
      <td>610400.0</td>
      <td>17.0</td>
      <td>0.0</td>
      <td>17.0</td>
      <td>0.0</td>
    </tr>
  </tbody>
</table>
</div>



分析：每日数据的去重只保留第一个数据，因为前面已经对时间进行排序，第一个数据即为当天最新数据   
分析：考虑到合并dataframe需要用到concat，需要创建一个初始china


```python
real = CHINA.loc[data['cityName'] == cities[1]]
real.drop_duplicates(subset='updateTime', keep='first', inplace=True)
china = real
```

    D:\Anaconda\envs\python32\lib\site-packages\ipykernel_launcher.py:2: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      
    

分析：遍历每个城市dataframe进行每日数据的去重，否则会出现相同日期只保留一个城市的数据的情况


```python
for city in cities[2:]:
    real_data = CHINA.loc[data['cityName'] == city]
    real_data.drop_duplicates(subset='updateTime', keep='first', inplace=True)
    china = pd.concat([real_data, china],sort=False)
```

    D:\Anaconda\envs\python32\lib\site-packages\ipykernel_launcher.py:3: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      This is separate from the ipykernel package so we can avoid doing imports until
    

查看数据信息，是否有缺失数据/数据类型是否正确。

提示：若不会处理缺失值，可以将其舍弃

分析：有的城市不是每日都上报的，如果某日只统计上报的那些城市，那些存在患者却不上报的城市就会被忽略，数据就失真了，需要补全所有城市每日的数据，即便不上报的城市也要每日记录数据统计，所以要进行插值处理补全部分数据，处理方法详见```数据透视与分析```


```python
china.info()
```


```python
china.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>continentName</th>
      <th>continentEnglishName</th>
      <th>countryName</th>
      <th>countryEnglishName</th>
      <th>provinceName</th>
      <th>provinceEnglishName</th>
      <th>province_zipCode</th>
      <th>province_confirmedCount</th>
      <th>province_suspectedCount</th>
      <th>province_curedCount</th>
      <th>province_deadCount</th>
      <th>updateTime</th>
      <th>cityName</th>
      <th>cityEnglishName</th>
      <th>city_zipCode</th>
      <th>city_confirmedCount</th>
      <th>city_suspectedCount</th>
      <th>city_curedCount</th>
      <th>city_deadCount</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>96106</th>
      <td>亚洲</td>
      <td>Asia</td>
      <td>中国</td>
      <td>China</td>
      <td>广西壮族自治区</td>
      <td>Guangxi</td>
      <td>450000</td>
      <td>254</td>
      <td>0.0</td>
      <td>252</td>
      <td>2</td>
      <td>2020-04-02</td>
      <td>贵港</td>
      <td>Guigang</td>
      <td>450800.0</td>
      <td>8.0</td>
      <td>0.0</td>
      <td>8.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>125120</th>
      <td>亚洲</td>
      <td>Asia</td>
      <td>中国</td>
      <td>China</td>
      <td>广西壮族自治区</td>
      <td>Guangxi</td>
      <td>450000</td>
      <td>254</td>
      <td>0.0</td>
      <td>250</td>
      <td>2</td>
      <td>2020-03-20</td>
      <td>贵港</td>
      <td>Guigang</td>
      <td>450800.0</td>
      <td>8.0</td>
      <td>0.0</td>
      <td>8.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>128762</th>
      <td>亚洲</td>
      <td>Asia</td>
      <td>中国</td>
      <td>China</td>
      <td>广西壮族自治区</td>
      <td>Guangxi</td>
      <td>450000</td>
      <td>253</td>
      <td>0.0</td>
      <td>250</td>
      <td>2</td>
      <td>2020-03-18</td>
      <td>贵港</td>
      <td>Guigang</td>
      <td>450800.0</td>
      <td>8.0</td>
      <td>0.0</td>
      <td>8.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>130607</th>
      <td>亚洲</td>
      <td>Asia</td>
      <td>中国</td>
      <td>China</td>
      <td>广西壮族自治区</td>
      <td>Guangxi</td>
      <td>450000</td>
      <td>253</td>
      <td>0.0</td>
      <td>248</td>
      <td>2</td>
      <td>2020-03-17</td>
      <td>贵港</td>
      <td>Guigang</td>
      <td>450800.0</td>
      <td>8.0</td>
      <td>0.0</td>
      <td>8.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>131428</th>
      <td>亚洲</td>
      <td>Asia</td>
      <td>中国</td>
      <td>China</td>
      <td>广西壮族自治区</td>
      <td>Guangxi</td>
      <td>450000</td>
      <td>252</td>
      <td>0.0</td>
      <td>248</td>
      <td>2</td>
      <td>2020-03-16</td>
      <td>贵港</td>
      <td>Guigang</td>
      <td>450800.0</td>
      <td>8.0</td>
      <td>0.0</td>
      <td>8.0</td>
      <td>0.0</td>
    </tr>
  </tbody>
</table>
</div>



#### 你所在省市疫情数据选取（最终选取的数据命名为myhome）

此步也可在后面用到的再做


```python
myhome = china.loc[data['provinceName'] == '广东省']
myhome.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>continentName</th>
      <th>continentEnglishName</th>
      <th>countryName</th>
      <th>countryEnglishName</th>
      <th>provinceName</th>
      <th>provinceEnglishName</th>
      <th>province_zipCode</th>
      <th>province_confirmedCount</th>
      <th>province_suspectedCount</th>
      <th>province_curedCount</th>
      <th>province_deadCount</th>
      <th>updateTime</th>
      <th>cityName</th>
      <th>cityEnglishName</th>
      <th>city_zipCode</th>
      <th>city_confirmedCount</th>
      <th>city_suspectedCount</th>
      <th>city_curedCount</th>
      <th>city_deadCount</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>205259</th>
      <td>亚洲</td>
      <td>Asia</td>
      <td>中国</td>
      <td>China</td>
      <td>广东省</td>
      <td>Guangdong</td>
      <td>440000</td>
      <td>277</td>
      <td>0.0</td>
      <td>5</td>
      <td>0</td>
      <td>2020-01-29</td>
      <td>外地来粤人员</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>5.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>206335</th>
      <td>亚洲</td>
      <td>Asia</td>
      <td>中国</td>
      <td>China</td>
      <td>广东省</td>
      <td>Guangdong</td>
      <td>440000</td>
      <td>207</td>
      <td>0.0</td>
      <td>4</td>
      <td>0</td>
      <td>2020-01-28</td>
      <td>河源市</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>205239</th>
      <td>亚洲</td>
      <td>Asia</td>
      <td>中国</td>
      <td>China</td>
      <td>广东省</td>
      <td>Guangdong</td>
      <td>440000</td>
      <td>277</td>
      <td>0.0</td>
      <td>5</td>
      <td>0</td>
      <td>2020-01-29</td>
      <td>外地来穗人员</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>5.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>252</th>
      <td>亚洲</td>
      <td>Asia</td>
      <td>中国</td>
      <td>China</td>
      <td>广东省</td>
      <td>Guangdong</td>
      <td>440000</td>
      <td>1634</td>
      <td>11.0</td>
      <td>1619</td>
      <td>8</td>
      <td>2020-06-23</td>
      <td>潮州</td>
      <td>Chaozhou</td>
      <td>445100.0</td>
      <td>6.0</td>
      <td>0.0</td>
      <td>6.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2655</th>
      <td>亚洲</td>
      <td>Asia</td>
      <td>中国</td>
      <td>China</td>
      <td>广东省</td>
      <td>Guangdong</td>
      <td>440000</td>
      <td>1634</td>
      <td>11.0</td>
      <td>1614</td>
      <td>8</td>
      <td>2020-06-21</td>
      <td>潮州</td>
      <td>Chaozhou</td>
      <td>445100.0</td>
      <td>6.0</td>
      <td>0.0</td>
      <td>6.0</td>
      <td>0.0</td>
    </tr>
  </tbody>
</table>
</div>



#### 国外疫情数据选取（最终选取的数据命名为world）

此步也可在后面用到的再做


```python
world = data.loc[data['countryName'] != '中国']
world.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>continentName</th>
      <th>continentEnglishName</th>
      <th>countryName</th>
      <th>countryEnglishName</th>
      <th>provinceName</th>
      <th>provinceEnglishName</th>
      <th>province_zipCode</th>
      <th>province_confirmedCount</th>
      <th>province_suspectedCount</th>
      <th>province_curedCount</th>
      <th>province_deadCount</th>
      <th>updateTime</th>
      <th>cityName</th>
      <th>cityEnglishName</th>
      <th>city_zipCode</th>
      <th>city_confirmedCount</th>
      <th>city_suspectedCount</th>
      <th>city_curedCount</th>
      <th>city_deadCount</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>北美洲</td>
      <td>North America</td>
      <td>美国</td>
      <td>United States of America</td>
      <td>美国</td>
      <td>United States of America</td>
      <td>971002</td>
      <td>2306247</td>
      <td>0.0</td>
      <td>640198</td>
      <td>120351</td>
      <td>2020-06-23 10:01:45</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>南美洲</td>
      <td>South America</td>
      <td>巴西</td>
      <td>Brazil</td>
      <td>巴西</td>
      <td>Brazil</td>
      <td>973003</td>
      <td>1106470</td>
      <td>0.0</td>
      <td>549386</td>
      <td>51271</td>
      <td>2020-06-23 10:01:45</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>欧洲</td>
      <td>Europe</td>
      <td>英国</td>
      <td>United Kingdom</td>
      <td>英国</td>
      <td>United Kingdom</td>
      <td>961007</td>
      <td>305289</td>
      <td>0.0</td>
      <td>539</td>
      <td>42647</td>
      <td>2020-06-23 10:01:45</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>欧洲</td>
      <td>Europe</td>
      <td>俄罗斯</td>
      <td>Russia</td>
      <td>俄罗斯</td>
      <td>Russia</td>
      <td>964006</td>
      <td>592280</td>
      <td>0.0</td>
      <td>344416</td>
      <td>8206</td>
      <td>2020-06-23 10:01:45</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>南美洲</td>
      <td>South America</td>
      <td>智利</td>
      <td>Chile</td>
      <td>智利</td>
      <td>Chile</td>
      <td>973004</td>
      <td>246963</td>
      <td>0.0</td>
      <td>44946</td>
      <td>4502</td>
      <td>2020-06-23 10:01:45</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



**数据透视与分析**

分析：对china进行插值处理补全部分数据


```python
china.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>continentName</th>
      <th>continentEnglishName</th>
      <th>countryName</th>
      <th>countryEnglishName</th>
      <th>provinceName</th>
      <th>provinceEnglishName</th>
      <th>province_zipCode</th>
      <th>province_confirmedCount</th>
      <th>province_suspectedCount</th>
      <th>province_curedCount</th>
      <th>province_deadCount</th>
      <th>updateTime</th>
      <th>cityName</th>
      <th>cityEnglishName</th>
      <th>city_zipCode</th>
      <th>city_confirmedCount</th>
      <th>city_suspectedCount</th>
      <th>city_curedCount</th>
      <th>city_deadCount</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>96106</th>
      <td>亚洲</td>
      <td>Asia</td>
      <td>中国</td>
      <td>China</td>
      <td>广西壮族自治区</td>
      <td>Guangxi</td>
      <td>450000</td>
      <td>254</td>
      <td>0.0</td>
      <td>252</td>
      <td>2</td>
      <td>2020-04-02</td>
      <td>贵港</td>
      <td>Guigang</td>
      <td>450800.0</td>
      <td>8.0</td>
      <td>0.0</td>
      <td>8.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>125120</th>
      <td>亚洲</td>
      <td>Asia</td>
      <td>中国</td>
      <td>China</td>
      <td>广西壮族自治区</td>
      <td>Guangxi</td>
      <td>450000</td>
      <td>254</td>
      <td>0.0</td>
      <td>250</td>
      <td>2</td>
      <td>2020-03-20</td>
      <td>贵港</td>
      <td>Guigang</td>
      <td>450800.0</td>
      <td>8.0</td>
      <td>0.0</td>
      <td>8.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>128762</th>
      <td>亚洲</td>
      <td>Asia</td>
      <td>中国</td>
      <td>China</td>
      <td>广西壮族自治区</td>
      <td>Guangxi</td>
      <td>450000</td>
      <td>253</td>
      <td>0.0</td>
      <td>250</td>
      <td>2</td>
      <td>2020-03-18</td>
      <td>贵港</td>
      <td>Guigang</td>
      <td>450800.0</td>
      <td>8.0</td>
      <td>0.0</td>
      <td>8.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>130607</th>
      <td>亚洲</td>
      <td>Asia</td>
      <td>中国</td>
      <td>China</td>
      <td>广西壮族自治区</td>
      <td>Guangxi</td>
      <td>450000</td>
      <td>253</td>
      <td>0.0</td>
      <td>248</td>
      <td>2</td>
      <td>2020-03-17</td>
      <td>贵港</td>
      <td>Guigang</td>
      <td>450800.0</td>
      <td>8.0</td>
      <td>0.0</td>
      <td>8.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>131428</th>
      <td>亚洲</td>
      <td>Asia</td>
      <td>中国</td>
      <td>China</td>
      <td>广西壮族自治区</td>
      <td>Guangxi</td>
      <td>450000</td>
      <td>252</td>
      <td>0.0</td>
      <td>248</td>
      <td>2</td>
      <td>2020-03-16</td>
      <td>贵港</td>
      <td>Guigang</td>
      <td>450800.0</td>
      <td>8.0</td>
      <td>0.0</td>
      <td>8.0</td>
      <td>0.0</td>
    </tr>
  </tbody>
</table>
</div>



分析：先创建省份列表和日期列表，并初始化一个draft


```python
province = list(set(china['provinceName']))#每个省份
#p_city = list(set(china[china['provinceName'] == province[0]]['cityName']))#每个省份的城市
date_0 = []
for dt in china.loc[china['provinceName'] ==  province[0]]['updateTime']:
    date_0.append(str(dt))
date_0 = list(set(date_0))
date_0.sort()
start = china.loc[china['provinceName'] ==  province[0]]['updateTime'].min()
end = china.loc[china['provinceName'] ==  province[0]]['updateTime'].max()
dates = pd.date_range(start=str(start), end=str(end))
aid_frame = pd.DataFrame({'updateTime': dates,'provinceName':[province[0]]*len(dates)})
aid_frame.updateTime = pd.to_datetime(aid_frame.updateTime,format="%Y-%m-%d",errors='coerce').dt.date
#draft = pd.merge(china.loc[china['provinceName'] ==  province[1]], aid_frame, on='updateTime', how='outer').sort_values('updateTime')
draft = pd.concat([china.loc[china['provinceName'] ==  province[0]], aid_frame], join='outer').sort_values('updateTime')
draft.province_confirmedCount.fillna(method="ffill",inplace=True)
draft.province_suspectedCount.fillna(method="ffill", inplace=True)
draft.province_curedCount.fillna(method="ffill", inplace=True)
draft.province_deadCount.fillna(method="ffill", inplace=True)
```

分析：补全部分时间，取前日的数据进行插值，因为有的省份从4月末开始陆续就不再有新增病患，不再上报，所以这些省份的数据只能补全到4月末，往后的数据逐渐失去真实性

分析：同时进行日期格式化


```python
for p in range(1,len(province)):
    date_d = []
    for dt in china.loc[china['provinceName'] ==  province[p]]['updateTime']:
        date_d.append(dt)
    date_d = list(set(date_d))
    date_d.sort()
    start = china.loc[china['provinceName'] ==  province[p]]['updateTime'].min()
    end = china.loc[china['provinceName'] ==  province[p]]['updateTime'].max()
    dates = pd.date_range(start=start, end=end)
    aid_frame = pd.DataFrame({'updateTime': dates,'provinceName':[province[p]]*len(dates)})
    aid_frame.updateTime = pd.to_datetime(aid_frame.updateTime,format="%Y-%m-%d",errors='coerce').dt.date
    X = china.loc[china['provinceName'] ==  province[p]]
    X.reset_index(drop= True)
    Y = aid_frame
    Y.reset_index(drop= True)
    draft_d = pd.concat([X,Y], join='outer').sort_values('updateTime')
    draft = pd.concat([draft,draft_d])
    draft.province_confirmedCount.fillna(method="ffill",inplace=True)
    draft.province_suspectedCount.fillna(method="ffill", inplace=True)
    draft.province_curedCount.fillna(method="ffill", inplace=True)
    draft.province_deadCount.fillna(method="ffill", inplace=True)
    #draft['updateTime'] = draft['updateTime'].strftime('%Y-%m-%d')
    #draft['updateTime'] = pd.to_datetime(draft['updateTime'],format="%Y-%m-%d",errors='coerce').dt.date
```


```python
china = draft
```


```python
china.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>continentName</th>
      <th>continentEnglishName</th>
      <th>countryName</th>
      <th>countryEnglishName</th>
      <th>provinceName</th>
      <th>provinceEnglishName</th>
      <th>province_zipCode</th>
      <th>province_confirmedCount</th>
      <th>province_suspectedCount</th>
      <th>province_curedCount</th>
      <th>province_deadCount</th>
      <th>updateTime</th>
      <th>cityName</th>
      <th>cityEnglishName</th>
      <th>city_zipCode</th>
      <th>city_confirmedCount</th>
      <th>city_suspectedCount</th>
      <th>city_curedCount</th>
      <th>city_deadCount</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>208226</th>
      <td>亚洲</td>
      <td>Asia</td>
      <td>中国</td>
      <td>China</td>
      <td>天津市</td>
      <td>Tianjin</td>
      <td>120000.0</td>
      <td>14.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>2020-01-26</td>
      <td>外地来津</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>208224</th>
      <td>亚洲</td>
      <td>Asia</td>
      <td>中国</td>
      <td>China</td>
      <td>天津市</td>
      <td>Tianjin</td>
      <td>120000.0</td>
      <td>14.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>2020-01-26</td>
      <td>河北区</td>
      <td>Hebei District</td>
      <td>120105.0</td>
      <td>5.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>208228</th>
      <td>亚洲</td>
      <td>Asia</td>
      <td>中国</td>
      <td>China</td>
      <td>天津市</td>
      <td>Tianjin</td>
      <td>120000.0</td>
      <td>14.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>2020-01-26</td>
      <td>和平区</td>
      <td>Heping District</td>
      <td>120101.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>208227</th>
      <td>亚洲</td>
      <td>Asia</td>
      <td>中国</td>
      <td>China</td>
      <td>天津市</td>
      <td>Tianjin</td>
      <td>120000.0</td>
      <td>14.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>2020-01-26</td>
      <td>滨海新区</td>
      <td>Binhai New Area</td>
      <td>120116.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>208230</th>
      <td>亚洲</td>
      <td>Asia</td>
      <td>中国</td>
      <td>China</td>
      <td>天津市</td>
      <td>Tianjin</td>
      <td>120000.0</td>
      <td>14.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>2020-01-26</td>
      <td>西青区</td>
      <td>Xiqing District</td>
      <td>120111.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
  </tbody>
</table>
</div>



## 四. 数据分析及可视化

在进行数据分析及可视化时，依据每个问题选取所需变量并新建DataFrame再进行分析和可视化展示，这样数据不易乱且条理更清晰。

### 基础分析

基础分析，**只允许使用numpy、pandas和matplotlib库**。

可以在一张图上多个坐标系展示也可以在多张图上展示

请根据分析目的选择图形的类型(折线图、饼图、直方图和散点图等等)，实在没有主意可以到百度疫情地图或其他疫情分析的站点激发激发灵感。

#### （一）全国累计确诊/疑似/治愈/死亡情况随时间变化趋势如何？

分析：要获得全国累计情况随时间变化趋势，首先需要整合每日全国累计确诊情况做成date_confirmed

分析：要整合每日全国累计确诊情况，首先得提取每个省份每日当天最新累计确诊人数，省份数据求和后形成dataframe，  
for循环拼接到date_confirmed中


```python
date = list(set(china['updateTime']))
date.sort()
date
```



```
    [datetime.date(2020, 1, 24),
     datetime.date(2020, 1, 25),
     datetime.date(2020, 1, 26),
     datetime.date(2020, 1, 27),
     datetime.date(2020, 1, 28),
     datetime.date(2020, 1, 29),
     datetime.date(2020, 1, 30),
     datetime.date(2020, 1, 31),
     datetime.date(2020, 2, 1),
     datetime.date(2020, 2, 2),
     datetime.date(2020, 2, 3),
     datetime.date(2020, 2, 4),
     datetime.date(2020, 2, 5),
     datetime.date(2020, 2, 6),
     datetime.date(2020, 2, 7),
     datetime.date(2020, 2, 8),
     datetime.date(2020, 2, 9),
     datetime.date(2020, 2, 10),
     datetime.date(2020, 2, 11),
     datetime.date(2020, 2, 12),
     datetime.date(2020, 2, 13),
     datetime.date(2020, 2, 14),
     datetime.date(2020, 2, 15),
     datetime.date(2020, 2, 16),
     datetime.date(2020, 2, 17),
     datetime.date(2020, 2, 18),
     datetime.date(2020, 2, 19),
     datetime.date(2020, 2, 20),
     datetime.date(2020, 2, 21),
     datetime.date(2020, 2, 22),
     datetime.date(2020, 2, 23),
     datetime.date(2020, 2, 24),
     datetime.date(2020, 2, 25),
     datetime.date(2020, 2, 26),
     datetime.date(2020, 2, 27),
     datetime.date(2020, 2, 28),
     datetime.date(2020, 2, 29),
     datetime.date(2020, 3, 1),
     datetime.date(2020, 3, 2),
     datetime.date(2020, 3, 3),
     datetime.date(2020, 3, 4),
     datetime.date(2020, 3, 5),
     datetime.date(2020, 3, 6),
     datetime.date(2020, 3, 7),
     datetime.date(2020, 3, 8),
     datetime.date(2020, 3, 9),
     datetime.date(2020, 3, 10),
     datetime.date(2020, 3, 11),
     datetime.date(2020, 3, 12),
     datetime.date(2020, 3, 13),
     datetime.date(2020, 3, 14),
     datetime.date(2020, 3, 15),
     datetime.date(2020, 3, 16),
     datetime.date(2020, 3, 17),
     datetime.date(2020, 3, 18),
     datetime.date(2020, 3, 19),
     datetime.date(2020, 3, 20),
     datetime.date(2020, 3, 21),
     datetime.date(2020, 3, 22),
     datetime.date(2020, 3, 23),
     datetime.date(2020, 3, 24),
     datetime.date(2020, 3, 25),
     datetime.date(2020, 3, 26),
     datetime.date(2020, 3, 27),
     datetime.date(2020, 3, 28),
     datetime.date(2020, 3, 29),
     datetime.date(2020, 3, 30),
     datetime.date(2020, 3, 31),
     datetime.date(2020, 4, 1),
     datetime.date(2020, 4, 2),
     datetime.date(2020, 4, 3),
     datetime.date(2020, 4, 4),
     datetime.date(2020, 4, 5),
     datetime.date(2020, 4, 6),
     datetime.date(2020, 4, 7),
     datetime.date(2020, 4, 8),
     datetime.date(2020, 4, 9),
     datetime.date(2020, 4, 10),
     datetime.date(2020, 4, 11),
     datetime.date(2020, 4, 12),
     datetime.date(2020, 4, 13),
     datetime.date(2020, 4, 14),
     datetime.date(2020, 4, 15),
     datetime.date(2020, 4, 16),
     datetime.date(2020, 4, 17),
     datetime.date(2020, 4, 18),
     datetime.date(2020, 4, 19),
     datetime.date(2020, 4, 20),
     datetime.date(2020, 4, 21),
     datetime.date(2020, 4, 22),
     datetime.date(2020, 4, 23),
     datetime.date(2020, 4, 24),
     datetime.date(2020, 4, 25),
     datetime.date(2020, 4, 26),
     datetime.date(2020, 4, 27),
     datetime.date(2020, 4, 28),
     datetime.date(2020, 4, 29),
     datetime.date(2020, 4, 30),
     datetime.date(2020, 5, 1),
     datetime.date(2020, 5, 2),
     datetime.date(2020, 5, 3),
     datetime.date(2020, 5, 4),
     datetime.date(2020, 5, 5),
     datetime.date(2020, 5, 6),
     datetime.date(2020, 5, 7),
     datetime.date(2020, 5, 8),
     datetime.date(2020, 5, 9),
     datetime.date(2020, 5, 10),
     datetime.date(2020, 5, 11),
     datetime.date(2020, 5, 12),
     datetime.date(2020, 5, 13),
     datetime.date(2020, 5, 14),
     datetime.date(2020, 5, 15),
     datetime.date(2020, 5, 16),
     datetime.date(2020, 5, 17),
     datetime.date(2020, 5, 18),
     datetime.date(2020, 5, 19),
     datetime.date(2020, 5, 20),
     datetime.date(2020, 5, 21),
     datetime.date(2020, 5, 22),
     datetime.date(2020, 5, 23),
     datetime.date(2020, 5, 24),
     datetime.date(2020, 5, 25),
     datetime.date(2020, 5, 26),
     datetime.date(2020, 5, 27),
     datetime.date(2020, 5, 28),
     datetime.date(2020, 5, 29),
     datetime.date(2020, 5, 30),
     datetime.date(2020, 5, 31),
     datetime.date(2020, 6, 1),
     datetime.date(2020, 6, 2),
     datetime.date(2020, 6, 3),
     datetime.date(2020, 6, 4),
     datetime.date(2020, 6, 5),
     datetime.date(2020, 6, 6),
     datetime.date(2020, 6, 7),
     datetime.date(2020, 6, 8),
     datetime.date(2020, 6, 9),
     datetime.date(2020, 6, 10),
     datetime.date(2020, 6, 11),
     datetime.date(2020, 6, 12),
     datetime.date(2020, 6, 13),
     datetime.date(2020, 6, 14),
     datetime.date(2020, 6, 15),
     datetime.date(2020, 6, 16),
     datetime.date(2020, 6, 17),
     datetime.date(2020, 6, 18),
     datetime.date(2020, 6, 19),
     datetime.date(2020, 6, 20),
     datetime.date(2020, 6, 21),
     datetime.date(2020, 6, 22),
     datetime.date(2020, 6, 23)]
```



```python
china = china.set_index('provinceName')
china = china.reset_index()
```

分析：循环遍历省份和日期获得每个省份每日累计确诊，因为需要拼接，先初始化一个date_confirmed


```python
list_p = []
list_d = []
list_e = []
for p in range(0,32):
    try:
        con_0 = china.loc[china['updateTime'] == date[2]].loc[china['provinceName'] ==  province[p]].iloc[[0]].iloc[0] 
        list_p.append(con_0['province_confirmedCount'])#该日每省的累计确诊人数
    except:
        continue
list_d.append(sum(list_p))
list_e.append(str(date[0]))
date_confirmed = pd.DataFrame(list_d,index=list_e)
date_confirmed.index.name="date"
date_confirmed.columns=["China_confirmedCount"]
date_confirmed
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>China_confirmedCount</th>
    </tr>
    <tr>
      <th>date</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2020-01-24</th>
      <td>1956.0</td>
    </tr>
  </tbody>
</table>
</div>



分析：遍历每个省份拼接每日的总确诊人数的dataframe


```python
l = 0
for i in date[3:]:
    list_p = []
    list_d = []
    list_e = []
    l +=1
    for p in range(0,32):
        try:
            con_0 = china.loc[china['updateTime'] == date[l]].loc[china['provinceName'] ==  province[p]].iloc[[0]].iloc[0] 
            list_p.append(con_0['province_confirmedCount'])#该日每省的累计确诊人数
        except:
            continue
    #con_0 = china.loc[china['updateTime'] == date[0]].loc[china['provinceName'] == '河北省'].loc[[0]].iloc[0]
    #list_p.append(con_0['province_confirmedCount'])#该日每省的累计确诊人数
    list_d.append(sum(list_p))
    list_e.append(str(date[l]))
    confirmed = pd.DataFrame(list_d, index=list_e)
    confirmed.index.name="date"
    confirmed.columns=["China_confirmedCount"]
    date_confirmed = pd.concat([date_confirmed,confirmed],sort=False)
date_confirmed
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>China_confirmedCount</th>
    </tr>
    <tr>
      <th>date</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2020-01-24</th>
      <td>1956.0</td>
    </tr>
    <tr>
      <th>2020-01-25</th>
      <td>2253.0</td>
    </tr>
    <tr>
      <th>2020-01-26</th>
      <td>1956.0</td>
    </tr>
    <tr>
      <th>2020-01-27</th>
      <td>2825.0</td>
    </tr>
    <tr>
      <th>2020-01-28</th>
      <td>4589.0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
    </tr>
    <tr>
      <th>2020-06-17</th>
      <td>8106.0</td>
    </tr>
    <tr>
      <th>2020-06-18</th>
      <td>6862.0</td>
    </tr>
    <tr>
      <th>2020-06-19</th>
      <td>6894.0</td>
    </tr>
    <tr>
      <th>2020-06-20</th>
      <td>6921.0</td>
    </tr>
    <tr>
      <th>2020-06-21</th>
      <td>6157.0</td>
    </tr>
  </tbody>
</table>
<p>150 rows × 1 columns</p>
</div>



分析：去除空值和不全的值


```python
date_confirmed.dropna(subset=['China_confirmedCount'],inplace=True)
date_confirmed.tail(20)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>China_confirmedCount</th>
    </tr>
    <tr>
      <th>date</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2020-06-02</th>
      <td>78782.0</td>
    </tr>
    <tr>
      <th>2020-06-03</th>
      <td>78780.0</td>
    </tr>
    <tr>
      <th>2020-06-04</th>
      <td>76903.0</td>
    </tr>
    <tr>
      <th>2020-06-05</th>
      <td>76908.0</td>
    </tr>
    <tr>
      <th>2020-06-06</th>
      <td>8777.0</td>
    </tr>
    <tr>
      <th>2020-06-07</th>
      <td>8782.0</td>
    </tr>
    <tr>
      <th>2020-06-08</th>
      <td>8628.0</td>
    </tr>
    <tr>
      <th>2020-06-09</th>
      <td>8634.0</td>
    </tr>
    <tr>
      <th>2020-06-10</th>
      <td>8638.0</td>
    </tr>
    <tr>
      <th>2020-06-11</th>
      <td>8649.0</td>
    </tr>
    <tr>
      <th>2020-06-12</th>
      <td>8658.0</td>
    </tr>
    <tr>
      <th>2020-06-13</th>
      <td>8665.0</td>
    </tr>
    <tr>
      <th>2020-06-14</th>
      <td>8733.0</td>
    </tr>
    <tr>
      <th>2020-06-15</th>
      <td>8772.0</td>
    </tr>
    <tr>
      <th>2020-06-16</th>
      <td>8055.0</td>
    </tr>
    <tr>
      <th>2020-06-17</th>
      <td>8106.0</td>
    </tr>
    <tr>
      <th>2020-06-18</th>
      <td>6862.0</td>
    </tr>
    <tr>
      <th>2020-06-19</th>
      <td>6894.0</td>
    </tr>
    <tr>
      <th>2020-06-20</th>
      <td>6921.0</td>
    </tr>
    <tr>
      <th>2020-06-21</th>
      <td>6157.0</td>
    </tr>
  </tbody>
</table>
</div>



分析：数据从4月末开始到5月末就因为缺失过多省份的数据(部分省份从4月末至今再也没有新增病患)而失真，自2020-06-06起完全失去真实性，所以我删除了2020-06-06往后的数据


```python
date_confirmed = date_confirmed.drop(['2020-06-06','2020-06-07','2020-06-08','2020-06-09','2020-06-10','2020-06-11','2020-06-12','2020-06-13','2020-06-14',
                     '2020-06-15','2020-06-16','2020-06-19','2020-06-18','2020-06-20','2020-06-17','2020-06-21'])
```

分析：构造拼接函数


```python
def data_frame(self,china,element):
    l = 0
    for i in date[3:]:
        list_p = []
        list_d = []
        list_e = []
        l +=1
        for p in range(0,32):
            try:
                con_0 = china.loc[china['updateTime'] == date[l]].loc[china['provinceName'] ==  province[p]].iloc[[0]].iloc[0] 
                list_p.append(con_0[element])
            except:
                continue
        #con_0 = china.loc[china['updateTime'] == date[0]].loc[china['provinceName'] == '河北省'].loc[[0]].iloc[0]
        #list_p.append(con_0['province_confirmedCount'])
        list_d.append(sum(list_p))
        list_e.append(str(date[l]))
        link = pd.DataFrame(list_d, index=list_e)
        link.index.name="date"
        link.columns=["China"]
        self = pd.concat([self,link],sort=False)
    self.dropna(subset=['China'],inplace=True)
    self = self.drop(['2020-06-06','2020-06-07','2020-06-08','2020-06-09','2020-06-10','2020-06-11','2020-06-12','2020-06-13','2020-06-14',
                  '2020-06-15','2020-06-16','2020-06-19','2020-06-18','2020-06-20','2020-06-17','2020-06-21'])
    return self
```

分析：初始化各个变量


```python
#累计治愈人数  date_curedCount
list_p = []
list_d = []
list_e = []
for p in range(0,32):
    try:
        con_0 = china.loc[china['updateTime'] == date[2]].loc[china['provinceName'] ==  province[p]].iloc[[0]].iloc[0] 
        list_p.append(con_0['province_curedCount'])
    except:
        continue
list_d.append(sum(list_p))
list_e.append(str(date[0]))
date_cured = pd.DataFrame(list_d, index=list_e)
date_cured.index.name="date"
date_cured.columns=["China"]



#累计死亡人数  date_dead
list_p = []
list_d = []
list_e = []
for p in range(0,32):
    try:
        con_0 = china.loc[china['updateTime'] == date[2]].loc[china['provinceName'] ==  province[p]].iloc[[0]].iloc[0] 
        list_p.append(con_0['province_deadCount'])
    except:
        continue
list_d.append(sum(list_p))
list_e.append(str(date[0]))
date_dead = pd.DataFrame(list_d, index=list_e)
date_dead.index.name="date"
date_dead.columns=["China"]
```


```python
#累计确诊患者  date_confirmed
plt.rcParams['font.sans-serif'] = ['SimHei'] #更改字体,否则无法显示汉字
fig = plt.figure( figsize=(16,6), dpi=100)
ax = fig.add_subplot(1,1,1)
x = date_confirmed.index
y = date_confirmed.values
ax.plot( x, y, color=dt_hex, linewidth=2, linestyle='-' )
ax.set_title('累计确诊患者',fontdict={
      'color':'black',
      'size':24
})
ax.set_xticks( range(0,len(x),30))
```


![在这里插入图片描述](https://img-blog.csdnimg.cn/20200704145349924.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3NwaXJpdExITA==,size_16,color_FFFFFF,t_70#pic_center)



```python
#累计治愈患者 date_curedCount
date_cured = data_frame(date_cured,china,'province_curedCount')
fig = plt.figure( figsize=(16,6), dpi=100)
ax = fig.add_subplot(1,1,1)
x = date_cured.index
y = date_cured.values
ax.set_title('累计治愈患者',fontdict={
      'color':'black',
      'size':24
})
ax.plot( x, y, color=dt_hex, linewidth=2, linestyle='-' )
ax.set_xticks( range(0,len(x),30))
```



![在这里插入图片描述](https://img-blog.csdnimg.cn/20200704145409832.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3NwaXJpdExITA==,size_16,color_FFFFFF,t_70#pic_center)

分析：累计疑似无法通过补全数据得到


```python
#累计死亡患者 date_dead
date_dead = data_frame(date_dead,china,'province_deadCount')
fig = plt.figure( figsize=(16,6), dpi=100)
ax = fig.add_subplot(1,1,1)
x = date_dead.index
y = date_dead.values
ax.plot( x, y, color=dt_hex, linewidth=2, linestyle='-' )
x_major_locator=MultipleLocator(12)
ax=plt.gca()
ax.set_title('累计死亡患者',fontdict={
      'color':'black',
      'size':24
})
ax.xaxis.set_major_locator(x_major_locator)
ax.set_xticks( range(0,len(x),30))
```




![在这里插入图片描述](https://img-blog.csdnimg.cn/2020070414542492.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3NwaXJpdExITA==,size_16,color_FFFFFF,t_70#pic_center)


分析：疫情自1月初开始爆发，到2月末开始减缓增速，到4月末趋于平缓。治愈人数自2月初开始大幅增加，到3月末趋于平缓，死亡人数自1月末开始增加，到2月末趋于平缓，到4月末因为统计因素死亡人数飙升后趋于平缓。      
分析总结：确诊人数数据和治愈数据从4月末开始到5月末就因为缺失过多省份的数据(部分省份至今再也没有新增病患)导致失真，其他数据尽量通过补全,越靠近尾部数据越失真。死亡数据补全较为成功，几乎没有错漏。

#### （二）全国新增确诊/疑似/治愈/死亡情况随时间变化趋势如何？

分析：新增确诊/治愈/死亡的数据需要对china进行运算，每省每日进行diff差值运算

分析：首先初始化各个数据，然后仿照上面的拼接函数，作适用于该题的拼接函数


```python
#新增确诊人数  date_new_confirmed
list_p = []
list_d = []
list_e = []
for p in range(0,32):
    try:
        con_0 = china.loc[china['updateTime'] == date[2]].loc[china['provinceName'] ==  province[p]].iloc[[0]].iloc[0] 
        list_p.append(con_0['province_confirmedCount'])#该日每省的累计确诊人数
    except:
        continue
list_d.append(sum(list_p))
list_e.append(str(date[0]))
date_new_confirmed = pd.DataFrame(list_d,index=list_e)
date_new_confirmed.index.name="date"
date_new_confirmed.columns=["China"]
date_new_confirmed


#新增治愈人数  date_new_curedCount
list_p = []
list_d = []
list_e = []
for p in range(0,32):
    try:
        con_0 = china.loc[china['updateTime'] == date[2]].loc[china['provinceName'] ==  province[p]].iloc[[0]].iloc[0] 
        list_p.append(con_0['province_curedCount'])
    except:
        continue
list_d.append(sum(list_p))
list_e.append(str(date[0]))
date_new_cured = pd.DataFrame(list_d, index=list_e)
date_new_cured.index.name="date"
date_new_cured.columns=["China"]


#新增死亡人数  date_new_dead
list_p = []
list_d = []
list_e = []
for p in range(0,32):
    try:
        con_0 = china.loc[china['updateTime'] == date[2]].loc[china['provinceName'] ==  province[p]].iloc[[0]].iloc[0] 
        list_p.append(con_0['province_deadCount'])
    except:
        continue
list_d.append(sum(list_p))
list_e.append(str(date[0]))
date_new_dead = pd.DataFrame(list_d, index=list_e)
date_new_dead.index.name="date"
date_new_dead.columns=["China"]
```

分析：构造拼接函数


```python
def data_new_frame(self,china,element):
    l = 0
    for i in date[3:]:
        list_p = []
        list_d = []
        list_e = []
        l +=1
        for p in range(0,32):
            try:
                con_0 = china.loc[china['updateTime'] == date[l]].loc[china['provinceName'] ==  province[p]].iloc[[0]].iloc[0] 
                list_p.append(con_0[element])
            except:
                continue
        #con_0 = china.loc[china['updateTime'] == date[0]].loc[china['provinceName'] == '河北省'].loc[[0]].iloc[0]
        #list_p.append(con_0['province_confirmedCount'])
        list_d.append(sum(list_p))
        list_e.append(str(date[l]))
        link = pd.DataFrame(list_d, index=list_e)
        link.index.name="date"
        link.columns=["China"]
        self = pd.concat([self,link],sort=False)
    self.dropna(subset=['China'],inplace=True)
    return self
```

分析：数据补全以及去除含缺失省份的数据


```python
d = data_new_frame(date_new_confirmed,china,'province_confirmedCount')
for i in range(len(d)):
    dr = []
    for a,b in zip(range(0,len(d)-1),range(1,len(d)-2)):
        if d.iloc[b].iloc[0] < d.iloc[a].iloc[0]:
            dr.append(d.iloc[b].iloc[0])
    d = d[~d['China'].isin(dr)]
```

分析：做差值运算


```python
d['China'] = d['China'].diff()
```

分析：去除两个含缺失省份的日期


```python
d.drop(['2020-06-20','2020-06-21'],inplace=True)
```

分析：作折线图表现时间趋势


```python
#新增确诊患者  date_confirmed
fig = plt.figure( figsize=(16,6), dpi=100)
ax = fig.add_subplot(1,1,1)
x = d.index
y = d.values
ax.set_title('新增确诊患者',fontdict={
      'color':'black',
      'size':24
})
ax.plot( x, y, color=dt_hex, linewidth=2, linestyle='-' )
ax.set_xticks( range(0,len(x),10))
```




    [<matplotlib.axis.XTick at 0x25552a9c898>,
     <matplotlib.axis.XTick at 0x25552a9c860>,
     <matplotlib.axis.XTick at 0x25552ab7550>,
     <matplotlib.axis.XTick at 0x25552ad50f0>,
     <matplotlib.axis.XTick at 0x25552ad5518>,
     <matplotlib.axis.XTick at 0x25552ad59b0>,
     <matplotlib.axis.XTick at 0x25552ad5e48>,
     <matplotlib.axis.XTick at 0x25552adc320>]



![在这里插入图片描述](https://img-blog.csdnimg.cn/20200704145442353.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3NwaXJpdExITA==,size_16,color_FFFFFF,t_70#pic_center)


分析：使用初始化数据构造date_new_cured的dataframe，然后作折线图表现时间趋势


```python
cu = data_new_frame(date_new_cured,china,'province_curedCount')
for i in range(len(cu)):
    dr = []
    for a,b in zip(range(0,len(cu)-1),range(1,len(cu)-2)):
        if cu.iloc[b].iloc[0] < cu.iloc[a].iloc[0]:
            dr.append(cu.iloc[b].iloc[0])
    cu = cu[~cu['China'].isin(dr)]
cu['China'] = cu['China'].diff()
cu.drop(['2020-06-20','2020-06-21'],inplace=True)
#新增治愈患者  date_new_cured
fig = plt.figure( figsize=(16,6), dpi=100)
ax = fig.add_subplot(1,1,1)
x = cu.index
y = cu.values
ax.set_title('新增治愈患者',fontdict={
      'color':'black',
      'size':24
})
ax.plot( x, y, color=dt_hex, linewidth=2, linestyle='-' )
ax.set_xticks( range(0,len(x),10))
```




    [<matplotlib.axis.XTick at 0x25552b13b00>,
     <matplotlib.axis.XTick at 0x25552b13ac8>,
     <matplotlib.axis.XTick at 0x25552b137b8>,
     <matplotlib.axis.XTick at 0x25552b3f470>,
     <matplotlib.axis.XTick at 0x25552b3f908>,
     <matplotlib.axis.XTick at 0x25552b3fda0>,
     <matplotlib.axis.XTick at 0x25552b47278>]




![在这里插入图片描述](https://img-blog.csdnimg.cn/20200704145506708.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3NwaXJpdExITA==,size_16,color_FFFFFF,t_70#pic_center)


分析：使用初始化数据构造date_new_dead的dataframe，然后作折线图表现时间趋势


```python
de = data_new_frame( date_new_dead,china,'province_deadCount')
for i in range(len(de)):
    dr = []
    for a,b in zip(range(0,len(de)-1),range(1,len(de)-2)):
        if de.iloc[b].iloc[0] < de.iloc[a].iloc[0]:
            dr.append(de.iloc[b].iloc[0])
    de = de[~de['China'].isin(dr)]
de['China'] = de['China'].diff()
de.drop(['2020-06-21'],inplace=True)
#新增死亡患者   date_new_dead
fig = plt.figure( figsize=(16,6), dpi=100)
ax = fig.add_subplot(1,1,1)
x = de.index
y = de.values
ax.set_title('新增死亡患者',fontdict={
      'color':'black',
      'size':24
})
ax.plot( x, y, color=dt_hex, linewidth=2, linestyle='-' )
ax.set_xticks( range(0,len(x),10))
```




    [<matplotlib.axis.XTick at 0x25553bdfd30>,
     <matplotlib.axis.XTick at 0x25553bdfcf8>,
     <matplotlib.axis.XTick at 0x25553c01f60>,
     <matplotlib.axis.XTick at 0x25553c146a0>,
     <matplotlib.axis.XTick at 0x25553c14b38>,
     <matplotlib.axis.XTick at 0x25553c14d68>,
     <matplotlib.axis.XTick at 0x25553c1b4a8>,
     <matplotlib.axis.XTick at 0x25553c1b940>,
     <matplotlib.axis.XTick at 0x25553c1bdd8>]




![在这里插入图片描述](https://img-blog.csdnimg.cn/202007041458429.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3NwaXJpdExITA==,size_16,color_FFFFFF,t_70#pic_center)

分析：新增患者自1月末开始增加，到2月14日前后到达顶点，后增数下降，趋于平缓。    
分析：新增治愈患者自1月末开始增加，到3月02日前后达到顶峰，后增数下降，从4月初开始趋于平缓。    
分析：新增死亡患者自1月末开始增加，到2月达到高峰，自3月初开始增数平缓，到4月17日前后因为统计因素飙升后回落。  

#### （三）全国新增境外输入随时间变化趋势如何？

分析：新增境外输入数据需要对CHINA进行运算，逐日相减。

分析：先从CHINA取出境外输入的数据，然后补全时间序列并作差。


```python
imported = CHINA.loc[CHINA['cityName'] == '境外输入']
imported.updateTime = pd.to_datetime(imported.updateTime,format="%Y-%m-%d",errors='coerce').dt.date
imported
```

    D:\Anaconda\envs\python32\lib\site-packages\pandas\core\generic.py:5303: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      self[name] = value
    




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>continentName</th>
      <th>continentEnglishName</th>
      <th>countryName</th>
      <th>countryEnglishName</th>
      <th>provinceName</th>
      <th>provinceEnglishName</th>
      <th>province_zipCode</th>
      <th>province_confirmedCount</th>
      <th>province_suspectedCount</th>
      <th>province_curedCount</th>
      <th>province_deadCount</th>
      <th>updateTime</th>
      <th>cityName</th>
      <th>cityEnglishName</th>
      <th>city_zipCode</th>
      <th>city_confirmedCount</th>
      <th>city_suspectedCount</th>
      <th>city_curedCount</th>
      <th>city_deadCount</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>136</th>
      <td>亚洲</td>
      <td>Asia</td>
      <td>中国</td>
      <td>China</td>
      <td>陕西省</td>
      <td>Shaanxi</td>
      <td>610000</td>
      <td>317</td>
      <td>1.0</td>
      <td>307</td>
      <td>3</td>
      <td>2020-06-23</td>
      <td>境外输入</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>72.0</td>
      <td>0.0</td>
      <td>65.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>150</th>
      <td>亚洲</td>
      <td>Asia</td>
      <td>中国</td>
      <td>China</td>
      <td>江苏省</td>
      <td>Jiangsu</td>
      <td>320000</td>
      <td>654</td>
      <td>3.0</td>
      <td>653</td>
      <td>0</td>
      <td>2020-06-23</td>
      <td>境外输入</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>23.0</td>
      <td>0.0</td>
      <td>22.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>201</th>
      <td>亚洲</td>
      <td>Asia</td>
      <td>中国</td>
      <td>China</td>
      <td>北京市</td>
      <td>Beijing</td>
      <td>110000</td>
      <td>843</td>
      <td>164.0</td>
      <td>584</td>
      <td>9</td>
      <td>2020-06-23</td>
      <td>境外输入</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>174.0</td>
      <td>3.0</td>
      <td>173.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>214</th>
      <td>亚洲</td>
      <td>Asia</td>
      <td>中国</td>
      <td>China</td>
      <td>河北省</td>
      <td>Hebei</td>
      <td>130000</td>
      <td>346</td>
      <td>0.0</td>
      <td>323</td>
      <td>6</td>
      <td>2020-06-23</td>
      <td>境外输入</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>10.0</td>
      <td>0.0</td>
      <td>10.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>218</th>
      <td>亚洲</td>
      <td>Asia</td>
      <td>中国</td>
      <td>China</td>
      <td>天津市</td>
      <td>Tianjin</td>
      <td>120000</td>
      <td>198</td>
      <td>48.0</td>
      <td>192</td>
      <td>3</td>
      <td>2020-06-23</td>
      <td>境外输入</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>61.0</td>
      <td>0.0</td>
      <td>59.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>115420</th>
      <td>亚洲</td>
      <td>Asia</td>
      <td>中国</td>
      <td>China</td>
      <td>陕西省</td>
      <td>Shaanxi</td>
      <td>610000</td>
      <td>250</td>
      <td>1.0</td>
      <td>240</td>
      <td>3</td>
      <td>2020-03-25</td>
      <td>境外输入</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>5.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>115956</th>
      <td>亚洲</td>
      <td>Asia</td>
      <td>中国</td>
      <td>China</td>
      <td>天津市</td>
      <td>Tianjin</td>
      <td>120000</td>
      <td>145</td>
      <td>0.0</td>
      <td>133</td>
      <td>3</td>
      <td>2020-03-24</td>
      <td>境外输入</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>9.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>116164</th>
      <td>亚洲</td>
      <td>Asia</td>
      <td>中国</td>
      <td>China</td>
      <td>甘肃省</td>
      <td>Gansu</td>
      <td>620000</td>
      <td>136</td>
      <td>0.0</td>
      <td>119</td>
      <td>2</td>
      <td>2020-03-24</td>
      <td>境外输入</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>45.0</td>
      <td>0.0</td>
      <td>30.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>117171</th>
      <td>亚洲</td>
      <td>Asia</td>
      <td>中国</td>
      <td>China</td>
      <td>上海市</td>
      <td>Shanghai</td>
      <td>310000</td>
      <td>414</td>
      <td>0.0</td>
      <td>330</td>
      <td>4</td>
      <td>2020-03-24</td>
      <td>境外输入</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>75.0</td>
      <td>0.0</td>
      <td>3.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>117597</th>
      <td>亚洲</td>
      <td>Asia</td>
      <td>中国</td>
      <td>China</td>
      <td>天津市</td>
      <td>Tianjin</td>
      <td>120000</td>
      <td>142</td>
      <td>0.0</td>
      <td>133</td>
      <td>3</td>
      <td>2020-03-24</td>
      <td>境外输入</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>6.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
  </tbody>
</table>
<p>607 rows × 19 columns</p>
</div>



分析：补全省份缺失时间的数据


```python
for i in range(0,len(province)):
    list_j_d = []
    date_b = []
    for dt in imported.loc[imported['provinceName'] ==  province[i]]['updateTime']:
        date_b.append(str(dt))
    list_j_d = list(set(date_b))
    list_j_d.sort()
    #imported.loc[imported['provinceName'] == province[3]]
    try:
        start = imported.loc[imported['provinceName'] ==  province[i]]['updateTime'].min()
        end = imported.loc[imported['provinceName'] ==  province[i]]['updateTime'].max()
        dates_b = pd.date_range(start=str(start), end=str(end))
        aid_frame_b = pd.DataFrame({'updateTime': dates_b,'provinceName':[province[i]]*len(dates_b)})
        aid_frame_b.updateTime = pd.to_datetime(aid_frame_b.updateTime,format="%Y-%m-%d",errors='coerce').dt.date
        #draft = pd.merge(china.loc[china['provinceName'] ==  province[1]], aid_frame, on='updateTime', how='outer').sort_values('updateTime')
        draft_b = pd.concat([imported.loc[imported['provinceName'] ==  province[i]], aid_frame_b], join='outer').sort_values('updateTime')
        draft_b.city_confirmedCount.fillna(method="ffill",inplace=True)
        draft_b.city_suspectedCount.fillna(method="ffill", inplace=True)
        draft_b.city_curedCount.fillna(method="ffill", inplace=True)
        draft_b.city_deadCount.fillna(method="ffill", inplace=True)
        draft_b.loc[draft_b['provinceName'] ==  province[i]].fillna(0,inplace=True,limit = 1)
        draft_b.loc[draft_b['provinceName'] ==  province[i]].loc[:,'city_confirmedCount':'city_deadCount'] = draft_b.loc[draft_b['provinceName'] ==  province[i]].loc[:,'city_confirmedCount':'city_deadCount'].diff()
        draft_b.dropna(subset=['city_confirmedCount','city_suspectedCount','city_curedCount','city_deadCount'],inplace=True)
        imported = pd.concat([imported,draft_b], join='outer').sort_values('updateTime')
    except:
        continue
imported
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>continentName</th>
      <th>continentEnglishName</th>
      <th>countryName</th>
      <th>countryEnglishName</th>
      <th>provinceName</th>
      <th>provinceEnglishName</th>
      <th>province_zipCode</th>
      <th>province_confirmedCount</th>
      <th>province_suspectedCount</th>
      <th>province_curedCount</th>
      <th>province_deadCount</th>
      <th>updateTime</th>
      <th>cityName</th>
      <th>cityEnglishName</th>
      <th>city_zipCode</th>
      <th>city_confirmedCount</th>
      <th>city_suspectedCount</th>
      <th>city_curedCount</th>
      <th>city_deadCount</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>115956</th>
      <td>亚洲</td>
      <td>Asia</td>
      <td>中国</td>
      <td>China</td>
      <td>天津市</td>
      <td>Tianjin</td>
      <td>120000.0</td>
      <td>145.0</td>
      <td>0.0</td>
      <td>133.0</td>
      <td>3.0</td>
      <td>2020-03-24</td>
      <td>境外输入</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>9.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>0</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>甘肃省</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2020-03-24</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>45.0</td>
      <td>0.0</td>
      <td>30.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>117597</th>
      <td>亚洲</td>
      <td>Asia</td>
      <td>中国</td>
      <td>China</td>
      <td>天津市</td>
      <td>Tianjin</td>
      <td>120000.0</td>
      <td>142.0</td>
      <td>0.0</td>
      <td>133.0</td>
      <td>3.0</td>
      <td>2020-03-24</td>
      <td>境外输入</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>6.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>117597</th>
      <td>亚洲</td>
      <td>Asia</td>
      <td>中国</td>
      <td>China</td>
      <td>天津市</td>
      <td>Tianjin</td>
      <td>120000.0</td>
      <td>142.0</td>
      <td>0.0</td>
      <td>133.0</td>
      <td>3.0</td>
      <td>2020-03-24</td>
      <td>境外输入</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>6.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>116164</th>
      <td>亚洲</td>
      <td>Asia</td>
      <td>中国</td>
      <td>China</td>
      <td>甘肃省</td>
      <td>Gansu</td>
      <td>620000.0</td>
      <td>136.0</td>
      <td>0.0</td>
      <td>119.0</td>
      <td>2.0</td>
      <td>2020-03-24</td>
      <td>境外输入</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>45.0</td>
      <td>0.0</td>
      <td>30.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>150</th>
      <td>亚洲</td>
      <td>Asia</td>
      <td>中国</td>
      <td>China</td>
      <td>江苏省</td>
      <td>Jiangsu</td>
      <td>320000.0</td>
      <td>654.0</td>
      <td>3.0</td>
      <td>653.0</td>
      <td>0.0</td>
      <td>2020-06-23</td>
      <td>境外输入</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>23.0</td>
      <td>0.0</td>
      <td>22.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>136</th>
      <td>亚洲</td>
      <td>Asia</td>
      <td>中国</td>
      <td>China</td>
      <td>陕西省</td>
      <td>Shaanxi</td>
      <td>610000.0</td>
      <td>317.0</td>
      <td>1.0</td>
      <td>307.0</td>
      <td>3.0</td>
      <td>2020-06-23</td>
      <td>境外输入</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>72.0</td>
      <td>0.0</td>
      <td>65.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>91</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>天津市</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2020-06-23</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>61.0</td>
      <td>0.0</td>
      <td>59.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>136</th>
      <td>亚洲</td>
      <td>Asia</td>
      <td>中国</td>
      <td>China</td>
      <td>陕西省</td>
      <td>Shaanxi</td>
      <td>610000.0</td>
      <td>317.0</td>
      <td>1.0</td>
      <td>307.0</td>
      <td>3.0</td>
      <td>2020-06-23</td>
      <td>境外输入</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>72.0</td>
      <td>0.0</td>
      <td>65.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>201</th>
      <td>亚洲</td>
      <td>Asia</td>
      <td>中国</td>
      <td>China</td>
      <td>北京市</td>
      <td>Beijing</td>
      <td>110000.0</td>
      <td>843.0</td>
      <td>164.0</td>
      <td>584.0</td>
      <td>9.0</td>
      <td>2020-06-23</td>
      <td>境外输入</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>174.0</td>
      <td>3.0</td>
      <td>173.0</td>
      <td>0.0</td>
    </tr>
  </tbody>
</table>
<p>2524 rows × 19 columns</p>
</div>



分析：作copy()防止数据处理失误使得原数据丢失


```python
draft_i = imported.copy()
```

分析：初始化一个省份数据，保证这个方法可行


```python
real_s = imported.loc[imported['provinceName'] == province[0]]
real_s.drop_duplicates(subset='updateTime', keep='first', inplace=True)
draft_i =  real_s
for p in province:
    real_data = imported.loc[imported['provinceName'] == p]
    real_data.drop_duplicates(subset='updateTime', keep='first', inplace=True)
    #imported = pd.concat([real_data, china],sort=False)
    draft_i = pd.concat([real_data,draft_i],sort=False)
```

    D:\Anaconda\envs\python32\lib\site-packages\ipykernel_launcher.py:2: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      
    D:\Anaconda\envs\python32\lib\site-packages\ipykernel_launcher.py:6: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      
    

分析：确认方法无误，对余下省份进行相同的处理


```python
imported = draft_i
```


```python
imported = imported.set_index('provinceName')
imported = imported.reset_index()
```

分析：进行各个省份的数据合并。


```python
list_p = []
list_d = []
list_e = []
for p in range(0,32):
    try:
        con_0 = imported.loc[imported['updateTime'] == date[2]].loc[imported['provinceName'] ==  province[p]].iloc[[0]].iloc[0] 
        list_p.append(con_0['city_confirmedCount'])#该日每省的累计确诊人数
    except:
        continue
list_d.append(sum(list_p))
list_e.append(str(date[0]))
date_new_foreign_confirmed = pd.DataFrame(list_d,index=list_e)
date_new_foreign_confirmed.index.name="date"
date_new_foreign_confirmed.columns=["imported_confirmedCount"]
date_new_foreign_confirmed
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>imported_confirmedCount</th>
    </tr>
    <tr>
      <th>date</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2020-01-24</th>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>




```python
l = 0
for i in date[3:]:
    list_p = []
    list_d = []
    list_e = []
    l +=1
    for p in range(0,32):
        try:
            con_0 = imported.loc[imported['updateTime'] == date[l]].loc[imported['provinceName'] ==  province[p]].iloc[[0]].iloc[0] 
            list_p.append(con_0['city_confirmedCount'])#该日每省的累计确诊人数
        except:
            continue
    #con_0 = imported.loc[imported['updateTime'] == date[0]].loc[imported['provinceName'] == '河北省'].loc[[0]].iloc[0]
    #list_p.append(con_0['city_confirmedCount'])#该日每省的累计确诊人数
    list_d.append(sum(list_p))
    list_e.append(str(date[l]))
    confirmed = pd.DataFrame(list_d, index=list_e)
    confirmed.index.name="date"
    confirmed.columns=["imported_confirmedCount"]
    date_new_foreign_confirmed = pd.concat([date_new_foreign_confirmed,confirmed],sort=False)
date_new_foreign_confirmed
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>imported_confirmedCount</th>
    </tr>
    <tr>
      <th>date</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2020-01-24</th>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2020-01-25</th>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2020-01-26</th>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2020-01-27</th>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2020-01-28</th>
      <td>0.0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
    </tr>
    <tr>
      <th>2020-06-17</th>
      <td>848.0</td>
    </tr>
    <tr>
      <th>2020-06-18</th>
      <td>800.0</td>
    </tr>
    <tr>
      <th>2020-06-19</th>
      <td>800.0</td>
    </tr>
    <tr>
      <th>2020-06-20</th>
      <td>802.0</td>
    </tr>
    <tr>
      <th>2020-06-21</th>
      <td>775.0</td>
    </tr>
  </tbody>
</table>
<p>150 rows × 1 columns</p>
</div>




```python
#新增境外输入
fig = plt.figure( figsize=(16,4), dpi=100)
ax = fig.add_subplot(1,1,1)
x = date_new_foreign_confirmed.index
y = date_new_foreign_confirmed.values
plot = ax.plot( x, y, color=dt_hex, linewidth=2, linestyle='-',label='date_new_foreign_confirmed' )
ax.set_xticks( range(0,len(x),10))
plt.xlabel('日期',fontsize=20)
plt.ylabel('人数',fontsize=20)
plt.title('COVID-19——新增境外输入',fontsize=30)
ax.legend( loc=0, frameon=True )
```


![在这里插入图片描述](https://img-blog.csdnimg.cn/20200704145550512.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3NwaXJpdExITA==,size_16,color_FFFFFF,t_70#pic_center)


分析总结：境外输入病例自3月末开始激增，到5月初增速趋于平缓，到6月初开始增速减缓。

#### （四）你所在的省市情况如何？

分析：首先取出广东省的所有时间序列,转换成string类型,然后进行排序


```python
m_dates = list(set(myhome['updateTime']))
aid_d = m_dates.copy()
for d in aid_d:
    a = str(d)
    m_dates.remove(d)
    m_dates.append(a)
m_dates.sort()
```


```python
myhome = myhome.set_index('provinceName')
myhome = myhome.reset_index()
```

分析：遍历我的城市对应的省份的时间构建对应的dataframe


```python
#广东省累计确诊人数
list_g = []
for i in range(0,len(m_dates)):
    try:
        con_m = myhome.loc[myhome['updateTime'] == date[i]].loc[myhome['cityName'] == '茂名'].iloc[[0]].iloc[0] 
        list_g.append(con_m['province_confirmedCount'])
    except:
        list_g.append(0)
        continue
g_date_confirmed = pd.DataFrame(list_g, index=m_dates)
g_date_confirmed.index.name="date"
g_date_confirmed.columns=["g_confirmed"]
g_date_confirmed=g_date_confirmed[~g_date_confirmed['g_confirmed'].isin([0])]


#广东省累计治愈人数
list_g = []
for i in range(0,len(m_dates)):
    try:
        con_m = myhome.loc[myhome['updateTime'] == date[i]].loc[myhome['cityName'] == '茂名'].iloc[[0]].iloc[0] 
        list_g.append(con_m['province_curedCount'])
    except:
        list_g.append(0)
        continue
g_date_cured = pd.DataFrame(list_g, index=m_dates)
g_date_cured.index.name="date"
g_date_cured.columns=["g_cured"]
g_date_cured=g_date_cured[~g_date_cured['g_cured'].isin([0])]


#广东省累计死亡人数
list_g = []
for i in range(0,len(m_dates)):
    try:
        con_m = myhome.loc[myhome['updateTime'] == date[i]].loc[myhome['cityName'] == '茂名'].iloc[[0]].iloc[0] 
        list_g.append(con_m['province_deadCount'])
    except:
        list_g.append(0)
        continue
g_date_dead = pd.DataFrame(list_g, index=m_dates)
g_date_dead.index.name="date"
g_date_dead.columns=["g_dead"]
g_date_dead=g_date_dead[~g_date_dead['g_dead'].isin([0])]
```

分析：作折线图表现疫情随时间变化趋势


```python
##广东省累计确诊人数  广东省累计治愈人数
plt.rcParams['font.sans-serif'] = ['SimHei'] 
x= g_date_confirmed.index
y1 = g_date_confirmed.values
y2 = g_date_cured.values
y3 = g_date_dead
#font_manager = font_manager.FontProperties(fname = 'C:/Windows/Fonts/simsun.ttc',size = 18)
plt.figure(figsize=(20,10),dpi = 80)
plt.plot(x,y1,color = r_hex,label = 'confirmed')
plt.plot(x,y2,color = g_hex,label = 'cured')
x_major_locator=MultipleLocator(12)
ax=plt.gca()
ax.xaxis.set_major_locator(x_major_locator)
plt.title('COVID-19 —— 广东省',fontsize=30)
plt.xlabel('日期',fontsize=20)
plt.ylabel('人数',fontsize=20)
plt.legend(loc=1, bbox_to_anchor=(1.00,0.90), bbox_transform=ax.transAxes)
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200704145714716.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3NwaXJpdExITA==,size_16,color_FFFFFF,t_70#pic_center)



```python
#广东省累计死亡人数
plt.rcParams['font.sans-serif'] = ['SimHei'] 
fig = plt.figure( figsize=(16,4), dpi=100)
ax = fig.add_subplot(1,1,1)
x = g_date_dead.index
y = g_date_dead.values
plot = ax.plot( x, y, color=dt_hex, linewidth=2, linestyle='-',label='dead' )
ax.set_xticks( range(0,len(x),10))
plt.xlabel('日期',fontsize=20)
plt.ylabel('人数',fontsize=20)
plt.title('COVID-19——广东省',fontsize=30)
ax.legend( loc=0, frameon=True )
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200704145741377.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3NwaXJpdExITA==,size_16,color_FFFFFF,t_70#pic_center)



分析：广东省的数据补全很成功，真实性高。    
分析：从折线图来看，广东省自1月末起感染人数激增，直到2月中旬趋于平缓，3月初开始由于检测普及以及统计因素，短期确诊患者人数小幅度增加。广东省自2月初开始治愈人数激增，直到6月初开始因为新增感染人数趋于平缓，所以治愈人数趋于平缓。广东省自3月初开始不再有新增死亡患者。

#### （五）国外疫情态势如何？

分析：数据去除空值


```python
world.dropna(axis=1, how='any', inplace=True)
#world.set_index('updateTime')
```

分析：创建国家列表country，创建日期列表date_y


```python
country = list(set(world['provinceName']))
date_y = []
for dt in world.loc[world['provinceName'] ==  country[0]]['updateTime']:
    date_y.append(str(dt))
date_y = list(set(date_0))
date_y.sort()
```

分析：遍历国家列表对world中的updateTime进行处理并去重。


```python
for c in country:
    world.loc[world['provinceName'] == c].sort_values(by = 'updateTime')
world.dropna(subset=['provinceName'],inplace=True)
world.updateTime = pd.to_datetime(world.updateTime,format="%Y-%m-%d",errors='coerce').dt.date
```

分析：取前15个国家的province_confirmedCount透视构成world_confirmed，并进行数据补全处理


```python
world_confirmed = world.loc[world['provinceName'] == world.head(15)['provinceName'][0]].pivot_table(index='updateTime', columns='provinceName', values='province_confirmedCount',aggfunc=np.mean)
for i in world.head(15)['provinceName'][1:]:
    draft_c = world.loc[world['provinceName'] == i].pivot_table(index='updateTime', columns='provinceName', values='province_confirmedCount',aggfunc=np.mean)
    world_confirmed = pd.merge(world_confirmed,draft_c,on='updateTime', how='outer',sort=True)
world_confirmed.fillna(0,inplace=True,limit = 1)
world_confirmed.fillna(method="ffill",inplace=True)
world_confirmed
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>provinceName</th>
      <th>美国</th>
      <th>巴西</th>
      <th>英国</th>
      <th>俄罗斯</th>
      <th>智利</th>
      <th>印度</th>
      <th>巴基斯坦</th>
      <th>秘鲁</th>
      <th>西班牙</th>
      <th>孟加拉国</th>
      <th>法国</th>
      <th>沙特阿拉伯</th>
      <th>瑞典</th>
      <th>南非</th>
      <th>厄瓜多尔</th>
    </tr>
    <tr>
      <th>updateTime</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2020-01-27</th>
      <td>5.000000e+00</td>
      <td>0.00</td>
      <td>0.000000</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>3.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.0</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>2020-01-29</th>
      <td>0.000000e+00</td>
      <td>0.00</td>
      <td>0.000000</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>4.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.0</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>2020-01-30</th>
      <td>0.000000e+00</td>
      <td>0.00</td>
      <td>0.000000</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>5.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.0</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>2020-01-31</th>
      <td>6.000000e+00</td>
      <td>0.00</td>
      <td>2.000000</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>1.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.0</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>2020-02-01</th>
      <td>6.000000e+00</td>
      <td>0.00</td>
      <td>2.000000</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>1.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>4.00</td>
      <td>0.00</td>
      <td>5.500000</td>
      <td>0.000000</td>
      <td>1.000000</td>
      <td>0.0</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>2020-06-19</th>
      <td>2.184912e+06</td>
      <td>976906.50</td>
      <td>300469.000000</td>
      <td>563084.0</td>
      <td>225103.0</td>
      <td>371474.666667</td>
      <td>162935.600000</td>
      <td>243518.000000</td>
      <td>245268.00</td>
      <td>102292.00</td>
      <td>158641.000000</td>
      <td>145991.000000</td>
      <td>55672.750000</td>
      <td>83020.5</td>
      <td>48256.400000</td>
    </tr>
    <tr>
      <th>2020-06-20</th>
      <td>2.221982e+06</td>
      <td>1038568.00</td>
      <td>302138.750000</td>
      <td>573007.5</td>
      <td>231393.0</td>
      <td>390209.333333</td>
      <td>169464.666667</td>
      <td>247925.000000</td>
      <td>245665.75</td>
      <td>105535.00</td>
      <td>159452.000000</td>
      <td>151277.250000</td>
      <td>56201.500000</td>
      <td>87715.0</td>
      <td>49519.666667</td>
    </tr>
    <tr>
      <th>2020-06-21</th>
      <td>2.253118e+06</td>
      <td>1068977.25</td>
      <td>303284.428571</td>
      <td>579160.0</td>
      <td>236748.0</td>
      <td>399451.714286</td>
      <td>174346.222222</td>
      <td>251338.000000</td>
      <td>245938.00</td>
      <td>109657.75</td>
      <td>160093.000000</td>
      <td>154715.714286</td>
      <td>56360.000000</td>
      <td>92681.0</td>
      <td>49731.000000</td>
    </tr>
    <tr>
      <th>2020-06-22</th>
      <td>2.279603e+06</td>
      <td>1084312.25</td>
      <td>304331.000000</td>
      <td>587720.0</td>
      <td>243276.6</td>
      <td>416389.400000</td>
      <td>179148.750000</td>
      <td>254336.333333</td>
      <td>246272.00</td>
      <td>112306.00</td>
      <td>160336.428571</td>
      <td>158177.500000</td>
      <td>57346.000000</td>
      <td>96377.8</td>
      <td>50092.600000</td>
    </tr>
    <tr>
      <th>2020-06-23</th>
      <td>2.299650e+06</td>
      <td>1106470.00</td>
      <td>305289.000000</td>
      <td>592280.0</td>
      <td>246963.0</td>
      <td>425282.000000</td>
      <td>182562.666667</td>
      <td>257447.000000</td>
      <td>246504.00</td>
      <td>115786.00</td>
      <td>160750.000000</td>
      <td>161005.000000</td>
      <td>59060.666667</td>
      <td>101590.0</td>
      <td>50487.666667</td>
    </tr>
  </tbody>
</table>
<p>144 rows × 15 columns</p>
</div>



分析：作前15个国家的疫情随时间变动表


```python
#plt.rcParams['font.sans-serif'] = ['SimHei']  
fig = plt.figure(figsize=(16,10))
plt.plot(world_confirmed)
plt.legend(world_confirmed.columns)
plt.title('前15个国家累计确诊人数',fontsize=20)
plt.xlabel('日期',fontsize=20)
plt.ylabel('人数/百万',fontsize=20);
```


![在这里插入图片描述](https://img-blog.csdnimg.cn/20200704145629714.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3NwaXJpdExITA==,size_16,color_FFFFFF,t_70#pic_center)


分析：国外数据的补全较为成功，有一定的真实性。   
分析：国外新冠确诊人数自3月末开始激增，排名前四的国家的疫情没有受到控制的趋势，国外疫情的趋势为确诊人数继续激增。

#### （六）结合你的分析结果，对个人和社会在抗击疫情方面有何建议？

 从国内疫情折线图来看，从4月末开始疫情趋于平缓，相反，国外疫情从4月初开始爆发，至今没有看到平缓的趋势。       
 从境外输入案例来看，我们需要谨防境外输入病例，遏制国内新冠再次传播，一切都不能放松警惕。    
 对于个人，我们要避免到人员密集的区域，外出一定要戴好口罩，回家要做全面的消毒。   
 对于社会，在交通发达区域和人员密集区域，需要普及病毒检测和场所消毒措施，切断病毒的传播途径，维护我国疫情防控的成果。   

### 附加分析(选做)

附加分析，所使用的库不限，比如可以使用seaborn、pyecharts等库。

限于个人能力，没有做。

