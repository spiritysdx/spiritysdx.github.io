# Node2Vec


## 前言

DeepWalk 仅能反映相邻节点的社群相似信息，无法反映节点的功能角色相似的信息，因为DeespWalk仅使用连接信息(结构信息)学习。

## 有偏随机游走

![https://github.com/spiritysdx/images/blob/main/20230924/1.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230924/1.png?raw=true)

通过p和q的参数，可以控制随机游走时，游走方向和游走距离。区别在于更偏向DFS还是BFS。

![https://github.com/spiritysdx/images/blob/main/20230924/2.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230924/2.png?raw=true)

二阶随机游走，下一个节点不仅取决于当前节点，也取决于上一节点，区别于deepwalk的一阶随机游走仅取决于当前节点。

![https://github.com/spiritysdx/images/blob/main/20230924/3.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230924/3.png?raw=true)

DeepWalk可看作p=1和q=1的特例。

![https://github.com/spiritysdx/images/blob/main/20230924/4.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230924/4.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20230924/5.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230924/5.png?raw=true)

![https://github.com/spiritysdx/images/blob/main/20230924/6.png?raw=true](https://github.com/spiritysdx/images/blob/main/20230924/6.png?raw=true)

后续神经网络需要输入的数据的特征内含丰富、有分类区分性且相互独立。

编码的方式：

1. 手动设计构造特征
2. 基于矩阵分解(PCA)
3. 基于随机游走
4. 基于图神经网络

条件独立 = 马尔可夫假设

## alias sampling

用于产生下一个随机游走节点

时间复杂度O(1)，用空间(预处理)(O(n))换时间

大量反复抽样的情况下优势更突出

离散分布抽样转化为均匀分布抽样，生成连续随机数对离散世界采样。

## 具体代码实现

```python
import warnings
warnings.filterwarnings('ignore')
import argparse
import numpy as np
import networkx as nx
#import node2vec
from gensim.models import Word2Vec
import random
```

```python

def read_graph(input,weighted,directed):
    '''
    Reads the input network in networkx.
    '''
    # 权重图
    if weighted:
        G = nx.read_edgelist(input, nodetype=int, data=(('weight', float),), create_using=nx.DiGraph())
    # 无权图
    else:
        G = nx.read_edgelist(input, nodetype=int, create_using=nx.DiGraph())
        for edge in G.edges():
            G[edge[0]][edge[1]]['weight'] = 1
    # 无向操作
    if not directed:
        G = G.to_undirected()

    return G
```

```python
nx_G = read_graph('karate.edgelist',False,False)
```

```python
print (len(nx_G))

```

    34

```python
import matplotlib.pyplot as plt
nx.draw(nx_G, with_labels=True)
plt.show()
```

## 实现alias-sampling

```python

class Graph():
    # 出事化设置参数
    def __init__(self, nx_G, is_directed, p, q):
        self.G = nx_G
        self.is_directed = is_directed
        self.p = p
        self.q = q
    
    
    def node2vec_walk(self, walk_length, start_node):
        '''
        Simulate a random walk starting from start node.
        '''
        G = self.G
        # 上一步计算出的alias table，完成O(1)的采样
        alias_nodes = self.alias_nodes
        alias_edges = self.alias_edges

        walk = [start_node]

        #  直到生成长度为walk_length的节点序列位为止
        while len(walk) < walk_length:
            cur = walk[-1]
            # 对邻居节点排序，目的是和alias table计算时的顺序对应起来
            cur_nbrs = sorted(G.neighbors(cur))
            if len(cur_nbrs) > 0:
                # 节点序列只有一个节点的情况
                if len(walk) == 1:
                    # 直接使用alias——node对应的结果
                    walk.append(cur_nbrs[alias_draw(alias_nodes[cur][0], alias_nodes[cur][1])])
                # 节点序列大于一个节点的情况
                else:
                    # 看前一个节点,prev是论文中的节点t
                    prev = walk[-2]
                    next = cur_nbrs[alias_draw(alias_edges[(prev, cur)][0],
                        alias_edges[(prev, cur)][1])]
                    walk.append(next)
            else:
                break

        return walk

    def simulate_walks(self, num_walks, walk_length):
        '''
        Repeatedly simulate random walks from each node.
        '''
        G = self.G
        walks = []
        nodes = list(G.nodes())
        print ('Walk iteration:')
        for walk_iter in range(num_walks):
            print (str(walk_iter+1), '/', str(num_walks))
            # 打乱节点顺序
            random.shuffle(nodes)
            for node in nodes:
                # node2vec_walk是一次有偏的随机游走
                walks.append(self.node2vec_walk(walk_length=walk_length, start_node=node))

        return walks

    def get_alias_edge(self, src, dst):
        '''
        Get the alias edge setup lists for a given edge.
        '''
        G = self.G
        p = self.p
        q = self.q

        unnormalized_probs = []
        # 论文3.2.2节核心算法
        for dst_nbr in sorted(G.neighbors(dst)):
            if dst_nbr == src:
                unnormalized_probs.append(G[dst][dst_nbr]['weight']/p)
            elif G.has_edge(dst_nbr, src):
                unnormalized_probs.append(G[dst][dst_nbr]['weight'])
            else:
                unnormalized_probs.append(G[dst][dst_nbr]['weight']/q)
        
        # 归一化
        norm_const = sum(unnormalized_probs)
        normalized_probs =  [float(u_prob)/norm_const for u_prob in unnormalized_probs]

        return alias_setup(normalized_probs)

    def preprocess_transition_probs(self):
        '''
        Preprocessing of transition probabilities for guiding the random walks.
        '''
        G = self.G
        is_directed = self.is_directed

        alias_nodes = {}
        # 节点概率alias sampling和归一化
        for node in G.nodes():
            unnormalized_probs = [G[node][nbr]['weight'] for nbr in sorted(G.neighbors(node))]
            norm_const = sum(unnormalized_probs)
            normalized_probs =  [float(u_prob)/norm_const for u_prob in unnormalized_probs]
            alias_nodes[node] = alias_setup(normalized_probs)
            # 信息展示
            if node == 2:
                print (unnormalized_probs)
                print (norm_const)
                print (normalized_probs)
                print (alias_nodes[node])

        alias_edges = {}
        triads = {}
        
        # 边概率alias sampling和归一化
        if is_directed:
            for edge in G.edges():
                alias_edges[edge] = self.get_alias_edge(edge[0], edge[1])
        else:
            for edge in G.edges():
                alias_edges[edge] = self.get_alias_edge(edge[0], edge[1])
                alias_edges[(edge[1], edge[0])] = self.get_alias_edge(edge[1], edge[0])
        
        print ('edges alias')
        print (alias_edges[(2, 3)])
        
        self.alias_nodes = alias_nodes
        self.alias_edges = alias_edges

        return

```

```python
p = 1
q= 1
directed = False
G = Graph(nx_G, directed, p, q)

```

```python

def alias_setup(probs):
    '''
    Compute utility lists for non-uniform sampling from discrete distributions.
    Refer to https://hips.seas.harvard.edu/blog/2013/03/03/the-alias-method-efficient-sampling-with-many-discrete-outcomes/
    for details
    '''
    # 总过K个长度
    K = len(probs)
    # q个0
    q = np.zeros(K)
    J = np.zeros(K, dtype=np.int_)

    smaller = []
    larger = []
    
    # 将各个概率分成两组，一组的概率值大于1，另一组的概率值小于1
    for kk, prob in enumerate(probs):
        q[kk] = K*prob
        if q[kk] < 1.0:
            smaller.append(kk)
        else:
            larger.append(kk)
    
    # 使用贪心算法，将概率值小于1的不断填满
    # pseudo code step 3
    while len(smaller) > 0 and len(larger) > 0:
        small = smaller.pop()
        large = larger.pop()

        J[small] = large
        # 更新概率值
        q[large] = q[large] + q[small] - 1.0
        if q[large] < 1.0:
            smaller.append(large)
        else:
            larger.append(large)

    return J, q
```

```python

def alias_draw(J, q):
    '''
    Draw sample from a non-uniform discrete distribution using alias sampling.
    '''
    K = len(J)

    kk = int(np.floor(np.random.rand()*K))
    # 取自己 
    if np.random.rand() < q[kk]:
        return kk
    # 取alias table存的节点
    else:
        return J[kk]

```

```python
G.preprocess_transition_probs()

```

    [1, 1, 1, 1, 1, 1, 1, 1, 1]
    9
    [0.1111111111111111, 0.1111111111111111, 0.1111111111111111, 0.1111111111111111, 0.1111111111111111, 0.1111111111111111, 0.1111111111111111, 0.1111111111111111, 0.1111111111111111]
    (array([0, 0, 0, 0, 0, 0, 0, 0, 0]), array([1., 1., 1., 1., 1., 1., 1., 1., 1.]))
    edges alias
    (array([0, 0, 0, 0, 0, 0, 0, 0, 0, 0]), array([1., 1., 1., 1., 1., 1., 1., 1., 1., 1.]))

```python
num_walks = 10
walk_length = 20
# 有偏的随机游走生成节点序列
walks = G.simulate_walks(num_walks, walk_length)
# 展示一个节点序列的长度
print (len(walks[0]))
```

    Walk iteration:
    1 / 10
    2 / 10
    3 / 10
    4 / 10
    5 / 10
    6 / 10
    7 / 10
    8 / 10
    9 / 10
    10 / 10
    20

## 训练embedding

```python
def learn_embeddings(walks,dimensions,window_size,workers,iter):
    '''
    Learn embeddings by optimizing the Skipgram objective using SGD.
    '''
    # 将node的类型int转化为string
    # walks = [map(str, walk) for walk in walks]
    walk_lol = []
    for walk in walks:
        tmp = []
        for node in walk:
            tmp.append(str(node))
        walk_lol.append(tmp)
    # 调用gensim包运行word2vec
    model = Word2Vec(walk_lol, vector_size=dimensions, window=window_size, min_count=0, sg=1, workers=workers,
                     epochs=iter)
    # model.save_word2vec_format(args.output)
    # 保存embedding信息
#     model.wv.save_word2vec_format(args.output)

    return model

```

```python
model = learn_embeddings(walks,16,3,-1,10)
print ('finished')
```

    finished

```python
import matplotlib.pyplot as plt
nx.draw(nx_G, with_labels=True)
plt.show()
```

```python
# # 找到和节点最相似的一组点
print (model.wv.most_similar('34'))
```

    [('31', 0.49450308084487915), ('2', 0.48848462104797363), ('21', 0.4063515067100525), ('27', 0.3625999391078949), ('29', 0.3572307527065277), ('26', 0.32403403520584106), ('4', 0.3191637098789215), ('16', 0.31352993845939636), ('17', 0.2968163788318634), ('13', 0.18988074362277985)]

```python
type(model)
```

    gensim.models.word2vec.Word2Vec

```python
model
```

    <gensim.models.word2vec.Word2Vec at 0x7f33c7208450>

```python

from scipy import spatial

# 余弦相似度
def cos_similarity(v1, v2):
    return 1 - spatial.distance.cosine(v1, v2)

# 相似节点组1
print(cos_similarity(model.wv['17'], model.wv['6']))
print(cos_similarity(model.wv['7'], model.wv['6']))
print(cos_similarity(model.wv['7'], model.wv['5']))



# # 相似节点组2
# print (cos_similarity(model.wv['34'], model.wv['33']))
# print (cos_similarity(model.wv['34'], model.wv['9']))
# print (cos_similarity(model.wv['34'], model.wv['31']))

# # 不相似节点组
# print (cos_similarity(model.wv['17'], model.wv['25']))
# print (cos_similarity(model.wv['7'], model.wv['25']))

```

    -0.19091002643108368
    0.38975000381469727
    -0.3603137731552124

```python

# k-means聚类
from sklearn import  cluster
from sklearn.metrics import adjusted_rand_score
from sklearn.model_selection import train_test_split
import pandas as pd
embedding_node=[]
for i in range(1,35):
    j=str(i)
    embedding_node.append(model.wv[j])
embedding_node=np.array(embedding_node).reshape((34,-1))
y_pred = cluster.KMeans(n_clusters=3, random_state=9).fit_predict(embedding_node) # 调用 test_RandomForestClassifier
y_pred


```

    array([0, 2, 0, 1, 1, 1, 2, 0, 0, 0, 0, 2, 1, 0, 1, 1, 2, 1, 1, 0, 1, 1,
           1, 0, 1, 2, 1, 1, 0, 0, 1, 1, 1, 1], dtype=int32)

```python

import matplotlib.pyplot as plt
nx.draw(nx_G, with_labels=True)
plt.show()

```

