# 决策树2-简单调参

## 代码


```python
from sklearn import tree
from sklearn.datasets import load_wine
from sklearn.model_selection import train_test_split
```


```python
wine = load_wine()
```


```python
wine.data.shape
```




    (178, 13)




```python
wine.target
```




    array([0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
           0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
           0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 1, 1, 1, 1, 1, 1,
           1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,
           1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,
           1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 2,
           2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2,
           2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2,
           2, 2])




```python
import pandas as pd
pd.concat([pd.DataFrame(wine.data),pd.DataFrame(wine.target)],axis=1)
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
      <th>3</th>
      <th>4</th>
      <th>5</th>
      <th>6</th>
      <th>7</th>
      <th>8</th>
      <th>9</th>
      <th>10</th>
      <th>11</th>
      <th>12</th>
      <th>0</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>14.23</td>
      <td>1.71</td>
      <td>2.43</td>
      <td>15.6</td>
      <td>127.0</td>
      <td>2.80</td>
      <td>3.06</td>
      <td>0.28</td>
      <td>2.29</td>
      <td>5.64</td>
      <td>1.04</td>
      <td>3.92</td>
      <td>1065.0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>13.20</td>
      <td>1.78</td>
      <td>2.14</td>
      <td>11.2</td>
      <td>100.0</td>
      <td>2.65</td>
      <td>2.76</td>
      <td>0.26</td>
      <td>1.28</td>
      <td>4.38</td>
      <td>1.05</td>
      <td>3.40</td>
      <td>1050.0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>13.16</td>
      <td>2.36</td>
      <td>2.67</td>
      <td>18.6</td>
      <td>101.0</td>
      <td>2.80</td>
      <td>3.24</td>
      <td>0.30</td>
      <td>2.81</td>
      <td>5.68</td>
      <td>1.03</td>
      <td>3.17</td>
      <td>1185.0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>14.37</td>
      <td>1.95</td>
      <td>2.50</td>
      <td>16.8</td>
      <td>113.0</td>
      <td>3.85</td>
      <td>3.49</td>
      <td>0.24</td>
      <td>2.18</td>
      <td>7.80</td>
      <td>0.86</td>
      <td>3.45</td>
      <td>1480.0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>13.24</td>
      <td>2.59</td>
      <td>2.87</td>
      <td>21.0</td>
      <td>118.0</td>
      <td>2.80</td>
      <td>2.69</td>
      <td>0.39</td>
      <td>1.82</td>
      <td>4.32</td>
      <td>1.04</td>
      <td>2.93</td>
      <td>735.0</td>
      <td>0</td>
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
    </tr>
    <tr>
      <th>173</th>
      <td>13.71</td>
      <td>5.65</td>
      <td>2.45</td>
      <td>20.5</td>
      <td>95.0</td>
      <td>1.68</td>
      <td>0.61</td>
      <td>0.52</td>
      <td>1.06</td>
      <td>7.70</td>
      <td>0.64</td>
      <td>1.74</td>
      <td>740.0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>174</th>
      <td>13.40</td>
      <td>3.91</td>
      <td>2.48</td>
      <td>23.0</td>
      <td>102.0</td>
      <td>1.80</td>
      <td>0.75</td>
      <td>0.43</td>
      <td>1.41</td>
      <td>7.30</td>
      <td>0.70</td>
      <td>1.56</td>
      <td>750.0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>175</th>
      <td>13.27</td>
      <td>4.28</td>
      <td>2.26</td>
      <td>20.0</td>
      <td>120.0</td>
      <td>1.59</td>
      <td>0.69</td>
      <td>0.43</td>
      <td>1.35</td>
      <td>10.20</td>
      <td>0.59</td>
      <td>1.56</td>
      <td>835.0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>176</th>
      <td>13.17</td>
      <td>2.59</td>
      <td>2.37</td>
      <td>20.0</td>
      <td>120.0</td>
      <td>1.65</td>
      <td>0.68</td>
      <td>0.53</td>
      <td>1.46</td>
      <td>9.30</td>
      <td>0.60</td>
      <td>1.62</td>
      <td>840.0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>177</th>
      <td>14.13</td>
      <td>4.10</td>
      <td>2.74</td>
      <td>24.5</td>
      <td>96.0</td>
      <td>2.05</td>
      <td>0.76</td>
      <td>0.56</td>
      <td>1.35</td>
      <td>9.20</td>
      <td>0.61</td>
      <td>1.60</td>
      <td>560.0</td>
      <td>2</td>
    </tr>
  </tbody>
</table>
<p>178 rows × 14 columns</p>
</div>




```python
wine.feature_names
```




    ['alcohol',
     'malic_acid',
     'ash',
     'alcalinity_of_ash',
     'magnesium',
     'total_phenols',
     'flavanoids',
     'nonflavanoid_phenols',
     'proanthocyanins',
     'color_intensity',
     'hue',
     'od280/od315_of_diluted_wines',
     'proline']




```python
wine.target_names
```




    array(['class_0', 'class_1', 'class_2'], dtype='<U7')




```python
#XXYY
Xtrain,Xtest,ytrain,ytest=train_test_split(wine.data,wine.target,test_size=0.3)
```


```python
Xtrain.shape
```




    (124, 13)




```python
Xtest.shape
```




    (54, 13)




```python
ytrain
```




    array([2, 0, 0, 1, 1, 2, 0, 2, 0, 0, 1, 0, 1, 2, 1, 2, 1, 0, 1, 1, 0, 1,
           1, 1, 0, 0, 0, 2, 0, 2, 1, 0, 2, 1, 1, 1, 1, 2, 1, 0, 2, 0, 0, 2,
           0, 2, 1, 2, 1, 1, 1, 0, 0, 0, 0, 1, 2, 0, 1, 1, 0, 1, 1, 0, 0, 0,
           1, 1, 2, 2, 0, 0, 1, 2, 0, 1, 1, 0, 1, 1, 1, 1, 0, 0, 2, 1, 1, 0,
           2, 2, 2, 2, 1, 0, 1, 2, 0, 0, 1, 0, 0, 2, 1, 0, 0, 2, 2, 1, 2, 0,
           2, 1, 2, 1, 1, 1, 0, 1, 1, 1, 1, 2, 0, 0])




```python
#通过特征计算不纯度进行分类
clf = tree.DecisionTreeClassifier(criterion="entropy",random_state=30,splitter="random")#不纯度
#random_state=30 设置随机数种子复现结果，设置随机参数
#splitter 设置为best 或random 一个设置更重要分支进行分枝，一个更随机防止过拟合
clf = clf.fit(Xtrain,ytrain)
score = clf.score(Xtest,ytest)#返回预测的准确度accuracy
score
```




    0.8888888888888888




```python
feature_names=wine.feature_names
#画树
import graphviz
dot_data = tree.export_graphviz(clf
                                ,feature_names=feature_names
                                ,class_names=["白酒","红酒","伏特加"]
                                ,filled=True #填充颜色
                                ,rounded=True#直角框变成圆角框
                                ,out_file=None
                                )
