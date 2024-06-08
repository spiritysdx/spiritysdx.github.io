# 预处理3-连续值与特征选取


## 前言

使用的相关数据链接如下：

[点我跳转](https://gitee.com/spiritlhl/za/releases/%E6%B3%B0%E5%9D%A6%E5%B0%BC%E5%85%8B%E5%8F%B7%E6%95%B0%E6%8D%AE)

# 代码 

## 连续值预处理

```python
import pandas as pd
data = pd.read_csv("Narrativedata.csv"
                   ,index_col=0
                  )#index_col=0将第0列作为索引，不写则认为第0列为特征
data.loc[:,"Age"] = data.loc[:,"Age"].fillna(data.loc[:,"Age"].median())

#将年龄二值化 

data_2 = data.copy()
data_2.info()
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
from sklearn.preprocessing import Binarizer
X = data_2.iloc[:,0].values.reshape(-1,1)               #类为特征专用，所以不能使用一维数组
transformer = Binarizer(threshold=30).fit_transform(X)  #阈值threshold=n, 小于等于n的数值转为0, 大于n的数值转为1
```


```python
data_2.iloc[:,0] = transformer
data_2.head()
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
      <td>0.0</td>
      <td>male</td>
      <td>S</td>
      <td>No</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1.0</td>
      <td>female</td>
      <td>C</td>
      <td>Yes</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0.0</td>
      <td>female</td>
      <td>S</td>
      <td>Yes</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1.0</td>
      <td>female</td>
      <td>S</td>
      <td>Yes</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1.0</td>
      <td>male</td>
      <td>S</td>
      <td>No</td>
    </tr>
  </tbody>
</table>
</div>




```python
from sklearn.preprocessing import KBinsDiscretizer
X = data.iloc[:,0].values.reshape(-1,1)
```


```python
est = KBinsDiscretizer(n_bins=3, encode='ordinal', strategy='uniform')
est.fit_transform(X)
```



```python
#查看转换后分的箱：变成了一列中的三箱
set(est.fit_transform(X).ravel())
```




    {0.0, 1.0, 2.0}




```python
est = KBinsDiscretizer(n_bins=3, encode='onehot', strategy='uniform')

#查看转换后分的箱：变成了哑变量
est.fit_transform(X).toarray()
```




    array([[1., 0., 0.],
           [0., 1., 0.],
           [1., 0., 0.],
           ...,
           [0., 1., 0.],
           [1., 0., 0.],
           [0., 1., 0.]])


# 特征选取
```python
import pandas as pd
data = pd.read_csv("digit recognizor.csv")
```


```python
X = data.iloc[:,1:]
y = data.iloc[:,0]
```


```python
X.shape
```




    (42000, 784)



## Fliter过滤法

方差过滤


```python
"""
这个数据量相对夸张，如果使用支持向量机和神经网络，很可能会直接跑不出来。使用KNN跑一次大概需要半个小时。
用这个数据举例，能更够体现特征工程的重要性。
"""
from sklearn.feature_selection import VarianceThreshold
```


```python
selector = VarianceThreshold() #实例化，不填参数默认方差为0
```


```python
X_var0 = selector.fit_transform(X) #获取删除不合格特征之后的新特征矩阵，删除方差为0的特征
```


```python
#也可以直接写成 X = VairanceThreshold().fit_transform(X)
X_var0.shape
```




    (42000, 708)




```python
#我们希望留下一半的特征，
#那可以设定一个让特征总数减半的方差阈值，只要找到特征方差的中位数，再将这个中位数作为参数threshold的值输入就好了
import numpy as np
#X.var() 读取每一列的方差
X_fsvar = VarianceThreshold(np.median(X.var().values)).fit_transform(X)
#X.var().values
```


```python
np.median(X.var().values)
```




    1352.2867031797243




```python
X_fsvar.shape
```




    (42000, 392)




```python
#若特征是伯努利随机变量，假设p=0.8，即二分类特征中某种分类占到80%以上的时候删除特征
X_bvar = VarianceThreshold(.8 * (1 - .8)).fit_transform(X)
X_bvar.shape
```




    (42000, 685)



相关性过滤


```python
from sklearn.ensemble import RandomForestClassifier as RFC
from sklearn.model_selection import cross_val_score
from sklearn.feature_selection import SelectKBest
from sklearn.feature_selection import chi2
```


```python
#再结合feature_selection.SelectKBest这个可以输入”评分标准“来选出前K个分数最高的特征的类，
X_fschi = SelectKBest(chi2,k=300).fit_transform(X_fsvar,y)#评分标准是卡方chi2
X_fschi.shape
```




    (42000, 300)




```python
cross_val_score(RFC(n_estimators=10,random_state=0),X_fschi,y,cv=5).mean()
```




    0.9344761904761905




```python
#python中的魔法命令，可以直接使用%%timeit来计算运行这个cell中的代码所需的时间
#为了计算所需的时间，需要将这个cell中的代码运行很多次（通常是7次）后求平均值，因此运行%%timeit的时间会远远超过cell中的代码单独运行的时间
```


```python
X = data.iloc[:,1:]
y = data.iloc[:,0]
X_fsvar = VarianceThreshold(np.median(X.var().values)).fit_transform(X)
```

```
#======【TIME WARNING: 5 mins】======#
%matplotlib inline
import matplotlib.pyplot as plt
score = []
for i in range(390,200,-10):
    X_fschi = SelectKBest(chi2, k=i).fit_transform(X_fsvar, y)
    once = cross_val_score(RFC(n_estimators=10,random_state=0),X_fschi,y,cv=5).mean()
    score.append(once)
plt.plot(range(350,200,-10),score)
#plt.show()
```

卡方值越大越好，p值小于0.05时是数据相关


```python
chivalue, pvalues_chi = chi2(X_fsvar,y)
#chivalue
```


```python
#pvalues_chi
```


```python
#k取多少？我们想要消除所有p值大于设定值，比如0.05或0.01的特征：
k = chivalue.shape[0] - (pvalues_chi > 0.05).sum()
#X_fschi = SelectKBest(chi2, k=填写具体的k).fit_transform(X_fsvar, y)
#cross_val_score(RFC(n_estimators=10,random_state=0),X_fschi,y,cv=5).mean()
```


```python
k
```




    392




```python
from sklearn.feature_selection import f_classif

F,pvalues_f = f_classif(X_fsvar,y)

#F
```


```python
#pvalues_f
```


```python
k = F.shape[0] - (pvalues_f > 0.05).sum()
```


```python
k
```




    392




```python
#X_fsF = SelectKBest(f_classif, k=填写具体的k).fit_transform(X_fsvar, y)
#cross_val_score(RFC(n_estimators=10,random_state=0),X_fsF,y,cv=5).mean()
```


```python
from sklearn.feature_selection import mutual_info_classif as MIC
result = MIC(X_fsvar,y)
k = result.shape[0] - sum(result <= 0)
```


```python
k
```




    392




```python
#X_fsmic = SelectKBest(MIC, k=填写具体的k).fit_transform(X_fsvar, y)
#cross_val_score(RFC(n_estimators=10,random_state=0),X_fsmic,y,cv=5).mean()
```

## 嵌入法


```python
from sklearn.feature_selection import SelectFromModel
from sklearn.ensemble import RandomForestClassifier as RFC
RFC_ = RFC(n_estimators =10,random_state=0)
X_embedded = SelectFromModel(RFC_,threshold=0.005).fit_transform(X,y)
#在这里我只想取出来有限的特征。0.005这个阈值对于有780个特征的数据来说，是非常高的阈值，因为平均每个特征只能够分到大约0.001的feature_importances_
X_embedded.shape
```




    (42000, 47)




```python
#模型的维度明显被降低了
#同样的，我们也可以画学习曲线来找最佳阈值
#======【TIME WARNING：10 mins】======#
import numpy as np
import matplotlib.pyplot as plt
RFC_.fit(X,y).feature_importances_
threshold = np.linspace(0,(RFC_.fit(X,y).feature_importances_).max(),20)
score = []
for i in threshold:
    X_embedded = SelectFromModel(RFC_,threshold=i).fit_transform(X,y)
    once = cross_val_score(RFC_,X_embedded,y,cv=5).mean()
    score.append(once)
plt.plot(threshold,score)
plt.show()
```


    
![png](https://gitee.com/spiritlhl/picture/raw/master/output_35_0.png)
    



```python
#上述图找到合适的threshold区间
X_embedded = SelectFromModel(RFC_,threshold=0.00067).fit_transform(X,y)
X_embedded.shape
```




    (42000, 324)




```python
cross_val_score(RFC_,X_embedded,y,cv=5).mean()
```




    0.9391190476190475




```python
#======【TIME WARNING：10 mins】======#
#区间内再找合适的值
score2 = []
for i in np.linspace(0,0.00134,20):
    X_embedded = SelectFromModel(RFC_,threshold=i).fit_transform(X,y)
    once = cross_val_score(RFC_,X_embedded,y,cv=5).mean()
    score2.append(once)
plt.figure(figsize=[20,5])
plt.plot(np.linspace(0,0.00134,20),score2)
plt.xticks(np.linspace(0,0.00134,20))
plt.show()
```


    
![png](https://gitee.com/spiritlhl/picture/raw/master/output_38_0.png)
    



```python
X_embedded = SelectFromModel(RFC_,threshold=0.000564).fit_transform(X,y)
X_embedded.shape
```




    (42000, 340)




```python
cross_val_score(RFC_,X_embedded,y,cv=5).mean()
```




    0.9392857142857144




```python
#=====【TIME WARNING：2 min】=====#
#我们可能已经找到了现有模型下的最佳结果，如果我们调整一下随机森林的参数呢？
cross_val_score(RFC(n_estimators=100,random_state=0),X_embedded,y,cv=5).mean()
```




    0.9634285714285715



## 包装法-递归特征消除法


```python
from sklearn.feature_selection import RFE
RFC_ = RFC(n_estimators =10,random_state=0)
selector = RFE(RFC_, n_features_to_select=340, step=50).fit(X, y)
```


```python
selector.support_.sum()
```




    340




```python
#selector.ranking_
```


```python
X_wrapper = selector.transform(X)
cross_val_score(RFC_,X_wrapper,y,cv=5).mean()
```




    0.9379761904761905




```python
#======【TIME WARNING: 15 mins】======#
score = []
for i in range(1,751,50):
    X_wrapper = RFE(RFC_,n_features_to_select=i, step=50).fit_transform(X,y)
    once = cross_val_score(RFC_,X_wrapper,y,cv=5).mean()
    score.append(once)
plt.figure(figsize=[20,5])
plt.plot(range(1,751,50),score)
plt.xticks(range(1,751,50))
plt.show()

```


    
![png](https://gitee.com/spiritlhl/picture/raw/master/output_47_0.png)
    





