# NetworkX实用模板案例


# PageRank节点重要度

在NetworkX中，计算有向图节点的PageRank节点重要度。

## 参考资料

networkx官方教程：<https://networkx.org/documentation/stable/tutorial.html>

nx.Graph <https://networkx.org/documentation/stable/reference/classes/graph.html#networkx.Graph>

给图、节点、连接添加属性：<https://networkx.org/documentation/stable/tutorial.html#attributes>

读写图：<https://networkx.org/documentation/stable/reference/readwrite/index.html>

## 导入工具包

```python
# 图数据挖掘
import networkx as nx

# 数据可视化
import matplotlib.pyplot as plt
%matplotlib inline

plt.rcParams['font.sans-serif']=['SimHei']  # 用来正常显示中文标签  
plt.rcParams['axes.unicode_minus']=False  # 用来正常显示负号
```

```python
G = nx.star_graph(7)
```

```python
nx.draw(G, with_labels = True)
```

## 计算PageRank节点重要度¶

```python
pagerank = nx.pagerank(G, alpha=0.8)
```

```python
pagerank
```

    {0: 0.4583348922684132,
     1: 0.07738072967594098,
     2: 0.07738072967594098,
     3: 0.07738072967594098,
     4: 0.07738072967594098,
     5: 0.07738072967594098,
     6: 0.07738072967594098,
     7: 0.07738072967594098}

PageRank使用networkx实际只计算有向图的重要度，但传入无向图系统默认将其转换为双向图去计算了。

# 节点连接数Node Degree度分析

在NetworkX中，计算并统计图中每个节点的连接数Node Degree，绘制可视化和直方图。
参考文档：<https://networkx.org/documentation/stable/auto_examples/drawing/plot_degree.html#sphx-glr-auto-examples-drawing-plot-degree-py>

## 导入工具包

```python
# 图数据挖掘
import networkx as nx

import numpy as np

# 数据可视化
import matplotlib.pyplot as plt
%matplotlib inline

plt.rcParams['font.sans-serif']=['SimHei']  # 用来正常显示中文标签  
plt.rcParams['axes.unicode_minus']=False  # 用来正常显示负号
```

## 创建图

```python
# 创建 Erdős-Rényi 随机图，也称作 binomial graph
# n-节点数
# p-任意两个节点产生连接的概率

G = nx.gnp_random_graph(100, 0.02, seed=10374196)
```

```python
# 初步可视化
pos = nx.spring_layout(G, seed=10)
nx.draw(G, pos)
```

## 最大连通域子图

```python
Gcc = G.subgraph(sorted(nx.connected_components(G), key=len, reverse=True)[0])
```

```python
pos = nx.spring_layout(Gcc, seed=10396953)
# nx.draw(Gcc, pos)

nx.draw_networkx_nodes(Gcc, pos, node_size=20)
nx.draw_networkx_edges(Gcc, pos, alpha=0.4)


```

```python
nx.draw_networkx?
```

```python
plt.figure(figsize=(12,8))
pos = nx.spring_layout(Gcc, seed=10396953)

# 设置其它可视化样式
options = {
    "font_size": 12,
    "node_size": 350,
    "node_color": "white",
    "edgecolors": "black",
    "linewidths": 1, # 节点线宽
    "width": 2, # edge线宽
}

nx.draw_networkx(Gcc, pos, **options)

plt.title('Connected components of G', fontsize=20)
plt.axis('off')
plt.show()
```

## 每个节点的连接数（degree）

