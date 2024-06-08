# 传统图机器学习和图特征工程(全图层面的特征工程)


## 前言

需要提取特征反映全图结构特定

![https://github.com/spiritysdx/images/blob/main/20230919/14.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230919/14.png?raw=true)

依然是在数数字

## Bag-of-Words - (BoW)

数节点是否存在，存在为0，不存在为1：

![https://github.com/spiritysdx/images/blob/main/20230919/15.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230919/15.png?raw=true)

数节点度数为xx的节点有几个：

![https://github.com/spiritysdx/images/blob/main/20230919/16.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230919/16.png?raw=true)

拓展数的东西，这里可以拓展到数任何图的特征，比如图的自定义子图模式：

![https://github.com/spiritysdx/images/blob/main/20230919/17.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230919/17.png?raw=true)

全图层面和节点层面的graphlet的不同之处：

![https://github.com/spiritysdx/images/blob/main/20230919/18.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230919/18.png?raw=true)

graphlet 向量矩阵的构建方法：

![https://github.com/spiritysdx/images/blob/main/20230919/19.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230919/19.png?raw=true)

graphlet 向量的构建：

![https://github.com/spiritysdx/images/blob/main/20230919/20.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230919/20.png?raw=true)

两图的相似度/匹配度(graphlet kernel)(两图大小不同需要归一化)：

![https://github.com/spiritysdx/images/blob/main/20230919/21.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230919/21.png?raw=true)

缺陷：

需要枚举匹配子图模式，计算的量太大了，计算复杂度高

![https://github.com/spiritysdx/images/blob/main/20230919/22.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230919/22.png?raw=true)

## Weisfeiler-Lehman Graph Kernel - 威斯费勒-莱曼核

颜色微调法：

![https://github.com/spiritysdx/images/blob/main/20230919/23.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230919/23.png?raw=true)

具体操作方法：

初始化大家都为1(同色)

![https://github.com/spiritysdx/images/blob/main/20230919/24.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230919/24.png?raw=true)

求本节点的邻居数值列表

![https://github.com/spiritysdx/images/blob/main/20230919/25.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230919/25.png?raw=true)

求和作为本节点的数值(注意，这里求和的时候涵盖自身节点的数值，也涵盖邻居数值)

![https://github.com/spiritysdx/images/blob/main/20230919/26.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230919/26.png?raw=true)

再次求本节点的邻居数值列表(哈希表由两图共同构成)

![https://github.com/spiritysdx/images/blob/main/20230919/27.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230919/27.png?raw=true)

抽象为向量的写法：

![https://github.com/spiritysdx/images/blob/main/20230919/28.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230919/28.png?raw=true)

算 威斯费勒-莱曼核(Weisfeiler-Lehman Kernel) 的值：

![https://github.com/spiritysdx/images/blob/main/20230919/29.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230919/29.png?raw=true)

(W-L Kernel)总结：

构建哈希表，数哈希表编号在对应图中的个数

进行了K步 = 捕捉了图中K跳的连接

![https://github.com/spiritysdx/images/blob/main/20230919/30.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230919/30.png?raw=true)

步数越多捕捉的跳数越多，范围就越广

![https://github.com/spiritysdx/images/blob/main/20230919/31.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230919/31.png?raw=true)

该方法与连接的个数线性相关，与节点个数线性相关，算法复杂度为线性。

如果要计算对应每个节点的向量，需要使用```gklearn```库计算

## 后言

总结：

![https://github.com/spiritysdx/images/blob/main/20230919/32.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230919/32.png?raw=true)

W-L Kernal 方法和 GNN 图神经网络有微妙的联系。

![https://github.com/spiritysdx/images/blob/main/20230919/33.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230919/33.png?raw=true)

两张图的向量做数量积做点乘得到一个实数的标量，这种方法叫做核方法(Kernel Method)。

这种方法在传统图机器学习中用的特别多(两张图的标量代表图结构，而不是分开用两个向量去代表图结构了)。

