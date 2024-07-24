# 预处理2-随机森林填补缺失值

## 随机森林填补缺失值(实测回归比较好)


```python
%matplotlib inline
from sklearn.tree import DecisionTreeClassifier
from sklearn.ensemble import RandomForestClassifier#随机森林分类树
from sklearn.datasets import load_wine
from sklearn.model_selection import train_test_split
```


```python
wine = load_wine()
wine
```




    {'data': array([[1.423e+01, 1.710e+00, 2.430e+00, ..., 1.040e+00, 3.920e+00,
             1.065e+03],
            [1.320e+01, 1.780e+00, 2.140e+00, ..., 1.050e+00, 3.400e+00,
             1.050e+03],
            [1.316e+01, 2.360e+00, 2.670e+00, ..., 1.030e+00, 3.170e+00,
             1.185e+03],
            ...,
            [1.327e+01, 4.280e+00, 2.260e+00, ..., 5.900e-01, 1.560e+00,
             8.350e+02],
            [1.317e+01, 2.590e+00, 2.370e+00, ..., 6.000e-01, 1.620e+00,
             8.400e+02],
            [1.413e+01, 4.100e+00, 2.740e+00, ..., 6.100e-01, 1.600e+00,
             5.600e+02]]),
     'target': array([0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
            0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
            0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 1, 1, 1, 1, 1, 1,
            1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,
            1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,
            1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 2,
            2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2,
            2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2,
            2, 2]),
     'frame': None,
     'target_names': array(['class_0', 'class_1', 'class_2'], dtype='<U7'),
     'DESCR': '.. _wine_dataset:\n\nWine recognition dataset\n------------------------\n\n**Data Set Characteristics:**\n\n    :Number of Instances: 178 (50 in each of three classes)\n    :Number of Attributes: 13 numeric, predictive attributes and the class\n    :Attribute Information:\n \t\t- Alcohol\n \t\t- Malic acid\n \t\t- Ash\n\t\t- Alcalinity of ash  \n \t\t- Magnesium\n\t\t- Total phenols\n \t\t- Flavanoids\n \t\t- Nonflavanoid phenols\n \t\t- Proanthocyanins\n\t\t- Color intensity\n \t\t- Hue\n \t\t- OD280/OD315 of diluted wines\n \t\t- Proline\n\n    - class:\n            - class_0\n            - class_1\n            - class_2\n\t\t\n    :Summary Statistics:\n    \n    ============================= ==== ===== ======= =====\n                                   Min   Max   Mean     SD\n    ============================= ==== ===== ======= =====\n    Alcohol:                      11.0  14.8    13.0   0.8\n    Malic Acid:                   0.74  5.80    2.34  1.12\n    Ash:                          1.36  3.23    2.36  0.27\n    Alcalinity of Ash:            10.6  30.0    19.5   3.3\n    Magnesium:                    70.0 162.0    99.7  14.3\n    Total Phenols:                0.98  3.88    2.29  0.63\n    Flavanoids:                   0.34  5.08    2.03  1.00\n    Nonflavanoid Phenols:         0.13  0.66    0.36  0.12\n    Proanthocyanins:              0.41  3.58    1.59  0.57\n    Colour Intensity:              1.3  13.0     5.1   2.3\n    Hue:                          0.48  1.71    0.96  0.23\n    OD280/OD315 of diluted wines: 1.27  4.00    2.61  0.71\n    Proline:                       278  1680     746   315\n    ============================= ==== ===== ======= =====\n\n    :Missing Attribute Values: None\n    :Class Distribution: class_0 (59), class_1 (71), class_2 (48)\n    :Creator: R.A. Fisher\n    :Donor: Michael Marshall (MARSHALL%PLU@io.arc.nasa.gov)\n    :Date: July, 1988\n\nThis is a copy of UCI ML Wine recognition datasets.\nhttps://archive.ics.uci.edu/ml/machine-learning-databases/wine/wine.data\n\nThe data is the results of a chemical analysis of wines grown in the same\nregion in Italy by three different cultivators. There are thirteen different\nmeasurements taken for different constituents found in the three types of\nwine.\n\nOriginal Owners: \n\nForina, M. et al, PARVUS - \nAn Extendible Package for Data Exploration, Classification and Correlation. \nInstitute of Pharmaceutical and Food Analysis and Technologies,\nVia Brigata Salerno, 16147 Genoa, Italy.\n\nCitation:\n\nLichman, M. (2013). UCI Machine Learning Repository\n[https://archive.ics.uci.edu/ml]. Irvine, CA: University of California,\nSchool of Information and Computer Science. \n\n.. topic:: References\n\n  (1) S. Aeberhard, D. Coomans and O. de Vel, \n  Comparison of Classifiers in High Dimensional Settings, \n  Tech. Rep. no. 92-02, (1992), Dept. of Computer Science and Dept. of  \n  Mathematics and Statistics, James Cook University of North Queensland. \n  (Also submitted to Technometrics). \n\n  The data was used with many others for comparing various \n  classifiers. The classes are separable, though only RDA \n  has achieved 100% correct classification. \n  (RDA : 100%, QDA 99.4%, LDA 98.9%, 1NN 96.1% (z-transformed data)) \n  (All results using the leave-one-out technique) \n\n  (2) S. Aeberhard, D. Coomans and O. de Vel, \n  "THE CLASSIFICATION PERFORMANCE OF RDA" \n  Tech. Rep. no. 92-01, (1992), Dept. of Computer Science and Dept. of \n  Mathematics and Statistics, James Cook University of North Queensland. \n  (Also submitted to Journal of Chemometrics).\n',
     'feature_names': ['alcohol',
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
      'proline']}