```python
G.degree()
```

    DegreeView({0: 2, 1: 4, 2: 4, 3: 4, 4: 2, 5: 4, 6: 4, 7: 2, 8: 2, 9: 1, 10: 3, 11: 0, 12: 1, 13: 2, 14: 6, 15: 2, 16: 0, 17: 0, 18: 3, 19: 1, 20: 3, 21: 1, 22: 1, 23: 1, 24: 1, 25: 3, 26: 0, 27: 2, 28: 2, 29: 0, 30: 2, 31: 1, 32: 1, 33: 0, 34: 1, 35: 4, 36: 2, 37: 2, 38: 1, 39: 5, 40: 5, 41: 1, 42: 4, 43: 1, 44: 0, 45: 2, 46: 3, 47: 1, 48: 2, 49: 2, 50: 3, 51: 2, 52: 0, 53: 3, 54: 0, 55: 3, 56: 1, 57: 2, 58: 2, 59: 2, 60: 1, 61: 1, 62: 0, 63: 2, 64: 4, 65: 5, 66: 2, 67: 0, 68: 2, 69: 2, 70: 1, 71: 3, 72: 2, 73: 4, 74: 1, 75: 2, 76: 2, 77: 2, 78: 5, 79: 2, 80: 0, 81: 1, 82: 2, 83: 1, 84: 2, 85: 0, 86: 4, 87: 2, 88: 4, 89: 2, 90: 2, 91: 3, 92: 0, 93: 2, 94: 0, 95: 4, 96: 1, 97: 4, 98: 0, 99: 2})

```python
degree_sequence = sorted((d for n, d in G.degree()), reverse=True)
```

```python
degree_sequence
```

    [6,
     5,
     5,
     5,
     5,
     4,
     4,
     4,
     4,
     4,
     4,
     4,
     4,
     4,
     4,
     4,
     4,
     4,
     3,
     3,
     3,
     3,
     3,
     3,
     3,
     3,
     3,
     3,
     2,
     2,
     2,
     2,
     2,
     2,
     2,
     2,
     2,
     2,
     2,
     2,
     2,
     2,
     2,
     2,
     2,
     2,
     2,
     2,
     2,
     2,
     2,
     2,
     2,
     2,
     2,
     2,
     2,
     2,
     2,
     2,
     2,
     2,
     1,
     1,
     1,
     1,
     1,
     1,
     1,
     1,
     1,
     1,
     1,
     1,
     1,
     1,
     1,
     1,
     1,
     1,
     1,
     1,
     1,
     1,
     0,
     0,
     0,
     0,
     0,
     0,
     0,
     0,
     0,
     0,
     0,
     0,
     0,
     0,
     0,
     0]

```python
plt.figure(figsize=(12,8))
plt.plot(degree_sequence, "b-", marker="o")
plt.title('Degree Rank Plot', fontsize=20)
plt.ylabel('Degree', fontsize=25)
plt.xlabel('Rank', fontsize=25)
plt.tick_params(labelsize=20) # 设置坐标文字大小
plt.show()
```

## 节点Degree直方图

```python
X = np.unique(degree_sequence, return_counts=True)[0]
Y = np.unique(degree_sequence, return_counts=True)[1]
```

```python
X
```

    array([0, 1, 2, 3, 4, 5, 6])

```python
Y
```

    array([16, 22, 34, 10, 13,  4,  1])

```python
plt.figure(figsize=(12,8))
# plt.bar(*np.unique(degree_sequence, return_counts=True))
plt.bar(X, Y)

plt.title('Degree Histogram', fontsize=20)
plt.ylabel('Number', fontsize=25)
plt.xlabel('Degree', fontsize=25)
plt.tick_params(labelsize=20) # 设置坐标文字大小
plt.show()
plt.show()
```

# 棒棒糖图特征分析

参考文档：<https://networkx.org/documentation/stable/auto_examples/basic/plot_properties.html#sphx-glr-auto-examples-basic-plot-properties-py>

## 导入工具包

```python
# 图数据挖掘
import networkx as nx

# 数据可视化
import matplotlib.pyplot as plt
%matplotlib inline

# plt.rcParams['font.sans-serif']=['SimHei']  # 用来正常显示中文标签  
plt.rcParams['axes.unicode_minus']=False  # 用来正常显示负号
```

## 导入图

```python
# 第一个参数指定头部节点数，第二个参数指定尾部节点数
G = nx.lollipop_graph(4, 7)
```

## 可视化

```python
pos = nx.spring_layout(G, seed=3068)
nx.draw(G, pos=pos, with_labels=True)
plt.show()
```

## 图数据分析

```python
# 半径
nx.radius(G)
```

    4

```python
# 直径
nx.diameter(G)
```

    8

