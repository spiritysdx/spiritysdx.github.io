# 图神经网络基础


## 传统机器学习

默认二者独立同分布，只需要拟合决策边界分类或拟合回归的曲线即可。

## 现代神经网络

斯坦福CS的相关课程：

| 网络类型             | 数据类型           | 课程      |
|----------------------|--------------------|-----------|
| 全连接神经网络     | 表格               | 无        |
| 卷积神经网络         | 图像               | CS231N   |
| 循环神经网络、Transformer | 文本语音带序列 | CS224N   |
| 图神经网络             | 图数据             | CS244W   |

## 复杂的图结构

1. 任意尺寸输入
2. 没有固定的节点顺序和参考锚点
3. 动态变化，多模态特征

## 表示学习 - 图嵌入 - node embedding

把一个复杂的图节点表示为一个d维向量，能充分表示原始数据的语义。

做这件事的实际就是图神经网络干的事情。

图神经网络无需专门的特征提取设计，可以直接端到端的表示学习出特征。(自动学习特征)(类似CNN)

## CS224W的概述

1. 传统图机器学习方法
2. 图(节点)嵌入(node embeddings)：DeepWalk、node2vec (这里还没开始用图神经网络)
3. 图神经网络：GCN(图卷积)、GraphSAGE、GAT(图注意力网络)、GNN
4. 知识图谱和推理：TransE、BetaE
5. 生成一个新图：GraphRNN
6. 图数据挖掘

## 推荐的图工具库

pyg 类同 pytorch 但有高级封装可以直接调用

graphgym

networkx

dgl 复现了很多顶会论文的源码，学术推荐

echarts 可视化必备

graphxr 做知识图谱渲染好用

## 图数据的排名

<https://db-engines.com/en/ranking/graph+dbms>

## 图神经网络任务层级

1. 节点层面 node-level
2. 连接层面(边层) edg-level
3. 子图层面 subgraph-level
4. 全图层面 graph-level

## 后言

好玩的网站

<https://open-leaderboard.x-lab.info/>

<https://crx.hypertrons.io/>

