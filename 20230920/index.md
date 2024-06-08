# 矩阵分解角度 - 图嵌入(节点嵌入)和随机游走


## 前言

![https://github.com/spiritysdx/images/blob/main/20230921/0.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230921/0.png?raw=true)

矩阵分解和随机游走在数学上意义是一样的。

![https://github.com/spiritysdx/images/blob/main/20230921/1.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230921/1.png?raw=true)

## 矩阵分解计算

![https://github.com/spiritysdx/images/blob/main/20230921/2.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230921/2.png?raw=true)

矩阵分解这里可能解析解不能直接求得，且可能不唯一，所以实际计算用的数值解。

![https://github.com/spiritysdx/images/blob/main/20230921/3.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230921/3.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20230921/4.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230921/4.png?raw=true)

具体的证明和推导：<https://arxiv.org/pdf/1710.02971.pdf>

![https://github.com/spiritysdx/images/blob/main/20230921/5.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230921/5.png?raw=true)

## 缺点

1. 适合静态图而对于动态图过拟合，不能很快的泛化

![https://github.com/spiritysdx/images/blob/main/20230921/6.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230921/6.png?raw=true)

2. 仅探索节点相邻的局部信息，采样只能采样邻近位置的节点，远处节点可能也相似但没采样到

![https://github.com/spiritysdx/images/blob/main/20230921/7.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230921/7.png?raw=true)

3. 仅利用了连接信息，没使用节点本身的信息(属性信息)

![https://github.com/spiritysdx/images/blob/main/20230921/8.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230921/8.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20230921/9.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230921/9.png?raw=true)

## 总结

图用矩阵形式表现出来是关键。

![https://github.com/spiritysdx/images/blob/main/20230921/10.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230921/10.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20230921/11.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230921/11.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20230921/12.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230921/12.png?raw=true)