```python
# 偏心度：每个节点到图中其它节点的最远距离
nx.eccentricity(G)
```

    {0: 8, 1: 8, 2: 8, 3: 7, 4: 6, 5: 5, 6: 4, 7: 5, 8: 6, 9: 7, 10: 8}

```python
# 中心节点，偏心度与半径相等的节点
nx.center(G)
```

    [6]

```python
# 外围节点，偏心度与直径相等的节点
nx.periphery(G)
```

    [0, 1, 2, 10]

```python
nx.density?
```

```python
nx.density(G)
```

    0.23636363636363636

n为节点个数，m为连接个数

对于无向图：

$$
density = \frac{2m}{n(n-1)}
$$

对于有向图：

$$
density = \frac{m}{n(n-1)}
$$

无连接图的density为0，全连接图的density为1，Multigraph（多重连接图）和带self loop图的density可能大于1。

## 3号节点到图中其它节点的最短距离

```python
node_id = 3
nx.single_source_shortest_path_length(G, node_id)
```

    {3: 0, 0: 1, 1: 1, 2: 1, 4: 1, 5: 2, 6: 3, 7: 4, 8: 5, 9: 6, 10: 7}

## 每两个节点之间的最短距离

```python
pathlengths = []
for v in G.nodes():
    spl = nx.single_source_shortest_path_length(G, v)
    for p in spl:
        print('{} --> {} 最短距离 {}'.format(v, p, spl[p]))
        pathlengths.append(spl[p])
```

    0 --> 0 最短距离 0
    0 --> 1 最短距离 1
    0 --> 2 最短距离 1
    0 --> 3 最短距离 1
    0 --> 4 最短距离 2
    0 --> 5 最短距离 3
    0 --> 6 最短距离 4
    0 --> 7 最短距离 5
    0 --> 8 最短距离 6
    0 --> 9 最短距离 7
    0 --> 10 最短距离 8
    1 --> 1 最短距离 0
    1 --> 0 最短距离 1
    1 --> 2 最短距离 1
    1 --> 3 最短距离 1
    1 --> 4 最短距离 2
    1 --> 5 最短距离 3
    1 --> 6 最短距离 4
    1 --> 7 最短距离 5
    1 --> 8 最短距离 6
    1 --> 9 最短距离 7
    1 --> 10 最短距离 8
    2 --> 2 最短距离 0
    2 --> 0 最短距离 1
    2 --> 1 最短距离 1
    2 --> 3 最短距离 1
    2 --> 4 最短距离 2
    2 --> 5 最短距离 3
    2 --> 6 最短距离 4
    2 --> 7 最短距离 5
    2 --> 8 最短距离 6
    2 --> 9 最短距离 7
    2 --> 10 最短距离 8
    3 --> 3 最短距离 0
    3 --> 0 最短距离 1
    3 --> 1 最短距离 1
    3 --> 2 最短距离 1
    3 --> 4 最短距离 1
    3 --> 5 最短距离 2
    3 --> 6 最短距离 3
    3 --> 7 最短距离 4
    3 --> 8 最短距离 5
    3 --> 9 最短距离 6
    3 --> 10 最短距离 7
    4 --> 4 最短距离 0
    4 --> 3 最短距离 1
    4 --> 5 最短距离 1
    4 --> 0 最短距离 2
    4 --> 1 最短距离 2
    4 --> 2 最短距离 2
    4 --> 6 最短距离 2
    4 --> 7 最短距离 3
    4 --> 8 最短距离 4
    4 --> 9 最短距离 5
    4 --> 10 最短距离 6
    5 --> 5 最短距离 0
    5 --> 4 最短距离 1
    5 --> 6 最短距离 1
    5 --> 3 最短距离 2
    5 --> 7 最短距离 2
    5 --> 0 最短距离 3
    5 --> 1 最短距离 3
    5 --> 2 最短距离 3
    5 --> 8 最短距离 3
    5 --> 9 最短距离 4
    5 --> 10 最短距离 5
    6 --> 6 最短距离 0
    6 --> 5 最短距离 1
    6 --> 7 最短距离 1
    6 --> 8 最短距离 2
    6 --> 4 最短距离 2
    6 --> 9 最短距离 3
    6 --> 3 最短距离 3
    6 --> 0 最短距离 4
    6 --> 1 最短距离 4
    6 --> 2 最短距离 4
    6 --> 10 最短距离 4
    7 --> 7 最短距离 0
    7 --> 8 最短距离 1
    7 --> 6 最短距离 1
    7 --> 9 最短距离 2
    7 --> 5 最短距离 2
    7 --> 10 最短距离 3
    7 --> 4 最短距离 3
    7 --> 3 最短距离 4
    7 --> 0 最短距离 5
    7 --> 1 最短距离 5
    7 --> 2 最短距离 5
    8 --> 8 最短距离 0
    8 --> 9 最短距离 1
    8 --> 7 最短距离 1
    8 --> 10 最短距离 2
    8 --> 6 最短距离 2
    8 --> 5 最短距离 3
    8 --> 4 最短距离 4
    8 --> 3 最短距离 5
    8 --> 0 最短距离 6
    8 --> 1 最短距离 6
    8 --> 2 最短距离 6
    9 --> 9 最短距离 0
    9 --> 8 最短距离 1
    9 --> 10 最短距离 1
    9 --> 7 最短距离 2
    9 --> 6 最短距离 3
    9 --> 5 最短距离 4
    9 --> 4 最短距离 5
    9 --> 3 最短距离 6
    9 --> 0 最短距离 7
    9 --> 1 最短距离 7
    9 --> 2 最短距离 7
    10 --> 10 最短距离 0
    10 --> 9 最短距离 1
    10 --> 8 最短距离 2
    10 --> 7 最短距离 3
    10 --> 6 最短距离 4
    10 --> 5 最短距离 5
    10 --> 4 最短距离 6
    10 --> 3 最短距离 7
    10 --> 0 最短距离 8
    10 --> 1 最短距离 8
    10 --> 2 最短距离 8