graph = graphviz.Source(dot_data)
graph
```




    
![svg](https://gitee.com/spiritlhl/picture/raw/master/output_13_0.svg)
    




```python
clf.feature_importances_
```




    array([0.26346976, 0.        , 0.        , 0.        , 0.        ,
           0.02075703, 0.36896763, 0.        , 0.        , 0.03519618,
           0.048165  , 0.20016672, 0.0632777 ])




```python
#特征重要性-元组列表
[*zip(feature_names,clf.feature_importances_)]
```




    [('alcohol', 0.2634697570171739),
     ('malic_acid', 0.0),
     ('ash', 0.0),
     ('alcalinity_of_ash', 0.0),
     ('magnesium', 0.0),
     ('total_phenols', 0.020757027052013897),
     ('flavanoids', 0.36896762677034245),
     ('nonflavanoid_phenols', 0.0),
     ('proanthocyanins', 0.0),
     ('color_intensity', 0.035196177552898056),
     ('hue', 0.04816499533787735),
     ('od280/od315_of_diluted_wines', 0.20016671726273566),
     ('proline', 0.0632776990069589)]




```python
scroe_train = clf.score(Xtrain,ytrain)
scroe_train
```




    1.0



# 剪枝策略


```python
#限制深度 max_depth 3
#min_samples_left 分支满足条件才能向下生长 5~1 或者用百分比0.几
#通过特征计算不纯度进行分类
clf = tree.DecisionTreeClassifier(criterion="entropy"#不纯度
                                  ,random_state=30
                                  ,splitter="random"
                                  #,max_depth=3
                                  ,min_samples_leaf=10
                                   #,min_impurity_split=1
                                 )
