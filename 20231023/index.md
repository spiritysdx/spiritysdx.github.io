# GraphSAGE 详解


## 前言

![链接](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20231022/50.png?raw=true)

空域提出 --> 频域提出 --> 图神经网络提出 --> 主要回到空域发展

图表示学习 和 图神经网络 平行的两个方向，互补的方向。

## MPNN

![链接](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20231022/53.png?raw=true)

后面两个图神经网络都是基于 MPNN 进行改进的。

## GraphSAGE

原文：Inductive Representation Learning on Large Graphs

![链接](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20231022/51.png?raw=true)

解决 GCN 需要将该节点的整个邻居输入显存进行聚合这个问题的优化，不使用所有邻居进行采样和聚合。

思路：每次更新嵌入时，不使用所有邻居节点进行更新，更新中加入采样(Sample)过程，使用采样后的邻居节点去更新节点的邻居信息。

这里如何采样和聚合是需要自己去定义的。

这里最简单的方式是按距离去采样，聚合则使用一个前馈神经网络。

从这开始 GCN 才能说是有了归纳式学习的特性，之前的 GCN 是直推式学习。

### 特点

方便在大图上使用，可以随时新增节点，无需重新训练就可以得到嵌入。

![链接](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20231022/63.png?raw=true)

![链接](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20231022/64.png?raw=true)

![链接](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20231022/65.png?raw=true)

训练过程中用到了节点本身的信息，以往的embedding都是用了所有邻居的信息，太多了，所以这里进行了采样。

以往的embedding大多才有SUM聚合邻居信息，没有偏重，GAT给每个边赋予权重，SUM的时候有重要性之分，但这里的GraphSAGE会用别的函数做聚合。

![链接](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20231022/66.png?raw=true)

![链接](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20231022/67.png?raw=true)

采样的时候，固定了这一层中的邻居数量的最大值，该层邻居数量不能比这个数多。

![链接](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20231022/68.png?raw=true)

![链接](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20231022/69.png?raw=true)

上面这里无监督的方式还是使用 Random Walk 去确定需要采样哪些节点，有监督的话是使用的一般的MLP做，且可以部分邻居无监督，部分邻居有监督。

![链接](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20231022/70.png?raw=true)

上面这里的聚合函数，需要尽量与邻居聚合的顺序无关(symmetric)，需要尽量可训练(trainable)，但可能不同时满足，但第一个性质一定满足。

三个聚合函数：

第一个是取平均值保证了顺序不变性，直接concat后取平均，注意是先平均嵌入后再进行训练，称为 GraphSAGE-mean 方法。

这里也有一种方法，不进行concat，而是使用它们做类似 GCN 的操作，然后直接取平均，被称作 GraphSAGE-GCN 方法。

第二个是多次随机顺序进行LSTM，保证最后学到的模型对顺序依赖尽量低，被称为 GraphSAGE-LSTM 方法。

第三个是取最大值，也与顺序无关，注意是训练后再进行取最大值的操作，被称为 GraphSAGE-pool 方法。

![链接](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20231022/71.png?raw=true)

![链接](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20231022/71.png?raw=true)

上面这里的表现可以看到，DW学到的参数迁移新的图时，又要重新训练了，而GraphSAGE学到的参数迁移到新的图时，已有的参数有学到一些消息传递聚合的结构，适配新图时很迅速。

同时，这里为了兼顾F1，选择了两层计算图，第一层25第二层10个邻居，来达到表现最好的情况。

## 相关资料

<https://arxiv.org/pdf/1706.02216.pdf>

<https://docs.google.com/presentation/d/1ipsFHlW4-kwjkPhJlMg4kLKRtmvcXBidBQgBS5lOaAM/edit#slide=id.g225699dd435_0_170>

<https://zhuanlan.zhihu.com/p/62750137>

<https://colah.github.io/posts/2015-08-Understanding-LSTMs/>

