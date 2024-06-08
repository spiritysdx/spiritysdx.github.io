# PageRank


## Links as votes

使用In-coming links作为投票，也就是别人引用了我，即一个节点被多少其他节点指向，那么这个节点被多少用户点击的概率就越大。

pagerank假设 In-Links 之间也是不一样，重要网站引用我和无名小将引用我肯定前者更重要。

所以这是一个递归问题。

## PageRank求解节点重要度

### 5个角度理解该问题

![https://github.com/spiritysdx/images/blob/main/20231019/13.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231019/13.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20231019/14.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231019/14.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20231019/15.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231019/15.png?raw=true)

#### 连立线性方程组迭代求解

![https://github.com/spiritysdx/images/blob/main/20231019/16.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231019/16.png?raw=true)

#### 迭代左乘M矩阵

![https://github.com/spiritysdx/images/blob/main/20231019/17.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231019/17.png?raw=true)

左乘的这个矩阵是M矩阵，右边向量叫做pagerank向量：

![https://github.com/spiritysdx/images/blob/main/20231019/18.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231019/18.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20231019/19.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231019/19.png?raw=true)

矩阵的特征向量：矩阵M代表一种线性变换(原点不变的变换)，特征向量也即是变换过程中方向不变的向量，特征值就是变换过后，特征向量前后缩放的系数。(非特征向量线性变换后反向和原来不一样了)

![https://github.com/spiritysdx/images/blob/main/20231019/20.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231019/20.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20231019/21.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231019/21.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20231019/22.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231019/22.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20231019/23.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231019/23.png?raw=true)

#### 随机游走

随机游走，每访问一个节点该节点计数+1，最后归一化整个有向图的节点得到概率，值就是pagerank值。

![https://github.com/spiritysdx/images/blob/main/20231019/24.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231019/24.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20231019/25.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231019/25.png?raw=true)

#### 马尔科夫链

![https://github.com/spiritysdx/images/blob/main/20231019/26.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231019/26.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20231019/27.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231019/27.png?raw=true)

### 复杂度

![https://github.com/spiritysdx/images/blob/main/20231019/28.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231019/28.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20231019/29.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231019/29.png?raw=true)

pagerank向量此时刻与上一时刻差值小于某个值就认为是收敛了

![https://github.com/spiritysdx/images/blob/main/20231019/30.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231019/30.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20231019/31.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231019/31.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20231019/32.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231019/32.png?raw=true)

### 收敛性分析

![https://github.com/spiritysdx/images/blob/main/20231019/33.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231019/33.png?raw=true)

不可约且非周期的马尔科夫链是收敛的。

![https://github.com/spiritysdx/images/blob/main/20231019/34.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231019/34.png?raw=true)

可约的 --> 孤立的首尾相连环的节点

周期的 --> 两个节点互连

收敛是收敛了，是否收敛到的是网页重要度的值呢？

![https://github.com/spiritysdx/images/blob/main/20231019/35.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231019/35.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20231019/36.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231019/36.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20231019/37.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231019/37.png?raw=true)

以上两种情况会导致收敛是收敛，但不会是重要度的值(都为0或都为1了)

![https://github.com/spiritysdx/images/blob/main/20231019/38.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231019/38.png?raw=true)

这种情况则是直接不收敛，因为连通域互相独立

PageRank需要解决上述问题

遇到爬虫仅指向自己的节点(spider trap)，解决方法：

1. 有β概率被传送回去
2. 有1-β的概率被传送到其他节点(任意一个节点)
3. β在0.8到0.9之间随机

![https://github.com/spiritysdx/images/blob/main/20231019/39.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231019/39.png?raw=true)

遇到死胡同(dead end)节点，解决方法：

1. 就让其100%被传送走到其他节点(任意一个节点，任意节点概率相等)

![https://github.com/spiritysdx/images/blob/main/20231019/40.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231019/40.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20231019/41.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231019/41.png?raw=true)

谷歌的解决方案 --> Damping Factor(阻尼系数)

![https://github.com/spiritysdx/images/blob/main/20231019/42.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231019/42.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20231019/43.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231019/43.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20231019/44.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231019/44.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20231019/45.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231019/45.png?raw=true)

### 总结

![https://github.com/spiritysdx/images/blob/main/20231019/46.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231019/46.png?raw=true)

## PageRank求解节点相似度

### 基于PageRank的变种

![https://github.com/spiritysdx/images/blob/main/20231019/47.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231019/47.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20231019/48.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231019/48.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20231019/49.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231019/49.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20231019/50.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231019/50.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20231019/51.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231019/51.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20231019/52.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231019/52.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20231019/53.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231019/53.png?raw=true)

以上数字反映了与起点节点Q的相似度

![https://github.com/spiritysdx/images/blob/main/20231019/54.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231019/54.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20231019/55.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231019/55.png?raw=true)

### 总结

![https://github.com/spiritysdx/images/blob/main/20231019/56.png?raw=true](https://github.com/spiritysdx/images/blob/main/20231019/56.png?raw=true)