```python
wine.data.shape#特征矩阵
```




    (178, 13)




```python
wine.target.shape#标签，以数的形式呈现
```




    (178,)




```python
#实例化
clf = DecisionTreeClassifier(random_state=25)
xtrain,xtest,ytrain,ytest = train_test_split(wine.data,wine.target,test_size=0.3)#数据和特征分开导入
```


```python
clf = DecisionTreeClassifier(random_state=0)
rfc = RandomForestClassifier(random_state=0)
```


```python
#导入训练
clf = clf.fit(xtrain,ytrain)
rfc = rfc.fit(xtrain,ytrain)
```


```python
#打分
score_c = clf.score(xtest,ytest)
score_r = rfc.score(xtest,ytest)
```


```python
print("Single Tree:{}".format(score_c)
      ,"Random Forest:{}".format(score_r))
```

    Single Tree:0.8703703703703703 Random Forest:0.9629629629629629



```python
# 交叉验证
from sklearn.model_selection import cross_val_score
import matplotlib.pyplot as plt

rfc = RandomForestClassifier(random_state=25)
rfc_s = cross_val_score(rfc,wine.data,wine.target,cv=10)

clf = DecisionTreeClassifier(random_state=25)
clf_s = cross_val_score(clf,wine.data,wine.target,cv=10)

plt.plot(range(1,11),rfc_s,label="RandomForest")
plt.plot(range(1,11),clf_s,label="DecisionTree")
plt.legend()
plt.show()
```


    
![png](https://gitee.com/spiritlhl/picture/raw/master/output_10_0.png)
    



```python
superpa = []
for i in range(200):
    rfc = RandomForestClassifier(n_estimators=i+1,n_jobs=-1)#n_jobs是-1时表示CPU所有core进行工作
    rfc_s = cross_val_score(rfc,wine.data,wine.target,cv=10).mean()
    superpa.append(rfc_s)
print (max(superpa),superpa.index(max(superpa)))
plt.figure(figsize=[20,5])
plt.plot(range(1, 201),superpa)
plt.show( )

```

    0.9888888888888889 35



    
![png](https://gitee.com/spiritlhl/picture/raw/master/output_11_1.png)
    



```python
rfc_l = []
clf_l = []

for i in range(10):
    #弱学习器的最大迭代次数，或者说最大的弱学习器的个数
    rfc = RandomForestClassifier(n_estimators=25)
    rfc_s = cross_val_score(rfc,wine.data,wine.target,cv=10).mean()
    rfc_l.append(rfc_s)
    clf = DecisionTreeClassifier()
    clf_s = cross_val_score(clf,wine.data,wine.target,cv=10).mean()
    clf_l.append(clf_s)

plt.plot(range(1,11),rfc_l,label="RandomForest")
plt.plot(range(1,11),clf_l,label="DecisionTree")
plt.legend()
plt.show()
```


    
![png](https://gitee.com/spiritlhl/picture/raw/master/output_12_0.png)
    



```python
rfc = RandomForestClassifier(n_estimators=25,random_state=2)#这里是森林生成的随机树模式固定，不是每个树都一致
rfc = rfc.fit(xtrain,ytrain)
```


```python
#rfc.estimators_查看森林中所有树参数的状况，发现每个树只有random_state变了
rfc.estimators_[0].random_state
```




    1872583848




```python
for i in range(len(rfc.estimators_)):
    print(rfc.estimators_[i].random_state)
```

    1872583848
    794921487
    111352301
    1853453896
    213298710
    1922988331
    1869695442
    2081981515
    1805465960
    1376693511
    1418777250
    663257521
    878959199
    854108747
    512264917
    515183663
    1287007039
    2083814687
    1146014426
    570104212
    520265852
    1366773364
    125164325
    786090663
    578016451



```python
#无需划分训练集和测试集
rfc = RandomForestClassifier(n_estimators=25,oob_score=True)
rfc = rfc.fit(wine.data,wine.target)
#袋外数据评分
rfc.oob_score_
```




    0.9606741573033708




```python
rfc = RandomForestClassifier(n_estimators=25)
rfc = rfc.fit(xtrain,ytrain)
```


```python
rfc.score(xtest,ytest)
```




    0.9629629629629629




```python
rfc.feature_importances_#特征重要性数值 #zip联系名字和特征值
```




    array([0.09877403, 0.04378921, 0.00627884, 0.03234554, 0.06983567,
           0.0588977 , 0.17206286, 0.00407809, 0.01527506, 0.18783169,
           0.07655246, 0.11355569, 0.12072316])




```python
rfc.apply(xtest)
```




    array([[ 2,  8,  6, ...,  3,  4,  8],
           [ 2,  9,  9, ...,  4,  4,  8],
           [ 9, 15,  9, ...,  8,  1,  5],
           ...,
           [14, 18, 18, ..., 19,  8, 16],
           [ 7, 18, 15, ..., 19,  8, 16],
           [ 2,  8, 10, ..., 19,  4,  8]])




```python
rfc.predict(xtest)#测试集预测结果
```




    array([2, 2, 1, 0, 0, 1, 1, 0, 0, 2, 1, 1, 2, 2, 0, 0, 0, 2, 0, 0, 2, 1,
           1, 2, 1, 2, 1, 0, 2, 0, 1, 2, 0, 1, 2, 2, 1, 2, 0, 1, 1, 0, 2, 0,
           2, 2, 1, 0, 2, 0, 0, 0, 0, 2])




```python
rfc.predict_proba(xtest)#多少样本多少行，多少特征多少列，按照平均值特征大小分类
```




    array([[0.  , 0.04, 0.96],
           [0.  , 0.36, 0.64],
           [0.  , 1.  , 0.  ],
           [0.56, 0.4 , 0.04],
           [0.96, 0.04, 0.  ],
           [0.  , 1.  , 0.  ],
           [0.  , 1.  , 0.  ],
           [0.8 , 0.2 , 0.  ],
           [0.68, 0.28, 0.04],
           [0.  , 0.16, 0.84],
           [0.04, 0.96, 0.  ],
           [0.12, 0.88, 0.  ],
           [0.  , 0.32, 0.68],
           [0.  , 0.16, 0.84],
           [0.96, 0.04, 0.  ],
           [1.  , 0.  , 0.  ],
           [0.96, 0.04, 0.  ],
           [0.08, 0.  , 0.92],
           [0.96, 0.04, 0.  ],
           [0.84, 0.08, 0.08],
           [0.  , 0.4 , 0.6 ],
           [0.  , 1.  , 0.  ],
           [0.04, 0.96, 0.  ],
           [0.04, 0.28, 0.68],
           [0.  , 1.  , 0.  ],
           [0.  , 0.  , 1.  ],
           [0.08, 0.92, 0.  ],
           [0.88, 0.12, 0.  ],
           [0.  , 0.16, 0.84],
           [0.52, 0.48, 0.  ],
           [0.  , 0.88, 0.12],
           [0.  , 0.12, 0.88],
           [0.96, 0.04, 0.  ],
           [0.04, 0.88, 0.08],
           [0.  , 0.04, 0.96],
           [0.04, 0.28, 0.68],
           [0.04, 0.92, 0.04],
           [0.08, 0.12, 0.8 ],
           [0.68, 0.28, 0.04],
           [0.  , 0.88, 0.12],
           [0.04, 0.92, 0.04],
           [1.  , 0.  , 0.  ],
           [0.16, 0.24, 0.6 ],
           [0.64, 0.36, 0.  ],
           [0.  , 0.32, 0.68],
           [0.  , 0.  , 1.  ],
           [0.08, 0.92, 0.  ],
           [0.92, 0.08, 0.  ],
           [0.  , 0.  , 1.  ],
           [0.48, 0.48, 0.04],
           [1.  , 0.  , 0.  ],
           [1.  , 0.  , 0.  ],
           [0.8 , 0.2 , 0.  ],
           [0.16, 0.  , 0.84]])




```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
from sklearn.datasets import load_boston
from sklearn.impute import SimpleImputer#填补缺失值的类
from sklearn.ensemble import RandomForestRegressor#随机森林回归器
from sklearn.model_selection import cross_val_score#交叉验证
```


```python
boston = load_boston()
regressor = RandomForestRegressor(n_estimators=100,random_state=0)#实例化模型
cross_val_score(regressor,boston.data,boston.target,cv=10
                ,scoring="neg_mean_squared_error")
```




    array([-11.22504076,  -5.3945749 ,  -4.74755867, -22.54699078,
           -12.31243335, -17.18030718,  -6.94019868, -94.14567212,
           -28.541145  , -14.6250416 ])




```python
#sklearn当中的模型评估指标（打分）列表
import sklearn
sorted(sklearn.metrics.SCORERS.keys())
```




    ['accuracy',
     'adjusted_mutual_info_score',
     'adjusted_rand_score',
     'average_precision',
     'balanced_accuracy',
     'completeness_score',
     'explained_variance',
     'f1',
     'f1_macro',
     'f1_micro',
     'f1_samples',
     'f1_weighted',
     'fowlkes_mallows_score',
     'homogeneity_score',
     'jaccard',
     'jaccard_macro',
     'jaccard_micro',
     'jaccard_samples',
     'jaccard_weighted',
     'max_error',
     'mutual_info_score',
     'neg_brier_score',
     'neg_log_loss',
     'neg_mean_absolute_error',
     'neg_mean_absolute_percentage_error',
     'neg_mean_gamma_deviance',
     'neg_mean_poisson_deviance',
     'neg_mean_squared_error',
     'neg_mean_squared_log_error',
     'neg_median_absolute_error',
     'neg_root_mean_squared_error',
     'normalized_mutual_info_score',
     'precision',
     'precision_macro',
     'precision_micro',
     'precision_samples',
     'precision_weighted',
     'r2',
     'rand_score',
     'recall',
     'recall_macro',
     'recall_micro',
     'recall_samples',
     'recall_weighted',
     'roc_auc',
     'roc_auc_ovo',
     'roc_auc_ovo_weighted',
     'roc_auc_ovr',
     'roc_auc_ovr_weighted',
     'top_k_accuracy',
     'v_measure_score']




```python
boston.target#标签是连续型变量做回归
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
dataset = boston
dataset.data.shape
```




    (506, 13)




```python
X_full, y_full = dataset.data, dataset.target
n_samples = X_full.shape[0]#506
n_features = X_full.shape[1]#13
```


```python
#首先确定我们希望放入的缺失数据的比例，在这里我们假设是50%，那总共就要有3289个数据缺失
 
rng = np.random.RandomState(0)#设置一个随机种子，方便观察
missing_rate = 0.5
n_missing_samples = int(np.floor(n_samples * n_features * missing_rate))#3289
#np.floor向下取整，返回.0格式的浮点数
 
#所有数据要随机遍布在数据集的各行各列当中，而一个缺失的数据会需要一个行索引和一个列索引
#如果能够创造一个数组，包含3289个分布在0~506中间的行索引，和3289个分布在0~13之间的列索引，那我们就可以利用索引来为数据中的任意3289个位置赋空值
#然后我们用0，均值和随机森林来填写这些缺失值，然后查看回归的结果如何
 
missing_features = rng.randint(0,n_features,n_missing_samples)#randint（下限，上限，n）指在下限和上限之间取出n个整数
len(missing_features)#3289
missing_samples = rng.randint(0,n_samples,n_missing_samples)
len(missing_samples)#3289
 
#missing_samples = rng.choice(n_samples,n_missing_samples,replace=False)
#我们现在采样了3289个数据，远远超过我们的样本量506，所以我们使用随机抽取的函数randint。
# 但如果我们需要的数据量小于我们的样本量506，那我们可以采用np.random.choice来抽样，choice会随机抽取不重复的随机数，
# 因此可以帮助我们让数据更加分散，确保数据不会集中在一些行中!
#这里我们不采用np.random.choice,因为我们现在采样了3289个数据，远远超过我们的样本量506，使用np.random.choice会报错
 
X_missing = X_full.copy()
y_missing = y_full.copy()
 
X_missing[missing_samples,missing_features] = np.nan
 
```


```python
X_missing = pd.DataFrame(X_missing)
#转换成DataFrame是为了后续方便各种操作，numpy对矩阵的运算速度快到拯救人生，但是在索引等功能上却不如pandas来得好用
X_missing.head()

#并没有对y_missing进行缺失值填补，原因是有监督学习，不能缺标签啊
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
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>NaN</td>
      <td>18.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.538</td>
      <td>NaN</td>
      <td>65.2</td>
      <td>4.0900</td>
      <td>1.0</td>
      <td>296.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>4.98</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0.02731</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>0.469</td>
      <td>NaN</td>
      <td>78.9</td>
      <td>4.9671</td>
      <td>2.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>396.9</td>
      <td>9.14</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0.02729</td>
      <td>NaN</td>
      <td>7.07</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>7.185</td>
      <td>61.1</td>
      <td>NaN</td>
      <td>2.0</td>
      <td>242.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>0.458</td>
      <td>NaN</td>
      <td>45.8</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>222.0</td>
      <td>18.7</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>NaN</td>
      <td>0.0</td>
      <td>2.18</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>7.147</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>18.7</td>
      <td>NaN</td>
      <td>5.33</td>
    </tr>
  </tbody>
</table>
</div>




```python
#使用均值进行填补
from sklearn.impute import SimpleImputer
imp_mean = SimpleImputer(missing_values=np.nan, strategy='mean')#实例化
X_missing_mean = imp_mean.fit_transform(X_missing)#特殊的接口fit_transform = 训练fit + 导出predict
#pd.DataFrame(X_missing_mean).isnull()#但是数据量大的时候还是看不全
#布尔值False = 0， True = 1 
# pd.DataFrame(X_missing_mean).isnull().sum()#如果求和为0可以彻底确认是否有NaN
pd.DataFrame(X_missing_mean).isnull().sum()
```




    0     0
    1     0
    2     0
    3     0
    4     0
    5     0
    6     0
    7     0
    8     0
    9     0
    10    0
    11    0
    12    0
    dtype: int64




```python
#使用0进行填补
imp_0 = SimpleImputer(missing_values=np.nan, strategy="constant",fill_value=0)#constant指的是常数
X_missing_0 = imp_0.fit_transform(X_missing)
pd.DataFrame(X_missing_0).isnull().sum()
```




    0     0
    1     0
    2     0
    3     0
    4     0
    5     0
    6     0
    7     0
    8     0
    9     0
    10    0
    11    0
    12    0
    dtype: int64




```python
X_missing_reg = X_missing.copy()

#找出数据集中，缺失值从小到大排列的特征们的顺序，并且有了这些的索引
sortindex = np.argsort(X_missing_reg.isnull().sum(axis=0)).values#np.argsort()返回的是从小到大排序的顺序所对应的索引
 
for i in sortindex:
    
    #构建我们的新特征矩阵（没有被选中去填充的特征 + 原始的标签）和新标签（被选中去填充的特征）
    df = X_missing_reg.copy()
    fillc = df.iloc[:,i]#新标签
    #所有列除了选中的列取出来构建新特征矩阵
    df = pd.concat([df.iloc[:,df.columns != i],pd.DataFrame(y_full)],axis=1)#新特征矩阵
    
    #在新特征矩阵中，对含有缺失值的列，进行0的填补
    df_0 =SimpleImputer(missing_values=np.nan,strategy='constant',fill_value=0).fit_transform(df)
                        
    #找出我们的训练集和测试集
    Ytrain = fillc[fillc.notnull()]# Ytrain是被选中要填充的特征中（现在是我们的标签），存在的那些值：非空值
    Ytest = fillc[fillc.isnull()]#Ytest 是被选中要填充的特征中（现在是我们的标签），不存在的那些值：空值。注意我们需要的不是Ytest的值，需要的是Ytest所带的索引
    Xtrain = df_0[Ytrain.index,:]#在新特征矩阵上，被选出来的要填充的特征的非空值所对应的记录
    Xtest = df_0[Ytest.index,:]#在新特征矩阵上，被选出来的要填充的特征的空值所对应的记录
    
    #用随机森林回归来填补缺失值
    rfc = RandomForestRegressor(n_estimators=100)#实例化
    rfc = rfc.fit(Xtrain, Ytrain)#导入训练集进行训练
    Ypredict = rfc.predict(Xtest)#用predict接口将Xtest导入，得到我们的预测结果（回归结果），就是我们要用来填补空值的这些值
    
    #将填补好的特征返回到我们的原始的特征矩阵中
    X_missing_reg.loc[X_missing_reg.iloc[:,i].isnull(),i] = Ypredict

#检验是否有空值
X_missing_reg.isnull().sum()
```




    0     0
    1     0
    2     0
    3     0
    4     0
    5     0
    6     0
    7     0
    8     0
    9     0
    10    0
    11    0
    12    0
    dtype: int64




```python
#对所有数据进行建模，取得MSE结果
 
X = [X_full,X_missing_mean,X_missing_0,X_missing_reg]
 
mse = []
std = []
for x in X:
    estimator = RandomForestRegressor(random_state=0, n_estimators=100)#实例化
    scores = cross_val_score(estimator,x,y_full,scoring='neg_mean_squared_error', cv=5).mean()
    mse.append(scores * -1)
```


```python
[*zip(['Full data','Zero Imputation','Mean Imputation','Regressor Imputation'],mse)]
```




    [('Full data', 21.571667100368845),
     ('Zero Imputation', 40.848037216676374),
     ('Mean Imputation', 49.626793201980185),
     ('Regressor Imputation', 17.66041418029508)]




```python
x_labels = ['Full data',
            'Zero Imputation',
            'Mean Imputation',
            'Regressor Imputation']
colors = ['r', 'g', 'b', 'orange']
 
plt.figure(figsize=(12, 6))#画出画布
ax = plt.subplot(111)#添加子图
for i in np.arange(len(mse)):#range(len(mse))
    ax.barh(i, mse[i],color=colors[i], alpha=0.6, align='center')
    #bar为条形图，barh为横向条形图，alpha表示条的透明度
ax.set_title('Imputation Techniques with Boston Data')
ax.set_xlim(left=np.min(mse) * 0.9,
             right=np.max(mse) * 1.1)#设置x轴取值范围
ax.set_yticks(np.arange(len(mse)))#刻度
ax.set_xlabel('MSE')
ax.set_yticklabels(x_labels)
plt.show()
```


    
![png](https://gitee.com/spiritlhl/picture/raw/master/output_36_0.png)
    


