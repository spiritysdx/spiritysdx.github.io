# 预处理1-分类数据


## 前言

使用的相关数据链接如下：

[点我跳转](https://gitee.com/spiritlhl/za/releases/%E6%B3%B0%E5%9D%A6%E5%B0%BC%E5%85%8B%E5%8F%B7%E6%95%B0%E6%8D%AE)

## 数据预处理

无量纲化


```python
#归一化 收到0~1之间
#正则化不是归一化 
```


```python
from sklearn.preprocessing import MinMaxScaler
 
data = [[-1, 2], [-0.5, 6], [0, 10], [1, 18]]
 
#不太熟悉numpy的小伙伴，能够判断data的结构吗？
#如果换成表是什么样子？
import pandas as pd
pd.DataFrame(data)
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
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>-1.0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>1</th>
      <td>-0.5</td>
      <td>6</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0.0</td>
      <td>10</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1.0</td>
      <td>18</td>
    </tr>
  </tbody>
</table>
</div>




```python
#实现归一化
#当X中的特征数量非常多的时候，fit会报错并表示，数据量太大了我计算不了
#此时使用partial_fit作为训练接口
#scaler = scaler.partial_fit(data)
scaler = MinMaxScaler()                             #实例化
scaler = scaler.fit(data)                           #fit，在这里本质是生成min(x)和max(x)
result = scaler.transform(data)                     #通过接口导出结果,归一化
result
```




    array([[0.  , 0.  ],
           [0.25, 0.25],
           [0.5 , 0.5 ],
           [1.  , 1.  ]])




```python
result_ = scaler.fit_transform(data)                #训练和导出结果一步达成
result
```




    array([[0.  , 0.  ],
           [0.25, 0.25],
           [0.5 , 0.5 ],
           [1.  , 1.  ]])




```python
scaler.inverse_transform(result)                    #将归一化后的结果逆转
```




    array([[-1. ,  2. ],
           [-0.5,  6. ],
           [ 0. , 10. ],
           [ 1. , 18. ]])




```python
#使用MinMaxScaler的参数feature_range实现将数据归一化到[0,1]以外的范围中
data = [[-1, 2], [-0.5, 6], [0, 10], [1, 18]]
scaler = MinMaxScaler(feature_range=[5,10])         #依然实例化
result = scaler.fit_transform(data)                 #fit_transform一步导出结果
result
```




    array([[ 5.  ,  5.  ],
           [ 6.25,  6.25],
           [ 7.5 ,  7.5 ],
           [10.  , 10.  ]])




```python
import numpy as np
X = np.array([[-1, 2], [-0.5, 6], [0, 10], [1, 18]])
X
```




    array([[-1. ,  2. ],
           [-0.5,  6. ],
           [ 0. , 10. ],
           [ 1. , 18. ]])




```python
#归一化
X_nor = (X - X.min(axis=0)) / (X.max(axis=0) - X.min(axis=0))
X_nor
```




    array([[0.  , 0.  ],
           [0.25, 0.25],
           [0.5 , 0.5 ],
           [1.  , 1.  ]])




```python
#逆转归一化
X_returned = X_nor * (X.max(axis=0) - X.min(axis=0)) + X.min(axis=0)
X_returned
```




    array([[-1. ,  2. ],
           [-0.5,  6. ],
           [ 0. , 10. ],
           [ 1. , 18. ]])




```python
from sklearn.preprocessing import StandardScaler
data = [[-1, 2], [-0.5, 6], [0, 10], [1, 18]]
```


```python
scaler = StandardScaler()                           #实例化
scaler.fit(data)                                    #fit，本质是生成均值和方差
```




    StandardScaler()




```python
scaler.mean_                                        #查看均值的属性mean_
```




    array([-0.125,  9.   ])




```python
scaler.var_                                         #查看方差的属性var_
```




    array([ 0.546875, 35.      ])




```python
x_std = scaler.transform(data)                      #通过接口导出结果
x_std
```




    array([[-1.18321596, -1.18321596],
           [-0.50709255, -0.50709255],
           [ 0.16903085,  0.16903085],
           [ 1.52127766,  1.52127766]])




```python
x_std.mean()                                        #导出的结果是一个数组，用mean()查看均值
```




    0.0




```python
x_std.std()                                         #用std()查看方差
```




    1.0




```python
scaler.fit_transform(data)                          #使用fit_transform(data)一步达成结果
```




    array([[-1.18321596, -1.18321596],
           [-0.50709255, -0.50709255],
           [ 0.16903085,  0.16903085],
           [ 1.52127766,  1.52127766]])




```python
scaler.inverse_transform(x_std)                     #使用inverse_transform逆转标准化
```




    array([[-1. ,  2. ],
           [-0.5,  6. ],
           [ 0. , 10. ],
           [ 1. , 18. ]])



处理缺失值


```python
import pandas as pd
data = pd.read_csv(r"Narrativedata.csv"
                   ,index_col=0
                  )#index_col=0将第0列作为索引，不写则认为第0列为特征
 
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
      <th>Age</th>
      <th>Sex</th>
      <th>Embarked</th>
      <th>Survived</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>22.0</td>
      <td>male</td>
      <td>S</td>
      <td>No</td>
    </tr>
    <tr>
      <th>1</th>
      <td>38.0</td>
      <td>female</td>
      <td>C</td>
      <td>Yes</td>
    </tr>
    <tr>
      <th>2</th>
      <td>26.0</td>
      <td>female</td>
      <td>S</td>
      <td>Yes</td>
    </tr>
    <tr>
      <th>3</th>
      <td>35.0</td>
      <td>female</td>
      <td>S</td>
      <td>Yes</td>
    </tr>
    <tr>
      <th>4</th>
      <td>35.0</td>
      <td>male</td>
      <td>S</td>
      <td>No</td>
    </tr>
  </tbody>
</table>
</div>




```python
data.info()
#填补年龄
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 891 entries, 0 to 890
    Data columns (total 4 columns):
     #   Column    Non-Null Count  Dtype  
    ---  ------    --------------  -----  
     0   Age       714 non-null    float64
     1   Sex       891 non-null    object 
     2   Embarked  889 non-null    object 
     3   Survived  891 non-null    object 
    dtypes: float64(1), object(3)
    memory usage: 34.8+ KB



```python
Age = data.loc[:,"Age"].values.reshape(-1,1)            #sklearn当中特征矩阵必须是二维
Age[:20]
```




    array([[22.],
           [38.],
           [26.],
           [35.],
           [35.],
           [nan],
           [54.],
           [ 2.],
           [27.],
           [14.],
           [ 4.],
           [58.],
           [20.],
           [39.],
           [14.],
           [55.],
           [ 2.],
           [nan],
           [31.],
           [nan]])




```python
from sklearn.impute import SimpleImputer
imp_mean = SimpleImputer()                              #实例化，默认均值填补
imp_median = SimpleImputer(strategy="median")           #用中位数填补
imp_0 = SimpleImputer(strategy="constant",fill_value=0) #用0填补
```


```python
imp_mean = imp_mean.fit_transform(Age)                  #fit_transform一步完成调取结果
imp_median = imp_median.fit_transform(Age)
imp_0 = imp_0.fit_transform(Age)
```


```python
imp_mean[:20]
```




    array([[22.        ],
           [38.        ],
           [26.        ],
           [35.        ],
           [35.        ],
           [29.69911765],
           [54.        ],
           [ 2.        ],
           [27.        ],
           [14.        ],
           [ 4.        ],
           [58.        ],
           [20.        ],
           [39.        ],
           [14.        ],
           [55.        ],
           [ 2.        ],
           [29.69911765],
           [31.        ],
           [29.69911765]])




```python
imp_median[:20]
```




    array([[22.],
           [38.],
           [26.],
           [35.],
           [35.],
           [28.],
           [54.],
           [ 2.],
           [27.],
           [14.],
           [ 4.],
           [58.],
           [20.],
           [39.],
           [14.],
           [55.],
           [ 2.],
           [28.],
           [31.],
           [28.]])




```python
imp_0[:20]
```




    array([[22.],
           [38.],
           [26.],
           [35.],
           [35.],
           [ 0.],
           [54.],
           [ 2.],
           [27.],
           [14.],
           [ 4.],
           [58.],
           [20.],
           [39.],
           [14.],
           [55.],
           [ 2.],
           [ 0.],
           [31.],
           [ 0.]])




```python
 
#在这里我们使用中位数填补Age
data.loc[:,"Age"] = imp_median

data.info()
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 891 entries, 0 to 890
    Data columns (total 4 columns):
     #   Column    Non-Null Count  Dtype  
    ---  ------    --------------  -----  
     0   Age       891 non-null    float64
     1   Sex       891 non-null    object 
     2   Embarked  889 non-null    object 
     3   Survived  891 non-null    object 
    dtypes: float64(1), object(3)
    memory usage: 34.8+ KB



```python
#使用众数填补Embarked
Embarked = data.loc[:,"Embarked"].values.reshape(-1,1)
imp_mode = SimpleImputer(strategy = "most_frequent")#可以填文字，不一定是数值
data.loc[:,"Embarked"] = imp_mode.fit_transform(Embarked)
 
data.info()
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 891 entries, 0 to 890
    Data columns (total 4 columns):
     #   Column    Non-Null Count  Dtype  
    ---  ------    --------------  -----  
     0   Age       891 non-null    float64
     1   Sex       891 non-null    object 
     2   Embarked  891 non-null    object 
     3   Survived  891 non-null    object 
    dtypes: float64(1), object(3)
    memory usage: 34.8+ KB



```python
import pandas as pd
data_ = pd.read_csv("Narrativedata.csv"
                   ,index_col=0
                  )#index_col=0将第0列作为索引，不写则认为第0列为特征

data_.head()
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
      <th>Age</th>
      <th>Sex</th>
      <th>Embarked</th>
      <th>Survived</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>22.0</td>
      <td>male</td>
      <td>S</td>
      <td>No</td>
    </tr>
    <tr>
      <th>1</th>
      <td>38.0</td>
      <td>female</td>
      <td>C</td>
      <td>Yes</td>
    </tr>
    <tr>
      <th>2</th>
      <td>26.0</td>
      <td>female</td>
      <td>S</td>
      <td>Yes</td>
    </tr>
    <tr>
      <th>3</th>
      <td>35.0</td>
      <td>female</td>
      <td>S</td>
      <td>Yes</td>
    </tr>
    <tr>
      <th>4</th>
      <td>35.0</td>
      <td>male</td>
      <td>S</td>
      <td>No</td>
    </tr>
  </tbody>
</table>
</div>




```python
data_.loc[:,"Age"] = data_.loc[:,"Age"].fillna(data_.loc[:,"Age"].median())
#.fillna 在DataFrame里面直接进行填补
```


```python
data_.dropna(axis=0,inplace=True)
#.dropna(axis=0)删除所有有缺失值的行，.dropna(axis=1)删除所有有缺失值的列
#参数inplace，为True表示在原数据集上进行修改，为False表示生成一个复制对象，不修改原数据，默认False
# _data_ = data_.drop(axis=0,inplace=False)
```

编码与哑变量(字符转数值)


```python
from sklearn.preprocessing import LabelEncoder
 
y = data_.iloc[:,-1]                         #要输入的是标签，不是特征矩阵，所以允许一维
 
le = LabelEncoder()                         #实例化
le = le.fit(y)                              #导入数据
label = le.transform(y)                     #transform接口调取结果
```


```python
le.classes_                                 #属性.classes_查看标签中究竟有多少类别
```




    array(['No', 'Unknown', 'Yes'], dtype=object)




```python
label                                       #查看获取的结果label
```




    array([0, 2, 2, 2, 0, 0, 0, 0, 2, 2, 1, 2, 0, 0, 0, 1, 0, 2, 0, 2, 1, 2,
           2, 2, 0, 1, 0, 0, 2, 0, 0, 2, 2, 0, 0, 0, 2, 0, 0, 2, 0, 0, 0, 1,
           2, 0, 0, 2, 0, 0, 0, 0, 2, 2, 0, 2, 2, 0, 2, 0, 0, 0, 0, 0, 2, 2,
           0, 2, 0, 0, 0, 0, 0, 2, 1, 0, 1, 2, 2, 0, 2, 2, 0, 2, 2, 0, 0, 2,
           0, 0, 0, 0, 0, 0, 0, 1, 2, 2, 0, 0, 0, 0, 0, 0, 0, 2, 2, 0, 2, 0,
           0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 2, 0, 2, 0, 2, 2, 0, 1, 0, 0,
           2, 0, 0, 2, 0, 0, 0, 1, 1, 2, 0, 0, 0, 2, 0, 0, 1, 0, 1, 1, 0, 0,
           0, 2, 0, 0, 0, 0, 2, 0, 0, 0, 2, 2, 0, 0, 0, 1, 0, 2, 0, 0, 0, 1,
           0, 0, 0, 0, 0, 0, 1, 2, 0, 2, 2, 0, 0, 2, 0, 2, 1, 2, 2, 0, 0, 1,
           0, 0, 0, 0, 0, 2, 0, 0, 2, 2, 2, 1, 2, 1, 0, 0, 2, 2, 0, 2, 0, 2,
           0, 0, 0, 2, 0, 2, 0, 0, 0, 2, 1, 0, 2, 0, 0, 0, 2, 0, 0, 0, 2, 0,
           0, 0, 0, 0, 2, 2, 0, 0, 0, 0, 1, 0, 2, 2, 2, 2, 2, 0, 2, 0, 1, 0,
           0, 1, 2, 2, 1, 0, 2, 2, 0, 2, 2, 0, 0, 1, 1, 0, 0, 0, 2, 0, 0, 2,
           0, 2, 2, 2, 2, 0, 0, 0, 0, 0, 0, 2, 2, 2, 2, 1, 2, 0, 2, 2, 2, 0,
           2, 2, 2, 0, 0, 0, 2, 2, 1, 2, 2, 0, 1, 2, 2, 0, 2, 0, 1, 2, 2, 2,
           0, 1, 0, 2, 0, 0, 2, 1, 0, 2, 2, 0, 0, 0, 2, 2, 2, 2, 0, 0, 0, 0,
           0, 0, 0, 2, 0, 2, 2, 0, 0, 0, 0, 0, 0, 2, 2, 2, 1, 2, 0, 0, 0, 0,
           2, 2, 0, 0, 0, 2, 2, 0, 2, 0, 0, 0, 2, 0, 2, 2, 2, 0, 2, 2, 0, 0,
           0, 0, 2, 2, 1, 0, 0, 0, 1, 0, 2, 0, 0, 0, 0, 2, 0, 2, 0, 1, 2, 0,
           0, 0, 0, 0, 0, 0, 0, 2, 2, 0, 2, 2, 2, 2, 0, 0, 2, 0, 2, 0, 0, 2,
           0, 0, 2, 2, 2, 2, 2, 2, 2, 0, 0, 0, 2, 1, 2, 0, 2, 2, 0, 2, 1, 0,
           0, 0, 0, 0, 0, 0, 1, 1, 0, 2, 2, 0, 0, 1, 0, 0, 2, 0, 0, 0, 2, 2,
           1, 2, 0, 0, 1, 0, 0, 1, 0, 0, 0, 2, 1, 0, 0, 0, 0, 0, 0, 2, 1, 2,
           2, 1, 2, 2, 0, 2, 2, 1, 0, 2, 0, 2, 0, 2, 0, 0, 2, 0, 0, 2, 0, 0,
           0, 2, 0, 1, 2, 0, 2, 0, 2, 0, 2, 2, 0, 1, 2, 0, 0, 2, 2, 1, 2, 2,
           0, 0, 2, 2, 0, 2, 0, 2, 2, 0, 0, 0, 0, 1, 0, 1, 0, 1, 1, 2, 2, 1,
           2, 0, 0, 2, 2, 0, 2, 2, 2, 0, 0, 0, 2, 0, 2, 1, 0, 0, 2, 0, 0, 0,
           0, 2, 0, 0, 2, 2, 0, 0, 0, 2, 0, 1, 2, 2, 1, 0, 0, 2, 0, 0, 1, 0,
           0, 2, 0, 0, 2, 2, 0, 0, 0, 1, 2, 1, 0, 1, 0, 2, 0, 0, 2, 0, 0, 0,
           0, 0, 2, 0, 2, 2, 2, 1, 2, 0, 2, 0, 2, 0, 2, 0, 0, 0, 0, 0, 0, 1,
           0, 0, 0, 2, 0, 0, 0, 0, 2, 2, 0, 0, 2, 0, 0, 0, 2, 0, 2, 0, 2, 0,
           0, 0, 0, 0, 0, 0, 2, 2, 2, 2, 1, 1, 1, 1, 2, 0, 0, 2, 2, 0, 0, 0,
           0, 1, 2, 2, 2, 2, 0, 1, 0, 1, 1, 2, 1, 0, 0, 2, 0, 0, 0, 2, 0, 2,
           2, 1, 1, 2, 0, 1, 0, 0, 0, 0, 2, 0, 0, 2, 0, 2, 0, 2, 0, 0, 2, 0,
           0, 2, 2, 1, 0, 2, 2, 0, 0, 0, 2, 0, 0, 2, 2, 0, 2, 0, 0, 0, 0, 0,
           1, 0, 0, 1, 1, 0, 2, 0, 2, 2, 2, 0, 0, 0, 0, 2, 0, 2, 0, 0, 0, 0,
           0, 0, 0, 2, 2, 0, 0, 0, 2, 2, 2, 2, 0, 0, 0, 0, 2, 0, 1, 0, 1, 0,
           0, 0, 0, 0, 0, 2, 2, 0, 2, 0, 0, 0, 2, 2, 2, 2, 0, 0, 0, 2, 0, 0,
           2, 2, 0, 0, 2, 0, 0, 1, 0, 0, 0, 1, 1, 0, 1, 2, 0, 2, 2, 2, 2, 0,
           0, 0, 2, 0, 1, 1, 1, 0, 0, 2, 0, 1, 0, 0, 2, 2, 0, 0, 0, 2, 2, 0,
           0, 1, 0, 0, 0, 2, 0, 1, 0])




```python
#le.fit_transform(y)                         #也可以直接fit_transform一步到位
```


```python
#le.inverse_transform(label)                 #使用inverse_transform可以逆转
```


```python
#data.iloc[:,-1] = label                    #让标签等于我们运行出来的结果 
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
      <th>Age</th>
      <th>Sex</th>
      <th>Embarked</th>
      <th>Survived</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>22.0</td>
      <td>male</td>
      <td>S</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>38.0</td>
      <td>female</td>
      <td>C</td>
      <td>2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>26.0</td>
      <td>female</td>
      <td>S</td>
      <td>2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>35.0</td>
      <td>female</td>
      <td>S</td>
      <td>2</td>
    </tr>
    <tr>
      <th>4</th>
      <td>35.0</td>
      <td>male</td>
      <td>S</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>




```python
#如果不需要展示的话会这么写：
from sklearn.preprocessing import LabelEncoder
data.iloc[:,-1] = LabelEncoder().fit_transform(data.iloc[:,-1])#标签列=转码实例化.训练加调用(原始标签)
```

OrdinalEncoder将分类特征转换为分类数值


```python
from sklearn.preprocessing import OrdinalEncoder
 
#接口categories_对应LabelEncoder的接口classes_，一模一样的功能
data_ = data.copy()
data_.head()
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
      <th>Age</th>
      <th>Sex</th>
      <th>Embarked</th>
      <th>Survived</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>22.0</td>
      <td>male</td>
      <td>S</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>38.0</td>
      <td>female</td>
      <td>C</td>
      <td>2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>26.0</td>
      <td>female</td>
      <td>S</td>
      <td>2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>35.0</td>
      <td>female</td>
      <td>S</td>
      <td>2</td>
    </tr>
    <tr>
      <th>4</th>
      <td>35.0</td>
      <td>male</td>
      <td>S</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>




```python
OrdinalEncoder().fit(data_.iloc[:,1:-1]).categories_
```




    [array(['female', 'male'], dtype=object), array(['C', 'Q', 'S'], dtype=object)]




```python
data_.iloc[:,1:-1] = OrdinalEncoder().fit_transform(data_.iloc[:,1:-1])
```


```python
 data_.head()
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
      <th>Age</th>
      <th>Sex</th>
      <th>Embarked</th>
      <th>Survived</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>22.0</td>
      <td>1.0</td>
      <td>2.0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>38.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>26.0</td>
      <td>0.0</td>
      <td>2.0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>35.0</td>
      <td>0.0</td>
      <td>2.0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>4</th>
      <td>35.0</td>
      <td>1.0</td>
      <td>2.0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>



OrdinalEncoder 相当于 LabelEncoder 支持多列版，在列多时候有性能优势，处理特征时优先使用OrdinalEncoder.


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
      <th>Age</th>
      <th>Sex</th>
      <th>Embarked</th>
      <th>Survived</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>22.0</td>
      <td>male</td>
      <td>S</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>38.0</td>
      <td>female</td>
      <td>C</td>
      <td>2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>26.0</td>
      <td>female</td>
      <td>S</td>
      <td>2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>35.0</td>
      <td>female</td>
      <td>S</td>
      <td>2</td>
    </tr>
    <tr>
      <th>4</th>
      <td>35.0</td>
      <td>male</td>
      <td>S</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>




```python
from sklearn.preprocessing import OneHotEncoder
X = data.iloc[:,1:-1]#取sex和embarked两列

enc = OneHotEncoder(categories='auto').fit(X)
result = enc.transform(X).toarray()#训练后结果转换为数组
result
```




    array([[0., 1., 0., 0., 1.],
           [1., 0., 1., 0., 0.],
           [1., 0., 0., 0., 1.],
           ...,
           [1., 0., 0., 0., 1.],
           [0., 1., 1., 0., 0.],
           [0., 1., 0., 1., 0.]])




```python
#依然可以直接一步到位，但为了给大家展示模型属性，所以还是写成了三步
OneHotEncoder(categories='auto').fit_transform(X).toarray()
```




    array([[0., 1., 0., 0., 1.],
           [1., 0., 1., 0., 0.],
           [1., 0., 0., 0., 1.],
           ...,
           [1., 0., 0., 0., 1.],
           [0., 1., 1., 0., 0.],
           [0., 1., 0., 1., 0.]])




```python
#依然可以还原
pd.DataFrame(enc.inverse_transform(result))
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
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>male</td>
      <td>S</td>
    </tr>
    <tr>
      <th>1</th>
      <td>female</td>
      <td>C</td>
    </tr>
    <tr>
      <th>2</th>
      <td>female</td>
      <td>S</td>
    </tr>
    <tr>
      <th>3</th>
      <td>female</td>
      <td>S</td>
    </tr>
    <tr>
      <th>4</th>
      <td>male</td>
      <td>S</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>886</th>
      <td>male</td>
      <td>S</td>
    </tr>
    <tr>
      <th>887</th>
      <td>female</td>
      <td>S</td>
    </tr>
    <tr>
      <th>888</th>
      <td>female</td>
      <td>S</td>
    </tr>
    <tr>
      <th>889</th>
      <td>male</td>
      <td>C</td>
    </tr>
    <tr>
      <th>890</th>
      <td>male</td>
      <td>Q</td>
    </tr>
  </tbody>
</table>
<p>891 rows × 2 columns</p>
</div>




```python
enc.get_feature_names()#返回每一个经过哑变量后生成稀疏矩阵列的名字
```




    array(['x0_female', 'x0_male', 'x1_C', 'x1_Q', 'x1_S'], dtype=object)




```python
result
```




    array([[0., 1., 0., 0., 1.],
           [1., 0., 1., 0., 0.],
           [1., 0., 0., 0., 1.],
           ...,
           [1., 0., 0., 0., 1.],
           [0., 1., 1., 0., 0.],
           [0., 1., 0., 1., 0.]])




```python
result.shape
```




    (891, 5)




```python
#axis=1,表示跨行进行合并，也就是将两表左右相连，如果是axis=0，就是将量表上下相连
newdata = pd.concat([data,pd.DataFrame(result)],axis=1)
```


```python
newdata.head()#合并结果
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
      <th>Age</th>
      <th>Sex</th>
      <th>Embarked</th>
      <th>Survived</th>
      <th>0</th>
      <th>1</th>
      <th>2</th>
      <th>3</th>
      <th>4</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>22.0</td>
      <td>male</td>
      <td>S</td>
      <td>0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>38.0</td>
      <td>female</td>
      <td>C</td>
      <td>2</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>26.0</td>
      <td>female</td>
      <td>S</td>
      <td>2</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>35.0</td>
      <td>female</td>
      <td>S</td>
      <td>2</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>35.0</td>
      <td>male</td>
      <td>S</td>
      <td>0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
newdata.drop(["Sex","Embarked"],axis=1,inplace=True)#删除列
```


```python
newdata.columns = ["Age","Survived","Female","Male","Embarked_C","Embarked_Q","Embarked_S"] 
newdata.head()
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
      <th>Age</th>
      <th>Survived</th>
      <th>Female</th>
      <th>Male</th>
      <th>Embarked_C</th>
      <th>Embarked_Q</th>
      <th>Embarked_S</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>22.0</td>
      <td>0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>38.0</td>
      <td>2</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>26.0</td>
      <td>2</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>35.0</td>
      <td>2</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>35.0</td>
      <td>0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
    </tr>
  </tbody>
</table>
</div>


