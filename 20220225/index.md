# (MCM/ICM)比特币和黄金组合投资策略的策略代码部分


# 前言

美赛论文对应策略代码部分，吐槽一句美赛居然不收代码也是离谱。

论文配套的代码均为本人编写，运行无碍。

感谢组员组长的配合论文撰写。

# 正文


```python
import numpy as np
import pandas as pd
import re,math
import matplotlib.pyplot as plt


np.set_printoptions(suppress=True)
B = pd.read_csv(r'B.csv') # B
H = pd.read_csv(r'H.csv') # H
Times = pd.read_csv(r'022.csv') # Time
B = B.set_index("Unnamed: 0")
H = H.set_index("Unnamed: 0")
Times = Times.set_index("Unnamed: 0")
# 先向前取值填充，再先后取值填充
BH = pd.merge(H.iloc[:,0:2], B.iloc[:,0:2], how='outer',on='0').sort_values('0',ascending=True)
BH = pd.merge(BH, H.iloc[:,0:3:2], how="left", on=["0"])
BH = pd.merge(BH, B.iloc[:,0:3:2], how="left", on=["0"]).fillna(0)

```


```python
H
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
      <th>0</th>
      <th>1</th>
      <th>2</th>
    </tr>
    <tr>
      <th>Unnamed: 0</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>76</td>
      <td>-1</td>
      <td>1145.90</td>
    </tr>
    <tr>
      <th>1</th>
      <td>154</td>
      <td>1</td>
      <td>1281.85</td>
    </tr>
    <tr>
      <th>2</th>
      <td>171</td>
      <td>-1</td>
      <td>1257.40</td>
    </tr>
    <tr>
      <th>3</th>
      <td>233</td>
      <td>1</td>
      <td>1282.30</td>
    </tr>
    <tr>
      <th>4</th>
      <td>247</td>
      <td>-1</td>
      <td>1333.10</td>
    </tr>
    <tr>
      <th>5</th>
      <td>308</td>
      <td>1</td>
      <td>1291.85</td>
    </tr>
    <tr>
      <th>6</th>
      <td>324</td>
      <td>-1</td>
      <td>1264.55</td>
    </tr>
    <tr>
      <th>7</th>
      <td>532</td>
      <td>1</td>
      <td>1223.00</td>
    </tr>
    <tr>
      <th>8</th>
      <td>551</td>
      <td>-1</td>
      <td>1203.25</td>
    </tr>
    <tr>
      <th>9</th>
      <td>608</td>
      <td>1</td>
      <td>1312.40</td>
    </tr>
    <tr>
      <th>10</th>
      <td>628</td>
      <td>-1</td>
      <td>1285.85</td>
    </tr>
    <tr>
      <th>11</th>
      <td>685</td>
      <td>1</td>
      <td>1280.95</td>
    </tr>
    <tr>
      <th>12</th>
      <td>703</td>
      <td>-1</td>
      <td>1431.40</td>
    </tr>
    <tr>
      <th>13</th>
      <td>760</td>
      <td>1</td>
      <td>1503.10</td>
    </tr>
    <tr>
      <th>14</th>
      <td>781</td>
      <td>-1</td>
      <td>1490.60</td>
    </tr>
    <tr>
      <th>15</th>
      <td>839</td>
      <td>1</td>
      <td>1567.85</td>
    </tr>
    <tr>
      <th>16</th>
      <td>856</td>
      <td>-1</td>
      <td>1578.25</td>
    </tr>
    <tr>
      <th>17</th>
      <td>912</td>
      <td>1</td>
      <td>1682.05</td>
    </tr>
    <tr>
      <th>18</th>
      <td>931</td>
      <td>-1</td>
      <td>1737.95</td>
    </tr>
    <tr>
      <th>19</th>
      <td>988</td>
      <td>1</td>
      <td>2031.15</td>
    </tr>
    <tr>
      <th>20</th>
      <td>1008</td>
      <td>-1</td>
      <td>1928.45</td>
    </tr>
  </tbody>
</table>
</div>




```python
m = 10000
h = 0
b = 0
p = 0
q = 0
control_list = []
for i in range(len(Times)):
    x1 = 0
    x2 = 0
    j = 0
    t = 0
    if i <= 9:
        control_list.append([i,0,m,h,j,b,t,x1,x2])
        continue
    else:
        if i not in list(np.array(BH['0'])):
            control_list.append([i,0,m,h,j,b,t,x1,x2])
            continue
    if np.array(BH[BH['0'].isin([str(i)])]['1_x'])[0] != 0: # H
