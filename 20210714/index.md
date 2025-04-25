# 某区域流体数据处理(只会写操作，不知道具体意义)

## 前言

帮舍友整的，不知道具体实际意义。

## 代码

```
import numpy as np
import pandas as pd


data = pd.read_excel(r'C:\Users\祈LHL\Desktop\data.xlsx')
```

```
data.head()
```


 <div>
    <style scoped>
        .dataframe tbody tr th:only-of-type {   ,
                vertical-align: middle;   ,
            }   ,
           ,
            .dataframe tbody tr th {
                vertical-align: top;
            }
           ,
            .dataframe thead th {
                text-align: right;
            }   ,
        </style>
        <table border=\ 1\  class=\ dataframe\ >
          <thead>
            <tr style=\ text-align: right;\ >
              <th></th>
              <th>cellnumber</th>
              <th>x-coordinate</th> 
              <th>y-coordinate</th>
              <th>z-coordinate</th>   
              <th>density</th>   
              <th>z-velocity</th>   
              <th>relative-z-velocity</th>   
              <th>x-coordinate.1</th>   
              <th>y-coordinate</th>   
              <th>z-face-area</th>   
              <th>boundary-cell-dist</th>   
              <th>boundary-normal-dist</th>   
            </tr>   
          </thead>   
          <tbody>  
            <tr>   
              <th>0</th>   
              <td>1</td>   
              <td>-12.597898</td>   
              <td>-2.404495</td>   
              <td>-6.320497</td>   
              <td>1.226</td>   
              <td>-22.205814</td>   
              <td>-22.205814</td>   
              <td>-12.597899</td>   
              <td>-2.404528</td>   
              <td>-0.006824</td>   
              <td>1</td>   
              <td>0.010276</td>   
            </tr>   
            <tr>   
              <th>1</th>   
              <td>2</td>   
              <td>-12.597898</td>   
              <td>-2.321485</td>   
              <td>-6.320487</td>   
              <td>1.226</td>   
              <td>-23.532957</td>   
              <td>-23.532957</td>   
              <td>-12.597899</td>   
              <td>-2.321584</td>   
              <td>-0.006824</td>   
              <td>1</td>   
              <td>0.020553</td>   
            </tr>   
            <tr>   
              <th>2</th>   
              <td>3</td>   
              <td>-12.515688</td>   
              <td>-2.404495</td>   
              <td>-6.320500</td>  
              <td>1.226</td>   
              <td>-23.167622</td>   
              <td>-23.167622</td>   
              <td>-12.515688</td>   
              <td>-2.404528</td>   
              <td>-0.006824</td>   
              <td>1</td>   
              <td>0.020636</td>   
            </tr>   
            <tr>   
              <th>3</th>   
              <td>4</td>   
              <td>-12.515688</td>   
              <td>-2.321485</td>   
              <td>-6.320500</td>   
              <td>1.226</td>   
              <td>-24.882029</td>   
              <td>-24.882029</td>   
              <td>-12.515688</td>   
              <td>-2.321584</td>   
              <td>-0.006824</td>   
              <td>1</td>   
              <td>0.051111</td>   
            </tr>   
            <tr>   
              <th>4</th>   
              <td>5</td>   
              <td>-12.433477</td>   
              <td>-2.404495</td>   
              <td>-6.320500</td>   
              <td>1.226</td>   
              <td>-23.488083</td>   
              <td>-23.488083</td>   
              <td>-12.433478</td>   
              <td>-2.404528</td>   
              <td>-0.006824</td>   
              <td>1</td>   
              <td>0.020719</td>   
            </tr>   
          </tbody>   
    </table>
</div> 

```python
y = data.sort_values(by="y-coordinate" , ascending=False)
```

```python

step = 4.892/6
i = 0
y_list = []
section = []
#y6  x3
```

```python
#分y
while i < 6:
    step_y = float("%.8f"%(i*step))
    a_y = step_y-2.446
    y_min = y.loc[y['y-coordinate'] >= a_y]
    i +=1
    step_y = float("%.8f"%(i*step))
    b_y = step_y-2.446
    y_max = y_min.loc[y_min['y-coordinate'] < b_y]
    y_list.append(y_max)
    section_y = (a_y,b_y)
    section.append(section_y)
#section[5]
```

```python
j = 0
step = 1.562/3
x_list = []
section_x_list = []
section_y_list = []
```

```python
#在y中分x
while j <= 5:
    temp = y_list[j]
    m = 0
    while m <= 2:
        step_x = float("%.8f"%(m*step))
        a_x = step_x-12.639
        x_min = temp.loc[temp['x-coordinate'] >= a_x]
        m +=1
        step_x = float("%.8f"%(m*step))
        b_x = step_x-12.639
        x_max = x_min.loc[temp['x-coordinate'] < b_x]
        x_list.append(x_max)
        section_x = (a_x,b_x)
        section_x_list.append(section_x)
        section_y_list.append(section[j])
    j += 1
#18个块
```

```python
section_list = []
Q_list = []
i = 0
```

```python
while i <= 17:
    temp_l = x_list[i]
    Q = (temp_l["density"]*temp_l["z-velocity"]*temp_l["z-face-area"]).sum()
    i +=1
    Q_list.append(Q)
```

```
section_x_list
```

