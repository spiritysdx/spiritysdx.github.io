# 传统图机器学习和图特征工程(连接层面的特征工程)


## 前言

通过已知连接补全未知连接

![https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230919/1.png?raw=true](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230919/1.png?raw=true)

## 连接预测

![https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230919/2.png?raw=true](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230919/2.png?raw=true)

![https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230919/3.png?raw=true](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230919/3.png?raw=true)

## 连接的特征

![https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230919/4.png?raw=true](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230919/4.png?raw=true)

**两节点的距离特征**

两节点的最短路径长度：

![https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230919/5.png?raw=true](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230919/5.png?raw=true)

两节点之间的最短路径长度可能一样，但经过的节点不一样，那么可能有用的信息就不同了，所以只用最短路径长度是不够的。

**两节点的局部连接信息特征**

![https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230919/6.png?raw=true](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230919/6.png?raw=true)

可能两节点之间没有共同好友，那么上述的节点局部连接信息就没有意义了。

![https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230919/7.png?raw=true](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230919/7.png?raw=true)

**两节点的全图连接信息特征**

![https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230919/8.png?raw=true](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230919/8.png?raw=true)

邻接矩阵的n次幂表示路径长度为n的路径(假设每条路径长度均为1)

![https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230919/9.png?raw=true](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230919/9.png?raw=true)

![https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230919/10.png?raw=true](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230919/10.png?raw=true)

![https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230919/11.png?raw=true](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230919/11.png?raw=true)

以上三图为```Katz index```卡兹系数的直观推导过程，下面是数学推导过程：

![https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230919/12.png?raw=true](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230919/12.png?raw=true)

## 后言

总结：

![https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230919/13.png?raw=true](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230919/13.png?raw=true)