#         if np.array(BH[BH['0'].isin([str(i)])]['2_y'])[0] == 0: # B
        if np.array(BH[BH['0'].isin([str(i)])]['1_x'])[0] < 0: # 买
            j = np.array(H[H['0'].isin([i])]['2'])[0]
            print(j)
            x1 = np.array(Times.iloc[[i]]['0'])[0]
            x2 = np.array(Times.iloc[[i]]['1'])[0]
            p = m*x1#(x1/(x1+x2))
            q = m*x2#(x2/(x1+x2))
            h = (p-0.01*p)/j
            p = 0
            m = p + q
            control_list.append([i,11,m,h,j,b,0,x1,x2])
#             print(m)
        if np.array(BH[BH['0'].isin([str(i)])]['1_x'])[0] > 0: # 卖
            j = np.array(H[H['0'].isin([i])]['2'])[0]
            x1 = np.array(Times.iloc[[i]]['0'])[0]
            x2 = np.array(Times.iloc[[i]]['1'])[0]
            p = m*x1
            q = m*x2
            m = h*j-h*j*0.01+p+q
            h = 0
            control_list.append([i,-11,m,h,j,b,0,x1,x2])
#             print(m)
            
#             if np.array(BH[BH['0'].isin([str(i)])]['2_x'])[0] == 0: # H
    if np.array(BH[BH['0'].isin([str(i)])]['1_y'])[0] != 0: # B
        if np.array(BH[BH['0'].isin([str(i)])]['1_y'])[0] < 0: # 买
            t = np.array(B[B['0'].isin([i])]['2'])[0]
            x1 = np.array(Times.iloc[[i]]['0'])[0]
            x2 = np.array(Times.iloc[[i]]['1'])[0]
            p = m*x1#(x1/(x1+x2))
            q = m*x2#(x2/(x1+x2))
            b = (q-0.02*q)/t
            q = 0
            m = p + q
            control_list.append([i,22,m,h,0,b,t,x1,x2])
#             print(m)
        elif np.array(BH[BH['0'].isin([str(i)])]['1_y'])[0] > 0: # 卖
            t = np.array(B[B['0'].isin([i])]['2'])[0]
            x1 = np.array(Times.iloc[[i]]['0'])[0]
            x2 = np.array(Times.iloc[[i]]['1'])[0]
            p = m*x1
            q = m*x2
            m = b*t-b*t*0.02+p+q
            b = 0
            control_list.append([i,-22,m,h,0,b,t,x1,x2])
#             print(m)

m
```

    1145.9
    1257.4
    1333.1
    1264.55
    1203.25
    1285.85
    1431.4
    1490.6
    1578.25
    1737.95
    1928.45
    




    2.935705996261112e-06




```python

```


```python
m = 10000
h = 0
b = 0
p = 0
q = 0
control_list = []
for i in range(len(Times)):
    x1 = 0
    x2 = 0
    j = 0
    t = 0
    if i <= 9:
        control_list.append([i,0,m,h,j,b,t,x1,x2])
        continue
    else:
        if i not in list(np.array(BH['0'])):
            control_list.append([i,0,m,h,j,b,t,x1,x2])
            continue
    if np.array(BH[BH['0'].isin([str(i)])]['1_x'])[0] != 0: # H
#         if np.array(BH[BH['0'].isin([str(i)])]['2_y'])[0] == 0: # B
        if np.array(BH[BH['0'].isin([str(i)])]['1_x'])[0] < 0: # 买
            j = np.array(H[H['0'].isin([i])]['2'])[0]
            x1 = np.array(Times.iloc[[i]]['0'])[0]
            x2 = np.array(Times.iloc[[i]]['1'])[0]
            p = m*x1#(x1/(x1+x2))
            q = m*x2#(x2/(x1+x2))
            h = (p-0.01*p)/j
            p = 0
            m = p + q
            control_list.append([i,11,m,h,j,b,t,x1,x2])
#             print(m)
        if np.array(BH[BH['0'].isin([str(i)])]['1_x'])[0] > 0: # 卖
            j = np.array(H[H['0'].isin([i])]['2'])[0]
            x1 = np.array(Times.iloc[[i]]['0'])[0]
            x2 = np.array(Times.iloc[[i]]['1'])[0]
            p = m*x1
            q = m*x2
            m = h*j-h*j*0.01+p+q
            h = 0
            control_list.append([i,-11,m,h,j,b,t,x1,x2])