```python
# 平均最短距离
sum(pathlengths) / len(pathlengths)
```

    3.2231404958677685

## 不同距离的节点对个数

```python
dist = {}
for p in pathlengths:
    if p in dist:
        dist[p] += 1
    else:
        dist[p] = 1
```

```python
dist
```

    {0: 11, 1: 26, 2: 18, 3: 16, 4: 14, 5: 12, 6: 10, 7: 8, 8: 6}

# 计算节点特征

计算无向图和有向图的节点特征。

## 导入工具包

```python
import networkx as nx
import matplotlib.pyplot as plt
import matplotlib.colors as mcolors
%matplotlib inline
```

## 可视化辅助函数

```python
def draw(G, pos, measures, measure_name):
    
    nodes = nx.draw_networkx_nodes(G, pos, node_size=250, cmap=plt.cm.plasma, 
                                   node_color=list(measures.values()),
                                   nodelist=measures.keys())
    nodes.set_norm(mcolors.SymLogNorm(linthresh=0.01, linscale=1, base=10))
    # labels = nx.draw_networkx_labels(G, pos)
    edges = nx.draw_networkx_edges(G, pos)

    # plt.figure(figsize=(10,8))
    plt.title(measure_name)
    plt.colorbar(nodes)
    plt.axis('off')
    plt.show()
```

## 导入无向图

```python
G = nx.karate_club_graph()
```

```python
pos = nx.spring_layout(G, seed=675)
nx.draw(G, pos, with_labels=True)
```

## 导入有向图

```python
DiG = nx.DiGraph()
DiG.add_edges_from([(2, 3), (3, 2), (4, 1), (4, 2), (5, 2), (5, 4),
                    (5, 6), (6, 2), (6, 5), (7, 2), (7, 5), (8, 2),
                    (8, 5), (9, 2), (9, 5), (10, 5), (11, 5)])
# dpos = {1: [0.1, 0.9], 2: [0.4, 0.8], 3: [0.8, 0.9], 4: [0.15, 0.55],
#         5: [0.5,  0.5], 6: [0.8,  0.5], 7: [0.22, 0.3], 8: [0.30, 0.27],
#         9: [0.38, 0.24], 10: [0.7,  0.3], 11: [0.75, 0.35]}
```

