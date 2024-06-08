# Pandas (下)

## 前言

## 本篇鸣谢 马川-燕大 的增删整理， 王圣元 ——[原创文章](https://mp.weixin.qq.com/s/Fo-UIGnsoU2nBVLxSw_nVw)，与原文不同之处包含我的学习记录。

匹配Jupyter Notebook的ipynb文档链接下载地址在资源页面里

接着上篇继续后面三个章节

**提纲**

![](https://pic./2020/06/09/1394ec0b9f88c.png)

## 4 数据表的合并和连接

数据表可以按<font color="red"><b>「键」</b></font>合并，用 merge 函数；可以按<font color="red"><b>「轴」</b></font>来连接，用 concat 函数。

### 4.1 合并

合并用 merge 函数，语法如下：

>pd.merge( df1, df2, how=s, on=c )

c 是 df1 和 df2 共有的一栏，合并方式 (how=s) 有四种：

1. 左连接 (left join)：合并之后显示 df1 的所有行

2. 右连接 (right join)：合并之后显示 df2 的所有行

3. 外连接 (outer join)：合并所有行

4. 内连接 (inner join)：合并df1 和 df2 共有的所有行 (默认情况)

首先创建两个 DataFrame：

* df_price：4 天的价格 (2019-01-01 到 2019-01-04)

* df_volume：5 天的交易量  (2019-01-02 到 2019-01-06)


```python
import pandas as pd

df_price = pd.DataFrame( {'Date': pd.date_range('2019-1-1', periods=4),
                          'Adj Close': [24.42, 25.00, 25.25, 25.64]})
df_price
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
      <th>Date</th>
      <th>Adj Close</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2019-01-01</td>
      <td>24.42</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2019-01-02</td>
      <td>25.00</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2019-01-03</td>
      <td>25.25</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2019-01-04</td>
      <td>25.64</td>
    </tr>
  </tbody>
</table>
</div>




```python
df_volume = pd.DataFrame( {'Date': pd.date_range('2019-1-2', periods=5),
                           'Volume' : [56081400, 99455500, 83028700, 100234000, 73829000]})
df_volume
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
      <th>Date</th>
      <th>Volume</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2019-01-02</td>
      <td>56081400</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2019-01-03</td>
      <td>99455500</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2019-01-04</td>
      <td>83028700</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2019-01-05</td>
      <td>100234000</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2019-01-06</td>
      <td>73829000</td>
    </tr>
  </tbody>
</table>
</div>



接下来用 df_price  和 df_volume 展示四种合并。

<font color="red"><b>left join</b></font>


```python
pd.merge( df_price, df_volume, how='left' )
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
      <th>Date</th>
      <th>Adj Close</th>
      <th>Volume</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2019-01-01</td>
      <td>24.42</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2019-01-02</td>
      <td>25.00</td>
      <td>56081400.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2019-01-03</td>
      <td>25.25</td>
      <td>99455500.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2019-01-04</td>
      <td>25.64</td>
      <td>83028700.0</td>
    </tr>
  </tbody>
</table>
</div>



按 **df_price** 里 Date 栏里的值来合并数据

* df_volume 里 Date 栏里没有 2019-01-01，因此 Volume 为 NaN

* df_volume 里 Date 栏里的 2019-01-05 和 2019-01-06 不在 df_price 里 Date 栏，因此丢弃

<font color="red"><b>right join</b></font>


```python
pd.merge( df_price, df_volume, how='right' )
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
      <th>Date</th>
      <th>Adj Close</th>
      <th>Volume</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2019-01-02</td>
      <td>25.00</td>
      <td>56081400</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2019-01-03</td>
      <td>25.25</td>
      <td>99455500</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2019-01-04</td>
      <td>25.64</td>
      <td>83028700</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2019-01-05</td>
      <td>NaN</td>
      <td>100234000</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2019-01-06</td>
      <td>NaN</td>
      <td>73829000</td>
    </tr>
  </tbody>
</table>
</div>



按 **df_volume** 里 Date 栏里的值来合并数据

* df_price 里 Date 栏里没有 2019-01-05 和 2019-01-06，因此 Adj Close 为 NaN

* df_price 里 Date 栏里的 2019-01-01 不在 df_volume 里 Date 栏，因此丢弃

<font color="red"><b>outer join</b></font>


```python
pd.merge( df_price, df_volume, how='outer' )
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
      <th>Date</th>
      <th>Adj Close</th>
      <th>Volume</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2019-01-01</td>
      <td>24.42</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2019-01-02</td>
      <td>25.00</td>
      <td>56081400.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2019-01-03</td>
      <td>25.25</td>
      <td>99455500.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2019-01-04</td>
      <td>25.64</td>
      <td>83028700.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2019-01-05</td>
      <td>NaN</td>
      <td>100234000.0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>2019-01-06</td>
      <td>NaN</td>
      <td>73829000.0</td>
    </tr>
  </tbody>
</table>
</div>



按 df_price 和 df_volume 里 Date 栏里的**所有值**来合并数据

* df_price 里 Date 栏里没有 2019-01-05 和 2019-01-06，因此 Adj Close 为 NaN

* df_volume 里 Date 栏里没有 2019-01-01，因此 Volume 为 NaN

<font color="red"><b>inner join</b></font>


```python
pd.merge( df_price, df_volume, how='inner' )
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
      <th>Date</th>
      <th>Adj Close</th>
      <th>Volume</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2019-01-02</td>
      <td>25.00</td>
      <td>56081400</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2019-01-03</td>
      <td>25.25</td>
      <td>99455500</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2019-01-04</td>
      <td>25.64</td>
      <td>83028700</td>
    </tr>
  </tbody>
</table>
</div>



按 df_price 和 df_volume 里 Date 栏里的**共有值**来合并数据

* df_price 里 Date 栏里的 2019-01-01 不在 df_volume 里 Date 栏，因此丢弃

* df_volume 里 Date 栏里的 2019-01-05 和 2019-01-06 不在 df_price 里 Date 栏，因此丢弃

### 4.2 连接

Numpy 数组可相互连接，用 np.concat；同理，Series 和 DataFrame 也可相互连接，用 pd.concat。

#### 连接 Series

在 concat 函数也可设定参数 axis，

* axis = 0 (默认)，沿着轴 0 (行) 连接，得到一个更长的 Series

* axis = 1，沿着轴 1 (列) 连接，得到一个 DataFrame

被连接的 Series 它们的 index 可以重复 (overlapping)，也可以不同。

**<font color="red">non-overlapping index</font>**

先定义三个 Series，它们的 index 各不同。


```python
s1 = pd.Series([0, 1], index=['a', 'b'])
s2 = pd.Series([2, 3, 4], index=['c', 'd', 'e'])
s3 = pd.Series([5, 6], index=['f', 'g'])
print(s1)
print(s2)
print(s3)
```

    a    0
    b    1
    dtype: int64
    c    2
    d    3
    e    4
    dtype: int64
    f    5
    g    6
    dtype: int64
    

沿着「轴 0」连接得到一个更长的 Series。


```python
pd.concat([s1, s2, s3])
```




    a    0
    b    1
    c    2
    d    3
    e    4
    f    5
    g    6
    dtype: int64



沿着「轴 1」连接得到一个 DataFrame。


```python
pd.concat([s1, s2, s3], axis=1)
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
      <th>a</th>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>b</th>
      <td>1.0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>c</th>
      <td>NaN</td>
      <td>2.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>d</th>
      <td>NaN</td>
      <td>3.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>e</th>
      <td>NaN</td>
      <td>4.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>f</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>5.0</td>
    </tr>
    <tr>
      <th>g</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>6.0</td>
    </tr>
  </tbody>
</table>
</div>



<font color="red"><b>overlapping index</b></font>

将 s1 和 s3 沿「轴 0」连接来创建 s4，这样 s4 和 s1 的 index 是有重复的。


```python
s4 = pd.concat([s1, s3])
print(s1)
print(s4)
```

    a    0
    b    1
    dtype: int64
    a    0
    b    1
    f    5
    g    6
    dtype: int64
    

将 s1 和 s4 沿「轴 1」内连接 (即只连接它们共有 index 对应的值)


```python
pd.concat([s1, s4], axis = 1, join='inner')
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
      <th>a</th>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>b</th>
      <td>1</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>



<font color="red"><b>hierarchical index</b></font>

最后还可以将 n 个 Series 沿「轴 0」连接起来，再赋予 3 个 keys 创建多层 Series。


```python
pd.concat( [s1, s1, s3], 
           keys=['one','two','three'])
```




    one    a    0
           b    1
    two    a    0
           b    1
    three  f    5
           g    6
    dtype: int64



#### 连接 DataFrame

连接 DataFrame 的逻辑和连接 Series 的一模一样。

<font color="red"><b>沿着行连接 (axis = 0)</b></font>

先创建两个 DataFrame，df1 和 df2。


```python
import numpy as np

df1 = pd.DataFrame( np.arange(12).reshape(3,4), 
                    columns=['a','b','c','d'])
df1
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
      <th>a</th>
      <th>b</th>
      <th>c</th>
      <th>d</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>1</td>
      <td>2</td>
      <td>3</td>
    </tr>
    <tr>
      <th>1</th>
      <td>4</td>
      <td>5</td>
      <td>6</td>
      <td>7</td>
    </tr>
    <tr>
      <th>2</th>
      <td>8</td>
      <td>9</td>
      <td>10</td>
      <td>11</td>
    </tr>
  </tbody>
</table>
</div>




```python
df2 = pd.DataFrame( np.arange(6).reshape(2,3),
                    columns=['b','d','a'])
df2
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
      <th>b</th>
      <th>d</th>
      <th>a</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>1</td>
      <td>2</td>
    </tr>
    <tr>
      <th>1</th>
      <td>3</td>
      <td>4</td>
      <td>5</td>
    </tr>
  </tbody>
</table>
</div>



沿着行连接分两步

1. 先把 df1 和 df2 列标签补齐

2. 再把 df1 和 df2 纵向连起来


```python
pd.concat( [df1, df2] )
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
      <th>a</th>
      <th>b</th>
      <th>c</th>
      <th>d</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>1</td>
      <td>2.0</td>
      <td>3</td>
    </tr>
    <tr>
      <th>1</th>
      <td>4</td>
      <td>5</td>
      <td>6.0</td>
      <td>7</td>
    </tr>
    <tr>
      <th>2</th>
      <td>8</td>
      <td>9</td>
      <td>10.0</td>
      <td>11</td>
    </tr>
    <tr>
      <th>0</th>
      <td>2</td>
      <td>0</td>
      <td>NaN</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>5</td>
      <td>3</td>
      <td>NaN</td>
      <td>4</td>
    </tr>
  </tbody>
</table>
</div>



得到的 DataFrame 的 index = [0,1,2,0,1]，有重复值。如果 index 不包含重要信息 (如上例)，可以将 ignore_index 设置为 True，这样就得到默认的 index 值了。


```python
pd.concat( [df1, df2], ignore_index=True )
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
      <th>a</th>
      <th>b</th>
      <th>c</th>
      <th>d</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>1</td>
      <td>2.0</td>
      <td>3</td>
    </tr>
    <tr>
      <th>1</th>
      <td>4</td>
      <td>5</td>
      <td>6.0</td>
      <td>7</td>
    </tr>
    <tr>
      <th>2</th>
      <td>8</td>
      <td>9</td>
      <td>10.0</td>
      <td>11</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2</td>
      <td>0</td>
      <td>NaN</td>
      <td>1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>3</td>
      <td>NaN</td>
      <td>4</td>
    </tr>
  </tbody>
</table>
</div>



<font color="red"><b>沿着列连接 (axis = 1)</b></font>

先创建两个 DataFrame，df1 和 df2。


```python
df1 = pd.DataFrame( np.arange(6).reshape(3,2), 
                    index=['a','b','c'],
                    columns=['one','two'] )
df1
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
      <th>one</th>
      <th>two</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>a</th>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>b</th>
      <td>2</td>
      <td>3</td>
    </tr>
    <tr>
      <th>c</th>
      <td>4</td>
      <td>5</td>
    </tr>
  </tbody>
</table>
</div>




```python
df2 = pd.DataFrame( 5 + np.arange(4).reshape(2,2), 
                    index=['a','c'], 
                    columns=['three','four'])
df2
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
      <th>three</th>
      <th>four</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>a</th>
      <td>5</td>
      <td>6</td>
    </tr>
    <tr>
      <th>c</th>
      <td>7</td>
      <td>8</td>
    </tr>
  </tbody>
</table>
</div>



沿着列连接分两步

1. 先把 df1 和 df2 行标签补齐

2. 再把 df1 和 df2 横向连起来


```python
pd.concat( [df1, df2], axis=1 )
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
      <th>one</th>
      <th>two</th>
      <th>three</th>
      <th>four</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>a</th>
      <td>0</td>
      <td>1</td>
      <td>5.0</td>
      <td>6.0</td>
    </tr>
    <tr>
      <th>b</th>
      <td>2</td>
      <td>3</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>c</th>
      <td>4</td>
      <td>5</td>
      <td>7.0</td>
      <td>8.0</td>
    </tr>
  </tbody>
</table>
</div>



## 5 数据表的重塑和透视

有许多用于重新排列表格型数据的基础运算。这些函数也称作重塑（reshape）或轴向旋转（pivot）运算。

重塑 (reshape) 和透视 (pivot) 两个操作只改变数据表的布局 (layout)：

* 重塑用 stack 和 unstack 函数 (互为逆转操作)

* 透视用 pivot 和 melt 函数 (互为逆转操作)



### 5.1 重塑

重塑就是通过改变数据表里面的「行索引」和「列索引」来改变展示形式，从本质上说，就是重塑层次化索引(多层索引)。

**行列旋转**

* 列索引 → 行索引，用 stack 函数

* 行索引 → 列索引，用 unstack 函数

#### 单层 DataFrame

创建 DataFrame df (1 层行索引，1 层列索引)


```python
symbol = ['JD', 'AAPL']
data = {'行业': ['电商', '科技'],
        '价格': [25.95, 172.97],
        '交易量': [27113291, 18913154]}
df = pd.DataFrame( data, index=symbol )
df.columns.name = '特征'
df.index.name = '代号'
df
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
      <th>特征</th>
      <th>行业</th>
      <th>价格</th>
      <th>交易量</th>
    </tr>
    <tr>
      <th>代号</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>JD</th>
      <td>电商</td>
      <td>25.95</td>
      <td>27113291</td>
    </tr>
    <tr>
      <th>AAPL</th>
      <td>科技</td>
      <td>172.97</td>
      <td>18913154</td>
    </tr>
  </tbody>
</table>
</div>



从上表中可知：

* 行索引 = [JD, AAPL]，名称是代号

* 列索引 = [行业, 价格, 交易量]，名称是特征

<font color="red"><b>stack: 列索引 → 行索引</b></font>

列索引 (特征) 变成了行索引，原来的 DataFrame df 变成了两层 Series (第一层索引是代号，第二层索引是特征)。

![](https://pic./2020/06/09/ec1783aa4454e.png)


```python
c2i_Series = df.stack()
c2i_Series
```




    代号    特征 
    JD    行业           电商
          价格        25.95
          交易量    27113291
    AAPL  行业           科技
          价格       172.97
          交易量    18913154
    dtype: object



<font color="red"><b>unstack: 行索引 → 列索引</b></font>

行索引 (代号) 变成了列索引，原来的 DataFrame df 也变成了两层 Series (第一层索引是特征，第二层索引是代号)。

![](https://pic./2020/06/09/606409f8b33d4.png)


```python
i2c_Series = df.unstack()
i2c_Series
```




    特征   代号  
    行业   JD            电商
         AAPL          科技
    价格   JD         25.95
         AAPL      172.97
    交易量  JD      27113291
         AAPL    18913154
    dtype: object



<font color="red"><b>基于层和名称来 unstack</b></font>

对于多层索引的 Series，unstack 哪一层有两种方法来确定：

1. 基于层 (level-based)

2. 基于名称 (name-based)

拿 c2i_Series 举例 (读者也可以尝试 i2c_Series)：

```
代号    特征 
JD    交易量    27113291
      价格        25.95
      行业           电商
AAPL  交易量    18913154
      价格       172.97
      行业           科技
dtype: object
```

其索引列出如下：


```python
c2i_Series.index
```




    MultiIndex([(  'JD',  '行业'),
                (  'JD',  '价格'),
                (  'JD', '交易量'),
                ('AAPL',  '行业'),
                ('AAPL',  '价格'),
                ('AAPL', '交易量')],
               names=['代号', '特征'])



**1.基于层来 unstack() 时，没有填层数，默认为最后一层。**


```python
c2i_Series.unstack()
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
      <th>特征</th>
      <th>行业</th>
      <th>价格</th>
      <th>交易量</th>
    </tr>
    <tr>
      <th>代号</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>JD</th>
      <td>电商</td>
      <td>25.95</td>
      <td>27113291</td>
    </tr>
    <tr>
      <th>AAPL</th>
      <td>科技</td>
      <td>172.97</td>
      <td>18913154</td>
    </tr>
  </tbody>
</table>
</div>



c2i_Series 的最后一层 (看上面它的 MultiIndex) 就是 [交易量, 价格,行业 ]，从行索引转成列索引得到上面的 DataFrame。

**2.基于层来 unstack() 时，选择第一层 (参数放 0)**


```python
c2i_Series.unstack(0)
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
      <th>代号</th>
      <th>JD</th>
      <th>AAPL</th>
    </tr>
    <tr>
      <th>特征</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>行业</th>
      <td>电商</td>
      <td>科技</td>
    </tr>
    <tr>
      <th>价格</th>
      <td>25.95</td>
      <td>172.97</td>
    </tr>
    <tr>
      <th>交易量</th>
      <td>27113291</td>
      <td>18913154</td>
    </tr>
  </tbody>
</table>
</div>



c2i_Series 的第一层 (看上面它的 MultiIndex) 就是 [JD, AAPL]，从行索引转成列索引得到上面的 DataFrame。

**3.基于名称来 unstack** 


```python
c2i_Series.unstack('代号')
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
      <th>代号</th>
      <th>JD</th>
      <th>AAPL</th>
    </tr>
    <tr>
      <th>特征</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>行业</th>
      <td>电商</td>
      <td>科技</td>
    </tr>
    <tr>
      <th>价格</th>
      <td>25.95</td>
      <td>172.97</td>
    </tr>
    <tr>
      <th>交易量</th>
      <td>27113291</td>
      <td>18913154</td>
    </tr>
  </tbody>
</table>
</div>



c2i_Series 的代号层 (看上面它的 MultiIndex) 就是 [JD, AAPL]，从行索引转成列索引得到上面的 DataFrame。

#### 多层 DataFrame

创建 DataFrame df (2 层行索引，1 层列索引)


```python
data = [ ['电商', 101550, 176.92], 
         ['电商', 175336, 25.95], 
         ['金融', 60348, 41.79], 
         ['金融', 36600, 196.00] ]

midx = pd.MultiIndex( levels=[['中国','美国'],
                              ['BABA', 'JD', 'GS', 'MS']], 
                      codes=[[0,0,1,1],[0,1,2,3]],
                      names = ['地区', '代号'])

mcol = pd.Index(['行业','雇员','价格'], name='特征')

df = pd.DataFrame( data, index=midx, columns=mcol )
df
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
      <th>特征</th>
      <th>行业</th>
      <th>雇员</th>
      <th>价格</th>
    </tr>
    <tr>
      <th>地区</th>
      <th>代号</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="2" valign="top">中国</th>
      <th>BABA</th>
      <td>电商</td>
      <td>101550</td>
      <td>176.92</td>
    </tr>
    <tr>
      <th>JD</th>
      <td>电商</td>
      <td>175336</td>
      <td>25.95</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">美国</th>
      <th>GS</th>
      <td>金融</td>
      <td>60348</td>
      <td>41.79</td>
    </tr>
    <tr>
      <th>MS</th>
      <td>金融</td>
      <td>36600</td>
      <td>196.00</td>
    </tr>
  </tbody>
</table>
</div>



从上表中可知：

* 行索引第一层： r1 = [中国, 美国]，名称是地区

* 行索引第二层： r2 = [BABA, JD, GS, MS]，名称是代号

* 列索引： c = [行业, 雇员, 价格]，名称是特征

查看 df 的 index 和 columns 的信息


```python
df.index, df.columns
```




    (MultiIndex([('中国', 'BABA'),
                 ('中国',   'JD'),
                 ('美国',   'GS'),
                 ('美国',   'MS')],
                names=['地区', '代号']),
     Index(['行业', '雇员', '价格'], dtype='object', name='特征'))



那么

* df 的行索引 = [r1, r2]

* df 的列索引 = c

**1.基于层来 unstack() 时，选择第一层 (参数放 0)**


```python
df.unstack(0)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead tr th {
        text-align: left;
    }

    .dataframe thead tr:last-of-type th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th>特征</th>
      <th colspan="2" halign="left">行业</th>
      <th colspan="2" halign="left">雇员</th>
      <th colspan="2" halign="left">价格</th>
    </tr>
    <tr>
      <th>地区</th>
      <th>中国</th>
      <th>美国</th>
      <th>中国</th>
      <th>美国</th>
      <th>中国</th>
      <th>美国</th>
    </tr>
    <tr>
      <th>代号</th>
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
      <th>BABA</th>
      <td>电商</td>
      <td>NaN</td>
      <td>101550.0</td>
      <td>NaN</td>
      <td>176.92</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>JD</th>
      <td>电商</td>
      <td>NaN</td>
      <td>175336.0</td>
      <td>NaN</td>
      <td>25.95</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>GS</th>
      <td>NaN</td>
      <td>金融</td>
      <td>NaN</td>
      <td>60348.0</td>
      <td>NaN</td>
      <td>41.79</td>
    </tr>
    <tr>
      <th>MS</th>
      <td>NaN</td>
      <td>金融</td>
      <td>NaN</td>
      <td>36600.0</td>
      <td>NaN</td>
      <td>196.00</td>
    </tr>
  </tbody>
</table>
</div>



df 被 unstack(0) 之后变成 (行 → 列)

* 行索引 = r2

* 列索引 = [c, r1]

重塑后的 DataFrame 这时行索引只有一层 (代号)，而列索引有两层，第一层是特征，第二层是地区。

**2.基于层来 unstack() 时，选择第二层 (参数放 1)**


```python
df.unstack(1)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead tr th {
        text-align: left;
    }

    .dataframe thead tr:last-of-type th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th>特征</th>
      <th colspan="4" halign="left">行业</th>
      <th colspan="4" halign="left">雇员</th>
      <th colspan="4" halign="left">价格</th>
    </tr>
    <tr>
      <th>代号</th>
      <th>BABA</th>
      <th>JD</th>
      <th>GS</th>
      <th>MS</th>
      <th>BABA</th>
      <th>JD</th>
      <th>GS</th>
      <th>MS</th>
      <th>BABA</th>
      <th>JD</th>
      <th>GS</th>
      <th>MS</th>
    </tr>
    <tr>
      <th>地区</th>
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
      <th>中国</th>
      <td>电商</td>
      <td>电商</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>101550.0</td>
      <td>175336.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>176.92</td>
      <td>25.95</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>美国</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>金融</td>
      <td>金融</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>60348.0</td>
      <td>36600.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>41.79</td>
      <td>196.0</td>
    </tr>
  </tbody>
</table>
</div>



df 被 unstack(1) 之后变成 (行 → 列)

* 行索引 = r1

* 列索引 = [c, r2]

重塑后的 DataFrame 这时行索引只有一层 (地区)，而列索引有两层，第一层是地区，第二层是代号。

**3.基于层先 unstack(0) 再 stack(0)**


```python
df.unstack(0).stack(0)
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
      <th>地区</th>
      <th>中国</th>
      <th>美国</th>
    </tr>
    <tr>
      <th>代号</th>
      <th>特征</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="3" valign="top">BABA</th>
      <th>价格</th>
      <td>176.92</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>行业</th>
      <td>电商</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>雇员</th>
      <td>101550</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th rowspan="3" valign="top">JD</th>
      <th>价格</th>
      <td>25.95</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>行业</th>
      <td>电商</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>雇员</th>
      <td>175336</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th rowspan="3" valign="top">GS</th>
      <th>价格</th>
      <td>NaN</td>
      <td>41.79</td>
    </tr>
    <tr>
      <th>行业</th>
      <td>NaN</td>
      <td>金融</td>
    </tr>
    <tr>
      <th>雇员</th>
      <td>NaN</td>
      <td>60348</td>
    </tr>
    <tr>
      <th rowspan="3" valign="top">MS</th>
      <th>价格</th>
      <td>NaN</td>
      <td>196</td>
    </tr>
    <tr>
      <th>行业</th>
      <td>NaN</td>
      <td>金融</td>
    </tr>
    <tr>
      <th>雇员</th>
      <td>NaN</td>
      <td>36600</td>
    </tr>
  </tbody>
</table>
</div>



df 被 unstack(0) 之后变成 (行 → 列)

* 行索引 = r2

* 列索引 = [c, r1]

再被 stack(0) 之后变成 (列 → 行)

* 行索引 = [r2, c]

* 列索引 = r1



重塑后的 DataFrame 这时行索引有两层，第一层是代号，第二层是特征，而列索引只有一层 (地区)。

**4.基于层先 unstack(0) 再 stack(1)**


```python
df.unstack(0).stack(1)
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
      <th>特征</th>
      <th>行业</th>
      <th>雇员</th>
      <th>价格</th>
    </tr>
    <tr>
      <th>代号</th>
      <th>地区</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>BABA</th>
      <th>中国</th>
      <td>电商</td>
      <td>101550.0</td>
      <td>176.92</td>
    </tr>
    <tr>
      <th>JD</th>
      <th>中国</th>
      <td>电商</td>
      <td>175336.0</td>
      <td>25.95</td>
    </tr>
    <tr>
      <th>GS</th>
      <th>美国</th>
      <td>金融</td>
      <td>60348.0</td>
      <td>41.79</td>
    </tr>
    <tr>
      <th>MS</th>
      <th>美国</th>
      <td>金融</td>
      <td>36600.0</td>
      <td>196.00</td>
    </tr>
  </tbody>
</table>
</div>



df 被 unstack(0) 之后变成 (行 → 列)

* 行索引 = r2

* 列索引 = [c, r1]

再被 stack(1) 之后变成 (列 → 行)

* 行索引 = [r2, r1]

* 列索引 = c

重塑后的 DataFrame 这时行索引有两层，第一层是代号，第二层是地区，而列索引只有一层 (特征)。

**5.基于层先 unstack(1) 再 stack(0)**


```python
df.unstack(1).stack(0)
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
      <th>代号</th>
      <th>BABA</th>
      <th>GS</th>
      <th>JD</th>
      <th>MS</th>
    </tr>
    <tr>
      <th>地区</th>
      <th>特征</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="3" valign="top">中国</th>
      <th>价格</th>
      <td>176.92</td>
      <td>NaN</td>
      <td>25.95</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>行业</th>
      <td>电商</td>
      <td>NaN</td>
      <td>电商</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>雇员</th>
      <td>101550</td>
      <td>NaN</td>
      <td>175336</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th rowspan="3" valign="top">美国</th>
      <th>价格</th>
      <td>NaN</td>
      <td>41.79</td>
      <td>NaN</td>
      <td>196</td>
    </tr>
    <tr>
      <th>行业</th>
      <td>NaN</td>
      <td>金融</td>
      <td>NaN</td>
      <td>金融</td>
    </tr>
    <tr>
      <th>雇员</th>
      <td>NaN</td>
      <td>60348</td>
      <td>NaN</td>
      <td>36600</td>
    </tr>
  </tbody>
</table>
</div>



df 被 unstack(1) 之后变成 (行 → 列)

* 行索引 = r1

* 列索引 = [c, r2]

再被 stack(0) 之后变成 (列 → 行)

* 行索引 = [r1, c]

* 列索引 = r2

重塑后的 DataFrame 这时行索引有两层，第一层是地区，第二层是特征，而列索引只有一层 (代号)。

**6.基于层先 unstack(1) 再 stack(1)**


```python
df.unstack(1).stack(1)
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
      <th>特征</th>
      <th>行业</th>
      <th>雇员</th>
      <th>价格</th>
    </tr>
    <tr>
      <th>地区</th>
      <th>代号</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="2" valign="top">中国</th>
      <th>BABA</th>
      <td>电商</td>
      <td>101550.0</td>
      <td>176.92</td>
    </tr>
    <tr>
      <th>JD</th>
      <td>电商</td>
      <td>175336.0</td>
      <td>25.95</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">美国</th>
      <th>GS</th>
      <td>金融</td>
      <td>60348.0</td>
      <td>41.79</td>
    </tr>
    <tr>
      <th>MS</th>
      <td>金融</td>
      <td>36600.0</td>
      <td>196.00</td>
    </tr>
  </tbody>
</table>
</div>



df 被 unstack(1) 之后变成 (行 → 列)

* 行索引 = r1

* 列索引 = [c, r2]

再被 stack(1) 之后变成 (列 → 行)

* 行索引 = [r1, r2]

* 列索引 = c

重塑后的 DataFrame 这时行索引有两层，第一层是地区，第二层是特征，而列索引只有一层 (代号)。还原成原来的 df 了。

**7.基于层被 stack()，没有填层数，默认为最后一层。**


```python
df.stack()
```




    地区  代号    特征
    中国  BABA  行业        电商
              雇员    101550
              价格    176.92
        JD    行业        电商
              雇员    175336
              价格     25.95
    美国  GS    行业        金融
              雇员     60348
              价格     41.79
        MS    行业        金融
              雇员     36600
              价格       196
    dtype: object



df 被 stack() 之后变成 (列 → 行)

* 行索引 = [r1, r2, c]

* 列索引 = []

重塑后的 Series 只有行索引，有三层，第一层是地区，第二层是代号，第三层是特征。

**8.基于层被 unstack() 两次，没有填层数，默认为最后一层。**


```python
df.unstack().unstack()
```




    特征  代号    地区
    行业  BABA  中国        电商
              美国       NaN
        JD    中国        电商
              美国       NaN
        GS    中国       NaN
              美国        金融
        MS    中国       NaN
              美国        金融
    雇员  BABA  中国    101550
              美国       NaN
        JD    中国    175336
              美国       NaN
        GS    中国       NaN
              美国     60348
        MS    中国       NaN
              美国     36600
    价格  BABA  中国    176.92
              美国       NaN
        JD    中国     25.95
              美国       NaN
        GS    中国       NaN
              美国     41.79
        MS    中国       NaN
              美国       196
    dtype: object



df 被第一次 unstack() 之后变成 (行 → 列)

* 行索引 = r1

* 列索引 = [c, r2]

df 被第二次 unstack() 之后变成 (行 → 列)

* 行索引 = []

* 列索引 = [c, r2, r1]

重塑后的 Series 只有列索引 (实际上是个转置的 Series)，有三层，第一层是特征，第二层是代号，第三层是地区。

### 5.2 透视

多个**时间序列数据**(在多个时间点观察或测量到的数据)通常是以所谓的**“长格式”（long）或“堆叠格式”（stacked）**存储在数据库和CSV中的。

因此，经常有重复值出现在各列下，因而导致源表不能传递有价值的信息。这时可用「透视」方法调整源表的布局用作更清晰的展示。

在 Pandas 里透视的方法有两种：

* 用 pivot 函数将「长格式」**旋转**为「宽格式」，

* 用 melt 函数将「宽格式」**旋转**为「长格式」，

本节使用的数据描述如下：

* 5 只股票：AAPL, JD, BABA, FB, GS

* 4 个交易日：从 2019-02-21 到 2019-02-26


```python
data = pd.read_csv('data/Stock.csv', parse_dates=[0], dayfirst=True)
data
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
      <th>Date</th>
      <th>Symbol</th>
      <th>Open</th>
      <th>High</th>
      <th>Low</th>
      <th>Close</th>
      <th>Adj Close</th>
      <th>Volume</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2019-02-21</td>
      <td>AAPL</td>
      <td>171.800003</td>
      <td>172.369995</td>
      <td>170.300003</td>
      <td>171.059998</td>
      <td>171.059998</td>
      <td>17249700</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2019-02-21</td>
      <td>JD</td>
      <td>24.820000</td>
      <td>24.879999</td>
      <td>24.010000</td>
      <td>24.270000</td>
      <td>24.270000</td>
      <td>13542600</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2019-02-21</td>
      <td>BABA</td>
      <td>171.000000</td>
      <td>171.779999</td>
      <td>169.800003</td>
      <td>171.660004</td>
      <td>171.660004</td>
      <td>8434800</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2019-02-21</td>
      <td>GS</td>
      <td>198.970001</td>
      <td>199.449997</td>
      <td>195.050003</td>
      <td>196.360001</td>
      <td>196.360001</td>
      <td>2785900</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2019-02-21</td>
      <td>FB</td>
      <td>161.929993</td>
      <td>162.240005</td>
      <td>159.589996</td>
      <td>160.039993</td>
      <td>160.039993</td>
      <td>15607800</td>
    </tr>
    <tr>
      <th>5</th>
      <td>2019-02-22</td>
      <td>AAPL</td>
      <td>171.580002</td>
      <td>173.000000</td>
      <td>171.380005</td>
      <td>172.970001</td>
      <td>172.970001</td>
      <td>18913200</td>
    </tr>
    <tr>
      <th>6</th>
      <td>2019-02-22</td>
      <td>JD</td>
      <td>24.549999</td>
      <td>25.959999</td>
      <td>24.480000</td>
      <td>25.950001</td>
      <td>25.950001</td>
      <td>27113300</td>
    </tr>
    <tr>
      <th>7</th>
      <td>2019-02-22</td>
      <td>BABA</td>
      <td>172.800003</td>
      <td>177.020004</td>
      <td>172.520004</td>
      <td>176.919998</td>
      <td>176.919998</td>
      <td>16175600</td>
    </tr>
    <tr>
      <th>8</th>
      <td>2019-02-22</td>
      <td>GS</td>
      <td>196.600006</td>
      <td>197.750000</td>
      <td>195.199997</td>
      <td>196.000000</td>
      <td>196.000000</td>
      <td>2626600</td>
    </tr>
    <tr>
      <th>9</th>
      <td>2019-02-22</td>
      <td>FB</td>
      <td>160.580002</td>
      <td>162.410004</td>
      <td>160.309998</td>
      <td>161.889999</td>
      <td>161.889999</td>
      <td>15858500</td>
    </tr>
    <tr>
      <th>10</th>
      <td>2019-02-25</td>
      <td>AAPL</td>
      <td>174.160004</td>
      <td>175.869995</td>
      <td>173.949997</td>
      <td>174.229996</td>
      <td>174.229996</td>
      <td>21873400</td>
    </tr>
    <tr>
      <th>11</th>
      <td>2019-02-25</td>
      <td>JD</td>
      <td>27.110001</td>
      <td>27.379999</td>
      <td>26.040001</td>
      <td>26.190001</td>
      <td>26.190001</td>
      <td>29338500</td>
    </tr>
    <tr>
      <th>12</th>
      <td>2019-02-25</td>
      <td>BABA</td>
      <td>181.259995</td>
      <td>183.720001</td>
      <td>180.729996</td>
      <td>183.250000</td>
      <td>183.250000</td>
      <td>22831800</td>
    </tr>
    <tr>
      <th>13</th>
      <td>2019-02-25</td>
      <td>GS</td>
      <td>198.000000</td>
      <td>201.500000</td>
      <td>197.710007</td>
      <td>198.649994</td>
      <td>198.649994</td>
      <td>3032200</td>
    </tr>
    <tr>
      <th>14</th>
      <td>2019-02-25</td>
      <td>FB</td>
      <td>163.070007</td>
      <td>166.070007</td>
      <td>162.899994</td>
      <td>164.619995</td>
      <td>164.619995</td>
      <td>18737100</td>
    </tr>
    <tr>
      <th>15</th>
      <td>2019-02-26</td>
      <td>AAPL</td>
      <td>173.710007</td>
      <td>175.300003</td>
      <td>173.169998</td>
      <td>174.330002</td>
      <td>174.330002</td>
      <td>17006000</td>
    </tr>
    <tr>
      <th>16</th>
      <td>2019-02-26</td>
      <td>JD</td>
      <td>25.980000</td>
      <td>26.820000</td>
      <td>25.660000</td>
      <td>26.590000</td>
      <td>26.590000</td>
      <td>20264100</td>
    </tr>
    <tr>
      <th>17</th>
      <td>2019-02-26</td>
      <td>BABA</td>
      <td>179.789993</td>
      <td>184.350006</td>
      <td>179.369995</td>
      <td>183.539993</td>
      <td>183.539993</td>
      <td>13857900</td>
    </tr>
    <tr>
      <th>18</th>
      <td>2019-02-26</td>
      <td>GS</td>
      <td>198.470001</td>
      <td>200.559998</td>
      <td>196.550003</td>
      <td>198.899994</td>
      <td>198.899994</td>
      <td>2498000</td>
    </tr>
    <tr>
      <th>19</th>
      <td>2019-02-26</td>
      <td>FB</td>
      <td>164.339996</td>
      <td>166.240005</td>
      <td>163.800003</td>
      <td>164.130005</td>
      <td>164.130005</td>
      <td>13645200</td>
    </tr>
  </tbody>
</table>
</div>



从上表看出有 20 行 (5 × 4) 和 8 列，在 Date 和 Symbol 那两列下就有重复值，4 个日期和 5 个股票在 20 行中分别出现了 5 次和 4 次。

这就是多个时间序列（或者其它带有两个或多个键的可观察数据，这里，我们的键是Date和Symbol）的长格式。表中的每行代表一次观察。

关系型数据库（如MySQL）中的数据经常都是这样存储的，因为固定架构（即列名和数据类型）有一个好处：随着表中数据的添加，Symbol列中的值的种类能够增加。在前面的例子中，Date和Symbol通常就是主键（关系型数据库中的术语，是表中的一个或多个字段，它的值用于唯一地标识表中的某一条记录），不仅提供了关系完整性，而且提供了更为简单的查询支持。有的情况下，使用这样的数据会很麻烦，你可能会更喜欢不同的Symbol值分别形成一列，Date列中的时间戳则用作索引。DataFrame的pivot方法完全可以实现这个转换：

#### 从长到宽 (pivot)

当我们做数据分析时，只关注不同股票在不同日期下的 Adj Close


```python
data.iloc[:,[0,1,6]]
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
      <th>Date</th>
      <th>Symbol</th>
      <th>Adj Close</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2019-02-21</td>
      <td>AAPL</td>
      <td>171.059998</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2019-02-21</td>
      <td>JD</td>
      <td>24.270000</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2019-02-21</td>
      <td>BABA</td>
      <td>171.660004</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2019-02-21</td>
      <td>GS</td>
      <td>196.360001</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2019-02-21</td>
      <td>FB</td>
      <td>160.039993</td>
    </tr>
    <tr>
      <th>5</th>
      <td>2019-02-22</td>
      <td>AAPL</td>
      <td>172.970001</td>
    </tr>
    <tr>
      <th>6</th>
      <td>2019-02-22</td>
      <td>JD</td>
      <td>25.950001</td>
    </tr>
    <tr>
      <th>7</th>
      <td>2019-02-22</td>
      <td>BABA</td>
      <td>176.919998</td>
    </tr>
    <tr>
      <th>8</th>
      <td>2019-02-22</td>
      <td>GS</td>
      <td>196.000000</td>
    </tr>
    <tr>
      <th>9</th>
      <td>2019-02-22</td>
      <td>FB</td>
      <td>161.889999</td>
    </tr>
    <tr>
      <th>10</th>
      <td>2019-02-25</td>
      <td>AAPL</td>
      <td>174.229996</td>
    </tr>
    <tr>
      <th>11</th>
      <td>2019-02-25</td>
      <td>JD</td>
      <td>26.190001</td>
    </tr>
    <tr>
      <th>12</th>
      <td>2019-02-25</td>
      <td>BABA</td>
      <td>183.250000</td>
    </tr>
    <tr>
      <th>13</th>
      <td>2019-02-25</td>
      <td>GS</td>
      <td>198.649994</td>
    </tr>
    <tr>
      <th>14</th>
      <td>2019-02-25</td>
      <td>FB</td>
      <td>164.619995</td>
    </tr>
    <tr>
      <th>15</th>
      <td>2019-02-26</td>
      <td>AAPL</td>
      <td>174.330002</td>
    </tr>
    <tr>
      <th>16</th>
      <td>2019-02-26</td>
      <td>JD</td>
      <td>26.590000</td>
    </tr>
    <tr>
      <th>17</th>
      <td>2019-02-26</td>
      <td>BABA</td>
      <td>183.539993</td>
    </tr>
    <tr>
      <th>18</th>
      <td>2019-02-26</td>
      <td>GS</td>
      <td>198.899994</td>
    </tr>
    <tr>
      <th>19</th>
      <td>2019-02-26</td>
      <td>FB</td>
      <td>164.130005</td>
    </tr>
  </tbody>
</table>
</div>



那么可用 pivot 函数将原始 data「透视」成一个新的 DataFrame，起名 close_price。在 pivot 函数中

* 将 index 设置成 ‘Date’

* 将 columns 设置成 ‘Symbol’

* 将 values 设置 ‘Adj Close’

close_price 实际上把 data[‘Date’] 和 data[‘Symbol’] 的**唯一值**当成支点(pivot 就是支点的意思) 创建一个 DataFrame，其中

* 行标签 = 2019-02-21, 2019-02-22, 2019-02-25, 2019-02-26

* 列标签 = AAPL, JD, BABA, FB, GS

在把 data[‘Adj Close’] 的值放在以如上的行标签和列标签创建的 close_price 来展示。

代码如下：


```python
close_price = data.pivot( index='Date',
                          columns='Symbol',
                          values='Adj Close' )
close_price
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
      <th>Symbol</th>
      <th>AAPL</th>
      <th>BABA</th>
      <th>FB</th>
      <th>GS</th>
      <th>JD</th>
    </tr>
    <tr>
      <th>Date</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2019-02-21</th>
      <td>171.059998</td>
      <td>171.660004</td>
      <td>160.039993</td>
      <td>196.360001</td>
      <td>24.270000</td>
    </tr>
    <tr>
      <th>2019-02-22</th>
      <td>172.970001</td>
      <td>176.919998</td>
      <td>161.889999</td>
      <td>196.000000</td>
      <td>25.950001</td>
    </tr>
    <tr>
      <th>2019-02-25</th>
      <td>174.229996</td>
      <td>183.250000</td>
      <td>164.619995</td>
      <td>198.649994</td>
      <td>26.190001</td>
    </tr>
    <tr>
      <th>2019-02-26</th>
      <td>174.330002</td>
      <td>183.539993</td>
      <td>164.130005</td>
      <td>198.899994</td>
      <td>26.590000</td>
    </tr>
  </tbody>
</table>
</div>



如果觉得 Adj Close 不够，还想加个 Volume 看看，这时支点还是 data[‘Date’] 和 data[‘Symbol’]，但是要透视的值增加到 data[['Adj Close', 'Volume']] 了。pivot 函数返回的是两个透视表。


```python
# data.pivot( index='Date',
#             columns='Symbol',
#             values=['Adj Close','Volume'] )

data.pivot( index='Date',
           columns='Symbol')[['Adj Close','Volume']] 
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead tr th {
        text-align: left;
    }

    .dataframe thead tr:last-of-type th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th colspan="5" halign="left">Adj Close</th>
      <th colspan="5" halign="left">Volume</th>
    </tr>
    <tr>
      <th>Symbol</th>
      <th>AAPL</th>
      <th>BABA</th>
      <th>FB</th>
      <th>GS</th>
      <th>JD</th>
      <th>AAPL</th>
      <th>BABA</th>
      <th>FB</th>
      <th>GS</th>
      <th>JD</th>
    </tr>
    <tr>
      <th>Date</th>
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
      <th>2019-02-21</th>
      <td>171.059998</td>
      <td>171.660004</td>
      <td>160.039993</td>
      <td>196.360001</td>
      <td>24.270000</td>
      <td>17249700</td>
      <td>8434800</td>
      <td>15607800</td>
      <td>2785900</td>
      <td>13542600</td>
    </tr>
    <tr>
      <th>2019-02-22</th>
      <td>172.970001</td>
      <td>176.919998</td>
      <td>161.889999</td>
      <td>196.000000</td>
      <td>25.950001</td>
      <td>18913200</td>
      <td>16175600</td>
      <td>15858500</td>
      <td>2626600</td>
      <td>27113300</td>
    </tr>
    <tr>
      <th>2019-02-25</th>
      <td>174.229996</td>
      <td>183.250000</td>
      <td>164.619995</td>
      <td>198.649994</td>
      <td>26.190001</td>
      <td>21873400</td>
      <td>22831800</td>
      <td>18737100</td>
      <td>3032200</td>
      <td>29338500</td>
    </tr>
    <tr>
      <th>2019-02-26</th>
      <td>174.330002</td>
      <td>183.539993</td>
      <td>164.130005</td>
      <td>198.899994</td>
      <td>26.590000</td>
      <td>17006000</td>
      <td>13857900</td>
      <td>13645200</td>
      <td>2498000</td>
      <td>20264100</td>
    </tr>
  </tbody>
</table>
</div>



如果不设置 values 参数，那么 pivot 函数返回的是六个透视表。(源表 data 有八列，两列当了支点，剩下六列用来透视)


```python
all_pivot = data.pivot( index='Date', 
                        columns='Symbol' )
all_pivot
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead tr th {
        text-align: left;
    }

    .dataframe thead tr:last-of-type th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th colspan="5" halign="left">Open</th>
      <th colspan="5" halign="left">High</th>
      <th>...</th>
      <th colspan="5" halign="left">Adj Close</th>
      <th colspan="5" halign="left">Volume</th>
    </tr>
    <tr>
      <th>Symbol</th>
      <th>AAPL</th>
      <th>BABA</th>
      <th>FB</th>
      <th>GS</th>
      <th>JD</th>
      <th>AAPL</th>
      <th>BABA</th>
      <th>FB</th>
      <th>GS</th>
      <th>JD</th>
      <th>...</th>
      <th>AAPL</th>
      <th>BABA</th>
      <th>FB</th>
      <th>GS</th>
      <th>JD</th>
      <th>AAPL</th>
      <th>BABA</th>
      <th>FB</th>
      <th>GS</th>
      <th>JD</th>
    </tr>
    <tr>
      <th>Date</th>
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
      <th>2019-02-21</th>
      <td>171.800003</td>
      <td>171.000000</td>
      <td>161.929993</td>
      <td>198.970001</td>
      <td>24.820000</td>
      <td>172.369995</td>
      <td>171.779999</td>
      <td>162.240005</td>
      <td>199.449997</td>
      <td>24.879999</td>
      <td>...</td>
      <td>171.059998</td>
      <td>171.660004</td>
      <td>160.039993</td>
      <td>196.360001</td>
      <td>24.270000</td>
      <td>17249700</td>
      <td>8434800</td>
      <td>15607800</td>
      <td>2785900</td>
      <td>13542600</td>
    </tr>
    <tr>
      <th>2019-02-22</th>
      <td>171.580002</td>
      <td>172.800003</td>
      <td>160.580002</td>
      <td>196.600006</td>
      <td>24.549999</td>
      <td>173.000000</td>
      <td>177.020004</td>
      <td>162.410004</td>
      <td>197.750000</td>
      <td>25.959999</td>
      <td>...</td>
      <td>172.970001</td>
      <td>176.919998</td>
      <td>161.889999</td>
      <td>196.000000</td>
      <td>25.950001</td>
      <td>18913200</td>
      <td>16175600</td>
      <td>15858500</td>
      <td>2626600</td>
      <td>27113300</td>
    </tr>
    <tr>
      <th>2019-02-25</th>
      <td>174.160004</td>
      <td>181.259995</td>
      <td>163.070007</td>
      <td>198.000000</td>
      <td>27.110001</td>
      <td>175.869995</td>
      <td>183.720001</td>
      <td>166.070007</td>
      <td>201.500000</td>
      <td>27.379999</td>
      <td>...</td>
      <td>174.229996</td>
      <td>183.250000</td>
      <td>164.619995</td>
      <td>198.649994</td>
      <td>26.190001</td>
      <td>21873400</td>
      <td>22831800</td>
      <td>18737100</td>
      <td>3032200</td>
      <td>29338500</td>
    </tr>
    <tr>
      <th>2019-02-26</th>
      <td>173.710007</td>
      <td>179.789993</td>
      <td>164.339996</td>
      <td>198.470001</td>
      <td>25.980000</td>
      <td>175.300003</td>
      <td>184.350006</td>
      <td>166.240005</td>
      <td>200.559998</td>
      <td>26.820000</td>
      <td>...</td>
      <td>174.330002</td>
      <td>183.539993</td>
      <td>164.130005</td>
      <td>198.899994</td>
      <td>26.590000</td>
      <td>17006000</td>
      <td>13857900</td>
      <td>13645200</td>
      <td>2498000</td>
      <td>20264100</td>
    </tr>
  </tbody>
</table>
<p>4 rows × 30 columns</p>
</div>



**注意，pivot其实就是用set_index创建层次化索引，再用unstack重塑：**


```python
unstacked = data.set_index(['Date', 'Symbol']).unstack('Symbol')
unstacked
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead tr th {
        text-align: left;
    }

    .dataframe thead tr:last-of-type th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th colspan="5" halign="left">Open</th>
      <th colspan="5" halign="left">High</th>
      <th>...</th>
      <th colspan="5" halign="left">Adj Close</th>
      <th colspan="5" halign="left">Volume</th>
    </tr>
    <tr>
      <th>Symbol</th>
      <th>AAPL</th>
      <th>BABA</th>
      <th>FB</th>
      <th>GS</th>
      <th>JD</th>
      <th>AAPL</th>
      <th>BABA</th>
      <th>FB</th>
      <th>GS</th>
      <th>JD</th>
      <th>...</th>
      <th>AAPL</th>
      <th>BABA</th>
      <th>FB</th>
      <th>GS</th>
      <th>JD</th>
      <th>AAPL</th>
      <th>BABA</th>
      <th>FB</th>
      <th>GS</th>
      <th>JD</th>
    </tr>
    <tr>
      <th>Date</th>
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
      <th>2019-02-21</th>
      <td>171.800003</td>
      <td>171.000000</td>
      <td>161.929993</td>
      <td>198.970001</td>
      <td>24.820000</td>
      <td>172.369995</td>
      <td>171.779999</td>
      <td>162.240005</td>
      <td>199.449997</td>
      <td>24.879999</td>
      <td>...</td>
      <td>171.059998</td>
      <td>171.660004</td>
      <td>160.039993</td>
      <td>196.360001</td>
      <td>24.270000</td>
      <td>17249700</td>
      <td>8434800</td>
      <td>15607800</td>
      <td>2785900</td>
      <td>13542600</td>
    </tr>
    <tr>
      <th>2019-02-22</th>
      <td>171.580002</td>
      <td>172.800003</td>
      <td>160.580002</td>
      <td>196.600006</td>
      <td>24.549999</td>
      <td>173.000000</td>
      <td>177.020004</td>
      <td>162.410004</td>
      <td>197.750000</td>
      <td>25.959999</td>
      <td>...</td>
      <td>172.970001</td>
      <td>176.919998</td>
      <td>161.889999</td>
      <td>196.000000</td>
      <td>25.950001</td>
      <td>18913200</td>
      <td>16175600</td>
      <td>15858500</td>
      <td>2626600</td>
      <td>27113300</td>
    </tr>
    <tr>
      <th>2019-02-25</th>
      <td>174.160004</td>
      <td>181.259995</td>
      <td>163.070007</td>
      <td>198.000000</td>
      <td>27.110001</td>
      <td>175.869995</td>
      <td>183.720001</td>
      <td>166.070007</td>
      <td>201.500000</td>
      <td>27.379999</td>
      <td>...</td>
      <td>174.229996</td>
      <td>183.250000</td>
      <td>164.619995</td>
      <td>198.649994</td>
      <td>26.190001</td>
      <td>21873400</td>
      <td>22831800</td>
      <td>18737100</td>
      <td>3032200</td>
      <td>29338500</td>
    </tr>
    <tr>
      <th>2019-02-26</th>
      <td>173.710007</td>
      <td>179.789993</td>
      <td>164.339996</td>
      <td>198.470001</td>
      <td>25.980000</td>
      <td>175.300003</td>
      <td>184.350006</td>
      <td>166.240005</td>
      <td>200.559998</td>
      <td>26.820000</td>
      <td>...</td>
      <td>174.330002</td>
      <td>183.539993</td>
      <td>164.130005</td>
      <td>198.899994</td>
      <td>26.590000</td>
      <td>17006000</td>
      <td>13857900</td>
      <td>13645200</td>
      <td>2498000</td>
      <td>20264100</td>
    </tr>
  </tbody>
</table>
<p>4 rows × 30 columns</p>
</div>



再继续观察下，all_pivot 实际上是个多层 DataFrame (有多层 columns)。假设要获取 2019-02-25 和 2019-02-26 两天的 BABA 和 FB 的开盘价，用以下「多层索引和切片」的方法。


```python
all_pivot['Open'].iloc[2:,1:3]
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
      <th>Symbol</th>
      <th>BABA</th>
      <th>FB</th>
    </tr>
    <tr>
      <th>Date</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2019-02-25</th>
      <td>181.259995</td>
      <td>163.070007</td>
    </tr>
    <tr>
      <th>2019-02-26</th>
      <td>179.789993</td>
      <td>164.339996</td>
    </tr>
  </tbody>
</table>
</div>



**Mc补充：**

pandas.pivot的重点在于reshape, 通俗理解就是合并同类项，如果数据中有多个值(2个或以上)对应的行索引和列索引都相同时，**pivot将会报错**：Index contains duplicate entries, cannot reshape，**而pivot_table不会报错**，默认计算相同数据的均值并返回。
如果每个值对应的行索引和列索引都是唯一的，两者的结果则是一样的。


```python
from collections import OrderedDict    #有序字典

table = OrderedDict((("Item", ['Item0', 'Item0', 'Item1', 'Item1']),
                    ('CType',['Gold', 'Bronze', 'Gold', 'Silver']),
                    ('USD',  [1, 2, 3, 4]),
                    ('EU',   [1, 2, 3, 4])))
d = pd.DataFrame(table)
d
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
      <th>Item</th>
      <th>CType</th>
      <th>USD</th>
      <th>EU</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Item0</td>
      <td>Gold</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Item0</td>
      <td>Bronze</td>
      <td>2</td>
      <td>2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Item1</td>
      <td>Gold</td>
      <td>3</td>
      <td>3</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Item1</td>
      <td>Silver</td>
      <td>4</td>
      <td>4</td>
    </tr>
  </tbody>
</table>
</div>



![](https://pic./2020/06/09/b08552ae2f232.png)


```python
d.pivot(index='Item', columns='CType', values='USD')
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
      <th>CType</th>
      <th>Bronze</th>
      <th>Gold</th>
      <th>Silver</th>
    </tr>
    <tr>
      <th>Item</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Item0</th>
      <td>2.0</td>
      <td>1.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>Item1</th>
      <td>NaN</td>
      <td>3.0</td>
      <td>4.0</td>
    </tr>
  </tbody>
</table>
</div>



![](https://pic./2020/06/09/e3cde08e0e777.png)


```python
d.pivot(index='Item', columns='CType')
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead tr th {
        text-align: left;
    }

    .dataframe thead tr:last-of-type th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th colspan="3" halign="left">USD</th>
      <th colspan="3" halign="left">EU</th>
    </tr>
    <tr>
      <th>CType</th>
      <th>Bronze</th>
      <th>Gold</th>
      <th>Silver</th>
      <th>Bronze</th>
      <th>Gold</th>
      <th>Silver</th>
    </tr>
    <tr>
      <th>Item</th>
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
      <th>Item0</th>
      <td>2.0</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>2.0</td>
      <td>1.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>Item1</th>
      <td>NaN</td>
      <td>3.0</td>
      <td>4.0</td>
      <td>NaN</td>
      <td>3.0</td>
      <td>4.0</td>
    </tr>
  </tbody>
</table>
</div>



![](https://pic./2020/06/09/9d99c6add4b2c.png)

如果数据中的一个值对应的行索引和列索引都相同时，**pivot将会报错**


```python
d.loc[2,'Item'] = 'Item0'
d.pivot(index='Item',columns='CType',values='USD')
```


    ---------------------------------------------------------------------------

    ValueError                                Traceback (most recent call last)

    <ipython-input-50-edeae3aec8b5> in <module>
          1 d.loc[2,'Item'] = 'Item0'
    ----> 2 d.pivot(index='Item',columns='CType',values='USD')
    

    D:\Anaconda\envs\python32\lib\site-packages\pandas\core\frame.py in pivot(self, index, columns, values)
       5921         from pandas.core.reshape.pivot import pivot
       5922 
    -> 5923         return pivot(self, index=index, columns=columns, values=values)
       5924 
       5925     _shared_docs[
    

    D:\Anaconda\envs\python32\lib\site-packages\pandas\core\reshape\pivot.py in pivot(data, index, columns, values)
        448         else:
        449             indexed = data._constructor_sliced(data[values].values, index=index)
    --> 450     return indexed.unstack(columns)
        451 
        452 
    

    D:\Anaconda\envs\python32\lib\site-packages\pandas\core\series.py in unstack(self, level, fill_value)
       3548         from pandas.core.reshape.reshape import unstack
       3549 
    -> 3550         return unstack(self, level, fill_value)
       3551 
       3552     # ----------------------------------------------------------------------
    

    D:\Anaconda\envs\python32\lib\site-packages\pandas\core\reshape\reshape.py in unstack(obj, level, fill_value)
        417             level=level,
        418             fill_value=fill_value,
    --> 419             constructor=obj._constructor_expanddim,
        420         )
        421         return unstacker.get_result()
    

    D:\Anaconda\envs\python32\lib\site-packages\pandas\core\reshape\reshape.py in __init__(self, values, index, level, value_columns, fill_value, constructor)
        139 
        140         self._make_sorted_values_labels()
    --> 141         self._make_selectors()
        142 
        143     def _make_sorted_values_labels(self):
    

    D:\Anaconda\envs\python32\lib\site-packages\pandas\core\reshape\reshape.py in _make_selectors(self)
        177 
        178         if mask.sum() < len(self.index):
    --> 179             raise ValueError("Index contains duplicate entries, cannot reshape")
        180 
        181         self.group_index = comp_index
    

    ValueError: Index contains duplicate entries, cannot reshape



```python
#可以删除其他同类项，但数据会不准确
dt = d.drop_duplicates(subset=['Item','CType'])
print(dt)
dt.pivot(index='Item',columns='CType',values='USD')
```

        Item   CType  USD  EU
    0  Item0    Gold    1   1
    1  Item0  Bronze    2   2
    3  Item1  Silver    4   4
    




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
      <th>CType</th>
      <th>Bronze</th>
      <th>Gold</th>
      <th>Silver</th>
    </tr>
    <tr>
      <th>Item</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Item0</th>
      <td>2.0</td>
      <td>1.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>Item1</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>4.0</td>
    </tr>
  </tbody>
</table>
</div>



![](https://pic./2020/06/09/a4c919e34c247.png)

**使用pivot_table不会报错**，默认计算相同数据的均值并返回。

**推荐使用pivot_table**


```python
d.pivot_table(index='Item',columns='CType',values='USD',aggfunc=np.mean) #默认取均值aggfunc=np.mean，你也可以指定为求和，即aggfunc=np.sum
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
      <th>CType</th>
      <th>Bronze</th>
      <th>Gold</th>
      <th>Silver</th>
    </tr>
    <tr>
      <th>Item</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Item0</th>
      <td>2.0</td>
      <td>2.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>Item1</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>4.0</td>
    </tr>
  </tbody>
</table>
</div>



#### 从宽到长 (melt)

旋转DataFrame的逆运算是pandas.melt，它合并多个列成为一个，产生一个比输入长的DataFrame。

当使用pandas.melt，必须指明哪些列是分组指标。具体来说，函数 melt 实际是将「源表」转化成 id-variable 类型的 DataFrame，下例将

* Date 和 Symbol 列当成 id，即分组指标

* 其他列 Open, High, Low, Close, Adj Close 和 Volume 当成 variable，而它们对应的值当成 value

代码如下：


```python
melted_data = pd.melt( data, id_vars=['Date','Symbol'] )
melted_data.head(5).append(melted_data.tail(5))
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
      <th>Date</th>
      <th>Symbol</th>
      <th>variable</th>
      <th>value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2019-02-21</td>
      <td>AAPL</td>
      <td>Open</td>
      <td>1.718000e+02</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2019-02-21</td>
      <td>JD</td>
      <td>Open</td>
      <td>2.482000e+01</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2019-02-21</td>
      <td>BABA</td>
      <td>Open</td>
      <td>1.710000e+02</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2019-02-21</td>
      <td>GS</td>
      <td>Open</td>
      <td>1.989700e+02</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2019-02-21</td>
      <td>FB</td>
      <td>Open</td>
      <td>1.619300e+02</td>
    </tr>
    <tr>
      <th>115</th>
      <td>2019-02-26</td>
      <td>AAPL</td>
      <td>Volume</td>
      <td>1.700600e+07</td>
    </tr>
    <tr>
      <th>116</th>
      <td>2019-02-26</td>
      <td>JD</td>
      <td>Volume</td>
      <td>2.026410e+07</td>
    </tr>
    <tr>
      <th>117</th>
      <td>2019-02-26</td>
      <td>BABA</td>
      <td>Volume</td>
      <td>1.385790e+07</td>
    </tr>
    <tr>
      <th>118</th>
      <td>2019-02-26</td>
      <td>GS</td>
      <td>Volume</td>
      <td>2.498000e+06</td>
    </tr>
    <tr>
      <th>119</th>
      <td>2019-02-26</td>
      <td>FB</td>
      <td>Volume</td>
      <td>1.364520e+07</td>
    </tr>
  </tbody>
</table>
</div>



新生成的 DataFrame 有 120 行 (4 × 5 × 6)

* 4 = data['Date'] 有 4 个日期

* 5 = data['Symbol'] 有 5 只股票

* 6 = Open, High, Low, Close, Adj Close 和 Volume 这 6 个变量

在新表 melted_data 中

* 在参数 id_vars 设置的 Date 和 Symbol 还保持为 columns

* 此外还多出两个 columns，一个叫 variable，一个叫 value

  * variable 列下的值为 Open, High, Low, Close, Adj Close 和 Volume

  * value 列下的值为前者在「源表 data」中的值

函数 melt 可以生成一张含有多个 id 的长表，然后可在 id 上筛选出想要的信息，比如


```python
melted_data[ lambda x: (x.Date=='25/02/2019') 
                     & ((x.Symbol=='BABA')|(x.Symbol=='FB')) ]
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
      <th>Date</th>
      <th>Symbol</th>
      <th>variable</th>
      <th>value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>12</th>
      <td>2019-02-25</td>
      <td>BABA</td>
      <td>Open</td>
      <td>1.812600e+02</td>
    </tr>
    <tr>
      <th>14</th>
      <td>2019-02-25</td>
      <td>FB</td>
      <td>Open</td>
      <td>1.630700e+02</td>
    </tr>
    <tr>
      <th>32</th>
      <td>2019-02-25</td>
      <td>BABA</td>
      <td>High</td>
      <td>1.837200e+02</td>
    </tr>
    <tr>
      <th>34</th>
      <td>2019-02-25</td>
      <td>FB</td>
      <td>High</td>
      <td>1.660700e+02</td>
    </tr>
    <tr>
      <th>52</th>
      <td>2019-02-25</td>
      <td>BABA</td>
      <td>Low</td>
      <td>1.807300e+02</td>
    </tr>
    <tr>
      <th>54</th>
      <td>2019-02-25</td>
      <td>FB</td>
      <td>Low</td>
      <td>1.629000e+02</td>
    </tr>
    <tr>
      <th>72</th>
      <td>2019-02-25</td>
      <td>BABA</td>
      <td>Close</td>
      <td>1.832500e+02</td>
    </tr>
    <tr>
      <th>74</th>
      <td>2019-02-25</td>
      <td>FB</td>
      <td>Close</td>
      <td>1.646200e+02</td>
    </tr>
    <tr>
      <th>92</th>
      <td>2019-02-25</td>
      <td>BABA</td>
      <td>Adj Close</td>
      <td>1.832500e+02</td>
    </tr>
    <tr>
      <th>94</th>
      <td>2019-02-25</td>
      <td>FB</td>
      <td>Adj Close</td>
      <td>1.646200e+02</td>
    </tr>
    <tr>
      <th>112</th>
      <td>2019-02-25</td>
      <td>BABA</td>
      <td>Volume</td>
      <td>2.283180e+07</td>
    </tr>
    <tr>
      <th>114</th>
      <td>2019-02-25</td>
      <td>FB</td>
      <td>Volume</td>
      <td>1.873710e+07</td>
    </tr>
  </tbody>
</table>
</div>



在 melted_data 上使用调用函数 (callable function) 做索引，可以得到在 2019-02-25 那天 BABA 和 FB 的信息。

## 6 数据表的分组和聚合

DataFrame 中的数据可以根据某些规则分组，然后在每组的数据上计算出不同统计量。这种操作称之为 split-apply-combine（拆分－应用－合并）。

![](https://pic./2020/06/09/17146b52080fc.png)

第一个阶段，pandas对象（无论是Series、DataFrame还是其他的）中的数据会根据你所提供的一个或多个键被拆分（split）为多组。拆分操作是在对象的特定轴上执行的。例如，DataFrame可以在其行（axis=0）或列（axis=1）上进行分组。然后，将一个函数应用（apply）到各个分组并产生一个新值。最后，所有这些函数的执行结果会被合并（combine）到最终的结果对象中。结果对象的形式一般取决于数据上所执行的操作。

该 split-apply-combine 过程有两步(apply-combine合为一步完成)：

**Step1 ：数据分组(split)**

* groupby 方法

**Step2 ：数据聚合(apply-combine)**

* 使用内置函数——sum / mean / max / min / count等

* 使用自定义函数—— agg ( aggregate ) 方法

* 自定义更丰富的分组运算—— apply 方法

agg 方法将一个函数使用在**一个数列**上，然后返回一个**标量**的值。也就是说agg每次传入的是一列数据，对其聚合后返回标量。

apply 是一个更一般化的方法，会将当前分组后的数据一起传入，返回多维数据。

### 6.1 数据准备

本节使用数据：**泰坦尼克数据集**

* PassengerId => 乘客编号

* Survived => 获救情况（1为获救，0为未获救）
 
* Pclass => 乘客等级(1/2/3等舱位)

* Name => 乘客姓名

* Sex => 性别

* Age => 年龄

* SibSp => 堂兄弟/妹个数

* Parch => 父母与小孩个数

* Ticket => 船票信息

* Fare => 票价

* Cabin => 客舱

* Embarked => 登船港口


```python
titanic = pd.read_csv(r'data\Titanic.csv')
titanic.head()
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
      <th>PassengerId</th>
      <th>Survived</th>
      <th>Pclass</th>
      <th>Name</th>
      <th>Sex</th>
      <th>Age</th>
      <th>SibSp</th>
      <th>Parch</th>
      <th>Ticket</th>
      <th>Fare</th>
      <th>Cabin</th>
      <th>Embarked</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>0</td>
      <td>3</td>
      <td>Braund, Mr. Owen Harris</td>
      <td>male</td>
      <td>22.0</td>
      <td>1</td>
      <td>0</td>
      <td>A/5 21171</td>
      <td>7.2500</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>Cumings, Mrs. John Bradley (Florence Briggs Th...</td>
      <td>female</td>
      <td>38.0</td>
      <td>1</td>
      <td>0</td>
      <td>PC 17599</td>
      <td>71.2833</td>
      <td>C85</td>
      <td>C</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>1</td>
      <td>3</td>
      <td>Heikkinen, Miss. Laina</td>
      <td>female</td>
      <td>26.0</td>
      <td>0</td>
      <td>0</td>
      <td>STON/O2. 3101282</td>
      <td>7.9250</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>1</td>
      <td>1</td>
      <td>Futrelle, Mrs. Jacques Heath (Lily May Peel)</td>
      <td>female</td>
      <td>35.0</td>
      <td>1</td>
      <td>0</td>
      <td>113803</td>
      <td>53.1000</td>
      <td>C123</td>
      <td>S</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>0</td>
      <td>3</td>
      <td>Allen, Mr. William Henry</td>
      <td>male</td>
      <td>35.0</td>
      <td>0</td>
      <td>0</td>
      <td>373450</td>
      <td>8.0500</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
  </tbody>
</table>
</div>



**用前面所学透视一下数据：**


```python
titanic.pivot_table(index='Sex',columns='Pclass',values='Survived')
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
      <th>Pclass</th>
      <th>1</th>
      <th>2</th>
      <th>3</th>
    </tr>
    <tr>
      <th>Sex</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>female</th>
      <td>0.968085</td>
      <td>0.921053</td>
      <td>0.500000</td>
    </tr>
    <tr>
      <th>male</th>
      <td>0.368852</td>
      <td>0.157407</td>
      <td>0.135447</td>
    </tr>
  </tbody>
</table>
</div>




```python
titanic.pivot_table(index='Sex',columns='Pclass',values='Survived',aggfunc='sum')
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
      <th>Pclass</th>
      <th>1</th>
      <th>2</th>
      <th>3</th>
    </tr>
    <tr>
      <th>Sex</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>female</th>
      <td>91</td>
      <td>70</td>
      <td>72</td>
    </tr>
    <tr>
      <th>male</th>
      <td>45</td>
      <td>17</td>
      <td>47</td>
    </tr>
  </tbody>
</table>
</div>




```python
titanic.pivot_table(index='Sex',columns='Pclass',aggfunc={'Survived':'sum','Age':'mean'})
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead tr th {
        text-align: left;
    }

    .dataframe thead tr:last-of-type th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th colspan="3" halign="left">Age</th>
      <th colspan="3" halign="left">Survived</th>
    </tr>
    <tr>
      <th>Pclass</th>
      <th>1</th>
      <th>2</th>
      <th>3</th>
      <th>1</th>
      <th>2</th>
      <th>3</th>
    </tr>
    <tr>
      <th>Sex</th>
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
      <th>female</th>
      <td>34.611765</td>
      <td>28.722973</td>
      <td>21.750000</td>
      <td>91</td>
      <td>70</td>
      <td>72</td>
    </tr>
    <tr>
      <th>male</th>
      <td>41.281386</td>
      <td>30.740707</td>
      <td>26.507589</td>
      <td>45</td>
      <td>17</td>
      <td>47</td>
    </tr>
  </tbody>
</table>
</div>



### 6.2 分组 (grouping)

用某一特定标签 (label) 将数据 (data) 分组的语法如下：

>data.groupBy( label )

<font color="red"><b>单标签分组</b></font>

首先来按 Symbol 来分组：


```python
grouped = titanic.groupby('Sex')
grouped
```




    <pandas.core.groupby.generic.DataFrameGroupBy object at 0x00000278AE849CC0>




```python
dir(grouped)
```




    ['Age',
     'Cabin',
     'Embarked',
     'Fare',
     'Name',
     'Parch',
     'PassengerId',
     'Pclass',
     'Sex',
     'SibSp',
     'Survived',
     'Ticket',
     '__annotations__',
     '__class__',
     '__delattr__',
     '__dict__',
     '__dir__',
     '__doc__',
     '__eq__',
     '__format__',
     '__ge__',
     '__getattr__',
     '__getattribute__',
     '__getitem__',
     '__gt__',
     '__hash__',
     '__init__',
     '__init_subclass__',
     '__iter__',
     '__le__',
     '__len__',
     '__lt__',
     '__module__',
     '__ne__',
     '__new__',
     '__reduce__',
     '__reduce_ex__',
     '__repr__',
     '__setattr__',
     '__sizeof__',
     '__str__',
     '__subclasshook__',
     '__weakref__',
     '_accessors',
     '_add_numeric_operations',
     '_agg_examples_doc',
     '_agg_see_also_doc',
     '_aggregate',
     '_aggregate_frame',
     '_aggregate_item_by_item',
     '_aggregate_multiple_funcs',
     '_apply_filter',
     '_apply_to_column_groupbys',
     '_apply_whitelist',
     '_assure_grouper',
     '_bool_agg',
     '_builtin_table',
     '_choose_path',
     '_concat_objects',
     '_constructor',
     '_cumcount_array',
     '_cython_agg_blocks',
     '_cython_agg_general',
     '_cython_table',
     '_cython_transform',
     '_define_paths',
     '_deprecations',
     '_dir_additions',
     '_dir_deletions',
     '_fill',
     '_get_cython_func',
     '_get_cythonized_result',
     '_get_data_to_aggregate',
     '_get_index',
     '_get_indices',
     '_gotitem',
     '_group_selection',
     '_insert_inaxis_grouper_inplace',
     '_internal_names',
     '_internal_names_set',
     '_is_builtin_func',
     '_iterate_column_groupbys',
     '_iterate_slices',
     '_make_wrapper',
     '_obj_with_exclusions',
     '_python_agg_general',
     '_python_apply_general',
     '_reindex_output',
     '_reset_cache',
     '_reset_group_selection',
     '_selected_obj',
     '_selection',
     '_selection_list',
     '_selection_name',
     '_set_group_selection',
     '_set_result_index_ordered',
     '_transform_fast',
     '_transform_general',
     '_transform_item_by_item',
     '_transform_should_cast',
     '_try_aggregate_string_function',
     '_try_cast',
     '_wrap_agged_blocks',
     '_wrap_aggregated_output',
     '_wrap_applied_output',
     '_wrap_frame_output',
     '_wrap_transformed_output',
     'agg',
     'aggregate',
     'all',
     'any',
     'apply',
     'backfill',
     'bfill',
     'boxplot',
     'corr',
     'corrwith',
     'count',
     'cov',
     'cumcount',
     'cummax',
     'cummin',
     'cumprod',
     'cumsum',
     'describe',
     'diff',
     'dtypes',
     'expanding',
     'ffill',
     'fillna',
     'filter',
     'first',
     'get_group',
     'groups',
     'head',
     'hist',
     'idxmax',
     'idxmin',
     'indices',
     'last',
     'mad',
     'max',
     'mean',
     'median',
     'min',
     'ndim',
     'ngroup',
     'ngroups',
     'nth',
     'nunique',
     'ohlc',
     'pad',
     'pct_change',
     'pipe',
     'plot',
     'prod',
     'quantile',
     'rank',
     'resample',
     'rolling',
     'sem',
     'shift',
     'size',
     'skew',
     'std',
     'sum',
     'tail',
     'take',
     'transform',
     'tshift',
     'var']



又要提起那句说了无数遍的话「万物皆对象」了。这个 grouped 也不例外，当你对如果使用某个对象感到迷茫时，用 dir() 来查看它的「属性」和「内置方法」。以下几个属性和方法是学生感兴趣的：

* ngroups: 组的个数 (int)

* size(): 每组元素的个数 (Series)

* groups: 每组元素在原 DataFrame 中的索引信息 (dict)

* get_groups(label): 标签 label 对应的数据 (DataFrame)

下面看看这些属性和方法的产出结果。

数据里性别为male和female，因此有2组。

列索引变行索引，同项合并得到新运算结果


```python
grouped.ngroups
```




    2



每组的信息条数


```python
grouped.size()
```




    Sex
    female    314
    male      577
    dtype: int64



女士 (female) 的索引 1,   2,   3,   8,   9,  ...，男士( male) 的索引0,   4,   5,   6,   7,...


```python
grouped.groups
```




    {'female': Int64Index([  1,   2,   3,   8,   9,  10,  11,  14,  15,  18,
                 ...
                 866, 871, 874, 875, 879, 880, 882, 885, 887, 888],
                dtype='int64', length=314),
     'male': Int64Index([  0,   4,   5,   6,   7,  12,  13,  16,  17,  20,
                 ...
                 873, 876, 877, 878, 881, 883, 884, 886, 889, 890],
                dtype='int64', length=577)}



查查 'male' 组里的数据的前五行。


```python
grouped.get_group('male').head()
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
      <th>PassengerId</th>
      <th>Survived</th>
      <th>Pclass</th>
      <th>Name</th>
      <th>Sex</th>
      <th>Age</th>
      <th>SibSp</th>
      <th>Parch</th>
      <th>Ticket</th>
      <th>Fare</th>
      <th>Cabin</th>
      <th>Embarked</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>0</td>
      <td>3</td>
      <td>Braund, Mr. Owen Harris</td>
      <td>male</td>
      <td>22.0</td>
      <td>1</td>
      <td>0</td>
      <td>A/5 21171</td>
      <td>7.2500</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>0</td>
      <td>3</td>
      <td>Allen, Mr. William Henry</td>
      <td>male</td>
      <td>35.0</td>
      <td>0</td>
      <td>0</td>
      <td>373450</td>
      <td>8.0500</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th>5</th>
      <td>6</td>
      <td>0</td>
      <td>3</td>
      <td>Moran, Mr. James</td>
      <td>male</td>
      <td>NaN</td>
      <td>0</td>
      <td>0</td>
      <td>330877</td>
      <td>8.4583</td>
      <td>NaN</td>
      <td>Q</td>
    </tr>
    <tr>
      <th>6</th>
      <td>7</td>
      <td>0</td>
      <td>1</td>
      <td>McCarthy, Mr. Timothy J</td>
      <td>male</td>
      <td>54.0</td>
      <td>0</td>
      <td>0</td>
      <td>17463</td>
      <td>51.8625</td>
      <td>E46</td>
      <td>S</td>
    </tr>
    <tr>
      <th>7</th>
      <td>8</td>
      <td>0</td>
      <td>3</td>
      <td>Palsson, Master. Gosta Leonard</td>
      <td>male</td>
      <td>2.0</td>
      <td>3</td>
      <td>1</td>
      <td>349909</td>
      <td>21.0750</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
  </tbody>
</table>
</div>



接下来定义个 print_groups 函数便于打印组的名字和前五行信息。


```python
def print_groups( group_obj ):
    for name, group in group_obj:
        print( name )
        print( group.head() )
```

用这个函数来调用 grouped (上面用 groupBy 得到的对象）


```python
print_groups( grouped )
```

    female
       PassengerId  Survived  Pclass  \
    1            2         1       1   
    2            3         1       3   
    3            4         1       1   
    8            9         1       3   
    9           10         1       2   
    
                                                    Name     Sex   Age  SibSp  \
    1  Cumings, Mrs. John Bradley (Florence Briggs Th...  female  38.0      1   
    2                             Heikkinen, Miss. Laina  female  26.0      0   
    3       Futrelle, Mrs. Jacques Heath (Lily May Peel)  female  35.0      1   
    8  Johnson, Mrs. Oscar W (Elisabeth Vilhelmina Berg)  female  27.0      0   
    9                Nasser, Mrs. Nicholas (Adele Achem)  female  14.0      1   
    
       Parch            Ticket     Fare Cabin Embarked  
    1      0          PC 17599  71.2833   C85        C  
    2      0  STON/O2. 3101282   7.9250   NaN        S  
    3      0            113803  53.1000  C123        S  
    8      2            347742  11.1333   NaN        S  
    9      0            237736  30.0708   NaN        C  
    male
       PassengerId  Survived  Pclass                            Name   Sex   Age  \
    0            1         0       3         Braund, Mr. Owen Harris  male  22.0   
    4            5         0       3        Allen, Mr. William Henry  male  35.0   
    5            6         0       3                Moran, Mr. James  male   NaN   
    6            7         0       1         McCarthy, Mr. Timothy J  male  54.0   
    7            8         0       3  Palsson, Master. Gosta Leonard  male   2.0   
    
       SibSp  Parch     Ticket     Fare Cabin Embarked  
    0      1      0  A/5 21171   7.2500   NaN        S  
    4      0      0     373450   8.0500   NaN        S  
    5      0      0     330877   8.4583   NaN        Q  
    6      0      0      17463  51.8625   E46        S  
    7      3      1     349909  21.0750   NaN        S  
    

这个 print_groups 函数在下面也多次被用到。

<font color="red"><b>多标签分组</b></font>

groupBy 函数除了支持单标签分组，也支持多标签分组 (将标签放入一个列表中)。


```python
grouped2 = titanic.groupby(['Sex','Pclass'])
print_groups( grouped2 )
```

    ('female', 1)
        PassengerId  Survived  Pclass  \
    1             2         1       1   
    3             4         1       1   
    11           12         1       1   
    31           32         1       1   
    52           53         1       1   
    
                                                     Name     Sex   Age  SibSp  \
    1   Cumings, Mrs. John Bradley (Florence Briggs Th...  female  38.0      1   
    3        Futrelle, Mrs. Jacques Heath (Lily May Peel)  female  35.0      1   
    11                           Bonnell, Miss. Elizabeth  female  58.0      0   
    31     Spencer, Mrs. William Augustus (Marie Eugenie)  female   NaN      1   
    52           Harper, Mrs. Henry Sleeper (Myna Haxtun)  female  49.0      1   
    
        Parch    Ticket      Fare Cabin Embarked  
    1       0  PC 17599   71.2833   C85        C  
    3       0    113803   53.1000  C123        S  
    11      0    113783   26.5500  C103        S  
    31      0  PC 17569  146.5208   B78        C  
    52      0  PC 17572   76.7292   D33        C  
    ('female', 2)
        PassengerId  Survived  Pclass  \
    9            10         1       2   
    15           16         1       2   
    41           42         0       2   
    43           44         1       2   
    53           54         1       2   
    
                                                     Name     Sex   Age  SibSp  \
    9                 Nasser, Mrs. Nicholas (Adele Achem)  female  14.0      1   
    15                   Hewlett, Mrs. (Mary D Kingcome)   female  55.0      0   
    41  Turpin, Mrs. William John Robert (Dorothy Ann ...  female  27.0      1   
    43           Laroche, Miss. Simonne Marie Anne Andree  female   3.0      1   
    53  Faunthorpe, Mrs. Lizzie (Elizabeth Anne Wilkin...  female  29.0      1   
    
        Parch         Ticket     Fare Cabin Embarked  
    9       0         237736  30.0708   NaN        C  
    15      0         248706  16.0000   NaN        S  
    41      0          11668  21.0000   NaN        S  
    43      2  SC/Paris 2123  41.5792   NaN        C  
    53      0           2926  26.0000   NaN        S  
    ('female', 3)
        PassengerId  Survived  Pclass  \
    2             3         1       3   
    8             9         1       3   
    10           11         1       3   
    14           15         0       3   
    18           19         0       3   
    
                                                     Name     Sex   Age  SibSp  \
    2                              Heikkinen, Miss. Laina  female  26.0      0   
    8   Johnson, Mrs. Oscar W (Elisabeth Vilhelmina Berg)  female  27.0      0   
    10                    Sandstrom, Miss. Marguerite Rut  female   4.0      1   
    14               Vestrom, Miss. Hulda Amanda Adolfina  female  14.0      0   
    18  Vander Planke, Mrs. Julius (Emelia Maria Vande...  female  31.0      1   
    
        Parch            Ticket     Fare Cabin Embarked  
    2       0  STON/O2. 3101282   7.9250   NaN        S  
    8       2            347742  11.1333   NaN        S  
    10      1           PP 9549  16.7000    G6        S  
    14      0            350406   7.8542   NaN        S  
    18      0            345763  18.0000   NaN        S  
    ('male', 1)
        PassengerId  Survived  Pclass                            Name   Sex   Age  \
    6             7         0       1         McCarthy, Mr. Timothy J  male  54.0   
    23           24         1       1    Sloper, Mr. William Thompson  male  28.0   
    27           28         0       1  Fortune, Mr. Charles Alexander  male  19.0   
    30           31         0       1        Uruchurtu, Don. Manuel E  male  40.0   
    34           35         0       1         Meyer, Mr. Edgar Joseph  male  28.0   
    
        SibSp  Parch    Ticket      Fare        Cabin Embarked  
    6       0      0     17463   51.8625          E46        S  
    23      0      0    113788   35.5000           A6        S  
    27      3      2     19950  263.0000  C23 C25 C27        S  
    30      0      0  PC 17601   27.7208          NaN        C  
    34      1      0  PC 17604   82.1708          NaN        C  
    ('male', 2)
        PassengerId  Survived  Pclass                          Name   Sex   Age  \
    17           18         1       2  Williams, Mr. Charles Eugene  male   NaN   
    20           21         0       2          Fynney, Mr. Joseph J  male  35.0   
    21           22         1       2         Beesley, Mr. Lawrence  male  34.0   
    33           34         0       2         Wheadon, Mr. Edward H  male  66.0   
    70           71         0       2    Jenkin, Mr. Stephen Curnow  male  32.0   
    
        SibSp  Parch      Ticket  Fare Cabin Embarked  
    17      0      0      244373  13.0   NaN        S  
    20      0      0      239865  26.0   NaN        S  
    21      0      0      248698  13.0   D56        S  
    33      0      0  C.A. 24579  10.5   NaN        S  
    70      0      0  C.A. 33111  10.5   NaN        S  
    ('male', 3)
        PassengerId  Survived  Pclass                            Name   Sex   Age  \
    0             1         0       3         Braund, Mr. Owen Harris  male  22.0   
    4             5         0       3        Allen, Mr. William Henry  male  35.0   
    5             6         0       3                Moran, Mr. James  male   NaN   
    7             8         0       3  Palsson, Master. Gosta Leonard  male   2.0   
    12           13         0       3  Saundercock, Mr. William Henry  male  20.0   
    
        SibSp  Parch     Ticket     Fare Cabin Embarked  
    0       1      0  A/5 21171   7.2500   NaN        S  
    4       0      0     373450   8.0500   NaN        S  
    5       0      0     330877   8.4583   NaN        Q  
    7       3      1     349909  21.0750   NaN        S  
    12      0      0  A/5. 2151   8.0500   NaN        S  
    

### 6.3 聚合 (aggregating)

**6.3.1 使用内置函数——sum / mean / max / min / count等**


```python
grouped.mean()
# grouped.sum()
# grouped.max()
# grouped.min()
# grouped.count()
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
      <th>PassengerId</th>
      <th>Survived</th>
      <th>Pclass</th>
      <th>Age</th>
      <th>SibSp</th>
      <th>Parch</th>
      <th>Fare</th>
    </tr>
    <tr>
      <th>Sex</th>
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
      <th>female</th>
      <td>431.028662</td>
      <td>0.742038</td>
      <td>2.159236</td>
      <td>27.915709</td>
      <td>0.694268</td>
      <td>0.649682</td>
      <td>44.479818</td>
    </tr>
    <tr>
      <th>male</th>
      <td>454.147314</td>
      <td>0.188908</td>
      <td>2.389948</td>
      <td>30.726645</td>
      <td>0.429809</td>
      <td>0.235702</td>
      <td>25.523893</td>
    </tr>
  </tbody>
</table>
</div>



**6.3.2 使用自定义函数—— agg ( aggregate ) 方法**

agg 方法将一个函数使用在**一个数列**上，然后返回一个**标量**的值。也就是说agg每次传入的是**一列数据**，对其聚合后返回标量。


```python
# grouped['Survived'].agg(np.mean)
grouped.agg(np.mean)
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
      <th>PassengerId</th>
      <th>Survived</th>
      <th>Pclass</th>
      <th>Age</th>
      <th>SibSp</th>
      <th>Parch</th>
      <th>Fare</th>
    </tr>
    <tr>
      <th>Sex</th>
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
      <th>female</th>
      <td>431.028662</td>
      <td>0.742038</td>
      <td>2.159236</td>
      <td>27.915709</td>
      <td>0.694268</td>
      <td>0.649682</td>
      <td>44.479818</td>
    </tr>
    <tr>
      <th>male</th>
      <td>454.147314</td>
      <td>0.188908</td>
      <td>2.389948</td>
      <td>30.726645</td>
      <td>0.429809</td>
      <td>0.235702</td>
      <td>25.523893</td>
    </tr>
  </tbody>
</table>
</div>




```python
titanic.groupby(['Sex','Pclass'])['Survived'].agg(['mean','sum'])
# 或者这样写
# titanic.groupby(['Sex','Pclass'])['Survived'].agg([np.mean,np.sum])  
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
      <th></th>
      <th>mean</th>
      <th>sum</th>
    </tr>
    <tr>
      <th>Sex</th>
      <th>Pclass</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="3" valign="top">female</th>
      <th>1</th>
      <td>0.968085</td>
      <td>91</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0.921053</td>
      <td>70</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0.500000</td>
      <td>72</td>
    </tr>
    <tr>
      <th rowspan="3" valign="top">male</th>
      <th>1</th>
      <td>0.368852</td>
      <td>45</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0.157407</td>
      <td>17</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0.135447</td>
      <td>47</td>
    </tr>
  </tbody>
</table>
</div>



将 np.mean 和 np.std 放进列表中，当成是高阶函数 agg() 的参数。上面代码按性别和乘客等级对获救情况求均值与和。

既然 agg() 是高阶函数，参数当然也可以是匿名函数 (lambda 函数)，下面先定义一个对 grouped2 里面每个标签下求最大值和最小值，再求差。注意 lambda 函数里面的 x 就是 grouped2。


```python
grouped2.agg( lambda x: np.max(x)-np.min(x) )
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
      <th></th>
      <th>PassengerId</th>
      <th>Survived</th>
      <th>Age</th>
      <th>SibSp</th>
      <th>Parch</th>
      <th>Fare</th>
    </tr>
    <tr>
      <th>Sex</th>
      <th>Pclass</th>
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
      <th rowspan="3" valign="top">female</th>
      <th>1</th>
      <td>886</td>
      <td>1</td>
      <td>61.00</td>
      <td>3</td>
      <td>2</td>
      <td>486.4000</td>
    </tr>
    <tr>
      <th>2</th>
      <td>871</td>
      <td>1</td>
      <td>55.00</td>
      <td>3</td>
      <td>3</td>
      <td>54.5000</td>
    </tr>
    <tr>
      <th>3</th>
      <td>886</td>
      <td>1</td>
      <td>62.25</td>
      <td>8</td>
      <td>6</td>
      <td>62.8000</td>
    </tr>
    <tr>
      <th rowspan="3" valign="top">male</th>
      <th>1</th>
      <td>883</td>
      <td>1</td>
      <td>79.08</td>
      <td>3</td>
      <td>4</td>
      <td>512.3292</td>
    </tr>
    <tr>
      <th>2</th>
      <td>869</td>
      <td>1</td>
      <td>69.33</td>
      <td>2</td>
      <td>2</td>
      <td>73.5000</td>
    </tr>
    <tr>
      <th>3</th>
      <td>890</td>
      <td>1</td>
      <td>73.58</td>
      <td>8</td>
      <td>5</td>
      <td>69.5500</td>
    </tr>
  </tbody>
</table>
</div>



上面代码对每个分组在Age、Fare、Parch、PassengerId、SibSp和Survived上求「最大值」和「最小值」的差。真正有价值的信息在 Age、Parch 等栏，但是可以借此来验证agg使用自定义函数的用法。

**6.3.3 自定义更丰富的分组运算—— apply 方法**

apply 是一个更一般化的方法：将一个数据分拆-应用-汇总，会将当前分组后的数据一起传入，返回多维数据。

有时候返回的值不一定是一个标量的值，有可能是一个数组或是其他类型。此时，agg无法胜任，就需要使用apply了。

在看具体例子之前，先定一个 top 函数，返回 DataFrame 某一栏中 n 个最大值。


```python
def top( df, n=5, column='Parch' ):
    return df.sort_values(by=column)[-n:]
```

df.sort_values

根据column排序，上一代码中是升序

将 top 函数用到最原始的数据 (从 csv 中读取出来的) 上。


```python
top( titanic )
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
      <th>PassengerId</th>
      <th>Survived</th>
      <th>Pclass</th>
      <th>Name</th>
      <th>Sex</th>
      <th>Age</th>
      <th>SibSp</th>
      <th>Parch</th>
      <th>Ticket</th>
      <th>Fare</th>
      <th>Cabin</th>
      <th>Embarked</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>13</th>
      <td>14</td>
      <td>0</td>
      <td>3</td>
      <td>Andersson, Mr. Anders Johan</td>
      <td>male</td>
      <td>39.0</td>
      <td>1</td>
      <td>5</td>
      <td>347082</td>
      <td>31.2750</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th>25</th>
      <td>26</td>
      <td>1</td>
      <td>3</td>
      <td>Asplund, Mrs. Carl Oscar (Selma Augusta Emilia...</td>
      <td>female</td>
      <td>38.0</td>
      <td>1</td>
      <td>5</td>
      <td>347077</td>
      <td>31.3875</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th>885</th>
      <td>886</td>
      <td>0</td>
      <td>3</td>
      <td>Rice, Mrs. William (Margaret Norton)</td>
      <td>female</td>
      <td>39.0</td>
      <td>0</td>
      <td>5</td>
      <td>382652</td>
      <td>29.1250</td>
      <td>NaN</td>
      <td>Q</td>
    </tr>
    <tr>
      <th>638</th>
      <td>639</td>
      <td>0</td>
      <td>3</td>
      <td>Panula, Mrs. Juha (Maria Emilia Ojala)</td>
      <td>female</td>
      <td>41.0</td>
      <td>0</td>
      <td>5</td>
      <td>3101295</td>
      <td>39.6875</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th>678</th>
      <td>679</td>
      <td>0</td>
      <td>3</td>
      <td>Goodwin, Mrs. Frederick (Augusta Tyler)</td>
      <td>female</td>
      <td>43.0</td>
      <td>1</td>
      <td>6</td>
      <td>CA 2144</td>
      <td>46.9000</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
  </tbody>
</table>
</div>



上面的**top函数**中，df 代表你传递给它的**DataFrame数据**，n代表取它的前n行，在这里，n的默认值是5，也就是说在调用这个函数的时候，如果没有给n赋值，n值等于5。column是排序列，函数会先按column升序排序，然后返回最大的n行。在这个时候，agg的方法就不管用的，要是强行使用，就会出错。

来，演示一遍错误！


```python
titanic.groupby('Sex').agg(top)
```


    ---------------------------------------------------------------------------

    ValueError                                Traceback (most recent call last)

    D:\Anaconda\envs\python32\lib\site-packages\pandas\core\groupby\generic.py in aggregate(self, func, *args, **kwargs)
        947                 try:
    --> 948                     result = self._aggregate_multiple_funcs([func], _axis=self.axis)
        949                 except ValueError as err:
    

    D:\Anaconda\envs\python32\lib\site-packages\pandas\core\base.py in _aggregate_multiple_funcs(self, arg, _axis)
        541         if not len(results):
    --> 542             raise ValueError("no results")
        543 
    

    ValueError: no results

    
    During handling of the above exception, another exception occurred:
    

    ValueError                                Traceback (most recent call last)

    D:\Anaconda\envs\python32\lib\site-packages\pandas\core\internals\managers.py in create_block_manager_from_arrays(arrays, names, axes)
       1670         blocks = form_blocks(arrays, names, axes)
    -> 1671         mgr = BlockManager(blocks, axes)
       1672         mgr._consolidate_inplace()
    

    D:\Anaconda\envs\python32\lib\site-packages\pandas\core\internals\managers.py in __init__(self, blocks, axes, do_integrity_check)
        138         if do_integrity_check:
    --> 139             self._verify_integrity()
        140 
    

    D:\Anaconda\envs\python32\lib\site-packages\pandas\core\internals\managers.py in _verify_integrity(self)
        333             if block._verify_integrity and block.shape[1:] != mgr_shape[1:]:
    --> 334                 construction_error(tot_items, block.shape[1:], self.axes)
        335         if len(self.items) != tot_items:
    

    D:\Anaconda\envs\python32\lib\site-packages\pandas\core\internals\managers.py in construction_error(tot_items, block_shape, axes, e)
       1693         raise ValueError("Empty data passed with indices specified.")
    -> 1694     raise ValueError(f"Shape of passed values is {passed}, indices imply {implied}")
       1695 
    

    ValueError: Shape of passed values is (12, 2), indices imply (11, 2)

    
    During handling of the above exception, another exception occurred:
    

    ValueError                                Traceback (most recent call last)

    <ipython-input-79-da50f6fb927d> in <module>
    ----> 1 titanic.groupby('Sex').agg(top)
    

    D:\Anaconda\envs\python32\lib\site-packages\pandas\core\groupby\generic.py in aggregate(self, func, *args, **kwargs)
        951                         # raised directly by _aggregate_multiple_funcs
        952                         raise
    --> 953                     result = self._aggregate_frame(func)
        954                 else:
        955                     # select everything except for the last level, which is the one
    

    D:\Anaconda\envs\python32\lib\site-packages\pandas\core\groupby\generic.py in _aggregate_frame(self, func, *args, **kwargs)
       1146                 result[name] = fres
       1147 
    -> 1148         return self._wrap_frame_output(result, obj)
       1149 
       1150     def _aggregate_item_by_item(self, func, *args, **kwargs) -> DataFrame:
    

    D:\Anaconda\envs\python32\lib\site-packages\pandas\core\groupby\generic.py in _wrap_frame_output(self, result, obj)
       1642 
       1643         if self.axis == 0:
    -> 1644             return DataFrame(result, index=obj.columns, columns=result_index).T
       1645         else:
       1646             return DataFrame(result, index=obj.index, columns=result_index)
    

    D:\Anaconda\envs\python32\lib\site-packages\pandas\core\frame.py in __init__(self, data, index, columns, dtype, copy)
        433             )
        434         elif isinstance(data, dict):
    --> 435             mgr = init_dict(data, index, columns, dtype=dtype)
        436         elif isinstance(data, ma.MaskedArray):
        437             import numpy.ma.mrecords as mrecords
    

    D:\Anaconda\envs\python32\lib\site-packages\pandas\core\internals\construction.py in init_dict(data, index, columns, dtype)
        252             arr if not is_datetime64tz_dtype(arr) else arr.copy() for arr in arrays
        253         ]
    --> 254     return arrays_to_mgr(arrays, data_names, index, columns, dtype=dtype)
        255 
        256 
    

    D:\Anaconda\envs\python32\lib\site-packages\pandas\core\internals\construction.py in arrays_to_mgr(arrays, arr_names, index, columns, dtype)
         72     axes = [ensure_index(columns), index]
         73 
    ---> 74     return create_block_manager_from_arrays(arrays, arr_names, axes)
         75 
         76 
    

    D:\Anaconda\envs\python32\lib\site-packages\pandas\core\internals\managers.py in create_block_manager_from_arrays(arrays, names, axes)
       1673         return mgr
       1674     except ValueError as e:
    -> 1675         construction_error(len(arrays), arrays[0].shape, axes, e)
       1676 
       1677 
    

    D:\Anaconda\envs\python32\lib\site-packages\pandas\core\internals\managers.py in construction_error(tot_items, block_shape, axes, e)
       1692     if block_shape[0] == 0:
       1693         raise ValueError("Empty data passed with indices specified.")
    -> 1694     raise ValueError(f"Shape of passed values is {passed}, indices imply {implied}")
       1695 
       1696 
    

    ValueError: Shape of passed values is (12, 2), indices imply (11, 2)


**<font color="red">Apply 函数</font>**

将 top() 函数 apply 到按 Sex 分的每个组上，按每个 Sex 分组打印出来了Parch 栏下的 5 个最大值。


```python
titanic.groupby('Sex').apply(top)
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
      <th></th>
      <th>PassengerId</th>
      <th>Survived</th>
      <th>Pclass</th>
      <th>Name</th>
      <th>Sex</th>
      <th>Age</th>
      <th>SibSp</th>
      <th>Parch</th>
      <th>Ticket</th>
      <th>Fare</th>
      <th>Cabin</th>
      <th>Embarked</th>
    </tr>
    <tr>
      <th>Sex</th>
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
      <th rowspan="5" valign="top">female</th>
      <th>638</th>
      <td>639</td>
      <td>0</td>
      <td>3</td>
      <td>Panula, Mrs. Juha (Maria Emilia Ojala)</td>
      <td>female</td>
      <td>41.0</td>
      <td>0</td>
      <td>5</td>
      <td>3101295</td>
      <td>39.6875</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th>610</th>
      <td>611</td>
      <td>0</td>
      <td>3</td>
      <td>Andersson, Mrs. Anders Johan (Alfrida Konstant...</td>
      <td>female</td>
      <td>39.0</td>
      <td>1</td>
      <td>5</td>
      <td>347082</td>
      <td>31.2750</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th>885</th>
      <td>886</td>
      <td>0</td>
      <td>3</td>
      <td>Rice, Mrs. William (Margaret Norton)</td>
      <td>female</td>
      <td>39.0</td>
      <td>0</td>
      <td>5</td>
      <td>382652</td>
      <td>29.1250</td>
      <td>NaN</td>
      <td>Q</td>
    </tr>
    <tr>
      <th>25</th>
      <td>26</td>
      <td>1</td>
      <td>3</td>
      <td>Asplund, Mrs. Carl Oscar (Selma Augusta Emilia...</td>
      <td>female</td>
      <td>38.0</td>
      <td>1</td>
      <td>5</td>
      <td>347077</td>
      <td>31.3875</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th>678</th>
      <td>679</td>
      <td>0</td>
      <td>3</td>
      <td>Goodwin, Mrs. Frederick (Augusta Tyler)</td>
      <td>female</td>
      <td>43.0</td>
      <td>1</td>
      <td>6</td>
      <td>CA 2144</td>
      <td>46.9000</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th rowspan="5" valign="top">male</th>
      <th>683</th>
      <td>684</td>
      <td>0</td>
      <td>3</td>
      <td>Goodwin, Mr. Charles Edward</td>
      <td>male</td>
      <td>14.0</td>
      <td>5</td>
      <td>2</td>
      <td>CA 2144</td>
      <td>46.9000</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th>86</th>
      <td>87</td>
      <td>0</td>
      <td>3</td>
      <td>Ford, Mr. William Neal</td>
      <td>male</td>
      <td>16.0</td>
      <td>1</td>
      <td>3</td>
      <td>W./C. 6608</td>
      <td>34.3750</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th>438</th>
      <td>439</td>
      <td>0</td>
      <td>1</td>
      <td>Fortune, Mr. Mark</td>
      <td>male</td>
      <td>64.0</td>
      <td>1</td>
      <td>4</td>
      <td>19950</td>
      <td>263.0000</td>
      <td>C23 C25 C27</td>
      <td>S</td>
    </tr>
    <tr>
      <th>360</th>
      <td>361</td>
      <td>0</td>
      <td>3</td>
      <td>Skoog, Mr. Wilhelm</td>
      <td>male</td>
      <td>40.0</td>
      <td>1</td>
      <td>4</td>
      <td>347088</td>
      <td>27.9000</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th>13</th>
      <td>14</td>
      <td>0</td>
      <td>3</td>
      <td>Andersson, Mr. Anders Johan</td>
      <td>male</td>
      <td>39.0</td>
      <td>1</td>
      <td>5</td>
      <td>347082</td>
      <td>31.2750</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
  </tbody>
</table>
</div>



上面在使用 top() 时，对于 n 和 column 大家都只用的默认值 5 和 'Parch'。如果用自己设定的值 n = 2, column = 'SibSp'，写法如下：


```python
titanic.groupby(['Sex','Pclass']).apply(top, n=2, column='SibSp')
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
      <th></th>
      <th></th>
      <th>PassengerId</th>
      <th>Survived</th>
      <th>Pclass</th>
      <th>Name</th>
      <th>Sex</th>
      <th>Age</th>
      <th>SibSp</th>
      <th>Parch</th>
      <th>Ticket</th>
      <th>Fare</th>
      <th>Cabin</th>
      <th>Embarked</th>
    </tr>
    <tr>
      <th>Sex</th>
      <th>Pclass</th>
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
      <th rowspan="6" valign="top">female</th>
      <th rowspan="2" valign="top">1</th>
      <th>88</th>
      <td>89</td>
      <td>1</td>
      <td>1</td>
      <td>Fortune, Miss. Mabel Helen</td>
      <td>female</td>
      <td>23.0</td>
      <td>3</td>
      <td>2</td>
      <td>19950</td>
      <td>263.00</td>
      <td>C23 C25 C27</td>
      <td>S</td>
    </tr>
    <tr>
      <th>341</th>
      <td>342</td>
      <td>1</td>
      <td>1</td>
      <td>Fortune, Miss. Alice Elizabeth</td>
      <td>female</td>
      <td>24.0</td>
      <td>3</td>
      <td>2</td>
      <td>19950</td>
      <td>263.00</td>
      <td>C23 C25 C27</td>
      <td>S</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">2</th>
      <th>618</th>
      <td>619</td>
      <td>1</td>
      <td>2</td>
      <td>Becker, Miss. Marion Louise</td>
      <td>female</td>
      <td>4.0</td>
      <td>2</td>
      <td>1</td>
      <td>230136</td>
      <td>39.00</td>
      <td>F4</td>
      <td>S</td>
    </tr>
    <tr>
      <th>726</th>
      <td>727</td>
      <td>1</td>
      <td>2</td>
      <td>Renouf, Mrs. Peter Henry (Lillian Jefferys)</td>
      <td>female</td>
      <td>30.0</td>
      <td>3</td>
      <td>0</td>
      <td>31027</td>
      <td>21.00</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">3</th>
      <th>180</th>
      <td>181</td>
      <td>0</td>
      <td>3</td>
      <td>Sage, Miss. Constance Gladys</td>
      <td>female</td>
      <td>NaN</td>
      <td>8</td>
      <td>2</td>
      <td>CA. 2343</td>
      <td>69.55</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th>863</th>
      <td>864</td>
      <td>0</td>
      <td>3</td>
      <td>Sage, Miss. Dorothy Edith "Dolly"</td>
      <td>female</td>
      <td>NaN</td>
      <td>8</td>
      <td>2</td>
      <td>CA. 2343</td>
      <td>69.55</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th rowspan="6" valign="top">male</th>
      <th rowspan="2" valign="top">1</th>
      <th>245</th>
      <td>246</td>
      <td>0</td>
      <td>1</td>
      <td>Minahan, Dr. William Edward</td>
      <td>male</td>
      <td>44.0</td>
      <td>2</td>
      <td>0</td>
      <td>19928</td>
      <td>90.00</td>
      <td>C78</td>
      <td>Q</td>
    </tr>
    <tr>
      <th>27</th>
      <td>28</td>
      <td>0</td>
      <td>1</td>
      <td>Fortune, Mr. Charles Alexander</td>
      <td>male</td>
      <td>19.0</td>
      <td>3</td>
      <td>2</td>
      <td>19950</td>
      <td>263.00</td>
      <td>C23 C25 C27</td>
      <td>S</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">2</th>
      <th>655</th>
      <td>656</td>
      <td>0</td>
      <td>2</td>
      <td>Hickman, Mr. Leonard Mark</td>
      <td>male</td>
      <td>24.0</td>
      <td>2</td>
      <td>0</td>
      <td>S.O.C. 14879</td>
      <td>73.50</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th>665</th>
      <td>666</td>
      <td>0</td>
      <td>2</td>
      <td>Hickman, Mr. Lewis</td>
      <td>male</td>
      <td>32.0</td>
      <td>2</td>
      <td>0</td>
      <td>S.O.C. 14879</td>
      <td>73.50</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">3</th>
      <th>159</th>
      <td>160</td>
      <td>0</td>
      <td>3</td>
      <td>Sage, Master. Thomas Henry</td>
      <td>male</td>
      <td>NaN</td>
      <td>8</td>
      <td>2</td>
      <td>CA. 2343</td>
      <td>69.55</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th>846</th>
      <td>847</td>
      <td>0</td>
      <td>3</td>
      <td>Sage, Mr. Douglas Bullen</td>
      <td>male</td>
      <td>NaN</td>
      <td>8</td>
      <td>2</td>
      <td>CA. 2343</td>
      <td>69.55</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
  </tbody>
</table>
</div>



按每个 Sex 和 Pclass 打印出来了 SibSp 栏下的最大的两个值。

### 6.4 排序(Mc补充)

排序分为对**索引排序 sort_index** 和对 **值排序 sort_values**

ascending：默认True升序排列；False降序排列


```python
obj = pd.Series(range(4), index=['d','a','b','c'])
print(obj)
#索引排序
print('索引排序\n',obj.sort_index())
#值排序
print('值排序\n',obj.sort_values(ascending=False))
```

    d    0
    a    1
    b    2
    c    3
    dtype: int64
    索引排序
     a    1
    b    2
    c    3
    d    0
    dtype: int64
    值排序
     c    3
    b    2
    a    1
    d    0
    dtype: int64
    


```python
frame = pd.DataFrame(np.arange(8).reshape((2,4)),index=['three','one'], columns=['d','a','b','c'])
print(frame)

# 索引排序
print(frame.sort_index())
# frame.sort_index(axis=1)
# 降序
# frame.sort_index(axis=1, ascending=False)

# 值排序
# frame.sort_values(by='a',ascending=False)
# frame.sort_values(by=['a','b'],ascending=False)
# frame.sort_values(by='one',axis=1,ascending=False)
```

           d  a  b  c
    three  0  1  2  3
    one    4  5  6  7
           d  a  b  c
    one    4  5  6  7
    three  0  1  2  3
    




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
      <th>c</th>
      <th>b</th>
      <th>a</th>
      <th>d</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>three</th>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>one</th>
      <td>7</td>
      <td>6</td>
      <td>5</td>
      <td>4</td>
    </tr>
  </tbody>
</table>
</div>



## 7 总结

【合并数据表】用 merge 函数按数据表的共有列进行左/右/内/外合并。

![](https://pic./2020/06/09/0ad386cb00270.png)

【连接数据表】用 concat 函数对 Series 和 DataFrame 沿着不同轴连接。

【重塑数据表】用 stack 函数将「列索引」变成「行索引」，用 unstack 函数将「行索引」变成「列索引」。它们只是改变数据表的布局和展示方式而已。

![](https://pic./2020/06/09/840d73aceff19.png)

![](https://pic./2020/06/09/ffbc80a4a0453.png)

![](https://pic./2020/06/09/31550e2d55644.png)

【透视数据表】用 pivot 函数将「一张长表」变成「多张宽表」，用 melt 函数将「多张宽表」变成「一张长表」。它们只是改变数据表的布局和展示方式而已。

![](https://pic./2020/06/09/3fca0d3b3631b.png)

![](https://pic./2020/06/09/1e689348cfd9c.png)

【分组数据表】用 groupBy 函数按不同「列索引」下的值分组。一个「列索引」或多个「列索引」就可以。

【聚合数据表】用 agg 函数对每个组做聚合而计算统计量。

【split-apply-combine】用 apply 函数做数据分析时美滋滋。

![](https://pic./2020/06/09/7556e49f8e447.png)

至此，可以说已经打好 Python Basics 的基础，能用 NumPy 做数组计算，能用 Pandas 做数据分析，现在已经搞很多事情了。现在我们唯一欠缺的是如何画图或可视化数据，下帖从最基础的可视化工具 Matplotlib 开始讲。Stay Tuned!

![结束](https://pic./2020/05/12/f0e6dce0060f9.png)

