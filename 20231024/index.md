# GAT 详解


## GAT

原文：

![链接](https://github.com/spiritysdx/images/blob/main/20231022/52.png?raw=true)

## 流程

![链接](https://github.com/spiritysdx/images/blob/main/20231022/73.png?raw=true)

![链接](https://github.com/spiritysdx/images/blob/main/20231022/74.png?raw=true)

上面这个是计算每个邻居节点和当前节点之间的重要性，下面这里是softmax这些值，计算每个节点和当前节点之间的权重，权重和为1.

![链接](https://github.com/spiritysdx/images/blob/main/20231022/75.png?raw=true)

![链接](https://github.com/spiritysdx/images/blob/main/20231022/76.png?raw=true)

上面这里的a是一个函数，进去两个嵌入，吐出e(重要性)(也可以是一个新嵌入)。

这个a函数是可以自行设定的，上面示例中是直接concat后做一个线性变换这两步加起来叫做a函数，实际可以替换为别的方式。

![链接](https://github.com/spiritysdx/images/blob/main/20231022/77.png?raw=true)

上面这种机制相当于同时学习同一对节点，不同a函数(不同的重要性计算方式)的重要性，然后最后再聚合起来。

## 总结

![链接](https://github.com/spiritysdx/images/blob/main/20231022/78.png?raw=true)

优点：

1. 每对节点可并行计算。

2. 这里的a函数是可学习的，在迁移新图时，原有的参数的模型会保存有一些关系，这些关系在新图中部分存在，会减少新图的训练时间，是可共享的参数。

3. 可以应用在完全没有训练过的新图上，因为某些重要性的关系是类似或者相同的，原有的a函数模型已经保存了这些关系，新图上也可以用。

GCN使用节点的度做为归一化的权重，而GAT使用自己学习到邻居的重要性去作为归一化的权重。

## 拓展

以下为常用GNN技术

![链接](https://github.com/spiritysdx/images/blob/main/20231022/79.png?raw=true)

