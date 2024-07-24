# Random Walk & Node2vec - 图表示学习起始


## 前言

本文讲述映射成D维向量的基于随机游走方法。

![https://github.com/spiritysdx/images/blob/main/20230920/1.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230920/1.png?raw=true)

学习路径：

![https://github.com/spiritysdx/images/blob/main/20230920/2.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230920/2.png?raw=true)

## 图嵌入 - Node Embedding

把节点映射为一个D维向量。

![https://github.com/spiritysdx/images/blob/main/20230920/3.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230920/3.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20230920/4.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230920/4.png?raw=true)

图表示学习用表示学习替代手工的特征工程，自动化特征的提取。

![https://github.com/spiritysdx/images/blob/main/20230920/5.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230920/5.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20230920/6.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230920/6.png?raw=true)

由于向量维数远小于图的节点数，所以当用D维向量表示某个节点时，相当于低维的一个向量嵌入了高维空间中表示一个点。

图嵌入示例(Deep Walk)：

![https://github.com/spiritysdx/images/blob/main/20230920/7.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230920/7.png?raw=true)

只要前面图嵌入提取特征提取的好，后面需要做分类或者预测都会效果更好

## 图嵌入的基本框架 - 编码器和解码器 - ENC & DEC

先设置示例图：

![https://github.com/spiritysdx/images/blob/main/20230920/8.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230920/8.png?raw=true)

编码器设计：

![https://github.com/spiritysdx/images/blob/main/20230920/9.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230920/9.png?raw=true)

解码器设计：

![https://github.com/spiritysdx/images/blob/main/20230920/10.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230920/10.png?raw=true)

这里的点乘 = 余弦相似度 的前提应该是节点值为1，向量模长为1。

这里的节点相似度也就是如果直接相连就为1表示极其相似，也就是两个向量互相平行，如果二者完全不相似就为0表示二者正交，也就是两个向量互相垂直。

这里的目标是让高相似的节点的函数值大，低相似的节点的函数值小，具体什么函数需要自己定义，虽然这里使用了余弦相似度，但这不是固定的，函数可选其他的。

![https://github.com/spiritysdx/images/blob/main/20230920/11.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230920/11.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20230920/12.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230920/12.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20230920/13.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230920/13.png?raw=true)

最简单的编码器 - 查表 又称为 浅编码器 区别于 深度学习方法 - 深编码器

![https://github.com/spiritysdx/images/blob/main/20230920/14.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230920/14.png?raw=true)

Z矩阵的每一列表示一个节点，行数就是向量的维度

![https://github.com/spiritysdx/images/blob/main/20230920/15.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230920/15.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20230920/16.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230920/16.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20230920/17.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230920/17.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20230920/18.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230920/18.png?raw=true)

## 随机游走 - Random Walk

如何采样随机游走序列(节点串起来的序列)：

![https://github.com/spiritysdx/images/blob/main/20230920/19.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230920/19.png?raw=true)

图机器学习和NLP有异曲同工之妙，二者哪边有新进展一般能在对面找到对应的东西

![https://github.com/spiritysdx/images/blob/main/20230920/20.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230920/20.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20230920/21.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230920/21.png?raw=true)

这里的余弦相似度不关心模长了，因为做完内积后模长通过softmax已经是同一数量级了(如上图)，所以这里只关系方向的相似度(使用内积表示)：

![https://github.com/spiritysdx/images/blob/main/20230920/22.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230920/22.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20230920/23.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230920/23.png?raw=true)

这里得到的随机游走序列是万金油向量，而且它没有用到任何节点本身的标签：

![https://github.com/spiritysdx/images/blob/main/20230920/24.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230920/24.png?raw=true)

上面这里的N是从u节点出发的节点序列的随机游走策略是R。

![https://github.com/spiritysdx/images/blob/main/20230920/25.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230920/25.png?raw=true)

这里的似然目标函数就是极大似然估计，让u节点出发的前提下，跟它出现在同一个随机游走序列中节点们出现的概率最大化。(找共线/相似的节点)

其实似然目标函数就是希望所有样本被预测正确这个事件的概率最大化，因为是独立通分布，所以是每个样本预测正确的事件的概率累乘，但由于都是在0到1上的概率(softmax)，累乘不好算，所以取对数变成了累加，由于累乘累加的转变没有改变单调性，所以依然是取最大。这就是极大似然估计，所以这个函数称为似然目标函数。

![https://github.com/spiritysdx/images/blob/main/20230920/26.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230920/26.png?raw=true)

这里的P就是softmax的计算方法：

![https://github.com/spiritysdx/images/blob/main/20230920/27.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230920/27.png?raw=true)

展开就是：

![https://github.com/spiritysdx/images/blob/main/20230920/28.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230920/28.png?raw=true)

这里有两个地方要遍历所有节点：

![https://github.com/spiritysdx/images/blob/main/20230920/29.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230920/29.png?raw=true)

所以该方法复杂度是节点数的平方，实际还是比较大的。

这里有两个优化方法，一个是分层softmax，一个是负采样。

负采样可以解决softmax难算的问题，在之前的原始softmax中，需要算u节点和其他所有节点的向量的数量积再，现在负采样只算u节点和k个负样本的数量积。(算n个变成算k个了，计算量大大减轻了)

![https://github.com/spiritysdx/images/blob/main/20230920/30.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230920/30.png?raw=true)

k该如何选择：(一般5到20就行，太大太小都不好)

![https://github.com/spiritysdx/images/blob/main/20230920/31.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230920/31.png?raw=true)

## 随机梯度下降 - 优化过程 - 最小化损失函数

两种方法，一种是求全局的随机梯度下降(Gradient Descent)，一种是求局部梯度下降(Stochastic Gradient Descent)。

用全局梯度去优化：

![https://github.com/spiritysdx/images/blob/main/20230920/32.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230920/32.png?raw=true)

每个随机游走序列都优化一次：

![https://github.com/spiritysdx/images/blob/main/20230920/33.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230920/33.png?raw=true)

区别：

![https://github.com/spiritysdx/images/blob/main/20230920/34.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230920/34.png?raw=true)

这里就引申出 MiniBatch 了，MiniBatch 就是每次只取一部分数据去优化，这样速度会快很多，但是准确率会下降，不取全局也不一个个序列取，，只取几个序列一起求梯度。实际上这才是最常用的优化方法。

总结：

![https://github.com/spiritysdx/images/blob/main/20230920/35.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230920/35.png?raw=true)

该方法的策略是随机的，实际可优化策略 - Node2Vec

![https://github.com/spiritysdx/images/blob/main/20230920/36.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230920/36.png?raw=true)

## Node2Vec

![https://github.com/spiritysdx/images/blob/main/20230920/37.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230920/37.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20230920/38.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230920/38.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20230920/39.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230920/39.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20230920/40.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230920/40.png?raw=true)

一个参数q控制要不要回上一个节点，一个参数p控制要不要走向更远的节点：

![https://github.com/spiritysdx/images/blob/main/20230920/41.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230920/41.png?raw=true)

由于每个节点都记得上一个节点是谁，所以叫2阶的随机游走：

![https://github.com/spiritysdx/images/blob/main/20230920/42.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230920/42.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20230920/43.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230920/43.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20230920/44.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230920/44.png?raw=true)

两个控制参数大小不同时的区别：

![https://github.com/spiritysdx/images/blob/main/20230920/45.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230920/45.png?raw=true)

探索远方应该是对应word2vec中生成句子下文，相当于找到所有有关联的词语。探索近邻其实是以一个中心词在不断生成句子，那么更关注这个中心词在文章中的作用。相同的中心词表示在文中有相类似地位的词。

具体算法：

![https://github.com/spiritysdx/images/blob/main/20230920/46.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230920/46.png?raw=true)

其他方法：

![https://github.com/spiritysdx/images/blob/main/20230920/47.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230920/47.png?raw=true)

## 总结

![https://github.com/spiritysdx/images/blob/main/20230920/48.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230920/48.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20230920/49.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230920/49.png?raw=true)

