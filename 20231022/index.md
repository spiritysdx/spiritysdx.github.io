# GCN 详解


## 前言

GCN 可以说是一个函数拟合器。

![链接](https://github.com/spiritysdx/images/blob/main/20231022/1.png?raw=true)

![链接](https://github.com/spiritysdx/images/blob/main/20231022/2.png?raw=true)

每个计算图就是一个样本

![链接](https://github.com/spiritysdx/images/blob/main/20231022/3.png?raw=true)

感受野是指神经网络中某一层输出的单元对输入的局部区域的敏感程度。实际上层数不宜过多，否则后续层数感受野会越来越相似。

![链接](https://github.com/spiritysdx/images/blob/main/20231022/4.png?raw=true)

![链接](https://github.com/spiritysdx/images/blob/main/20231022/5.png?raw=true)

求和节点本身属性构成的0层向量，然后取平均。

![链接](https://github.com/spiritysdx/images/blob/main/20231022/6.png?raw=true)

这一步求了一层的 embedding 。

![链接](https://github.com/spiritysdx/images/blob/main/20231022/7.png?raw=true)

![链接](https://github.com/spiritysdx/images/blob/main/20231022/8.png?raw=true)

## 空域中讨论GCN的求解过程

这是从图论那套玩意中延展出来的。

![链接](https://github.com/spiritysdx/images/blob/main/20231022/13.png?raw=true)

![链接](https://github.com/spiritysdx/images/blob/main/20231022/14.png?raw=true)

![链接](https://github.com/spiritysdx/images/blob/main/20231022/10.png?raw=true)

简单直接求平均其实不好，没有体现各节点的重要程度，可以按照各节点的度按比例分配。

![链接](https://github.com/spiritysdx/images/blob/main/20231022/9.png?raw=true)

还得约束特征值在-1到1之间，但最大特征值能到1，前者到不了1.

![链接](https://github.com/spiritysdx/images/blob/main/20231022/11.png?raw=true)

![链接](https://github.com/spiritysdx/images/blob/main/20231022/12.png?raw=true)

![链接](https://github.com/spiritysdx/images/blob/main/20231022/15.png?raw=true)

总结为一下图，其实就是用代数语言描绘每层的计算图之间怎么计算的，左边是数学表达式，聚合邻居的embedding求平均，右边是线性代数的表达式，只不过描绘的更详细，考虑了节点之间不通的重要程度用节点的度去刻画。

![链接](https://github.com/spiritysdx/images/blob/main/20231022/17.png?raw=true)

这个 Á 其实就是邻接矩阵替换了值，权值不再是0或1，而是根据我的连接数和对方的连接数，算出来的一个新的权值。

只要图已经构建出来了，这个 Á 就是固定的了，不需要学习就已给定。

![链接](https://github.com/spiritysdx/images/blob/main/20231022/18.png?raw=true)

图卷积原文提及的：

![链接](https://github.com/spiritysdx/images/blob/main/20231022/19.png?raw=true)

```l+1```层的嵌入通过```l```层的嵌入左乘```Á(Nomilarization adjacency matrix)```右乘```神经网络权重```然后经过```非线性激活函数```就得到了。可学习的参数只有神经网络的权重矩阵。

![链接](https://github.com/spiritysdx/images/blob/main/20231022/20.png?raw=true)

## 频域(谱域)里讨论GCN的求解过程

这是从信号与图像处理那套玩意延展出来的。

![链接](https://github.com/spiritysdx/images/blob/main/20231022/16.png?raw=true)

GCN（Graph Convolutional Network）是一种用于处理图数据的神经网络，它试图解决CNN（Convolutional Neural Network）在处理图数据时缺乏置换不变性的问题。

在CNN中，卷积操作是基于局部相邻区域的权重共享，这在图数据上并不直接适用，因为图数据的结构是任意的，节点之间的连接并非固定。GCN通过引入图卷积操作，考虑节点与其相邻节点之间的关系，以捕捉图结构的信息。

关于置换不变性，GCN通过采用图拉普拉斯矩阵来实现。图拉普拉斯矩阵考虑了节点之间的连接关系，因此在一定程度上提供了置换不变性。通过对图拉普拉斯矩阵进行归一化和转换，GCN能够在学习节点表示时保持一定的置换不变性。

总体来说，GCN通过在图结构上引入卷积操作，结合图拉普拉斯矩阵来考虑节点之间的关系，以解决CNN在图数据上缺乏置换不变性的问题。

![链接](https://github.com/spiritysdx/images/blob/main/20231022/54.png?raw=true)

谱分解 = 特征值分解 = 对角化 都是同一个意思

![链接](https://github.com/spiritysdx/images/blob/main/20231022/55.png?raw=true)

![链接](https://github.com/spiritysdx/images/blob/main/20231022/56.png?raw=true)

![链接](https://github.com/spiritysdx/images/blob/main/20231022/57.png?raw=true)

![链接](https://github.com/spiritysdx/images/blob/main/20231022/58.png?raw=true)

![链接](https://github.com/spiritysdx/images/blob/main/20231022/59.png?raw=true)

![链接](https://github.com/spiritysdx/images/blob/main/20231022/60.png?raw=true)

![链接](https://github.com/spiritysdx/images/blob/main/20231022/61.png?raw=true)

![链接](https://github.com/spiritysdx/images/blob/main/20231022/62.png?raw=true)

## 计算图 改进

![链接](https://github.com/spiritysdx/images/blob/main/20231022/21.png?raw=true)

加一个自己引向自己的信息。

![链接](https://github.com/spiritysdx/images/blob/main/20231022/22.png?raw=true)

![链接](https://github.com/spiritysdx/images/blob/main/20231022/23.png?raw=true)

扩展：邻域节点一套权重，selfembedding一套权重。(还是同一个网络，只不过有两套权重)

![链接](https://github.com/spiritysdx/images/blob/main/20231022/24.png?raw=true)

![链接](https://github.com/spiritysdx/images/blob/main/20231022/25.png?raw=true)

Bk去掉才是残差连接(Resnet 那种)，这里不是那种连接。

![链接](https://github.com/spiritysdx/images/blob/main/20231022/26.png?raw=true)

此处红色是邻域信息汇聚，蓝色是自己信息的转换。

## 如何训练图神经网络(GCN)

可通过有监督学习、无监督学习(自监督学习)的方式训练。

![链接](https://github.com/spiritysdx/images/blob/main/20231022/27.png?raw=true)

### 监督学习

![链接](https://github.com/spiritysdx/images/blob/main/20231022/28.png?raw=true)

![链接](https://github.com/spiritysdx/images/blob/main/20231022/29.png?raw=true)

![链接](https://github.com/spiritysdx/images/blob/main/20231022/30.png?raw=true)

### 无监督学习

本身没有标签，用图自身结构作为自监督的标注。

![链接](https://github.com/spiritysdx/images/blob/main/20231022/31.png?raw=true)

### 图神经网络(GNN)的优点(注意不是只有GCN的优点)

![链接](https://github.com/spiritysdx/images/blob/main/20231022/32.png?raw=true)

![链接](https://github.com/spiritysdx/images/blob/main/20231022/33.png?raw=true)

GCN（图卷积网络）属于直推式学习（transductive learning）。在直推式学习中，模型在训练和测试过程中都可以访问整个图结构，从而能够对图中的节点进行有效的表示学习。这与归纳式学习不同，归纳式学习在测试阶段只能访问训练时未见过的数据。

![链接](https://github.com/spiritysdx/images/blob/main/20231022/34.png?raw=true)

![链接](https://github.com/spiritysdx/images/blob/main/20231022/35.png?raw=true)

GCN相较于传统的机器学习方法，使用了节点的属性信息做特征，计算图又学习到了结构信息，这是优点。

![链接](https://github.com/spiritysdx/images/blob/main/20231022/36.png?raw=true)

![链接](https://github.com/spiritysdx/images/blob/main/20231022/37.png?raw=true)

![链接](https://github.com/spiritysdx/images/blob/main/20231022/38.png?raw=true)

![链接](https://github.com/spiritysdx/images/blob/main/20231022/39.png?raw=true)

新节点来了，可以马上获得新节点的嵌入和预测结果，在推荐系统中称呼为 冷启动 。

![链接](https://github.com/spiritysdx/images/blob/main/20231022/40.png?raw=true)

哪怕未经训练，仅计算原始的图嵌入，就已经大致可分类了：

![链接](https://github.com/spiritysdx/images/blob/main/20231022/41.png?raw=true)

![链接](https://github.com/spiritysdx/images/blob/main/20231022/42.png?raw=true)

![链接](https://github.com/spiritysdx/images/blob/main/20231022/43.png?raw=true)

![链接](https://github.com/spiritysdx/images/blob/main/20231022/44.png?raw=true)

CNN 不具备置换不变性，GCN 解决了这个问题。

![链接](https://github.com/spiritysdx/images/blob/main/20231022/45.png?raw=true)

![链接](https://github.com/spiritysdx/images/blob/main/20231022/46.png?raw=true)

Tansformers 可以看作是词图上的全连接GNN。

![链接](https://github.com/spiritysdx/images/blob/main/20231022/47.png?raw=true)

![链接](https://github.com/spiritysdx/images/blob/main/20231022/48.png?raw=true)

## 总结

![链接](https://github.com/spiritysdx/images/blob/main/20231022/49.png?raw=true)