```
[(-12.639, -12.118333329999999),
 (-12.118333329999999, -11.597666669999999),
 (-11.597666669999999, -11.077),
 (-12.639, -12.118333329999999),
 (-12.118333329999999, -11.597666669999999),
 (-11.597666669999999, -11.077),
 (-12.639, -12.118333329999999),
 (-12.118333329999999, -11.597666669999999),
 (-11.597666669999999, -11.077),
 (-12.639, -12.118333329999999),
 (-12.118333329999999, -11.597666669999999),
 (-11.597666669999999, -11.077),
 (-12.639, -12.118333329999999),
 (-12.118333329999999, -11.597666669999999),
 (-11.597666669999999, -11.077),
 (-12.639, -12.118333329999999),
 (-12.118333329999999, -11.597666669999999),
 (-11.597666669999999, -11.077)]
```

```python
section_y_list
```

```
[(-2.446, -1.63066667),
 (-2.446, -1.63066667),
 (-2.446, -1.63066667),
 (-1.63066667, -0.8153333300000001),
 (-1.63066667, -0.8153333300000001),
 (-1.63066667, -0.8153333300000001),
 (-0.8153333300000001, 0.0),
 (-0.8153333300000001, 0.0),
 (-0.8153333300000001, 0.0),
 (0.0, 0.8153333299999996),
 (0.0, 0.8153333299999996),
 (0.0, 0.8153333299999996),
 (0.8153333299999996, 1.6306666699999997),
 (0.8153333299999996, 1.6306666699999997),
 (0.8153333299999996, 1.6306666699999997),
 (1.6306666699999997, 2.446),
 (1.6306666699999997, 2.446),
 (1.6306666699999997, 2.446)]
```

```python
index = []
i = 0
while i <= 2:
    index.append(section_x_list[i])
    i+=1
index
```

```
[(-12.639, -12.118333329999999),
 (-12.118333329999999, -11.597666669999999),
 (-11.597666669999999, -11.077)]
```

```python
cols = []
i = 0
while i <= 17:
    cols.append(section_y_list[i])
    i+=3
cols
```

```
[(-2.446, -1.63066667),
 (-1.63066667, -0.8153333300000001),
 (-0.8153333300000001, 0.0),
 (0.0, 0.8153333299999996),
 (0.8153333299999996, 1.6306666699999997),
 (1.6306666699999997, 2.446)]
```

```python
j = 0
data = {}
while j <=5:
    col = cols[j]
    data[col]= 1
    j += 1
data
```

```
{(-2.446, -1.63066667): 1,
 (-1.63066667, -0.8153333300000001): 1,
 (-0.8153333300000001, 0.0): 1,
 (0.0, 0.8153333299999996): 1,
 (0.8153333299999996, 1.6306666699999997): 1,
 (1.6306666699999997, 2.446): 1}
```

```python
temp_list = Q_list.copy()
r = 0
l = 0
for i in data:
    content = []
    for j in range(3):
        content.append(Q_list[l])
        l+=1
    data[i] = content
data
```

```
{(-2.446, -1.63066667): [12.427282344087539,
  12.86438295520224,
  8.104732674789329],
 (-1.63066667, -0.8153333300000001): [14.059159859218617,
  14.0170078290342,
  6.50613023081204],
 (-0.8153333300000001, 0.0): [13.851341644545903,
  14.102576555137208,
  6.549793869809068],
 (0.0, 0.8153333299999996): [13.873905539737082,
  14.100577509379482,
  6.539991338980014],
 (0.8153333299999996, 1.6306666699999997): [14.030126072372612,
  13.966730831575635,
  6.467075694188603],
 (1.6306666699999997, 2.446): [11.644887356127308,
  12.004108151505823,
  7.422577844039829]}
```

```python
df = pd.DataFrame(data=data, index=index)
df
```
 
<div>
    <style scoped>
           .dataframe tbody tr th:only-of-type {
               vertical-align: middle;
           }
           .dataframe tbody tr th {
               vertical-align: top;,
           }
           .dataframe thead tr th {
               text-align: left;
           }
       </style>
       <table border=\1\ class=\dataframe\>
         <thead>
           <tr>
             <th></th>
             <th>-2.446000</th>
             <th>-1.630667</th>
             <th>-0.815333</th>
             <th>0.000000</th>
             <th>0.815333</th>
             <th>1.630667</th>
           </tr>
           <tr>
             <th></th>
             <th>-1.630667</th>
             <th>-0.815333</th>
             <th>0.000000</th>
             <th>0.815333</th>
             <th>1.630667</th>
             <th>2.446000</th>
           </tr>
         </thead>
         <tbody>
           <tr>
             <th>(-12.639, -12.118333329999999)</th>
             <td>12.427282</td>
             <td>14.059160</td>
             <td>13.851342</td>
             <td>13.873906</td>
             <td>14.030126</td>
             <td>11.644887</td>
           </tr>
           <tr>
             <th>(-12.118333329999999, -11.597666669999999)</th>
             <td>12.864383</td>
             <td>14.017008</td>
             <td>14.102577</td>
             <td>14.100578</td>
             <td>13.966731</td>
             <td>12.004108</td>
           </tr>
           <tr>
             <th>(-11.597666669999999, -11.077)</th>
             <td>8.104733</td>
             <td>6.506130</td>
             <td>6.549794</td>
             <td>6.539991</td>
             <td>6.467076</td>
             <td>7.422578</td>
           </tr>
         </tbody>
    </table>
</div>


相关资料：[数据集链接点此下载](https://spiritlhl.lanzoui.com/ixgxro6hwba)