```python
nx.draw(DiG, pos, with_labels=True)
```

## Node Degree

```python
list(nx.degree(G))
```

    [(0, 16),
     (1, 9),
     (2, 10),
     (3, 6),
     (4, 3),
     (5, 4),
     (6, 4),
     (7, 4),
     (8, 5),
     (9, 2),
     (10, 3),
     (11, 1),
     (12, 2),
     (13, 5),
     (14, 2),
     (15, 2),
     (16, 2),
     (17, 2),
     (18, 2),
     (19, 3),
     (20, 2),
     (21, 2),
     (22, 2),
     (23, 5),
     (24, 3),
     (25, 3),
     (26, 2),
     (27, 4),
     (28, 3),
     (29, 4),
     (30, 4),
     (31, 6),
     (32, 12),
     (33, 17)]

```python
dict(G.degree())
```

    {0: 16,
     1: 9,
     2: 10,
     3: 6,
     4: 3,
     5: 4,
     6: 4,
     7: 4,
     8: 5,
     9: 2,
     10: 3,
     11: 1,
     12: 2,
     13: 5,
     14: 2,
     15: 2,
     16: 2,
     17: 2,
     18: 2,
     19: 3,
     20: 2,
     21: 2,
     22: 2,
     23: 5,
     24: 3,
     25: 3,
     26: 2,
     27: 4,
     28: 3,
     29: 4,
     30: 4,
     31: 6,
     32: 12,
     33: 17}

```python
# 字典按值排序
sorted(dict(G.degree()).items(),key=lambda x : x[1], reverse=True)   
```

    [(33, 17),
     (0, 16),
     (32, 12),
     (2, 10),
     (1, 9),
     (3, 6),
     (31, 6),
     (8, 5),
     (13, 5),
     (23, 5),
     (5, 4),
     (6, 4),
     (7, 4),
     (27, 4),
     (29, 4),
     (30, 4),
     (4, 3),
     (10, 3),
     (19, 3),
     (24, 3),
     (25, 3),
     (28, 3),
     (9, 2),
     (12, 2),
     (14, 2),
     (15, 2),
     (16, 2),
     (17, 2),
     (18, 2),
     (20, 2),
     (21, 2),
     (22, 2),
     (26, 2),
     (11, 1)]

```python
draw(G, pos, dict(G.degree()), 'Node Degree')
```

## NetworkX文档：节点重要度特征 Centrality

<https://networkx.org/documentation/stable/reference/algorithms/centrality.html>

## Degree Centrality-无向图

```python
nx.degree_centrality(G)
```

    {0: 0.48484848484848486,
     1: 0.2727272727272727,
     2: 0.30303030303030304,
     3: 0.18181818181818182,
     4: 0.09090909090909091,
     5: 0.12121212121212122,
     6: 0.12121212121212122,
     7: 0.12121212121212122,
     8: 0.15151515151515152,
     9: 0.06060606060606061,
     10: 0.09090909090909091,
     11: 0.030303030303030304,
     12: 0.06060606060606061,
     13: 0.15151515151515152,
     14: 0.06060606060606061,
     15: 0.06060606060606061,
     16: 0.06060606060606061,
     17: 0.06060606060606061,
     18: 0.06060606060606061,
     19: 0.09090909090909091,
     20: 0.06060606060606061,
     21: 0.06060606060606061,
     22: 0.06060606060606061,
     23: 0.15151515151515152,
     24: 0.09090909090909091,
     25: 0.09090909090909091,
     26: 0.06060606060606061,
     27: 0.12121212121212122,
     28: 0.09090909090909091,
     29: 0.12121212121212122,
     30: 0.12121212121212122,
     31: 0.18181818181818182,
     32: 0.36363636363636365,
     33: 0.5151515151515151}

```python
draw(G, pos, nx.degree_centrality(G), 'Degree Centrality')
```

## Degree Centrality-有向图

```python
nx.in_degree_centrality(DiG)
```

    {2: 0.7000000000000001,
     3: 0.1,
     4: 0.1,
     1: 0.1,
     5: 0.6000000000000001,
     6: 0.1,
     7: 0.0,
     8: 0.0,
     9: 0.0,
     10: 0.0,
     11: 0.0}

