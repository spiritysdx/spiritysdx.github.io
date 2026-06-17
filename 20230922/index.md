# DeepWalk


## 前言

深度学习思想用于图嵌入的开山之作 - DeepWalk

学习路径：

![https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230921/27.png?raw=true](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230921/27.png?raw=true)

Random Walk 是一种遍历图的基本方法，而 Deep Walk 则是一种利用 Random Walk 生成的序列来学习节点嵌入的具体方法。

## 背景

![https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230921/28.png?raw=true](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230921/28.png?raw=true)

![https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230921/29.png?raw=true](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230921/29.png?raw=true)

## Embedding - 嵌入的艺术 - Representation Learning - 表示学习

类同 word2vec - 词向量

![https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230921/30.png?raw=true](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230921/30.png?raw=true)

输入周围词预测中心词 - CBOW

输入中心词预测周围词 - Skip-gram

![https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230921/31.png?raw=true](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230921/31.png?raw=true)

以 Skip-gram 为例：

![https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230921/32.png?raw=true](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230921/32.png?raw=true)

随机游走 - Random Walk

![https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230921/33.png?raw=true](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230921/33.png?raw=true)

编码前后可对比分类是否可区分，原图中相近的节点嵌入后依然相近，就证明是有效的。

编码后的D维向量再给算法操作，才是真正的机器学习。

## PPT 讲解

原作者讲解视频：

<https://dl.acm.org/doi/10.1145/2623330.2623732>

原作者PPT：

<http://www.perozzi.net/publications/14_kdd_deepwalk-slides.pdf>

![https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230921/34.png?raw=true](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230921/34.png?raw=true)

## 正文

机器学习最怕的就是数据是稀疏的

Deep Walk 参考NLP的word2vec，学习到连接的结构信息，每个节点都有向量表示。

i,i,d. 独立同分布假设

Deep Walk 没有使用节点本身的属性(label)

要求：

1. 可扩展性
2. 反映原图的结构信息
3. 生成的向量低维度，低纬度嵌入有助于防止过拟合
4. 连续的，细微差异导致的结构就不同了，拟合的边界是平滑的决策边界

实际网络是无标度网络(Scale-Free Network)，网络的节点数和度数分布验证不均匀，少数的Hub中枢节点拥有极多的连接，大部分节点只有很少的连接，服从幂律分布。

Node degree 服从幂律分布，某一个Node在随机游走序列出现的次数页服从幂律分布。

## 方法

一个随机游走生成器和一个迭代优化器。

优化计算损失函数的复杂度，分层softmax操作

![https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230921/35.png?raw=true](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230921/35.png?raw=true)

使用霍夫曼树，n个叶子节点产生n-1个内部节点(逻辑回归)，每个有D个权重，每个内部节点有n个分支，每个分支指向一个叶子节点。

DeepWalk 有两套权重，N个节点的D维Embedding和N-1个逻辑回归，每个有D个权重。

