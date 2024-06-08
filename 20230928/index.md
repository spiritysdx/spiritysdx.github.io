# 半监督节点分类



## 前言

![https://github.com/spiritysdx/images/blob/main/20231020/1.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231020/1.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20231020/2.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231020/2.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20231020/3.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231020/3.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20231020/4.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231020/4.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20231020/5.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231020/5.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20231020/6.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231020/6.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20231020/7.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231020/7.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20231020/8.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231020/8.png?raw=true)

## Label Propagation (Relational Classification)

这是一种集体分类方法。

![https://github.com/spiritysdx/images/blob/main/20231020/9.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231020/9.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20231020/10.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231020/10.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20231020/11.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231020/11.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20231020/12.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231020/12.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20231020/13.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231020/13.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20231020/14.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231020/14.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20231020/15.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231020/15.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20231020/16.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231020/16.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20231020/17.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231020/17.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20231020/18.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231020/18.png?raw=true)

## Iterative Classification

这是一种集体分类方法。

既要用到节点的连接信息，也要用到节点本身的属性信息(特征)，分别训练两个分类器解决。

![https://github.com/spiritysdx/images/blob/main/20231020/19.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231020/19.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20231020/20.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231020/20.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20231020/21.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231020/21.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20231020/22.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231020/22.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20231020/23.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231020/23.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20231020/24.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231020/24.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20231020/25.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231020/25.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20231020/26.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231020/26.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20231020/27.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231020/27.png?raw=true)

我是什么类别仅取决于与我直接相连的节点是什么类别，而间接的邻居与我无关。(马尔可夫假设)

初始化所有节点类别(训练∂1) --> 捕捉相关关系和连接关系(训练∂2) --> 迭代去预测(传播)

![https://github.com/spiritysdx/images/blob/main/20231020/28.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231020/28.png?raw=true)

## Correct & Smooth

OGB数据集：<https://ogb.stanford.edu>

这是一种后处理的方法，修正结果变得更好。

![https://github.com/spiritysdx/images/blob/main/20231020/29.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231020/29.png?raw=true)

这里使用的soft_labels就是每个类别都有一个概率，概率之和是1。

![https://github.com/spiritysdx/images/blob/main/20231020/30.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231020/30.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20231020/31.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231020/31.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20231020/32.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231020/32.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20231020/33.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231020/33.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20231020/34.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231020/34.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20231020/35.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231020/35.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20231020/36.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231020/36.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20231020/37.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231020/37.png?raw=true)

L = D - A

![https://github.com/spiritysdx/images/blob/main/20231020/38.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231020/38.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20231020/39.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231020/39.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20231020/40.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231020/40.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20231020/41.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231020/41.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20231020/42.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231020/42.png?raw=true)

ā的值是超参数，越大表示更愿意相信传播过来的ERROR，越小表示更愿意相信上一时刻的ERROR。

![https://github.com/spiritysdx/images/blob/main/20231020/43.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231020/43.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20231020/44.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231020/44.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20231020/45.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231020/45.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20231020/46.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231020/46.png?raw=true)

结果求和不为一，但置信度仍较高。

![https://github.com/spiritysdx/images/blob/main/20231020/47.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231020/47.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20231020/48.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231020/48.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20231020/49.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231020/49.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20231020/50.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231020/50.png?raw=true)

## Loopy Belief Propagation

这是一种消息传递方法。

动态规划算法，下一时刻的状态仅取决于上一时刻。

![https://github.com/spiritysdx/images/blob/main/20231020/51.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231020/51.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20231020/52.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231020/52.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20231020/53.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231020/53.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20231020/54.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231020/54.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20231020/55.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231020/55.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20231020/56.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231020/56.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20231020/57.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231020/57.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20231020/58.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231020/58.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20231020/59.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231020/59.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20231020/60.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231020/60.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20231020/61.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231020/61.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20231020/62.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231020/62.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20231020/63.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231020/63.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20231020/64.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231020/64.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20231020/65.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231020/65.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20231020/66.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231020/66.png?raw=true)

## Masked Label Prediction

这是一种自监督方法。

![https://github.com/spiritysdx/images/blob/main/20231020/67.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231020/67.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20231020/68.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231020/68.png?raw=true)

已有的信息随机的抹除，然后用剩下的已有信息去预测，自监督地学习。

## 总结

都是用来解决半监督节点分类问题的。

![https://github.com/spiritysdx/images/blob/main/20231020/69.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231020/69.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20231020/70.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231020/70.png?raw=true)

直推式（也称为演绎推理）是从特定的前提或事实中推导出具体的结论，而归纳式（也称为归纳推理）则是从一般的观察或模式中推断出普遍的结论。可以这么说，直推是由特殊到一般，而归纳是由一般到特殊。

归纳式在有新的节点出现时可以很好的使用原模型，不用重新训练，而直推式需要重新训练。

