# 决策树1-分类器

## 代码

```python
import numpy as np
import matplotlib.pyplot as plt
from sklearn import datasets
from sklearn.model_selection import train_test_split     #分割训练集和测试集
from sklearn.neighbors import KNeighborsClassifier       #K近邻
```

# 简单案例


```python
iris=datasets.load_iris()
iris_X = iris.data
iris_y=iris.target
```


```python
iris_X[:2,:]#四个属性两朵花
```




    array([[5.1, 3.5, 1.4, 0.2],
           [4.9, 3. , 1.4, 0.2]])




```python
iris_y#三类花
```




    array([0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
           0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
           0, 0, 0, 0, 0, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,
           1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,
           1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2,
           2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2,
           2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2])




```python
#分割训练集和测试集
X_train,X_test,y_train,y_test=train_test_split(iris_X,iris_y,test_size=0.3)#测试占30%
```


```python
y_train#顺便打乱了数据
```




    array([0, 0, 2, 1, 0, 0, 2, 0, 0, 2, 0, 2, 1, 1, 2, 1, 2, 2, 2, 1, 2, 1,
           1, 0, 0, 2, 1, 1, 2, 1, 2, 1, 0, 2, 2, 0, 1, 1, 1, 0, 2, 0, 1, 0,
           1, 2, 2, 1, 0, 1, 2, 1, 2, 0, 2, 1, 2, 1, 1, 0, 0, 2, 2, 0, 1, 2,
           1, 0, 0, 0, 2, 0, 0, 1, 2, 2, 2, 1, 2, 1, 0, 1, 1, 0, 1, 2, 0, 2,
           1, 2, 0, 1, 0, 0, 0, 1, 1, 0, 0, 2, 2, 2, 0, 2, 0])




```python
knn = KNeighborsClassifier() #使用K近邻分类
knn.fit(X_train,y_train)     #输入测试集
```




    KNeighborsClassifier()




```python
knn.predict(X_test)#预测
```




    array([2, 2, 2, 1, 0, 2, 0, 2, 2, 0, 0, 0, 1, 0, 0, 2, 1, 0, 1, 1, 0, 2,
           2, 1, 1, 1, 0, 1, 0, 2, 2, 2, 2, 0, 1, 0, 1, 2, 2, 0, 1, 0, 1, 1,
           1])




```python
y_test
```




    array([2, 2, 2, 1, 0, 1, 0, 2, 2, 0, 0, 0, 2, 0, 0, 2, 1, 0, 1, 1, 0, 2,
           2, 1, 1, 1, 0, 1, 0, 2, 1, 2, 2, 0, 1, 0, 1, 2, 2, 0, 1, 0, 1, 1,
           1])



# 模型导入训练


```python
from sklearn import datasets #导入基础数据
from sklearn.linear_model import LinearRegression  #导入模型
```


```python
#导入数据
loaded_data = datasets.load_boston()
data_X = loaded_data.data
data_y = loaded_data.target
```


```python
#定义模型并训练
model = LinearRegression()#线性模型
model.fit(data_X,data_y)
```




    LinearRegression()




```python
#预测
model.predict(data_X[:4,:])
```




    array([30.00384338, 25.02556238, 30.56759672, 28.60703649])




```python
data_y[:4]
```




    array([24. , 21.6, 34.7, 33.4])



import matplotlib.pyplot as plt

#创造数据

X,y = datasets.make_regression(n_samples=100,n_features=1,n_targets=1,noise=1)

#noise 离散程度

plt.scatter(X,y)#画点


```python
#y=0.1x+0.3
#斜率 截距
```


```python
model.coef_
```




    array([-1.08011358e-01,  4.64204584e-02,  2.05586264e-02,  2.68673382e+00,
           -1.77666112e+01,  3.80986521e+00,  6.92224640e-04, -1.47556685e+00,
            3.06049479e-01, -1.23345939e-02, -9.52747232e-01,  9.31168327e-03,
           -5.24758378e-01])




```python
model.intercept_
```




    36.459488385090125




```python
#输出之前模型定义的参数
model.get_params()
```




    {'copy_X': True,
     'fit_intercept': True,
     'n_jobs': None,
     'normalize': False,
     'positive': False}




```python
#打分评价
model.score(data_X,data_y)#R^2 coffiction of determination
```




    0.7406426641094095



# 归一化


```python
from sklearn import preprocessing
import numpy as np
```


```python
a = np.array([[10,2.7,3.6],
             [-100,5,-2],
             [120,20,40]],dtype=np.float64)
a
```




    array([[  10. ,    2.7,    3.6],
           [-100. ,    5. ,   -2. ],
           [ 120. ,   20. ,   40. ]])




```python
#归一化
preprocessing.scale(a)
```




    array([[ 0.        , -0.85170713, -0.55138018],
           [-1.22474487, -0.55187146, -0.852133  ],
           [ 1.22474487,  1.40357859,  1.40351318]])




```python
#生成可以标准化的数据
from sklearn.datasets import make_classification
```


```python
#支持向量机的分类器
from sklearn.svm import SVC
```


```python
X,y = datasets.make_classification(n_samples=300,n_features=2,n_redundant=0,\
                                   n_informative=2,random_state=22,\
                                   n_clusters_per_class=1,\
                                   scale=100)
```


```python
plt.scatter(X[:,0],X[:,1],c=y)
```




    <matplotlib.collections.PathCollection at 0x7f1842f7faf0>




    