#random_state=30 设置随机数种子复现结果，设置随机参数
#splitter 设置为best 或random 一个设置更重要分支进行分枝，一个更随机防止过拟合
clf = clf.fit(Xtrain,ytrain)
score = clf.score(Xtest,ytest)#返回预测的准确度accuracy
score
feature_names=wine.feature_names
#画树
import graphviz
dot_data = tree.export_graphviz(clf
                                ,feature_names=feature_names
                                ,class_names=["白酒","红酒","伏特加"]
                                ,filled=True #填充颜色
                                ,rounded=True#直角框变成圆角框
                                ,out_file=None
                                )
graph = graphviz.Source(dot_data)
graph
```




    
![svg](https://gitee.com/spiritlhl/picture/raw/master/output_18_0.svg)
    




```python
score = clf.score(Xtest,ytest)#返回预测的准确度accuracy
score
```




    0.8518518518518519




```python
#max_features 设置特征数量用几个
#min_impurity_decrease 限制信息增益 父子节点信息增益差
```


```python
#画超参数曲线看打分确定参数
import matplotlib.pyplot as plt
test = []
for i in range(10):
    clf = tree.DecisionTreeClassifier(max_depth=i+1
                                      ,criterion="entropy"
                                      ,random_state=30
                                      ,splitter="random"
                                     )
    clf = clf.fit(Xtrain,ytrain)
    score = clf.score(Xtest,ytest)#返回预测的准确度accuracy
    test.append(score)