```python
nx.out_degree_centrality(DiG)
```

    {2: 0.1,
     3: 0.1,
     4: 0.2,
     1: 0.0,
     5: 0.30000000000000004,
     6: 0.2,
     7: 0.2,
     8: 0.2,
     9: 0.2,
     10: 0.1,
     11: 0.1}

```python
draw(DiG, pos, nx.in_degree_centrality(DiG), 'DiGraph Degree Centrality')
```

```python
draw(DiG, pos, nx.out_degree_centrality(DiG), 'DiGraph Degree Centrality')
```

## Eigenvector Centrality-无向图

```python
nx.eigenvector_centrality(G)
```

    {0: 0.3554834941851943,
     1: 0.2659538704545025,
     2: 0.31718938996844476,
     3: 0.2111740783205706,
     4: 0.07596645881657382,
     5: 0.07948057788594247,
     6: 0.07948057788594247,
     7: 0.17095511498035434,
     8: 0.2274050914716605,
     9: 0.10267519030637758,
     10: 0.07596645881657381,
     11: 0.05285416945233648,
     12: 0.08425192086558088,
     13: 0.22646969838808148,
     14: 0.10140627846270832,
     15: 0.10140627846270832,
     16: 0.023634794260596875,
     17: 0.09239675666845953,
     18: 0.10140627846270832,
     19: 0.14791134007618667,
     20: 0.10140627846270832,
     21: 0.09239675666845953,
     22: 0.10140627846270832,
     23: 0.15012328691726787,
     24: 0.05705373563802805,
     25: 0.05920820250279008,
     26: 0.07558192219009324,
     27: 0.13347932684333308,
     28: 0.13107925627221215,
     29: 0.13496528673866567,
     30: 0.17476027834493085,
     31: 0.19103626979791702,
     32: 0.3086510477336959,
     33: 0.373371213013235}

```python
draw(G, pos, nx.eigenvector_centrality(G), 'Eigenvector Centrality')
```

## Eigenvector Centrality-有向图

```python
nx.eigenvector_centrality_numpy(DiG)
```

    {2: 0.7071067894491988,
     3: 0.7071067729238959,
     4: 1.1016868750601816e-08,
     1: 1.1016867973445699e-08,
     5: 1.1016868778357392e-08,
     6: 1.101686872284624e-08,
     7: -5.551115123125783e-17,
     8: -2.7755575615628914e-17,
     9: -0.0,
     10: -0.0,
     11: 5.551115123125783e-17}

```python
draw(DiG, pos, nx.eigenvector_centrality_numpy(DiG), 'DiGraph Eigenvector Centrality')
```

## Betweenness Centrality（必经之地）

```python
nx.betweenness_centrality?
```

```python
nx.betweenness_centrality??
```

```python
nx.betweenness_centrality(G)
```

    {0: 0.43763528138528146,
     1: 0.053936688311688304,
     2: 0.14365680615680618,
     3: 0.011909271284271283,
     4: 0.0006313131313131313,
     5: 0.02998737373737374,
     6: 0.029987373737373736,
     7: 0.0,
     8: 0.05592682780182781,
     9: 0.0008477633477633478,
     10: 0.0006313131313131313,
     11: 0.0,
     12: 0.0,
     13: 0.04586339586339586,
     14: 0.0,
     15: 0.0,
     16: 0.0,
     17: 0.0,
     18: 0.0,
     19: 0.03247504810004811,
     20: 0.0,
     21: 0.0,
     22: 0.0,
     23: 0.017613636363636363,
     24: 0.0022095959595959595,
     25: 0.0038404882154882154,
     26: 0.0,
     27: 0.02233345358345358,
     28: 0.0017947330447330447,
     29: 0.0029220779220779218,
     30: 0.014411976911976909,
     31: 0.13827561327561325,
     32: 0.145247113997114,
     33: 0.30407497594997596}

```python
draw(G, pos, nx.betweenness_centrality(G), 'Betweenness Centrality')
```

## Closeness Centrality（去哪儿都近）

