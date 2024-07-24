# 决策树线性回归

## 代码


```python
import numpy as np
from sklearn.tree import DecisionTreeRegressor #回归树
import matplotlib.pyplot as plt
```


```python
#生成噪声曲线
rng = np.random.RandomState(1)
rng.rand(2,3)#生成区分行列的二维数据，一维数据不分行列，生成0~1之间的数
```




    array([[4.17022005e-01, 7.20324493e-01, 1.14374817e-04],
           [3.02332573e-01, 1.46755891e-01, 9.23385948e-02]])




```python
X = np.sort(5*rng.rand(80,1),axis=0)
y = np.sin(X).ravel()#导入二维数据，ravel降维y，可将任意维度降维为一维
plt.scatter(X,y,s=20,edgecolors="black",c="darkorange",label="data")
```




    <matplotlib.collections.PathCollection at 0x7f87d52f56a0>




    
![png](https://spiritlhl-tc.oss-cn-beijing.aliyuncs.com/output_3_1.png)
    



```python
y[::5] += 3 * (0.5 - rng.rand(16))#每5个数字加随机+-0.5之间的数当成噪声
plt.scatter(X,y,s=20,edgecolors="black",c="darkorange",label="data")
```




    <matplotlib.collections.PathCollection at 0x7f87d5113e50>




    
![png](https://spiritlhl-tc.oss-cn-beijing.aliyuncs.com/output_4_1.png)
    



```python
#设定参数的模型导入并训练
regr_1 = DecisionTreeRegressor(max_depth=2)
regr_2 = DecisionTreeRegressor(max_depth=5)
regr_1.fit(X,y)
regr_2.fit(X,y)
```




    DecisionTreeRegressor(max_depth=5)




```python
X_test = np.arange(0.0,5.0,0.01)[:,np.newaxis]
#取0~5以0.01为步长的有顺序点，后面将1维升维2，跟reshape（-1，1）作用一致
```


```python
y_1 = regr_1.predict(X_test)
y_2 = regr_2.predict(X_test)
```


```python
plt.figure()
plt.scatter(X,y,s=20,edgecolors="black",c="darkorange",label="data")#边框，点，图例名称
plt.plot(X_test,y_1,color="cornflowerblue",label="max_depth=2",linewidth=2)
plt.plot(X_test,y_2,color="yellowgreen",label="max_depth=5",linewidth=2)
plt.xlabel("data")
plt.ylabel("target")
plt.title("Decision Tree Regression")
plt.legend()#显示图例
plt.show()
```


    
![png](https://spiritlhl-tc.oss-cn-beijing.aliyuncs.com/output_8_0.png)
    