#             print(m)
            
#             if np.array(BH[BH['0'].isin([str(i)])]['2_x'])[0] == 0: # H
    if np.array(BH[BH['0'].isin([str(i)])]['1_y'])[0] != 0: # B
        if np.array(BH[BH['0'].isin([str(i)])]['1_y'])[0] < 0: # 买
            t = np.array(B[B['0'].isin([i])]['2'])[0]
            x1 = np.array(Times.iloc[[i]]['0'])[0]
            x2 = np.array(Times.iloc[[i]]['1'])[0]
            p = m*x1#(x1/(x1+x2))
            q = m*x2#(x2/(x1+x2))
            b = (q-0.02*q)/t
            q = 0
            m = p + q
            control_list.append([i,22,m,h,j,b,t,x1,x2])
#             print(m)
        elif np.array(BH[BH['0'].isin([str(i)])]['1_y'])[0] > 0: # 卖
            t = np.array(B[B['0'].isin([i])]['2'])[0]
            x1 = np.array(Times.iloc[[i]]['0'])[0]
            x2 = np.array(Times.iloc[[i]]['1'])[0]
            p = m*x1
            q = m*x2
            m = b*t-b*t*0.02+p+q
            b = 0
            control_list.append([i,-22,m,h,j,b,t,x1,x2])
#             print(m)

m
```


```python
data_data = pd.DataFrame(control_list,columns=["day","process","USD","H","Hprice","B","Bprice","Hx1","Bx2"])#.to_csv("Process.csv")
data_data
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
      <th>day</th>
      <th>process</th>
      <th>USD</th>
      <th>H</th>
      <th>Hprice</th>
      <th>B</th>
      <th>Bprice</th>
      <th>Hx1</th>
      <th>Bx2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>0</td>
      <td>10000.000000</td>
      <td>0.000000</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>0</td>
      <td>10000.000000</td>
      <td>0.000000</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>0</td>
      <td>10000.000000</td>
      <td>0.000000</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>0</td>
      <td>10000.000000</td>
      <td>0.000000</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>0</td>
      <td>10000.000000</td>
      <td>0.000000</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
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
    </tr>
    <tr>
      <th>1820</th>
      <td>1819</td>
      <td>0</td>
      <td>0.000003</td>
      <td>5.729175</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>1821</th>
      <td>1820</td>
      <td>0</td>
      <td>0.000003</td>
      <td>5.729175</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>1822</th>
      <td>1821</td>
      <td>0</td>
      <td>0.000003</td>
      <td>5.729175</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>1823</th>
      <td>1822</td>
      <td>0</td>
      <td>0.000003</td>
      <td>5.729175</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>1824</th>
      <td>1823</td>
      <td>0</td>
      <td>0.000003</td>
      <td>5.729175</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
  </tbody>
</table>
<p>1825 rows × 9 columns</p>
</div>




```python
ttp = []
price = pd.read_csv(r'price.csv', index_col=["Unnamed: 0"])
ttp.append(np.array(data_data["USD"])+np.array(data_data["H"])*np.array(price["USD (PM)"])[1:]+np.array(data_data["H"])*np.array(price["USD (PM)"])[1:])
ttp.append(np.array(data_data["H"])*np.array(price["USD (PM)"])[1:])
ttp.append(np.array(data_data["B"])*np.array(price["Value"])[1:])
# ttp.append(price["Value"])
# ttp.append(price["USD (PM)"])
```


```python
pd.DataFrame(ttp).T
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
      <th>0</th>
      <th>1</th>
      <th>2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>10000.000000</td>
      <td>0.000000</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>10000.000000</td>
      <td>0.000000</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>10000.000000</td>
      <td>0.000000</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>10000.000000</td>
      <td>0.000000</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>10000.000000</td>
      <td>0.000000</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>1820</th>
      <td>20872.528860</td>
      <td>10436.264429</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>1821</th>
      <td>20649.663969</td>
      <td>10324.831983</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>1822</th>
      <td>20464.611630</td>
      <td>10232.305813</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>1823</th>
      <td>20490.392915</td>
      <td>10245.196456</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>1824</th>
      <td>20563.153432</td>
      <td>10281.576715</td>
      <td>0.0</td>
    </tr>
  </tbody>
</table>
<p>1825 rows × 3 columns</p>
</div>


