# PageRank实战-西游记人物节点重要程度


# PageRank节点重要度

在NetworkX中，计算有向图节点的PageRank节点重要度。

## 参考资料

networkx官方教程：<https://networkx.org/documentation/stable/tutorial.html>

nx.Graph <https://networkx.org/documentation/stable/reference/classes/graph.html#networkx.Graph>

给图、节点、连接添加属性：<https://networkx.org/documentation/stable/tutorial.html#attributes>

读写图：<https://networkx.org/documentation/stable/reference/readwrite/index.html>

## 导入工具包

```python
import networkx as nx # 图数据挖掘
import numpy as np # 数据分析
import random # 随机数
import pandas as pd
 
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

## 计算PageRank节点重要度

数据下载地址：http://www.openkg.cn/dataset/ch4masterpieces

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

```python
# 导入 csv 文件定义的有向图
df = pd.read_csv('./triples.csv')
```

```python
df
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>head</th>
      <th>tail</th>
      <th>relation</th>
      <th>label</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>金蝉子</td>
      <td>唐僧</td>
      <td>past_life</td>
      <td>前世</td>
    </tr>
    <tr>
      <th>1</th>
      <td>孙悟空</td>
      <td>唐僧</td>
      <td>apprentice</td>
      <td>徒弟</td>
    </tr>
    <tr>
      <th>2</th>
      <td>猪八戒</td>
      <td>唐僧</td>
      <td>apprentice</td>
      <td>徒弟</td>
    </tr>
    <tr>
      <th>3</th>
      <td>沙僧</td>
      <td>唐僧</td>
      <td>apprentice</td>
      <td>徒弟</td>
    </tr>
    <tr>
      <th>4</th>
      <td>白龙马</td>
      <td>唐僧</td>
      <td>apprentice</td>
      <td>徒弟</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>104</th>
      <td>毗蓝婆菩萨</td>
      <td>昴日星官</td>
      <td>mother</td>
      <td>母亲</td>
    </tr>
    <tr>
      <th>105</th>
      <td>嫦娥</td>
      <td>后羿</td>
      <td>wife</td>
      <td>妻</td>
    </tr>
    <tr>
      <th>106</th>
      <td>敖摩昂</td>
      <td>敖闰</td>
      <td>son</td>
      <td>儿</td>
    </tr>
    <tr>
      <th>107</th>
      <td>哪吒</td>
      <td>李靖</td>
      <td>son</td>
      <td>儿</td>
    </tr>
    <tr>
      <th>108</th>
      <td>哪吒</td>
      <td>如来</td>
      <td>apprentice</td>
      <td>徒弟</td>
    </tr>
  </tbody>
</table>
<p>109 rows × 4 columns</p>
</div>

```python
edges = [edge for edge in zip(df['head'], df['tail'])]
 
G = nx.DiGraph()
G.add_edges_from(edges)
```

```python
# 可视化
plt.figure(figsize=(15,14))
pos = nx.spring_layout(G, iterations=3, seed=5) #设置为基于弹簧布局，迭代次数为3，次数越多，根据节点之间的相互作用力找到的节点位置越合理
nx.draw(G, pos, with_labels=True)
plt.show()
```

```python
pagerank = nx.pagerank(G,                     # NetworkX graph 有向图，如果是无向图则自动转为双向有向图
                       alpha=0.85,            # Damping Factor，阻尼系数，理解这个得理解谷歌矩阵，也就是明白算法本身才行
                       personalization=None,  # 是否开启Personalized PageRank，随机传送至指定节点集合的概率更高或更低
                       max_iter=100,          # 最大迭代次数
                       tol=1e-06,             # 判定收敛的误差
                       nstart=None,           # 每个节点初始PageRank值      
                       dangling=None,         # Dead End死胡同节点
                      )
```