plt.plot(range(1,11),test,color="red",label="max_depth")
plt.legend()
plt.show()
```


    
![png](https://gitee.com/spiritlhl/picture/raw/master/output_21_0.png)
    



```python
# 目标权重参数
#class_weight
#class_weight_fraction_leaf
#默认偏向平衡，偏向主导类，可设置为偏向少数类
```


```python
#apply返回每个测试样本所在的叶子节点的索引
clf.apply(Xtest)
```




    array([ 9,  4, 32, 16,  4,  4, 32, 22, 28, 32, 27, 16, 25, 16, 32, 16, 10,
           16,  4,  4, 32,  4, 16,  4, 25, 16, 16, 16, 32, 32,  6, 22, 10,  4,
            4,  4, 32, 29, 25, 13, 16,  4,  9, 25, 32,  4,  4, 22,  4, 22, 32,
           32, 22,  4])




```python
#predict返回每个测试样本的分类/回归结果
clf.predict(Xtest)
```




    array([1, 2, 0, 1, 2, 2, 0, 1, 0, 0, 1, 1, 1, 1, 0, 1, 2, 1, 2, 2, 0, 2,
           1, 2, 1, 1, 1, 1, 0, 0, 1, 1, 2, 2, 2, 2, 0, 1, 1, 1, 1, 2, 1, 1,
           0, 2, 2, 1, 2, 1, 0, 0, 1, 2])




```python
#特征维度起码是2维，如果是一维reshape（-1，1）来给数据增加维度
```

# 交叉验证


```python
from sklearn.datasets import load_boston
from sklearn.model_selection import cross_val_score
from sklearn.tree import DecisionTreeRegressor
```


```python
boston = load_boston()
boston.data
```




    array([[6.3200e-03, 1.8000e+01, 2.3100e+00, ..., 1.5300e+01, 3.9690e+02,
            4.9800e+00],
           [2.7310e-02, 0.0000e+00, 7.0700e+00, ..., 1.7800e+01, 3.9690e+02,
            9.1400e+00],
           [2.7290e-02, 0.0000e+00, 7.0700e+00, ..., 1.7800e+01, 3.9283e+02,
            4.0300e+00],
           ...,
           [6.0760e-02, 0.0000e+00, 1.1930e+01, ..., 2.1000e+01, 3.9690e+02,
            5.6400e+00],
           [1.0959e-01, 0.0000e+00, 1.1930e+01, ..., 2.1000e+01, 3.9345e+02,
            6.4800e+00],
           [4.7410e-02, 0.0000e+00, 1.1930e+01, ..., 2.1000e+01, 3.9690e+02,
            7.8800e+00]])




```python
boston.target
```




    array([24. , 21.6, 34.7, 33.4, 36.2, 28.7, 22.9, 27.1, 16.5, 18.9, 15. ,
           18.9, 21.7, 20.4, 18.2, 19.9, 23.1, 17.5, 20.2, 18.2, 13.6, 19.6,
           15.2, 14.5, 15.6, 13.9, 16.6, 14.8, 18.4, 21. , 12.7, 14.5, 13.2,
           13.1, 13.5, 18.9, 20. , 21. , 24.7, 30.8, 34.9, 26.6, 25.3, 24.7,
           21.2, 19.3, 20. , 16.6, 14.4, 19.4, 19.7, 20.5, 25. , 23.4, 18.9,
           35.4, 24.7, 31.6, 23.3, 19.6, 18.7, 16. , 22.2, 25. , 33. , 23.5,
           19.4, 22. , 17.4, 20.9, 24.2, 21.7, 22.8, 23.4, 24.1, 21.4, 20. ,
           20.8, 21.2, 20.3, 28. , 23.9, 24.8, 22.9, 23.9, 26.6, 22.5, 22.2,
           23.6, 28.7, 22.6, 22. , 22.9, 25. , 20.6, 28.4, 21.4, 38.7, 43.8,
           33.2, 27.5, 26.5, 18.6, 19.3, 20.1, 19.5, 19.5, 20.4, 19.8, 19.4,
           21.7, 22.8, 18.8, 18.7, 18.5, 18.3, 21.2, 19.2, 20.4, 19.3, 22. ,
           20.3, 20.5, 17.3, 18.8, 21.4, 15.7, 16.2, 18. , 14.3, 19.2, 19.6,
           23. , 18.4, 15.6, 18.1, 17.4, 17.1, 13.3, 17.8, 14. , 14.4, 13.4,
           15.6, 11.8, 13.8, 15.6, 14.6, 17.8, 15.4, 21.5, 19.6, 15.3, 19.4,
           17. , 15.6, 13.1, 41.3, 24.3, 23.3, 27. , 50. , 50. , 50. , 22.7,
           25. , 50. , 23.8, 23.8, 22.3, 17.4, 19.1, 23.1, 23.6, 22.6, 29.4,
           23.2, 24.6, 29.9, 37.2, 39.8, 36.2, 37.9, 32.5, 26.4, 29.6, 50. ,
           32. , 29.8, 34.9, 37. , 30.5, 36.4, 31.1, 29.1, 50. , 33.3, 30.3,
           34.6, 34.9, 32.9, 24.1, 42.3, 48.5, 50. , 22.6, 24.4, 22.5, 24.4,
           20. , 21.7, 19.3, 22.4, 28.1, 23.7, 25. , 23.3, 28.7, 21.5, 23. ,
           26.7, 21.7, 27.5, 30.1, 44.8, 50. , 37.6, 31.6, 46.7, 31.5, 24.3,
           31.7, 41.7, 48.3, 29. , 24. , 25.1, 31.5, 23.7, 23.3, 22. , 20.1,
           22.2, 23.7, 17.6, 18.5, 24.3, 20.5, 24.5, 26.2, 24.4, 24.8, 29.6,
           42.8, 21.9, 20.9, 44. , 50. , 36. , 30.1, 33.8, 43.1, 48.8, 31. ,
           36.5, 22.8, 30.7, 50. , 43.5, 20.7, 21.1, 25.2, 24.4, 35.2, 32.4,
           32. , 33.2, 33.1, 29.1, 35.1, 45.4, 35.4, 46. , 50. , 32.2, 22. ,
           20.1, 23.2, 22.3, 24.8, 28.5, 37.3, 27.9, 23.9, 21.7, 28.6, 27.1,
           20.3, 22.5, 29. , 24.8, 22. , 26.4, 33.1, 36.1, 28.4, 33.4, 28.2,
           22.8, 20.3, 16.1, 22.1, 19.4, 21.6, 23.8, 16.2, 17.8, 19.8, 23.1,
           21. , 23.8, 23.1, 20.4, 18.5, 25. , 24.6, 23. , 22.2, 19.3, 22.6,
           19.8, 17.1, 19.4, 22.2, 20.7, 21.1, 19.5, 18.5, 20.6, 19. , 18.7,
           32.7, 16.5, 23.9, 31.2, 17.5, 17.2, 23.1, 24.5, 26.6, 22.9, 24.1,
           18.6, 30.1, 18.2, 20.6, 17.8, 21.7, 22.7, 22.6, 25. , 19.9, 20.8,
           16.8, 21.9, 27.5, 21.9, 23.1, 50. , 50. , 50. , 50. , 50. , 13.8,
           13.8, 15. , 13.9, 13.3, 13.1, 10.2, 10.4, 10.9, 11.3, 12.3,  8.8,
            7.2, 10.5,  7.4, 10.2, 11.5, 15.1, 23.2,  9.7, 13.8, 12.7, 13.1,
           12.5,  8.5,  5. ,  6.3,  5.6,  7.2, 12.1,  8.3,  8.5,  5. , 11.9,
           27.9, 17.2, 27.5, 15. , 17.2, 17.9, 16.3,  7. ,  7.2,  7.5, 10.4,
            8.8,  8.4, 16.7, 14.2, 20.8, 13.4, 11.7,  8.3, 10.2, 10.9, 11. ,
            9.5, 14.5, 14.1, 16.1, 14.3, 11.7, 13.4,  9.6,  8.7,  8.4, 12.8,
           10.5, 17.1, 18.4, 15.4, 10.8, 11.8, 14.9, 12.6, 14.1, 13. , 13.4,
           15.2, 16.1, 17.8, 14.9, 14.1, 12.7, 13.5, 14.9, 20. , 16.4, 17.7,
           19.5, 20.2, 21.4, 19.9, 19. , 19.1, 19.1, 20.1, 19.9, 19.6, 23.2,
           29.8, 13.8, 13.3, 16.7, 12. , 14.6, 21.4, 23. , 23.7, 25. , 21.8,
           20.6, 21.2, 19.1, 20.6, 15.2,  7. ,  8.1, 13.6, 20.1, 21.8, 24.5,
           23.1, 19.7, 18.3, 21.2, 17.5, 16.8, 22.4, 20.6, 23.9, 22. , 11.9])




```python
regressor = DecisionTreeRegressor(random_state=0)#实例化
cross_val_score(regressor      #模型
                ,boston.data   #数据
                ,boston.target #特征
                ,cv=10         #交叉验证次数
                ,scoring="neg_mean_squared_error" #返回负的均方误差，不写默认返回R方
               )
```




    array([-18.08941176, -10.61843137, -16.31843137, -44.97803922,
           -17.12509804, -49.71509804, -12.9986    , -88.4514    ,
           -55.7914    , -25.0816    ])



