# 图的基本表示


## 图基础

![https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230918/1.png?raw=true](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230918/1.png?raw=true)

![https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230918/2.png?raw=true](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230918/2.png?raw=true)

本体图是导入图之前就应该设计好的，本体图和具体图的关系类同类和实例的关系。如何设计本体图取决于将来的图的用途，比如图的用途是分析，那么本体图的设计应该包含所有图的属性，比如节点和边的属性，以及节点和边的类型，比如节点是用户还是物品，边是用户对物品感兴趣还是用户对用户感兴趣。

![https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230918/3.png?raw=true](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230918/3.png?raw=true)

![https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230918/4.png?raw=true](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230918/4.png?raw=true)

## 图分类

有向图 & 无向图

![https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230918/5.png?raw=true](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230918/5.png?raw=true)

异质图

![https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230918/6.png?raw=true](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230918/6.png?raw=true)

如果异质图中节点只分为两类，则称为二部图(二分图)

![https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230918/7.png?raw=true](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230918/7.png?raw=true)

二部图可分别展开为图

![https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230918/8.png?raw=true](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230918/8.png?raw=true)

![https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230918/9.png?raw=true](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230918/9.png?raw=true)

## 图的属性

图节点的度

![https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230918/10.png?raw=true](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230918/10.png?raw=true)

无向图 - 度

有向图 - 入度 & 出度

节点的度一定程度上表现了图中节点的重要性

## 图的表示形式

**邻接矩阵**

无向图不分起点终点，是对称矩阵

有向图中行是起点，列是终点，一般不是对称矩阵

![https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230918/11.png?raw=true](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230918/11.png?raw=true)

无向图求度，求某个点的度按行或按列求和即可，总度数求整体度看上/下对角阵即可

有向图求度，求某个点的出度按行求和，入度按列求和，总度数求整体和即可

![https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230918/12.png?raw=true](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230918/12.png?raw=true)

实际矩阵会是稀疏矩阵，不能直接用邻接矩阵表示

**连接列表**

![https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230918/13.png?raw=true](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230918/13.png?raw=true)

**邻接列表**

![https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230918/14.png?raw=true](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230918/14.png?raw=true)

**带权重的图**

![https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230918/15.png?raw=true](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230918/15.png?raw=true)

**自环图和多通路图**

![https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230918/16.png?raw=true](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230918/16.png?raw=true)

## 图的连通性

![https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230918/17.png?raw=true](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230918/17.png?raw=true)

![https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230918/18.png?raw=true](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230918/18.png?raw=true)

强连通图和弱连通图

![https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230918/19.png?raw=true](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230918/19.png?raw=true)

强连通域(SCC)

![https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230918/20.png?raw=true](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230918/20.png?raw=true)

