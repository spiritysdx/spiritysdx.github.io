# 传统图机器学习和图特征工程(节点层面的特征工程)


## 前言

这里就是解决之前生成D维向量的问题，使用人工设置特征，后续会使用GNN也就是图神经网络自动学习特征而不再需要人工设置特征了，这块等同于是被替代掉的工作，但由于后续的自动特征提取还是起源于这里的，所以还是需要了解一下。

## 设计特征

节点特征、连接特征、子图/全图特征 都需要转换为向量形式输入后续的神经网络训练，这些向量组成了各种矩阵。

![https://github.com/spiritysdx/images/blob/main/20230918/21.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230918/21.png?raw=true)

特征分为两种：

①节点自己的有的特征，称为```属性特征```，很多时候这些特征都是多模态的，属性本身就有很多类别

![https://github.com/spiritysdx/images/blob/main/20230918/22.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230918/22.png?raw=true)

②```连接特征```，节点在图中与其他节点的连接关系，也就是所谓的```结构信息```

![https://github.com/spiritysdx/images/blob/main/20230918/23.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230918/23.png?raw=true)

## 训练通道

人工选取模型可能更容易学习的特征，构造人工特征，就叫做特征工程。

![https://github.com/spiritysdx/images/blob/main/20230918/24.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230918/24.png?raw=true)

构造的人工特征可能有用可能没用，需要后面的机器学习自己筛选，重要的特征机器学习会选出重要的特征给出更大的权重。

![https://github.com/spiritysdx/images/blob/main/20230918/25.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230918/25.png?raw=true)

## 节点层面的特征工程

需要构建D维向量，且质量需要足够高，后续的机器学习才能学习到好的特征。

![https://github.com/spiritysdx/images/blob/main/20230918/26.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230918/26.png?raw=true)

后续以节点分类场景为例(以下为半监督节点分类问题)

![https://github.com/spiritysdx/images/blob/main/20230918/27.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230918/27.png?raw=true)

结构特征(连接特征)分为以下几类：

1. 节点的度
2. 节点的重要程度
3. 节点的聚集系数
4. 节点的子图模式(从小波变换中得来，分解复杂的图为简单图的合集)

**节点的度 - 度中心度 - degree centrality**

以下是节点的重要程度的示例：(某些节点的度一样，但实际的重要程度可能不一样)

![https://github.com/spiritysdx/images/blob/main/20230918/28.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230918/28.png?raw=true)

以下是按行求和，得到节点的连接数做特征：(某些节点的度可以当作节点的重要程度)

![https://github.com/spiritysdx/images/blob/main/20230918/29.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230918/29.png?raw=true)

连接数就是无向图的度，度也是可以做重要度的。

**节点的特征值/特征向量 - 特征值中心度 - eigenvalue centrality**

![https://github.com/spiritysdx/images/blob/main/20230918/30.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230918/30.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20230918/31.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230918/31.png?raw=true)

节点的重要程度 = 邻居们的重要程度求和的平均 = 求邻接矩阵的特征向量 = 此时总会存在一个唯一的且为正的最大特征值(求邻接矩阵的特征值)

这里和PageRank的算法有点像，但PageRank是需要根据连接来等分重要度的，这里的重要度却是不等分的，比如一个节点有重要度1，连接数是3，那么PageRank是分给每个连接分到1/3，这里却是每个连接分到1。

以下是度的中心度和特征值中心度的区别：

![https://github.com/spiritysdx/images/blob/main/20230918/32.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230918/32.png?raw=true)

很明显，特征值中心度更好的体现了节点的重要程度，因为哪怕某个节点连接数很多，也不一定重要。

**节点间的最短路径 - 最短距离路线经过的数量 - betweenness centrality**

![https://github.com/spiritysdx/images/blob/main/20230918/33.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230918/33.png?raw=true)

这里只有分子需要计算且有不同，因为分母在同一个图的话值都一样。

**节点的最短距离 - closeness centrality**

节点到别的节点最短的距离之和的倒数。

这个值哪个节点的大，这个节点去哪都近。

![https://github.com/spiritysdx/images/blob/main/20230918/34.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230918/34.png?raw=true)

**集群系数 - clustering coefficient**

衡量一个节点周围的节点有多抱团

![https://github.com/spiritysdx/images/blob/main/20230918/35.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230918/35.png?raw=true)

分母是```周围(和该节点相连)```节点有几对(两两一对)，分子是这些对中```实际也相连```的对数。

这种网络又叫```自我中心网络```，也就是数三角形个数。

![https://github.com/spiritysdx/images/blob/main/20230918/36.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230918/36.png?raw=true)

这里预定义的子图是三角形，实际也可以换成别的图形做子图。

![https://github.com/spiritysdx/images/blob/main/20230918/37.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230918/37.png?raw=true)

**子图模式 - graphlets**

![https://github.com/spiritysdx/images/blob/main/20230918/38.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230918/38.png?raw=true)

类似同分异构体查找，然后统计每种模式的频率，构成频率向量。

![https://github.com/spiritysdx/images/blob/main/20230918/39.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230918/39.png?raw=true)

实际都是在数数，数的是不同东西的数量罢了。

![https://github.com/spiritysdx/images/blob/main/20230918/40.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230918/40.png?raw=true)

## 后言

图的人工特征总结如下

![https://github.com/spiritysdx/images/blob/main/20230918/41.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230918/41.png?raw=true)

重要度特征 & 结构特征