```python
sorted(pagerank.items(), key=lambda x: x[1], reverse=True)
```

    [('唐僧', 0.13349105557884888),
     ('孙悟空', 0.10498354112014094),
     ('白龙马', 0.09531260474698808),
     ('猪八戒', 0.09247797536009736),
     ('沙僧', 0.07627154154696374),
     ('李世民', 0.052002919751408624),
     ('观音菩萨', 0.026625716774094633),
     ('高翠兰', 0.02579183411604112),
     ('卵二姐', 0.01860884001045803),
     ('太上老君', 0.014430996933862522),
     ('如来', 0.013334300311185142),
     ('牛魔王', 0.010256020230003658),
     ('哪吒', 0.009171370913926254),
     ('灵吉菩萨', 0.007800320258156309),
     ('宼栋', 0.007432108638238391),
     ('昴日星官', 0.007432108638238391),
     ('后羿', 0.007432108638238391),
     ('李靖', 0.006787403654483575),
     ('殷温娇', 0.005344620286308959),
     ('寇梁', 0.005344620286308959),
     ('袁天罡', 0.005344620286308959),
     ('金角', 0.005344620286308959),
     ('银角', 0.005344620286308959),
     ('西海龙王太子', 0.005344620286308959),
     ('弥勒佛', 0.005344620286308959),
     ('毗蓝婆菩萨', 0.005344620286308959),
     ('文殊菩萨', 0.005344620286308959),
     ('普贤菩萨', 0.005344620286308959),
     ('太乙救苦天尊', 0.005344620286308959),
     ('嫦娥', 0.005344620286308959),
     ('南极寿星', 0.005344620286308959),
     ('东来佛祖笑和尚', 0.005344620286308959),
     ('敖闰', 0.005344620286308959),
     ('木吒', 0.004812288400108432),
     ('金吒', 0.004812288400108432),
     ('高玉兰', 0.004116770300385284),
     ('金蝉子', 0.0028889203144616088),
     ('陈光蕊', 0.0028889203144616088),
     ('法明和尚', 0.0028889203144616088),
     ('殷开山', 0.0028889203144616088),
     ('菩提老祖', 0.0028889203144616088),
     ('镇元子', 0.0028889203144616088),
     ('蛟魔王', 0.0028889203144616088),
     ('鹏魔王', 0.0028889203144616088),
     ('狮驼王', 0.0028889203144616088),
     ('猕猴王', 0.0028889203144616088),
     ('禺狨王', 0.0028889203144616088),
     ('天蓬元帅', 0.0028889203144616088),
     ('卷帘大将', 0.0028889203144616088),
     ('西海龙王', 0.0028889203144616088),
     ('西海龙母', 0.0028889203144616088),
     ('敖摩昂太子', 0.0028889203144616088),
     ('西海龙女', 0.0028889203144616088),
     ('李渊', 0.0028889203144616088),
     ('李建成', 0.0028889203144616088),
     ('李元吉', 0.0028889203144616088),
     ('王珪', 0.0028889203144616088),
     ('秦琼', 0.0028889203144616088),
     ('萧瑀', 0.0028889203144616088),
     ('傅奕', 0.0028889203144616088),
     ('魏征', 0.0028889203144616088),
     ('李玉英', 0.0028889203144616088),
     ('房玄龄', 0.0028889203144616088),
     ('杜如晦', 0.0028889203144616088),
     ('徐世绩', 0.0028889203144616088),
     ('徐茂公', 0.0028889203144616088),
     ('许敬宗', 0.0028889203144616088),
     ('马三宝', 0.0028889203144616088),
     ('段志贤', 0.0028889203144616088),
     ('程咬金', 0.0028889203144616088),
     ('虞世南', 0.0028889203144616088),
     ('张道源', 0.0028889203144616088),
     ('张士衡', 0.0028889203144616088),
     ('高太公', 0.0028889203144616088),
     ('高香兰', 0.0028889203144616088),
     ('寇洪', 0.0028889203144616088),
     ('袁守诚', 0.0028889203144616088),
     ('正元龙', 0.0028889203144616088),
     ('二十四路诸天', 0.0028889203144616088),
     ('守山大神', 0.0028889203144616088),
     ('善财童子', 0.0028889203144616088),
     ('捧珠龙女', 0.0028889203144616088),
     ('红孩儿', 0.0028889203144616088),
     ('黑风怪', 0.0028889203144616088),
     ('黄风怪', 0.0028889203144616088),
     ('黄毛貂鼠', 0.0028889203144616088),
     ('铁扇公主', 0.0028889203144616088),
     ('九尾狐狸', 0.0028889203144616088),
     ('狐阿七', 0.0028889203144616088),
     ('鼍龙怪', 0.0028889203144616088),
     ('灵感大王', 0.0028889203144616088),
     ('独角兕大王', 0.0028889203144616088),
     ('玉面公主', 0.0028889203144616088),
     ('金毛犼', 0.0028889203144616088),
     ('黄眉道童', 0.0028889203144616088),
     ('百眼魔君', 0.0028889203144616088),
     ('青狮', 0.0028889203144616088),
     ('白象', 0.0028889203144616088),
     ('大鹏金翅雕', 0.0028889203144616088),
     ('九头狮子', 0.0028889203144616088),
     ('玉兔精', 0.0028889203144616088),
     ('白鹿精', 0.0028889203144616088),
     ('黄眉大王', 0.0028889203144616088),
     ('敖摩昂', 0.0028889203144616088)]