```python
nx.closeness_centrality(G)
```

    {0: 0.5689655172413793,
     1: 0.4852941176470588,
     2: 0.559322033898305,
     3: 0.4647887323943662,
     4: 0.3793103448275862,
     5: 0.38372093023255816,
     6: 0.38372093023255816,
     7: 0.44,
     8: 0.515625,
     9: 0.4342105263157895,
     10: 0.3793103448275862,
     11: 0.36666666666666664,
     12: 0.3707865168539326,
     13: 0.515625,
     14: 0.3707865168539326,
     15: 0.3707865168539326,
     16: 0.28448275862068967,
     17: 0.375,
     18: 0.3707865168539326,
     19: 0.5,
     20: 0.3707865168539326,
     21: 0.375,
     22: 0.3707865168539326,
     23: 0.39285714285714285,
     24: 0.375,
     25: 0.375,
     26: 0.3626373626373626,
     27: 0.4583333333333333,
     28: 0.4520547945205479,
     29: 0.38372093023255816,
     30: 0.4583333333333333,
     31: 0.5409836065573771,
     32: 0.515625,
     33: 0.55}

```python
draw(G, pos, nx.closeness_centrality(G), 'Closeness Centrality')
```

## PageRank

```python
nx.pagerank(DiG, alpha=0.85)
```

    {2: 0.38439863456604384,
     3: 0.3429125997558898,
     4: 0.039087092099966095,
     1: 0.03278149315934399,
     5: 0.08088569323449774,
     6: 0.039087092099966095,
     7: 0.016169479016858404,
     8: 0.016169479016858404,
     9: 0.016169479016858404,
     10: 0.016169479016858404,
     11: 0.016169479016858404}

```python
draw(DiG, pos, nx.pagerank(DiG, alpha=0.85), 'DiGraph PageRank')
```

## Katz Centrality

```python
nx.katz_centrality(G, alpha=0.1, beta=1.0)
```

    {0: 0.3213245969592325,
     1: 0.2354842531944946,
     2: 0.2657658848154288,
     3: 0.1949132024917254,
     4: 0.12190440564948413,
     5: 0.1309722793286492,
     6: 0.1309722793286492,
     7: 0.166233052026894,
     8: 0.2007178109661081,
     9: 0.12420150029869696,
     10: 0.12190440564948413,
     11: 0.09661674181730141,
     12: 0.11610805572826272,
     13: 0.19937368057318847,
     14: 0.12513342642033795,
     15: 0.12513342642033795,
     16: 0.09067874388549631,
     17: 0.12016515915440099,
     18: 0.12513342642033795,
     19: 0.15330578770069542,
     20: 0.12513342642033795,
     21: 0.12016515915440099,
     22: 0.12513342642033795,
     23: 0.16679064809871574,
     24: 0.11021106930146936,
     25: 0.11156461274962841,
     26: 0.11293552094158042,
     27: 0.1519016658208186,
     28: 0.143581654735333,
     29: 0.15310603655041516,
     30: 0.16875361802889585,
     31: 0.19380160170200547,
     32: 0.2750851434662392,
     33: 0.3314063975218936}

```python
draw(G, pos, nx.katz_centrality(G, alpha=0.1, beta=1.0), 'Katz Centrality')
```

```python
draw(DiG, pos, nx.katz_centrality(DiG, alpha=0.1, beta=1.0), 'DiGraph Katz Centrality')
```

## HITS Hubs and Authorities

```python
h, a = nx.hits(DiG)
draw(DiG, pos, h, 'DiGraph HITS Hubs')
draw(DiG, pos, a, 'DiGraph HITS Authorities')
```

## NetworkX文档：社群属性 Clustering

<https://networkx.org/documentation/stable/reference/algorithms/clustering.html>

```python
nx.draw(G, pos, with_labels=True)
```

## 三角形个数

```python
nx.triangles(G)
```

    {0: 18,
     1: 12,
     2: 11,
     3: 10,
     4: 2,
     5: 3,
     6: 3,
     7: 6,
     8: 5,
     9: 0,
     10: 2,
     11: 0,
     12: 1,
     13: 6,
     14: 1,
     15: 1,
     16: 1,
     17: 1,
     18: 1,
     19: 1,
     20: 1,
     21: 1,
     22: 1,
     23: 4,
     24: 1,
     25: 1,
     26: 1,
     27: 1,
     28: 1,
     29: 4,
     30: 3,
     31: 3,
     32: 13,
     33: 15}

