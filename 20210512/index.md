# 2021新型冠状病毒（COVID-19/2019-nCoV）疫情分析


# 新型冠状病毒（COVID-19/2019-nCoV）疫情分析

> **spiritLHL**

## <font color='red'>重要说明</font>

帮同一个选修课的学妹码的结课作业，这是我个人完善后的版本(她的还有很多错漏)

分析文档：完成度：代码质量 3:5:2

其中分析文档是指你数据分析的过程中，对各问题分析的思路、对结果的解释、说明(要求言简意赅，不要为写而写)

ps:<font color='red'><b>你自己写的代码远胜一切之代笔，无关美丑，只问今日比昨日更长进！加油！</b></font>

## 源文档

**源文档详见：[博客相关资源-新冠疫情数据分析文件](https://www.spiritlhl.top/ziyuan/)**

**温馨提示**：

**疫情尚肆虐，请积极防护，保护自己**

由于数据过多，查看数据尽量使用head()或tail()，以免程序长时间无响应

=======================

本项目数据来源于丁香园。本项目主要目的是**通过对疫情历史数据的分析研究，以更好的了解疫情与疫情的发展态势，为抗击疫情之决策提供数据支持。**

## 一. 提出问题

从全国范围，你所在省市，国外疫情等三个方面主要研究以下几个问题：

（一）全国累计确诊/疑似/治愈/死亡情况随时间变化趋势如何？

（二）你所在的省市情况如何？

（三）全球疫情总体态势如何？

（四）结合你的分析结果，对未来半年的疫情趋势给出你的判断，对个人和社会在抗击疫情方面有何建议？


## 二. 理解数据

原始数据集：AreaInfo.csv，导入相关包及读取数据，并赋值为 **areas**


```python
#导入需要的数据库和文件
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
from matplotlib.pyplot import MultipleLocator
areas=pd.read_csv(r'data/AreaInfo.csv')
plt.rcParams['font.sans-serif'] = ['SimHei'] 
plt.rcParams['axes.unicode_minus'] = False 
```


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

**查看与统计数据，以对数据有一个大致了解**


```python
#数据过多，查看前几行
areas.head()
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
      <td>亚洲</td>
      <td>Asia</td>
      <td>中国</td>
      <td>China</td>
      <td>澳门</td>
      <td>Macau</td>
      <td>820000</td>
      <td>47</td>
      <td>9.0</td>
      <td>46</td>
      <td>0</td>
      <td>2021-01-22 23:40:08</td>
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
      <td>北美洲</td>
      <td>North America</td>
      <td>美国</td>
      <td>United States of America</td>
      <td>美国</td>
      <td>United States of America</td>
      <td>971002</td>
      <td>24632468</td>
      <td>0.0</td>
      <td>10845438</td>
      <td>410378</td>
      <td>2021-01-22 23:40:08</td>
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
      <td>南美洲</td>
      <td>South America</td>
      <td>巴西</td>
      <td>Brazil</td>
      <td>巴西</td>
      <td>Brazil</td>
      <td>973003</td>
      <td>8699814</td>
      <td>0.0</td>
      <td>7580741</td>
      <td>214228</td>
      <td>2021-01-22 23:40:08</td>
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
      <td>比利时</td>
      <td>Belgium</td>
      <td>比利时</td>
      <td>Belgium</td>
      <td>961001</td>
      <td>686827</td>
      <td>0.0</td>
      <td>19239</td>
      <td>20620</td>
      <td>2021-01-22 23:40:08</td>
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
      <td>欧洲</td>
      <td>Europe</td>
      <td>俄罗斯</td>
      <td>Russia</td>
      <td>俄罗斯</td>
      <td>Russia</td>
      <td>964006</td>
      <td>3677352</td>
      <td>0.0</td>
      <td>3081536</td>
      <td>68412</td>
      <td>2021-01-22 23:40:08</td>
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



**相关字段含义介绍：**

**小提示：**

国外数据的provinceName并非是省名，而是用其国家名标注，即数据不再细分到省。

中国数据的provinceName中也有'中国'这样的记录，代表当日全国各省之合计。善用之，对全国情况进行分析时就方便多了。

continentName介绍了哪些大洲参与统计；中国数据中有的给出了省名，有的是用“中国”标注，代表当日全国各省之合计；国外数据的provinceName并非是省名，而是用其国家名标注，即数据不再细分到省
province_suspectedCount一栏中有过多的缺失值，需填补或舍弃；数据中没有详细给到城市数据。

## 三. 数据清洗

### （一）基本数据处理

数据清洗主要包括：**选取子集，缺失数据处理、数据格式转换、异常值数据处理**等。

**提示：因数据皆赖各国上报，情势危杂之际，难免瞒报漏报，故存在较多缺失值，<font color='red'>可以将其补全或舍弃</font>，参见"Pandas之缺失值的处理.ipynb"**

####  <font color='red'><b>国内疫情数据选取（最终选取的数据命名为china）</b></font>

1. 选取国内疫情数据

2. 对于更新时间(updateTime)列，需将其转换为日期类型并提取出年-月-日，并查看处理结果。(提示：dt.date)

3. 因数据每天按小时更新，一天之内有很多重复数据，请去重并只保留一天之内最新的数据。

> 提示：df.drop_duplicates(subset=['provinceName', 'updateTime'], keep='first', inplace=False)

> **其中df是你选择的国内疫情数据的DataFrame**

4. 去除不在此次研究范围内的列,只留下['continentName','countryName','provinceName','province_confirmedCount','province_suspectedCount','province_curedCount','province_deadCount','updateTime']这几列，并以'updateTime'为行索引。

> 提示：两种方法都可以：(1)选取这几列  或  (2)去除其余的列


```python
# 此处给出代码，后面省市数据和全球数据的获取与此大同小异
china = areas.loc[areas.countryName=='中国',:].copy()
china['updateTime'] = pd.to_datetime(china.updateTime,format="%Y-%m-%d",errors='coerce').dt.date
china = china.drop_duplicates(subset=['provinceName', 'updateTime'], keep='first', inplace=False)

# 将"字符类型的日期列(Index)"转为"时间戳索引(DatetimeIndex)"
china['updateTime'] = pd.to_datetime(china['updateTime'])
china.set_index('updateTime',inplace=True)
china = china[['continentName','countryName','provinceName','province_confirmedCount','province_suspectedCount','province_curedCount','province_deadCount']]
china = china[china.provinceName=='中国']
china.head(2)
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
      <th>countryName</th>
      <th>provinceName</th>
      <th>province_confirmedCount</th>
      <th>province_suspectedCount</th>
      <th>province_curedCount</th>
      <th>province_deadCount</th>
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
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2021-01-22</th>
      <td>亚洲</td>
      <td>中国</td>
      <td>中国</td>
      <td>99667</td>
      <td>0.0</td>
      <td>92275</td>
      <td>4810</td>
    </tr>
    <tr>
      <th>2021-01-21</th>
      <td>亚洲</td>
      <td>中国</td>
      <td>中国</td>
      <td>99513</td>
      <td>0.0</td>
      <td>92198</td>
      <td>4809</td>
    </tr>
  </tbody>
</table>
</div>




```python
china.index
```




查看数据信息，是否有缺失数据/数据类型是否正确。若有缺失值，**<font color='red'>可以将其补全或舍弃</font>**，参见**"Pandas之缺失值的处理.ipynb"**


```python
#有些城市不是每天上报，如果只统计那天上报的，那些不上报的就会被忽略，数据就会有错误,查看缺失值
#china.info()
china.province_suspectedCount[china.province_suspectedCount.isnull()] = china.province_suspectedCount.dropna().mode().values
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
      <th>countryName</th>
      <th>provinceName</th>
      <th>province_confirmedCount</th>
      <th>province_suspectedCount</th>
      <th>province_curedCount</th>
      <th>province_deadCount</th>
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
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2021-01-22</th>
      <td>亚洲</td>
      <td>中国</td>
      <td>中国</td>
      <td>99667</td>
      <td>0.0</td>
      <td>92275</td>
      <td>4810</td>
    </tr>
    <tr>
      <th>2021-01-21</th>
      <td>亚洲</td>
      <td>中国</td>
      <td>中国</td>
      <td>99513</td>
      <td>0.0</td>
      <td>92198</td>
      <td>4809</td>
    </tr>
    <tr>
      <th>2021-01-20</th>
      <td>亚洲</td>
      <td>中国</td>
      <td>中国</td>
      <td>99285</td>
      <td>0.0</td>
      <td>92130</td>
      <td>4808</td>
    </tr>
    <tr>
      <th>2021-01-19</th>
      <td>亚洲</td>
      <td>中国</td>
      <td>中国</td>
      <td>99094</td>
      <td>0.0</td>
      <td>92071</td>
      <td>4806</td>
    </tr>
    <tr>
      <th>2021-01-18</th>
      <td>亚洲</td>
      <td>中国</td>
      <td>中国</td>
      <td>98922</td>
      <td>0.0</td>
      <td>91994</td>
      <td>4805</td>
    </tr>
  </tbody>
</table>
</div>



#### <font color='red'><b>你所在省市疫情数据选取（最终选取的数据命名为myhome）</b></font>

此步也可在后面用到的再做

1. 选取所在省市疫情数据(细化到市；若是直辖市，细化到区)

2. 对于更新时间(updateTime)列，需将其转换为日期类型并提取出年-月-日，并查看处理结果。(提示：dt.date)

3. 因数据每天按小时更新，一天之内有很多重复数据，请去重并只保留一天之内最新的数据，并以'updateTime'为行索引。

> 提示：df.drop_duplicates(subset=['cityName', 'updateTime'], keep='first', inplace=False)

4. 去除不在此次研究范围内的列

> 提示：df.drop(['continentName','continentEnglishName','countryName','countryEnglishName','provinceEnglishName',  
           'province_zipCode','cityEnglishName','updateTime','city_zipCode'],axis=1,inplace=True)

**其中df是你选择的省市疫情数据的DataFrame**


```python
#首先选取相应内容，后转换为日期类型并提取出年-月-日，去重
myhome=areas.loc[areas.provinceName=='河北省',:].copy()
myhome['updateTime'] = pd.to_datetime(myhome.updateTime,format="%Y-%m-%d",errors='coerce').dt.date
myhome.drop_duplicates(subset=['provinceName', 'updateTime'], keep='first', inplace=True)
# 将"字符类型的日期列(Index)"转为"时间戳索引(DatetimeIndex)"
myhome['updateTime']=pd.to_datetime(myhome['updateTime'])
myhome.set_index('updateTime',inplace=True)
#去除不在此次研究范围内的列
myhome.drop(['continentName','continentEnglishName','countryName','countryEnglishName','provinceEnglishName','province_zipCode','cityEnglishName','city_zipCode'],axis=1,inplace=True)
myhome.head(2)
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
      <th>provinceName</th>
      <th>province_confirmedCount</th>
      <th>province_suspectedCount</th>
      <th>province_curedCount</th>
      <th>province_deadCount</th>
      <th>cityName</th>
      <th>city_confirmedCount</th>
      <th>city_suspectedCount</th>
      <th>city_curedCount</th>
      <th>city_deadCount</th>
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
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2021-01-22</th>
      <td>河北省</td>
      <td>1252</td>
      <td>0.0</td>
      <td>405</td>
      <td>7</td>
      <td>石家庄</td>
      <td>843.0</td>
      <td>0.0</td>
      <td>54.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>2021-01-21</th>
      <td>河北省</td>
      <td>1245</td>
      <td>0.0</td>
      <td>395</td>
      <td>7</td>
      <td>石家庄</td>
      <td>842.0</td>
      <td>0.0</td>
      <td>54.0</td>
      <td>1.0</td>
    </tr>
  </tbody>
</table>
</div>



查看数据信息，是否有缺失数据/数据类型是否正确。若有缺失值，**<font color='red'>可以将其补全或舍弃</font>**，参见**"Pandas之缺失值的处理.ipynb"**


```python
#查看缺失值
#myhome.info()
myhome.province_suspectedCount[myhome.province_suspectedCount.isnull()] = myhome.province_suspectedCount.dropna().mode().values
myhome.tail()
```

    D:\Anaconda\envs\python32\lib\site-packages\ipykernel_launcher.py:3: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      This is separate from the ipykernel package so we can avoid doing imports until
    




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
      <th>provinceName</th>
      <th>province_confirmedCount</th>
      <th>province_suspectedCount</th>
      <th>province_curedCount</th>
      <th>province_deadCount</th>
      <th>cityName</th>
      <th>city_confirmedCount</th>
      <th>city_suspectedCount</th>
      <th>city_curedCount</th>
      <th>city_deadCount</th>
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
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2020-01-27</th>
      <td>河北省</td>
      <td>18</td>
      <td>0.0</td>
      <td>0</td>
      <td>1</td>
      <td>石家庄</td>
      <td>7.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2020-01-26</th>
      <td>河北省</td>
      <td>13</td>
      <td>0.0</td>
      <td>0</td>
      <td>1</td>
      <td>石家庄</td>
      <td>5.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2020-01-25</th>
      <td>河北省</td>
      <td>8</td>
      <td>0.0</td>
      <td>0</td>
      <td>1</td>
      <td>石家庄</td>
      <td>4.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2020-01-24</th>
      <td>河北省</td>
      <td>2</td>
      <td>0.0</td>
      <td>0</td>
      <td>1</td>
      <td>石家庄</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2020-01-22</th>
      <td>河北省</td>
      <td>1</td>
      <td>0.0</td>
      <td>0</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



#### <font color='red'><b>全球疫情数据选取（最终选取的数据命名为world）</b></font>

此步也可在后面用到的再做

1. 选取国外疫情数据

> 提示：选取国外疫情数据（countryName!='中国'）

2. 对于更新时间(updateTime)列，需将其转换为日期类型并提取出年-月-日，并查看处理结果。(提示：dt.date)

3. 因数据每天按小时更新，一天之内有很多重复数据，请去重并只保留一天之内最新的数据。

> 提示：df.drop_duplicates(subset=['provinceName', 'updateTime'], keep='first', inplace=False)

> **其中df是你选择的国内疫情数据的DataFrame**

4. 去除不在此次研究范围内的列,只留下['continentName','countryName','provinceName','province_confirmedCount','province_suspectedCount','province_curedCount','province_deadCount','updateTime']这几列，并以'updateTime'为行索引。

> 提示：两种方法都可以：(1)选取这几列  或  (2)去除其余的列

5. 得到全球数据

> 提示：用 concat 函数将前面的china与国外数据按「轴」连接得到全球数据。


```python
world=areas.loc[areas.countryName!='中国',:].copy()
world['updateTime'] = pd.to_datetime(world.updateTime,format="%Y-%m-%d",errors='coerce').dt.date
world.drop_duplicates(subset=['provinceName', 'updateTime'], keep='first', inplace=False)
# 将"字符类型的日期列(Index)"转为"时间戳索引(DatetimeIndex)"
world['updateTime']=pd.to_datetime(world['updateTime'])
world.set_index('updateTime',inplace=True)
#去除不在此次研究范围内的列
#world.drop(['continentName','countryName','provinceName','province_confirmedCount','province_suspectedCount','province_curedCount','province_deadCount'],axis=1,inplace=True)
#更改索引，按轴1链接

world = world.reset_index()
temp = china.copy()
temp = temp.reset_index()
pd.concat([world,temp], axis=1)
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
      <th>updateTime</th>
      <th>continentName</th>
      <th>continentEnglishName</th>
      <th>countryName</th>
      <th>countryEnglishName</th>
      <th>provinceName</th>
      <th>provinceEnglishName</th>
      <th>province_zipCode</th>
      <th>province_confirmedCount</th>
      <th>province_suspectedCount</th>
      <th>...</th>
      <th>city_curedCount</th>
      <th>city_deadCount</th>
      <th>updateTime</th>
      <th>continentName</th>
      <th>countryName</th>
      <th>provinceName</th>
      <th>province_confirmedCount</th>
      <th>province_suspectedCount</th>
      <th>province_curedCount</th>
      <th>province_deadCount</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2021-01-22</td>
      <td>北美洲</td>
      <td>North America</td>
      <td>美国</td>
      <td>United States of America</td>
      <td>美国</td>
      <td>United States of America</td>
      <td>971002</td>
      <td>24632468</td>
      <td>0.0</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2021-01-22</td>
      <td>亚洲</td>
      <td>中国</td>
      <td>中国</td>
      <td>99667.0</td>
      <td>0.0</td>
      <td>92275.0</td>
      <td>4810.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2021-01-22</td>
      <td>南美洲</td>
      <td>South America</td>
      <td>巴西</td>
      <td>Brazil</td>
      <td>巴西</td>
      <td>Brazil</td>
      <td>973003</td>
      <td>8699814</td>
      <td>0.0</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2021-01-21</td>
      <td>亚洲</td>
      <td>中国</td>
      <td>中国</td>
      <td>99513.0</td>
      <td>0.0</td>
      <td>92198.0</td>
      <td>4809.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2021-01-22</td>
      <td>欧洲</td>
      <td>Europe</td>
      <td>比利时</td>
      <td>Belgium</td>
      <td>比利时</td>
      <td>Belgium</td>
      <td>961001</td>
      <td>686827</td>
      <td>0.0</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2021-01-20</td>
      <td>亚洲</td>
      <td>中国</td>
      <td>中国</td>
      <td>99285.0</td>
      <td>0.0</td>
      <td>92130.0</td>
      <td>4808.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2021-01-22</td>
      <td>欧洲</td>
      <td>Europe</td>
      <td>俄罗斯</td>
      <td>Russia</td>
      <td>俄罗斯</td>
      <td>Russia</td>
      <td>964006</td>
      <td>3677352</td>
      <td>0.0</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2021-01-19</td>
      <td>亚洲</td>
      <td>中国</td>
      <td>中国</td>
      <td>99094.0</td>
      <td>0.0</td>
      <td>92071.0</td>
      <td>4806.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2021-01-22</td>
      <td>欧洲</td>
      <td>Europe</td>
      <td>塞尔维亚</td>
      <td>Republic of Serbia</td>
      <td>塞尔维亚</td>
      <td>Republic of Serbia</td>
      <td>965013</td>
      <td>436121</td>
      <td>0.0</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2021-01-18</td>
      <td>亚洲</td>
      <td>中国</td>
      <td>中国</td>
      <td>98922.0</td>
      <td>0.0</td>
      <td>91994.0</td>
      <td>4805.0</td>
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
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>301542</th>
      <td>2020-01-27</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>马来西亚</td>
      <td>Malaysia</td>
      <td>马来西亚</td>
      <td>Malaysia</td>
      <td>952007</td>
      <td>3</td>
      <td>0.0</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaT</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>301543</th>
      <td>2020-01-27</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>法国</td>
      <td>France</td>
      <td>法国</td>
      <td>France</td>
      <td>961002</td>
      <td>3</td>
      <td>0.0</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaT</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>301544</th>
      <td>2020-01-27</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>越南</td>
      <td>Vietnam</td>
      <td>越南</td>
      <td>Vietnam</td>
      <td>952011</td>
      <td>2</td>
      <td>0.0</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaT</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>301545</th>
      <td>2020-01-27</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>尼泊尔</td>
      <td>Nepal</td>
      <td>尼泊尔</td>
      <td>Nepal</td>
      <td>953005</td>
      <td>1</td>
      <td>0.0</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaT</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>301546</th>
      <td>2020-01-27</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>加拿大</td>
      <td>Canada</td>
      <td>加拿大</td>
      <td>Canada</td>
      <td>971001</td>
      <td>1</td>
      <td>0.0</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaT</td>
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
<p>301547 rows × 27 columns</p>
</div>



查看数据信息，是否有缺失数据/数据类型是否正确。

提示：因数据皆赖各国上报，情势危杂之际，难免瞒报漏报，故存在较多缺失值，**<font color='red'>可以将其补全或舍弃</font>**，参见**"Pandas之缺失值的处理.ipynb"**


```python
#因数据皆赖各国上报，情势危杂之际，难免瞒报漏报，故存在较多缺失值
#world.info()
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
      <th>updateTime</th>
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
      <td>2021-01-22</td>
      <td>北美洲</td>
      <td>North America</td>
      <td>美国</td>
      <td>United States of America</td>
      <td>美国</td>
      <td>United States of America</td>
      <td>971002</td>
      <td>24632468</td>
      <td>0.0</td>
      <td>10845438</td>
      <td>410378</td>
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
      <td>2021-01-22</td>
      <td>南美洲</td>
      <td>South America</td>
      <td>巴西</td>
      <td>Brazil</td>
      <td>巴西</td>
      <td>Brazil</td>
      <td>973003</td>
      <td>8699814</td>
      <td>0.0</td>
      <td>7580741</td>
      <td>214228</td>
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
      <td>2021-01-22</td>
      <td>欧洲</td>
      <td>Europe</td>
      <td>比利时</td>
      <td>Belgium</td>
      <td>比利时</td>
      <td>Belgium</td>
      <td>961001</td>
      <td>686827</td>
      <td>0.0</td>
      <td>19239</td>
      <td>20620</td>
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
      <td>2021-01-22</td>
      <td>欧洲</td>
      <td>Europe</td>
      <td>俄罗斯</td>
      <td>Russia</td>
      <td>俄罗斯</td>
      <td>Russia</td>
      <td>964006</td>
      <td>3677352</td>
      <td>0.0</td>
      <td>3081536</td>
      <td>68412</td>
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
      <td>2021-01-22</td>
      <td>欧洲</td>
      <td>Europe</td>
      <td>塞尔维亚</td>
      <td>Republic of Serbia</td>
      <td>塞尔维亚</td>
      <td>Republic of Serbia</td>
      <td>965013</td>
      <td>436121</td>
      <td>0.0</td>
      <td>50185</td>
      <td>5263</td>
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



## 四. 数据分析及可视化

在进行数据分析及可视化时，依据每个问题选取所需变量并新建DataFrame再进行分析和可视化展示，这样数据不易乱且条理更清晰。

### 基础分析

基础分析，**只允许使用numpy、pandas和matplotlib库**。

可以在一张图上多个坐标系展示也可以在多张图上展示

请根据分析目的选择图形的类型(折线图、饼图、直方图和散点图等等)，实在没有主意可以到百度疫情地图或其他疫情分析的站点激发激发灵感。

#### （一）全国累计确诊/治愈/死亡情况随时间变化趋势如何？


```python
china.index
```



```python
#首先整合每天全国累计确诊/治愈/死亡情况做成新的列表
list_a=china['province_confirmedCount']
#遍历时间将其改为字符串形式
list_updatetime=[]
for i in china.index:
    list_updatetime.append(str(i)[0:11])
updatetime_a=pd.DataFrame(list_a,index=list_updatetime)
updatetime_a.index.name='updatetime'
updatetime_a.columns=['province_confirmedCount']
updatetime_a.head()
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
      <th>province_confirmedCount</th>
    </tr>
    <tr>
      <th>updatetime</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2021-01-22</th>
      <td>99667</td>
    </tr>
    <tr>
      <th>2021-01-21</th>
      <td>99513</td>
    </tr>
    <tr>
      <th>2021-01-20</th>
      <td>99285</td>
    </tr>
    <tr>
      <th>2021-01-19</th>
      <td>99094</td>
    </tr>
    <tr>
      <th>2021-01-18</th>
      <td>98922</td>
    </tr>
  </tbody>
</table>
</div>




```python

#画折线图表示
fig, axes = plt.subplots(1,1,figsize=(16, 4))
x=updatetime_a.index
y=updatetime_a.values
plot=axes.plot(x,y,color=dt_hex,linewidth=2,linestyle='-',label='province_confirmedCount')
axes.set_xticks(range(0,len(x),25))
plt.xlabel('日期',fontsize=10)
plt.ylabel('人数',fontsize=10)
axes.legend(loc=0,frameon=True)
plt.show()
#重复以上步骤，绘制治愈/死亡的折线图
list_b=china['province_curedCount']
#遍历时间将其改为字符串形式
list_updatetime=[]
for i in china.index:
    list_updatetime.append(str(i)[0:11])
updatetime_b=pd.DataFrame(list_b,index=list_updatetime)
updatetime_b.index.name='updatetime'
updatetime_b.columns=['province_curedCount']
updatetime_b.head()
#画折线图表示
fig, axes = plt.subplots(1,1,figsize=(16, 4))
x=updatetime_b.index
y=updatetime_b.values
plot=axes.plot(x,y,color=dt_hex,linewidth=2,linestyle='-',label='province_curedCount')
axes.set_xticks(range(0,len(x),25))
plt.xlabel('日期',fontsize=10)
plt.ylabel('人数',fontsize=10)
axes.legend(loc=0,frameon=True)
plt.show()
#绘制死亡人数图
list_c=china['province_deadCount']
#遍历时间将其改为字符串形式
list_updatetime=[]
for i in china.index:
    list_updatetime.append(str(i)[0:11])
updatetime_c=pd.DataFrame(list_a,index=list_updatetime)
updatetime_c.index.name='updatetime'
updatetime_c.columns=['province_deadCount']
updatetime_c.head()
#画折线图表示
fig, axes = plt.subplots(1,1,figsize=(16, 4))
x=updatetime_c.index
y=updatetime_c.values
plot=axes.plot(x,y,color=dt_hex,linewidth=2,linestyle='-',label='province_deadCount')
axes.set_xticks(range(0,len(x),25))
plt.xlabel('日期',fontsize=10)
plt.ylabel('人数',fontsize=10)
axes.legend(loc=0,frameon=True)
plt.show()
#分析：确诊与死亡人数的折线图显示出人数在逐渐增加，增速有加快的趋势；治愈人数虽然也是再增加，但是增速逐渐变慢
```


![png](https://ftp.bmp.ovh/imgs/2021/07/1b67cdb784fde315.png)

第二幅图为

![png](https://ftp.bmp.ovh/imgs/2021/07/c69aaf11c7d1f9d9.png)

第三幅图为

![png](https://ftp.bmp.ovh/imgs/2021/07/38935c779266c522.png)


#### （二）你所在的省市情况如何？


```python
myhome
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
      <th>provinceName</th>
      <th>province_confirmedCount</th>
      <th>province_suspectedCount</th>
      <th>province_curedCount</th>
      <th>province_deadCount</th>
      <th>cityName</th>
      <th>city_confirmedCount</th>
      <th>city_suspectedCount</th>
      <th>city_curedCount</th>
      <th>city_deadCount</th>
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
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2021-01-22</th>
      <td>河北省</td>
      <td>1252</td>
      <td>0.0</td>
      <td>405</td>
      <td>7</td>
      <td>石家庄</td>
      <td>843.0</td>
      <td>0.0</td>
      <td>54.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>2021-01-21</th>
      <td>河北省</td>
      <td>1245</td>
      <td>0.0</td>
      <td>395</td>
      <td>7</td>
      <td>石家庄</td>
      <td>842.0</td>
      <td>0.0</td>
      <td>54.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>2021-01-20</th>
      <td>河北省</td>
      <td>1222</td>
      <td>0.0</td>
      <td>384</td>
      <td>7</td>
      <td>石家庄</td>
      <td>813.0</td>
      <td>0.0</td>
      <td>45.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>2021-01-19</th>
      <td>河北省</td>
      <td>1198</td>
      <td>0.0</td>
      <td>383</td>
      <td>7</td>
      <td>石家庄</td>
      <td>801.0</td>
      <td>0.0</td>
      <td>45.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>2021-01-18</th>
      <td>河北省</td>
      <td>1171</td>
      <td>0.0</td>
      <td>379</td>
      <td>7</td>
      <td>石家庄</td>
      <td>774.0</td>
      <td>0.0</td>
      <td>41.0</td>
      <td>1.0</td>
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
    </tr>
    <tr>
      <th>2020-01-27</th>
      <td>河北省</td>
      <td>18</td>
      <td>0.0</td>
      <td>0</td>
      <td>1</td>
      <td>石家庄</td>
      <td>7.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2020-01-26</th>
      <td>河北省</td>
      <td>13</td>
      <td>0.0</td>
      <td>0</td>
      <td>1</td>
      <td>石家庄</td>
      <td>5.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2020-01-25</th>
      <td>河北省</td>
      <td>8</td>
      <td>0.0</td>
      <td>0</td>
      <td>1</td>
      <td>石家庄</td>
      <td>4.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2020-01-24</th>
      <td>河北省</td>
      <td>2</td>
      <td>0.0</td>
      <td>0</td>
      <td>1</td>
      <td>石家庄</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2020-01-22</th>
      <td>河北省</td>
      <td>1</td>
      <td>0.0</td>
      <td>0</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>134 rows × 10 columns</p>
</div>




```python
plt.figure( figsize=(20,10), dpi=80)
list_updatetime=[]
for i in myhome.index:
    list_updatetime.append(str(i)[0:11])
x = list_updatetime
y1=myhome['province_confirmedCount']
y2=myhome['province_curedCount']
y3=myhome['province_deadCount']
plt.plot(x,y1,color=r_hex,label='province_confirmedCount')
plt.plot(x,y2,color=tl_hex,label='province_curedCount')
plt.plot(x,y3,color=g_hex,label='province_deadCount')
x_major_locator = MultipleLocator(12)
ax = plt.gca()
ax.xaxis.set_major_locator(x_major_locator)
plt.legend(loc=0,frameon=True)
plt.show()
```


![png](https://ftp.bmp.ovh/imgs/2021/07/9aa6f38f37bd3bfa.png)


#### （三）全球疫情态势如何？

1. 全球 TOP10 国家的疫情情况如何？

2. 各大洲情况对比？

3. 选一个你感兴趣的大洲，分析各国疫情之间的联系、分布、对比和构成情况。

> 提示：注意数据透视、分组和整合知识的运用


```python
world
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
      <th>updateTime</th>
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
      <td>2021-01-22</td>
      <td>北美洲</td>
      <td>North America</td>
      <td>美国</td>
      <td>United States of America</td>
      <td>美国</td>
      <td>United States of America</td>
      <td>971002</td>
      <td>24632468</td>
      <td>0.0</td>
      <td>10845438</td>
      <td>410378</td>
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
      <td>2021-01-22</td>
      <td>南美洲</td>
      <td>South America</td>
      <td>巴西</td>
      <td>Brazil</td>
      <td>巴西</td>
      <td>Brazil</td>
      <td>973003</td>
      <td>8699814</td>
      <td>0.0</td>
      <td>7580741</td>
      <td>214228</td>
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
      <td>2021-01-22</td>
      <td>欧洲</td>
      <td>Europe</td>
      <td>比利时</td>
      <td>Belgium</td>
      <td>比利时</td>
      <td>Belgium</td>
      <td>961001</td>
      <td>686827</td>
      <td>0.0</td>
      <td>19239</td>
      <td>20620</td>
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
      <td>2021-01-22</td>
      <td>欧洲</td>
      <td>Europe</td>
      <td>俄罗斯</td>
      <td>Russia</td>
      <td>俄罗斯</td>
      <td>Russia</td>
      <td>964006</td>
      <td>3677352</td>
      <td>0.0</td>
      <td>3081536</td>
      <td>68412</td>
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
      <td>2021-01-22</td>
      <td>欧洲</td>
      <td>Europe</td>
      <td>塞尔维亚</td>
      <td>Republic of Serbia</td>
      <td>塞尔维亚</td>
      <td>Republic of Serbia</td>
      <td>965013</td>
      <td>436121</td>
      <td>0.0</td>
      <td>50185</td>
      <td>5263</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
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
      <th>301542</th>
      <td>2020-01-27</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>马来西亚</td>
      <td>Malaysia</td>
      <td>马来西亚</td>
      <td>Malaysia</td>
      <td>952007</td>
      <td>3</td>
      <td>0.0</td>
      <td>0</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>301543</th>
      <td>2020-01-27</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>法国</td>
      <td>France</td>
      <td>法国</td>
      <td>France</td>
      <td>961002</td>
      <td>3</td>
      <td>0.0</td>
      <td>0</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>301544</th>
      <td>2020-01-27</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>越南</td>
      <td>Vietnam</td>
      <td>越南</td>
      <td>Vietnam</td>
      <td>952011</td>
      <td>2</td>
      <td>0.0</td>
      <td>0</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>301545</th>
      <td>2020-01-27</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>尼泊尔</td>
      <td>Nepal</td>
      <td>尼泊尔</td>
      <td>Nepal</td>
      <td>953005</td>
      <td>1</td>
      <td>0.0</td>
      <td>0</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>301546</th>
      <td>2020-01-27</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>加拿大</td>
      <td>Canada</td>
      <td>加拿大</td>
      <td>Canada</td>
      <td>971001</td>
      <td>1</td>
      <td>0.0</td>
      <td>0</td>
      <td>0</td>
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
<p>301547 rows × 19 columns</p>
</div>




```python
country = list(set(world['provinceEnglishName']))
date_y = []
for dt in world.loc[world['provinceEnglishName'] ==  country[0]]['updateTime']:
    date_y.append(str(dt))
date_y.sort()
for c in country:
    world.loc[world['provinceEnglishName'] == c].sort_values(by = 'updateTime')
world.dropna(subset=['provinceEnglishName'],inplace=True)
world.updateTime = pd.to_datetime(world.updateTime,format="%Y-%m-%d",errors='coerce').dt.date
world_confirmed = world.loc[world['provinceEnglishName'] == world.head(15)['provinceEnglishName'][0]].pivot_table(index='updateTime', columns='provinceEnglishName', values='province_confirmedCount',aggfunc=np.mean)
for i in world.head(15)['provinceEnglishName'][1:]:
    draft_c = world.loc[world['provinceEnglishName'] == i].pivot_table(index='updateTime', columns='provinceEnglishName', values='province_confirmedCount',aggfunc=np.mean)
    world_confirmed = pd.merge(world_confirmed,draft_c,on='updateTime', how='outer',sort=True)
world_confirmed.fillna(0,inplace=True,limit = 1)
world_confirmed.fillna(method="ffill",inplace=True)
world_confirmed
fig = plt.figure(figsize=(16,10))
plt.plot(world_confirmed)
plt.legend(world_confirmed.columns)
plt.title('前15个国家累计确诊人数',fontsize=20)
plt.xlabel('日期',fontsize=20)
plt.ylabel('人数/百万',fontsize=20);
plt.show()
```


![png](https://ftp.bmp.ovh/imgs/2021/07/7d534b3e928b244d.png)