```python
# 节点尺寸
node_sizes = (np.array(list(pagerank.values())) * 8000).astype(int)
```

```python
node_sizes
```

    array([  23, 1067,  839,  739,  610,  762,   23,   42,   23,  416,   23,
             23,   23,   82,   23,   23,   23,   23,   23,   23,  148,  206,
             23,   23,   23,   23,   23,   23,   23,   23,   23,   23,   23,
             23,   23,   23,   23,   23,   23,   23,   23,   23,   23,   23,
             23,   23,   23,   23,   23,   32,   23,   42,   59,   23,   42,
             54,   38,   73,   38,   23,  213,   23,   23,   23,   23,  106,
             23,   23,   23,   62,   23,   42,  115,   42,   23,   23,   23,
             23,   42,   23,   23,   23,   23,   23,   42,   23,   42,   23,
             42,   23,   42,   23,   23,   42,   23,   42,   23,   42,   23,
             42,   59,   59,   23,   42])

```python
# 节点颜色
M = G.number_of_edges()
edge_colors = range(2, M + 2)
 
 
plt.figure(figsize=(15,14))
 
# 绘制节点
nodes = nx.draw_networkx_nodes(G, pos, node_size=node_sizes, node_color=node_sizes)
 
# 绘制连接
edges = nx.draw_networkx_edges(
    G,
    pos,
    node_size=node_sizes,   # 节点尺寸
    arrowstyle="->",        # 箭头样式
    arrowsize=20,           # 箭头尺寸
    edge_color=edge_colors, # 连接颜色
    edge_cmap=plt.cm.plasma,# 连接配色方案，可选：plt.cm.Blues
    width=4                 # 连接线宽
)
 
# 设置每个连接的透明度
edge_alphas = [(5 + i) / (M + 4) for i in range(M)]
for i in range(M):
    edges[i].set_alpha(edge_alphas[i])
 
# # 图例
# pc = mpl.collections.PatchCollection(edges, cmap=cmap)
# pc.set_array(edge_colors)
# plt.colorbar(pc)
 
ax = plt.gca()
ax.set_axis_off()
plt.show()
```

    /tmp/ipykernel_206894/3554403709.py:12: DeprecationWarning: `alltrue` is deprecated as of NumPy 1.25.0, and will be removed in NumPy 2.0. Please use `all` instead.
      edges = nx.draw_networkx_edges(