```python
nx.triangles(G, 0)
```

    18

```python
draw(G, pos, nx.triangles(G), 'Triangles')
```

## Clustering Coefficient

```python
nx.clustering(G)
```

    {0: 0.15,
     1: 0.3333333333333333,
     2: 0.24444444444444444,
     3: 0.6666666666666666,
     4: 0.6666666666666666,
     5: 0.5,
     6: 0.5,
     7: 1.0,
     8: 0.5,
     9: 0,
     10: 0.6666666666666666,
     11: 0,
     12: 1.0,
     13: 0.6,
     14: 1.0,
     15: 1.0,
     16: 1.0,
     17: 1.0,
     18: 1.0,
     19: 0.3333333333333333,
     20: 1.0,
     21: 1.0,
     22: 1.0,
     23: 0.4,
     24: 0.3333333333333333,
     25: 0.3333333333333333,
     26: 1.0,
     27: 0.16666666666666666,
     28: 0.3333333333333333,
     29: 0.6666666666666666,
     30: 0.5,
     31: 0.2,
     32: 0.19696969696969696,
     33: 0.11029411764705882}

```python
nx.clustering(G, 0)
```

    0.15

```python
draw(G, pos, nx.clustering(G), 'Clustering Coefficient')
```

## Bridges

如果某个连接断掉，会使连通域个数增加，则该连接是bridge。

bridge连接不属于环的一部分。

```python
pos = nx.spring_layout(G, seed=675)
nx.draw(G, pos, with_labels=True)
```

```python
list(nx.bridges(G))
```

    [(0, 11)]

## Common Neighbors 和 Jaccard Coefficient

```python
pos = nx.spring_layout(G, seed=675)
nx.draw(G, pos, with_labels=True)
```

```python
sorted(nx.common_neighbors(G, 0, 4))
```

    [6, 10]

```python
preds = nx.jaccard_coefficient(G, [(0, 1), (2, 3)])
for u, v, p in preds:
    print(f"({u}, {v}) -> {p:.8f}")
```

    (0, 1) -> 0.38888889
    (2, 3) -> 0.33333333

```python
for u, v, p in nx.adamic_adar_index(G, [(0, 1), (2, 3)]):
    print(f"({u}, {v}) -> {p:.8f}")
```

    (0, 1) -> 6.13071687
    (2, 3) -> 2.15847583

## Katz Index

节点u到节点v，路径为k的路径个数。

```python
import networkx as nx
import numpy as np
from numpy.linalg import inv
G = nx.karate_club_graph()
```

```python
len(G.nodes)
```

    34

```python
# 计算主特征向量
L = nx.normalized_laplacian_matrix(G)
e = np.linalg.eigvals(L.A)
print('最大特征值', max(e))

# 折减系数
beta = 1/max(e)

# 创建单位矩阵
I = np.identity(len(G.nodes))

# 计算 Katz Index
S = inv(I - nx.to_numpy_array(G)*beta) - I
```

    最大特征值 1.7146113474736193

```python
S.shape
```

    (34, 34)

```python
S
```

    array([[-0.630971  ,  0.03760311, -0.50718655, ...,  0.22028562,
             0.08051109,  0.0187629 ],
           [ 0.03760311,  0.0313979 , -1.09231501, ...,  0.18920621,
            -0.09098329,  0.08188737],
           [-0.50718655, -1.09231501,  0.79993439, ..., -0.4511988 ,
             0.17631358, -0.23914987],
           ...,
           [ 0.22028562,  0.18920621, -0.4511988 , ..., -0.07349891,
             0.47525815, -0.0457034 ],
           [ 0.08051109, -0.09098329,  0.17631358, ...,  0.47525815,
            -0.28781332, -0.70104834],
           [ 0.0187629 ,  0.08188737, -0.23914987, ..., -0.0457034 ,
            -0.70104834, -0.50717615]])

