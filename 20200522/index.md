# NumPy (下)

# 前言

### 本篇鸣谢 *马川-燕大* 的增删整理， *王圣元* ——[原创文章](https://mp.weixin.qq.com/s/Fo-UIGnsoU2nBVLxSw_nVw)，与原文不同之处包含我的学习记录。

匹配Jupyter Notebook的ipynb文档链接下载地址如下

[**源文档**](https://spiritlhl.lanzous.com/icwc7ib)

接着上篇继续后面两个章节，**数组变形**和**数组计算**。

**提纲：**

![](https://pic./2020/05/22/5365f25ef9d6a.png)

## 4 数组的变形

本节介绍**四大类**数组层面上的操作，具体有

1. 重塑 (reshape) 和打平 (ravel, flatten)

2. 合并 (concatenate, stack) 和分裂 (split)

3. 重复 (repeat) 和拼接 (tile)

4. 其他操作 (sort, insert, delete, copy)

### 4.1 重塑和打平

重塑 (reshape) 和打平 (ravel, flatten) 这两个操作仅仅只改变数组的维度

* 重塑是**从低维到高维**

* 打平是**从高维到低维**

#### 重塑

用reshape()函数将一维数组 arr 重塑成二维数组。


```python
import numpy as np

arr = np.arange(12)
print( arr )
print( arr.reshape(4,3) )
```

    [ 0  1  2  3  4  5  6  7  8  9 10 11]
    [[ 0  1  2]
     [ 3  4  5]
     [ 6  7  8]
     [ 9 10 11]]
    

>思考：为什么重塑后的数组不是

    [[ 0 4 8]
     [ 1 5 9]
     [ 2 6 10]
     [ 3 7 11]]


当你重塑高维矩阵时，不想花时间算某一维度的元素个数时，可以用「-1」取代，程序会自动帮你计算出来。比如把 12 个元素重塑成 (2, 6)，你可以写成 (2,-1) 或者 (-1, 6)。



```python
print( arr.reshape((2,-1)) )
print( arr.reshape((-1,6)) )
```

    [[ 0  1  2  3  4  5]
     [ 6  7  8  9 10 11]]
    [[ 0  1  2  3  4  5]
     [ 6  7  8  9 10 11]]
    

#### 打平

用 ravel() 或flatten() 函数将二维数组 arr 打平成一维数组。


```python
arr = np.arange(12).reshape((4,3))
print( arr )

ravel_arr = arr.ravel()
print( ravel_arr )

flatten_arr = arr.flatten()
print( flatten_arr )
```

    [[ 0  1  2]
     [ 3  4  5]
     [ 6  7  8]
     [ 9 10 11]]
    [ 0  1  2  3  4  5  6  7  8  9 10 11]
    [ 0  1  2  3  4  5  6  7  8  9 10 11]
    

>思考：为什么打平后的数组不是

    [ 0 3 6 9 1 4 7 10 2 5 8 11]

要回答本节两个问题，需要了解 numpy 数组在内存块的存储方式。

#### 行主序和列主序

行主序 (row-major order) 指每行的元素在内存块中彼此相邻，而列主序 (column-major order) 指每列的元素在内存块中彼此相邻。

在众多计算机语言中，

默认行主序的有 **C** 语言(下图 order=‘**C**’ 等价于行主序)

默认列主序的有 **F**ortran 语言(下图 order=‘**F**’ 等价于列主序)

![](https://pic./2020/05/22/b605b92f359ab.png)

在 numpy 数组中，默认的是行主序，即 order ='C'。现在可以回答本节那两个问题了。

如果你真的想在「重塑」和「打平」时用列主序，只用把 order 设为 'F'，以重塑举例：


```python
print( np.arange(12).reshape((4,3), order='F') )
```

    [[ 0  4  8]
     [ 1  5  9]
     [ 2  6 10]
     [ 3  7 11]]
    

细心的读者可能已经发现为什么「打平」需要两个函数 ravel() 或 flatten()？它们的区别在哪里？

#### <font color="red"><b>知识点</b></font>

函数 ravel() 或 flatten() 的不同之处是

**1. ravel() 按「行主序」打平时<font color="red"><b>没有复制</b></font>原数组，按「列主序」在打平时<font color="red"><b>复制了</b></font>原数组**

**2. flatten() 在打平时<font color="red"><b>复制了</b></font>原数组**

用代码验证一下，首先看 flatten()，将打平后的数组 flatten 第一个元素更新为 10000，并没有对原数组 arr 产生任何影响 (证明 flatten() 是复制了原数组)



```python
arr = np.arange(6).reshape(2,3)
print( arr )
flatten = arr.flatten()
print( flatten )
flatten_arr[0] = 10000
print( arr )
```

    [[0 1 2]
     [3 4 5]]
    [0 1 2 3 4 5]
    [[0 1 2]
     [3 4 5]]
    

再看 ravel() 在「列主序」打平，将打平后的数组 ravel_F 第一个元素更新为 10000，并没有对原数组 arr 产生任何影响 (证明 ravel(order='F') 是复制了原数组)



```python
ravel_F = arr.ravel( order='F' )
ravel_F[0] = 10000
print( ravel_F )
print( arr )
```

    [10000     3     1     4     2     5]
    [[0 1 2]
     [3 4 5]]
    

最后看 ravel() 在「行主序」打平，将打平后的数组 ravel_C 第一个元素更新为 10000，原数组 arr[0][0] 也变成了 10000 (证明 ravel() 没有复制原数组)



```python
ravel_C = arr.ravel()
ravel_C[0] = 10000
print( ravel_C )
print( arr )
```

    [10000     1     2     3     4     5]
    [[10000     1     2]
     [    3     4     5]]
    

### 4.2 合并和分裂

合并 (concatenate, stack) 和分裂 (split) 这两个操作仅仅只改变数组的分合

* 合并是多合一

* 分裂是一分多

#### 合并

使用「合并」函数有两种选择

1. 有通用的 concatenate

2. 有专门的 vstack, hstack, dstack

用下面两个数组来举例：


```python
arr1 = np.array([[1, 2, 3], [4, 5, 6]])
arr2 = np.array([[7, 8, 9], [10, 11, 12]])
```

#### <font color="red"><b>concatenate</b></font>


```python
np.concatenate([arr1, arr2], axis=0)
```




    array([[ 1,  2,  3],
           [ 4,  5,  6],
           [ 7,  8,  9],
           [10, 11, 12]])




```python
np.concatenate([arr1, arr2], axis=1)
```




    array([[ 1,  2,  3,  7,  8,  9],
           [ 4,  5,  6, 10, 11, 12]])



在 concatenate() 函数里通过设定轴，来对数组进行竖直方向合并 (轴 0) 和水平方向合并 (轴 1)。 

#### <font color="red"><b>vstack, hstack, dstack</b></font>

通用的东西是好，但是可能效率不高，NumPy 里还有专门合并的函数

* vstack：v 代表 vertical，竖直合并，等价于 concatenate(axis=0)

* hstack：h 代表 horizontal，水平合并，等价于 concatenate(axis=1)

* dstack：d 代表 depth-wise，按深度合并，深度有点像彩色照片的 RGB 通道

一图胜千言：

![](https://pic./2020/05/22/55144614b11c0.png)

用代码验证一下：


```python
print( np.vstack((arr1, arr2)) )
print( np.hstack((arr1, arr2)) )
print( np.dstack((arr1, arr2)) )
```

    [[ 1  2  3]
     [ 4  5  6]
     [ 7  8  9]
     [10 11 12]]
    [[ 1  2  3  7  8  9]
     [ 4  5  6 10 11 12]]
    [[[ 1  7]
      [ 2  8]
      [ 3  9]]
    
     [[ 4 10]
      [ 5 11]
      [ 6 12]]]
    

和 vstack, hstack 不同，dstack 将原数组的维度增加了一维。


```python
np.dstack((arr1, arr2)).shape
```




    (2, 3, 2)



#### 分裂

使用「分裂」函数有两种选择

1. 有通用的 split

2. 有专门的 hsplit, vsplit

用下面数组来举例：


```python
arr = np.arange(25).reshape((5,5))
print( arr )
```

    [[ 0  1  2  3  4]
     [ 5  6  7  8  9]
     [10 11 12 13 14]
     [15 16 17 18 19]
     [20 21 22 23 24]]
    

#### <font color="red"><b>split</b></font>

和 concatenate() 函数一样，我们可以在 split() 函数里通过设定轴，来对数组沿着竖直方向分裂 (轴 0) 和沿着水平方向分裂 (轴 1)。


```python
first, second, third = np.split(arr,[1,3])
print( 'The first split is', first )
print( 'The second split is', second )
print( 'The third split is', third )
```

    The first split is [[0 1 2 3 4]]
    The second split is [[ 5  6  7  8  9]
     [10 11 12 13 14]]
    The third split is [[15 16 17 18 19]
     [20 21 22 23 24]]
    

split() 默认沿着轴 0 分裂，其第二个参数 [1, 3] 相当于是个切片操作，将数组分成三部分：

* 第一部分 - :1 (即第 1 行)

* 第二部分 - 1:3 (即第 2 到 3 行)

* 第二部分 - 3:  (即第 4 到 5 行)

#### <font color="red"><b>hsplit, vsplit</b></font>

vsplit() 和 split(axis=0) 等价，hsplit() 和 split(axis=1) 等价。一图胜千言：

![](https://pic./2020/05/22/0df0097a1a1d9.png)

为了和上面不重复，我们只看 hsplit。


```python
first, second, third = np.hsplit(arr,[1,3])
print( 'The first split is', first )
print( 'The second split is', second )
print( 'The third split is', third )
```

    The first split is [[ 0]
     [ 5]
     [10]
     [15]
     [20]]
    The second split is [[ 1  2]
     [ 6  7]
     [11 12]
     [16 17]
     [21 22]]
    The third split is [[ 3  4]
     [ 8  9]
     [13 14]
     [18 19]
     [23 24]]
    

### 4.3 重复和拼接

重复 (repeat) 和拼接 (tile) 这两个操作本质都是复制

* 重复是在元素层面复制

* 拼接是在数组层面复制



#### 重复


函数 repeat() 复制的是数组的每一个元素，参数有几种设定方法：

* 一维数组：用标量和列表来复制元素的个数

* 多维数组：用标量和列表来复制元素的个数，用轴来控制复制的行和列

<font color="red"><b>标量</b></font>


```python
arr = np.arange(3)
print( arr )
print( arr.repeat(3) )
```

    [0 1 2]
    [0 0 0 1 1 1 2 2 2]
    

标量参数 3 - 数组 arr 中每个元素复制 3 遍。

<font color="red"><b>列表</b></font>


```python
print( arr.repeat([2,3,4]) )
```

    [0 0 1 1 1 2 2 2 2]
    

列表参数 [2,3,4] - 数组 arr 中每个元素分别复制 2, 3, 4 遍。

<font color="red"><b>标量和轴</b></font>


```python
arr2d = np.arange(6).reshape((2,3))
print( arr2d )
print( arr2d.repeat(2, axis=0) )
```

    [[0 1 2]
     [3 4 5]]
    [[0 1 2]
     [0 1 2]
     [3 4 5]
     [3 4 5]]
    

标量参数 2 和轴 0 - 数组 arr2d 中每个元素沿着轴 0 复制 2 遍。

<font color="red"><b>列表和轴</b></font>


```python
print( arr2d.repeat([2,3,4], axis=1) )
```

    [[0 0 1 1 1 2 2 2 2]
     [3 3 4 4 4 5 5 5 5]]
    

列表参数 [2,3,4] 和轴 1 - 数组 arr2d 中每个元素沿着轴 1 分别复制 2, 3, 4 遍。

#### 拼接

函数 tile() 复制的是数组本身，参数有几种设定方法：

* 标量：把数组当成一个元素，一列一列复制

* 形状：把数组当成一个元素，按形状复制

<font color="red"><b>标量</b></font>


```python
arr2d = np.arange(6).reshape((2,3))
print( arr2d )
print( np.tile(arr2d,2) )
```

    [[0 1 2]
     [3 4 5]]
    [[0 1 2 0 1 2]
     [3 4 5 3 4 5]]
    

标量参数 2 - 数组 arr 按列复制 2 遍。

<font color="red"><b>形状</b></font>

tile 是瓷砖的意思，顾名思义，这个函数就是把数组像瓷砖一样铺展开来。


```python
arr2d2 = np.array([[1,2], [3, 4]])
print( arr2d2 )
```

    [[1 2]
     [3 4]]
    

**横向**

![](https://pic./2020/05/22/0a88690672dfb.png)


```python
np.tile(arr2d2, (1,4))    # 与 np.tile(arr2d2, 4) 等价
```




    array([[1, 2, 1, 2, 1, 2, 1, 2],
           [3, 4, 3, 4, 3, 4, 3, 4]])



**纵向**

![](https://pic./2020/05/22/b17e29bbe81eb.png)


```python
np.tile(arr2d2, (3,1))
```




    array([[1, 2],
           [3, 4],
           [1, 2],
           [3, 4],
           [1, 2],
           [3, 4]])



**横向+纵向**

![](https://pic./2020/05/22/fe15217fcf826.png)


```python
print( np.tile(arr2d2, (3,4)) )
```

    [[1 2 1 2 1 2 1 2]
     [3 4 3 4 3 4 3 4]
     [1 2 1 2 1 2 1 2]
     [3 4 3 4 3 4 3 4]
     [1 2 1 2 1 2 1 2]
     [3 4 3 4 3 4 3 4]]
    

形状参数 (3,4) - 数组 arr 按形状复制 12 (3×4) 遍，并以 (3,4) 的形式展现。

### 4.4 其他操作

本节讨论数组的其他操作，包括排序 (sort)，插入 (insert)，删除 (delete) 和复制 (copy)。

#### 排序

排序包括直接排序 (direct sort) 和间接排序 (indirect sort)。

<font color="red"><b>直接排序</b></font>


```python
arr = np.array([5,3,2,6,1,4])
print( 'Before sorting', arr )
arr.sort()
print( 'After sorting', arr )
```

    Before sorting [5 3 2 6 1 4]
    After sorting [1 2 3 4 5 6]
    

sort()函数是按升序 (ascending order) 排列的，该函数里没有参数可以控制 order，因此你想要按降序排列的数组，只需


```python
print( arr[::-1] )
```

    [6 5 4 3 2 1]
    

现在让人困惑的地方来了。

<font color="red"><b>知识点</b></font>

用来排序 numpy 用两种方式：

1. arr.sort()#原址排序

2. np.sort( arr )#副本排序

第一种 sort 会改变 arr，第二种 sort 在排序时创建了 arr 的一个复制品，不会改变 arr。看下面代码，用一个形状是 (3, 4) 的「二维随机整数」数组来举例，用整数是为了便于读者好观察排序前后的变化：


```python
arr = np.random.randint( 40, size=(3,4) )
print( arr )
```

    [[10  3 38 38]
     [27 32 14 11]
     [11  5 11  0]]
    

第一种 arr.sort()，对第一列排序，发现 arr 的元素**改变**了。


```python
arr[:, 0].sort() 
print( arr )
```

    [[10  3 38 38]
     [11 32 14 11]
     [27  5 11  0]]
    

第二种 np.sort(arr)，对第二列排序，但是 arr 的元素**不变**。


```python
np.sort(arr[:,1])
```




    array([ 3,  5, 32])




```python
print( arr )
```

    [[10  3 38 38]
     [11 32 14 11]
     [27  5 11  0]]
    

此外也可以在不同的轴上排序，对于二维数组，在「轴 0」上排序是「跨行」排序，在「轴 1」上排序是「跨列」排序。


```python
arr.sort(axis=1)
print( arr )
```

    [[ 3 10 38 38]
     [11 11 14 32]
     [ 0  5 11 27]]
    

<font color="red"><b>间接排序</b></font>

有时候我们不仅仅只想排序数组，还想在排序过程中<font color="red"><b>提取每个元素在原数组对应的索引</b></font>(index)，这时 argsort() 就派上用场了。以排列下面五个学生的数学分数为例：


```python
score = np.array([100, 60, 99, 80, 91])
idx = score.argsort()#得到排序索引
print( idx )
```

    [1 3 4 2 0]
    

这个 idx = [1 3 4 2 0] 怎么理解呢？很简单，排序完之后分数应该是 [60 80 91 99 100]，

* 60，即 score[1] 排在第0位， 因此 idx[0] =1

* 80，即 score[3] 排在第1 位， 因此 idx[1] =3

* 91，即 score[4] 排在第2 位， 因此 idx[2] =4

* 99，即 score[2] 排在第3 位， 因此 idx[3] =2

* 100，即 score[0] 排在第4 位， 因此 idx[4] =0

用这个 idx 对 score 做一个「花式索引」得到 (还记得上贴的内容吗？)


```python
print( score[idx] )
```

    [ 60  80  91  99 100]
    

再看一个二维数组的例子。


```python
arr = np.random.randint( 40, size=(3,4) )
print( arr )
```

    [[38 14 23 19]
     [15  8 38 37]
     [ 4  0 21 23]]
    

对其第一行 arr[0] 排序，获取索引，在应用到所用行上。


```python
arr[:, arr[0].argsort()]
```




    array([[14, 19, 23, 38],
           [ 8, 37, 38, 15],
           [ 0, 23, 21,  4]])



这不就是「花式索引」吗？来我们分解一下以上代码，先看看索引。传入[索引]


```python
print( arr[0].argsort() )
```

    [1 3 2 0]
    

「花式索引」来了，结果和上面一样的。


```python
arr[:, [2, 0, 3, 1]]
```




    array([[23, 38, 19, 14],
           [38, 15, 37,  8],
           [21,  4, 23,  0]])



#### 插入和删除

和列表一样，我们可以给 numpy 数组

* 用insert()函数在某个特定位置之前插入元素

* 用delete()函数删除某些特定元素


```python
a = np.arange(6)
print( a )
print( np.insert(a, 1, 100) )
print( np.delete(a, [1,3]) )
```

    [0 1 2 3 4 5]
    [  0 100   1   2   3   4   5]
    [0 2 4 5]
    

#### 复制

用copy()函数来复制数组 a 得到 a_copy，很明显，改变 a_copy 里面的元素不会改变 a。


```python
a = np.arange(6)
a_copy = a.copy()
print( 'Before changing value, a is', a )
print( 'Before changing value, a_copy is', a_copy )
a_copy[-1] = 99
print( 'After changing value, a_copy is', a_copy )
print( 'After changing value, a is', a )
```

    Before changing value, a is [0 1 2 3 4 5]
    Before changing value, a_copy is [0 1 2 3 4 5]
    After changing value, a_copy is [ 0  1  2  3  4 99]
    After changing value, a is [0 1 2 3 4 5]
    

## 5数组的计算

本节介绍**两大类**数组计算，具体有

1. 元素层面 (element-wise) 计算

2. 广播机制 (broadcasting) 计算



### 5.1 元素层面计算

Numpy 数组元素层面计算包括：

1. 二元运算 (binary operation)：加减乘除

2. 数学函数：倒数、平方、指数、对数

3. 比较运算 (comparison)

先定义两个数组 arr1 和 arr2。


```python
arr1 = np.array([[1., 2., 3.], [4., 5., 6.]])
arr2 = np.ones((2,3)) * 2
print( arr1 )
print( arr2 )
```

    [[1. 2. 3.]
     [4. 5. 6.]]
    [[2. 2. 2.]
     [2. 2. 2.]]
    

<font color="red"><b>加、减、乘、除</b></font>


```python
print( arr1 + arr2 + 1 )
print( arr1 - arr2 )
print( arr1 * arr2 )
print( arr1 / arr2 )
```

    [[4. 5. 6.]
     [7. 8. 9.]]
    [[-1.  0.  1.]
     [ 2.  3.  4.]]
    [[ 2.  4.  6.]
     [ 8. 10. 12.]]
    [[0.5 1.  1.5]
     [2.  2.5 3. ]]
    

<font color="red"><b>倒数、平方、指数、对数</b></font>


```python
print( 1 / arr1 )
print( arr1 ** 2 )
print( np.exp(arr1) )
print( np.log(arr1) )
```

    [[1.         0.5        0.33333333]
     [0.25       0.2        0.16666667]]
    [[ 1.  4.  9.]
     [16. 25. 36.]]
    [[  2.71828183   7.3890561   20.08553692]
     [ 54.59815003 148.4131591  403.42879349]]
    [[0.         0.69314718 1.09861229]
     [1.38629436 1.60943791 1.79175947]]
    

<font color="red"><b>比较</b></font>


```python
arr1 > arr2
arr1 > 3
```




    array([[False, False, False],
           [ True,  True,  True]])



从上面结果可知

* 「数组和数组间的二元运算」都是在元素层面上进行的

* 「作用在数组上的数学函数」都是作用在数组的元素层面上的。

* 「数组和数组间的比较」都是在元素层面上进行的

但是在「数组和标量间的比较」时，python 好像先把 3 复制了和 arr1 形状一样的数组 [[3,3,3], [3,3,3]]，然后再在元素层面上作比较。上述这个复制标量的操作叫做「广播机制」，是 NumPy 里最重要的一个特点，在下一节会详细讲到。

### 5.2 广播机制计算

#### 广播的引出

当两个数组的形状并不相同的时候，我们可以通过扩展数组的方法来实现相加、相减、相乘等操作，这种机制叫做广播（broadcasting）。

比如，一个二维数组减去列平均值，来对数组的每一列进行距平化处理：


```python
arr = np.random.randn(4,3)  #shape(4,3)
arr_mean = arr.mean(0)      #shape(3,)
demeaned = arr -arr_mean

print(arr)
print(arr_mean)
print(demeaned)
```

    [[ 0.48226402  1.20876697 -0.67351982]
     [ 0.65606798 -1.16182488 -1.68726346]
     [-0.92629614  0.49865982 -0.07100581]
     [ 0.73134776 -0.28327924  0.14857151]]
    [ 0.23584591  0.06558067 -0.5708044 ]
    [[ 0.24641812  1.1431863  -0.10271542]
     [ 0.42022208 -1.22740555 -1.11645907]
     [-1.16214204  0.43307915  0.49979858]
     [ 0.49550185 -0.34885991  0.71937591]]
    

很明显上式arr和arr_mean维度并不形同，但是它们可以进行相减操作，这就是通过广播机制来实现的。

**广播的原则**

**如果两个数组的后缘维度（trailing dimension，即从末尾开始算起的维度）的轴长度相符，或其中的一方的长度为1**，则认为它们是广播兼容的。广播会在缺失和（或）长度为1的维度上进行。

这句话乃是理解广播的核心。广播主要发生在两种情况，一种是两个数组的维数不相等，但是它们的后缘维度的轴长相符，另外一种是有一方的长度为1。

**<font color="red">数组维度不同，后缘维度的轴长相符</font>**

我们来看一个例子：


```python
import numpy as np
arr1 = np.array([[0, 0, 0],[1, 1, 1],[2, 2, 2], [3, 3, 3]])  #arr1.shape = (4,3)
arr2 = np.array([1, 2, 3])    #arr2.shape = (3,)
arr_sum = arr1 + arr2
print(arr_sum)
```

    [[1 2 3]
     [2 3 4]
     [3 4 5]
     [4 5 6]]
    

上例中arr1的shape为（4,3），arr2的shape为（3，）。可以说前者是二维的，而后者是一维的。但是它们的后缘维度相等，arr1的第二维长度为3，和arr2的维度相同。arr1和arr2的shape并不一样，但是它们可以执行相加操作，这就是通过广播完成的，在这个例子当中是将arr2沿着0轴进行扩展。

上面程序当中的广播如下图所示(**一维数据在轴0上的广播**):

![](https://pic./2020/05/22/32fa6840c39a3.png)

同样的例子还有(**三维数据在轴0上的广播**)：

![](https://pic./2020/05/22/0275b84b7cdd2.png)

从上面的图可以看到，（3,4,2）和（4,2）的维度是不相同的，前者为3维，后者为2维。但是它们后缘维度的轴长相同，都为（4,2），所以可以沿着0轴进行广播。

同样，还有一些例子：（4,2,3）和（2,3）是兼容的，（4,2,3）还和（3）是兼容的，后者需要在两个轴上面进行扩展。

**<font color="red">数组维度相同，其中有个轴为1</font>**

我们来看下面的例子：


```python
import numpy as np

arr1 = np.array([[0, 0, 0],[1, 1, 1],[2, 2, 2], [3, 3, 3]])  #arr1.shape = (4,3)
arr2 = np.array([[1],[2],[3],[4]])    #arr2.shape = (4, 1)

arr_sum = arr1 + arr2
print(arr_sum)
```

    [[1 1 1]
     [3 3 3]
     [5 5 5]
     [7 7 7]]
    

arr1的shape为（4,3），arr2的shape为（4,1），它们都是二维的，但是第二个数组在1轴上的长度为1，所以，可以在1轴上面进行广播，如下图所示(**二维数组在轴1上的广播**)：

![](https://pic./2020/05/22/fc69ffdf4fbb2.png)

在这种情况下，两个数组的维度要保证相等，其中有一个轴的长度为1，这样就会沿着长度为1的轴进行扩展。这样的例子还有：（4,6）和（1,6） 。（3,5,6）和（1,5,6）、（3,1,6）、（3,5,1），后面三个分别会沿着0轴，1轴，2轴进行广播。

人们经常需要通过算术运算过程将较低维度的数组在除0轴以外的其他轴向上广播。根据广播的原则，较小数组的“广播维”必须为1。

对于三维的情况，在三维中的任何一维上广播其实也就是将数据重塑为兼容的形状而已。下图说明了要在三维数组各维度上广播的形状需求(**能在该三维数组上广播的二维数组的形状**)。

![](https://pic./2020/05/22/fd09423ea77c9.png)

## 6 总结

NumPy 篇终于完结！即上贴讨论过的数组创建、数组存载和数组获取，本贴讨论了数组变形、数组计算。

数组变形有以下重要操作：

* 改变维度的重塑和打平

* 改变分合的合并和分裂

* 复制本质的重复和拼接

* 其他排序插入删除复制

数组计算有以下重要操作：

1. 元素层面：四则运算、函数，比较

2. 广播机制：太重要了，大量用于科学计算和机器学习中！

![结束](https://pic./2020/05/12/f0e6dce0060f9.png)