![png](https://raw.githubusercontent.com/spiritLHL/tuchuang/master/output_31_1.png)
    



```python
X_train,X_test,y_train,y_test=train_test_split(X,y,test_size=0.3)
clf = SVC()
clf.fit(X_train,y_train)
clf.score(X_test,y_test)
```




    0.9333333333333333




```python
#归一化
#X=preprocessing.scale(X,r)
#指定范围归一化
X=preprocessing.minmax_scale(X,feature_range=(-1,1))
#归一化后
X_train,X_test,y_train,y_test=train_test_split(X,y,test_size=0.3)
clf = SVC()
clf.fit(X_train,y_train)
clf.score(X_test,y_test)
```




    0.9555555555555556



# 交叉验证


```python
# cross_validation 修改为 model_selection
```


```python
from sklearn.datasets import load_iris
from sklearn.model_selection import train_test_split     #分割训练集和测试集
from sklearn.neighbors import KNeighborsClassifier       #K近邻
```


```python
iris = load_iris()
X=iris.data
y=iris.target
```


```python
X_train,X_test,y_train,y_test=train_test_split(X,y,random_state=4)#随机数复现
knn=KNeighborsClassifier(n_neighbors=5)#通过多少近邻预测
knn.fit(X_test,y_test)
y_pred = knn.predict(X_test)
knn.score(X_test,y_test)
```




    0.9736842105263158




```python
from sklearn.model_selection import cross_val_score
knn = KNeighborsClassifier(n_neighbors=5)#按照这个看邻近个数
scores = cross_val_score(knn,X,y,cv=5,scoring='accuracy')#按准确度分5次
scores.mean()
```




    0.9733333333333334




```python
k_range = range(1,31)
k_scores = []
for k in k_range:
    knn = KNeighborsClassifier(n_neighbors=k)#按照这个看邻近个数
    scores = cross_val_score(knn,X,y,cv=10,scoring='accuracy')#按准确度分10次
    #loss = -cross_val_score(knn,X,y,cv=10,scoring='neg_mean_squared_error')#误差，线性
    #scores = cross_val_score(knn,X,y,cv=10,scoring='accuracy')#准确度
    #k_scores.append(loss.mean())#越小越好
    k_scores.append(scores.mean())#越大越好
```


```python
#选取K
plt.plot(k_range,k_scores)
plt.xlabel('Value of K for KNN')
plt.ylabel('Cross-Validated Accurancy')
plt.show()
```


    
![png](https://raw.githubusercontent.com/spiritLHL/tuchuang/master/output_41_0.png)
    


# 处理过拟合


```python
from sklearn.model_selection import learning_curve
from sklearn.datasets import load_digits #数字库
from sklearn.svm import SVC
import matplotlib.pyplot as plt
import numpy as np
```


```python
digits = load_digits()
X = digits.data
y = digits.target
```


```python
train_sizes,train_loss,test_loss=learning_curve(SVC(gamma=0.001),X,y,cv=10,
                                               scoring='neg_mean_squared_error',
                                               train_sizes=[0.1,0.25,0.5,0.75,1]
                                               )
#train_sizes记录评分点 
train_loss_mean = -np.mean(train_loss,axis=1)
test_loss_mean = -np.mean(test_loss,axis=1)
```


```python
plt.plot(train_sizes,train_loss_mean,'o-',color="r",label="Training")
plt.plot(train_sizes,test_loss_mean,'o-',color="g",label="Cross-validation")
plt.xlabel("Training example")
plt.ylabel("Loss")
plt.legend(loc="best")
plt.show
```




    <function matplotlib.pyplot.show(close=None, block=None)>




    
![png](https://raw.githubusercontent.com/spiritLHL/tuchuang/master/output_46_1.png)
    



```python
from sklearn.model_selection import validation_curve
from sklearn.datasets import load_digits #数字库
from sklearn.svm import SVC
import matplotlib.pyplot as plt
import numpy as np
digits = load_digits()
X = digits.data
y = digits.target
param_range = np.logspace(-6,-2.3,5)#定义SVC的伽马范围
train_loss,test_loss=validation_curve(SVC(),X,y,param_name='gamma',
                                      param_range=param_range,cv=10,
                                      scoring='neg_mean_squared_error'
                                      )
#train_sizes记录评分点 
train_loss_mean = -np.mean(train_loss,axis=1)
test_loss_mean = -np.mean(test_loss,axis=1)
plt.plot(param_range,train_loss_mean,'o-',color="r",label="Training")
plt.plot(param_range,test_loss_mean,'o-',color="g",label="Cross-validation")
#验证曲线 选取合适gamma值 学习率
plt.xlabel("gamma")
plt.ylabel("Loss")
plt.legend(loc="best")
plt.show
```




    <function matplotlib.pyplot.show(close=None, block=None)>




    
![png](https://raw.githubusercontent.com/spiritLHL/tuchuang/master/output_47_1.png)
    


# 保存model


```python
from sklearn import svm
from sklearn import datasets
clf = svm.SVC()
iris = datasets.load_iris()
X,y = iris.data,iris.target
clf.fit(X,y)
```




    SVC()




```python
#method pickle
#import _pickle as cPickle
import pickle
with open('SAVE/clf.pickle','wb') as f:
    pickle.dump(clf,f)
```


```python
with open('SAVE/clf.pickle','rb') as f:
    clf2 = pickle.load(f)
```


```python
clf2.predict(X[0:1])
```




    array([0])




```python
# method joblib
import joblib
#Save
joblib.dump(clf,'SAVE/clf.pkl')
```




    ['SAVE/clf.pkl']




```python
clf3 = joblib.load('SAVE/clf.pkl')
clf3.predict(X[0:1])
```




    array([0])




```python

```

