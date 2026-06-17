# 嵌入整张图


## 如何嵌入

### 全图进行编码嵌入

![https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230921/13.png?raw=true](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230921/13.png?raw=true)

![https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230921/14.png?raw=true](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230921/14.png?raw=true)

### 引入虚拟节点

![https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230921/15.png?raw=true](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230921/15.png?raw=true)

### 匿名随机游走

![https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230921/16.png?raw=true](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230921/16.png?raw=true)

<https://arxiv.org/pdf/1805.11921.pdf>

![https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230921/17.png?raw=true](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230921/17.png?raw=true)

只要是新节点就标记新的一号，认号不认原来的节点标识。

![https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230921/18.png?raw=true](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230921/18.png?raw=true)

![https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230921/19.png?raw=true](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230921/19.png?raw=true)

使用不同匿名随机游走序列的个数构成的向量作为全图随机游走的向量。

![https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230921/20.png?raw=true](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230921/20.png?raw=true)

### 给每种匿名随机游走的序列单独学习一个嵌入编码，全图也单独学习一个嵌入编码

![https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230921/21.png?raw=true](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230921/21.png?raw=true)

![https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230921/22.png?raw=true](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230921/22.png?raw=true)

## 总结

![https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230921/23.png?raw=true](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230921/23.png?raw=true)

实际还有一种方法：分层聚类

![https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230921/24.png?raw=true](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230921/24.png?raw=true)

![https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230921/25.png?raw=true](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230921/25.png?raw=true)

![https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230921/26.png?raw=true](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230921/26.png?raw=true)

