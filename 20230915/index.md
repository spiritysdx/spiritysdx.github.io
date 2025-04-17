# NetworkX基本操作


# 前言

当然，还是习惯 linux + jupyterlab 的组合，当然还是基于

<https://www.spiritlhl.net/case/case1.html#%E4%B8%80%E9%94%AE%E5%AE%89%E8%A3%85jupyter%E7%8E%AF%E5%A2%83>

构建基础环境

# 安装

由于是非国内服务器，无需清华镜像

```
!pip3 install numpy pandas matplotlib tqdm networkx
# -i https://pypi.tuna.tsinghua.edu.cn/simple
```

验证是否安装成功

```
import networkx as nx
nx.__version__
```

设置matplotlib中文字体

```
import matplotlib.pyplot as plt
%matplotlib inline
plt.rcParams['font.sans-serif'] = ['SimHei'] 
plt.rcParams['axes.unicode_minus'] = False 
```

如果本身不存在对应字体，需要执行

```
import os
import shutil
import urllib.request
import matplotlib

# 设置字体文件的URL和目标文件路径
font_url = "https://github.com/StellarCN/scp_zh/raw/master/fonts/SimHei.ttf"
target_font_path = os.path.join(matplotlib.get_data_path(), "fonts/ttf/SimHei.ttf")

# 下载字体文件
urllib.request.urlretrieve(font_url, target_font_path)

# 找到matplotlibrc文件的路径
matplotlibrc_path = matplotlib.matplotlib_fname()
# 获取matplotlibrc文件所在的文件夹路径
matplotlibrc_dir = os.path.dirname(matplotlibrc_path)

# 找到fonts/ttf目录的路径
fonts_dir = os.path.join(matplotlib.get_data_path(), "fonts/ttf")

# 删除缓存文件夹中的内容
cache_dir = matplotlib.get_cachedir()
shutil.rmtree(cache_dir, ignore_errors=True)

print("字体文件已下载到:", target_font_path)
print("matplotlibrc文件所在目录:", matplotlibrc_dir)
print("删除缓存文件夹:", cache_dir)
```

进行字体下载和配置，然后需要重启Python内核，此处需要在KERNELS中删除已经存在的活动内核，重新运行ipynb文件(运行后会要求你选择使用什么内核，使用默认的就行了)。

如果要查询某个函数的用法和源代码，在jupyterlab中只需要在后面加上一个英文?号就能查看用法，两个英文?号就是查询函数源代码，在命令前面加上英文!号就相当于在命令行执行命令而不是使用python解释器去解释执行。

# 参考文档

NetworkX主页：<https://networkx.org>

networkx创建图：<https://networkx.org/documentation/stable/reference/generators.html>

# 正式使用

## 导入工具包

```python
import networkx as nx

# 数据可视化
import matplotlib.pyplot as plt
%matplotlib inline

plt.rcParams['font.sans-serif']=['SimHei']  # 用来正常显示中文标签  
plt.rcParams['axes.unicode_minus']=False  # 用来正常显示负号
```

## 经典图结构

### 全连接无向图

```python
G = nx.complete_graph(7)
```

```python
nx.draw(G)
```

```python
# 全图连接数
G.size()
```

    21

### 全连接有向图

```python
G = nx.complete_graph(7, nx.DiGraph())
```

```python
nx.draw(G)
```

```python
G.is_directed()
```

    True

### 环状图

```python
G = nx.cycle_graph(5)
```

```python
nx.draw(G)
```

### 梯状图

```python
G = nx.ladder_graph(5)
```

```python
nx.draw(G)
```

### 线性串珠图

```python
G = nx.path_graph(15)
```

```python
nx.draw(G)
```

### 星状图

```python
G = nx.star_graph(7)
```

```python
nx.draw(G)
```

### 轮辐图

```python
G = nx.wheel_graph(8)
```

```python
nx.draw(G)
```

### 二项树

```python
G = nx.binomial_tree(5)
```

```python
nx.draw(G)
```

## 栅格图

### 二维矩形网格图

```python
G = nx.grid_2d_graph(3,5)
```

```python
nx.draw(G)
```

### 多维矩形网格图

```python
G = nx.grid_graph(dim=(2, 3, 4))
```

```python
nx.draw(G)
```

### 二维三角形网格图

```python
G = nx.triangular_lattice_graph(2,5)
```

```python
nx.draw(G)
```

### 二维六边形蜂窝图

```python
G = nx.hexagonal_lattice_graph(2,3)
```

```python
nx.draw(G)
```

### n维超立方体图

```python
G = nx.hypercube_graph(4)
```

```python
nx.draw(G)
```

## NetworkX内置图

```python
G = nx.diamond_graph()
```

```python
nx.draw(G)
```

```python
G = nx.bull_graph()
nx.draw(G)
```

```python
G = nx.frucht_graph()
nx.draw(G)
```

```python
G = nx.house_graph()
nx.draw(G)
```

```python
G = nx.house_x_graph()
nx.draw(G)
```

```python
G = nx.petersen_graph()
nx.draw(G)
```

```python
G = nx.krackhardt_kite_graph()
nx.draw(G)
```

## 随机图

```python
G = nx.erdos_renyi_graph(10, 0.5)
nx.draw(G)
```

## 有向图

### 无标度有向图

```python
G = nx.scale_free_graph(100)
nx.draw(G)
```

## 社交网络

```python
# 空手道俱乐部数据集
G = nx.karate_club_graph()
nx.draw(G, with_labels=True)
```

```python
G.nodes[5]["club"]
```

    'Mr. Hi'

```python
G.nodes[9]["club"]
```

    'Officer'

```python
# 雨果《悲惨世界》人物关系
G = nx.les_miserables_graph()
plt.figure(figsize=(12,10))
pos = nx.spring_layout(G, seed=10)
nx.draw(G, pos, with_labels=True)
```

```python
# Florentine families graph
G = nx.florentine_families_graph()
nx.draw(G, with_labels=True)
```

## 社群聚类图

```python
G = nx.caveman_graph(4, 3)
nx.draw(G, with_labels=True)
```

## 树

```python
tree = nx.random_tree(n=10, seed=0)
print(nx.forest_str(tree, sources=[0]))
```

    ╙── 0
        ├── 3
        └── 4
            ├── 6
            │   ├── 1
            │   ├── 2
            │   └── 7
            │       └── 8
            │           └── 5
            └── 9

# 连接表和邻接表创建图

## 导入三元组连接表

数据来源：OpenKG-四大名著人物关系知识图谱和OWL本体：<http://www.openkg.cn/dataset/ch4masterpieces>

```python
# 导入 csv 文件定义的三元组连接表，构建有向图
df = pd.read_csv('triples.csv')
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
      <td>关羽</td>
      <td>刘备</td>
      <td>younger_sworn_brother</td>
      <td>义弟</td>
    </tr>
    <tr>
      <th>1</th>
      <td>张飞</td>
      <td>刘备</td>
      <td>younger_sworn_brother</td>
      <td>义弟</td>
    </tr>
    <tr>
      <th>2</th>
      <td>关羽</td>
      <td>张飞</td>
      <td>elder_sworn_brother</td>
      <td>义兄</td>
    </tr>
    <tr>
      <th>3</th>
      <td>张苞</td>
      <td>张飞</td>
      <td>son</td>
      <td>儿子</td>
    </tr>
    <tr>
      <th>4</th>
      <td>关兴</td>
      <td>关羽</td>
      <td>son</td>
      <td>儿子</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>148</th>
      <td>曹植</td>
      <td>曹丕</td>
      <td>younger_brother</td>
      <td>弟弟</td>
    </tr>
    <tr>
      <th>149</th>
      <td>马谡</td>
      <td>诸葛亮</td>
      <td>colleague</td>
      <td>同事</td>
    </tr>
    <tr>
      <th>150</th>
      <td>马谡</td>
      <td>刘备</td>
      <td>minister</td>
      <td>臣</td>
    </tr>
    <tr>
      <th>151</th>
      <td>孙坚</td>
      <td>孙权</td>
      <td>father</td>
      <td>父亲</td>
    </tr>
    <tr>
      <th>152</th>
      <td>吴国太</td>
      <td>孙权</td>
      <td>mother</td>
      <td>母亲</td>
    </tr>
  </tbody>
</table>
<p>153 rows × 4 columns</p>
</div>

## 通过连接表Edge List创建图

```python
G = nx.DiGraph()
```

```python
edges = [edge for edge in zip(df['head'], df['tail'])]

G.add_edges_from(edges)
```

```python
G.edges('关羽')
```

    OutEdgeDataView([('关羽', '刘备'), ('关羽', '张飞')])

## 可视化

```python
# 节点排版布局-默认弹簧布局
pos = nx.spring_layout(G, seed=123)

plt.figure(figsize=(15,15))
nx.draw(G, pos=pos, with_labels=True)
```

## 查看全图参数

```python
print(G)
```

    DiGraph with 123 nodes and 144 edges

```python
len(G)
```

    123

```python
G.size()
```

    144

```python
G.nodes
```

    NodeView(('关羽', '刘备', '张飞', '张苞', '关兴', '关平', '卢植', '公孙瓒', '甘氏', '刘禅', '诸葛瞻', '诸葛亮', '姜维', '黄月英', '黄承彦', '诸葛瑾', '公孙越', '马超', '马腾', '韩遂', '徐庶', '曹操', '刘胜', '刘启', '刘辩', '孙权', '孙尚香', '糜氏', '糜芳', '糜竺', '魏延', '赵云', '黄忠', '庞统', '法正', '蒋琬', '马良', '孟获', '沙摩柯', '庞德公', '马谡', '祝融', '孙韶', '孙策', '孙氏', '陆逊', '刘协', '董卓', '王允', '貂蝉', '吕布', '丁原', '高顺', '陈宫', '张辽', '刘表', '蔡氏', '蔡瑁', '蒯越', '黄祖', '文聘', '张宝', '张角', '张梁', '袁绍', '袁术', '袁谭', '袁熙', '袁尚', '吴国太', '孙坚', '大乔', '小乔', '周瑜', '丁奉', '徐盛', '鲁肃', '张昭', '蒋钦', '太史慈', '周泰', '凌统', '吕蒙', '甘宁', '黄盖', '韩当', '程普', '曹嵩', '吕伯奢', '邹氏', '张绣', '清河公主', '夏侯楙', '夏侯渊', '夏侯淳', '曹真', '曹爽', '郭嘉', '徐晃', '乐进', '张郃', '许褚', '典韦', '荀彧', '荀攸', '贾诩', '司马懿', '程昱', '于禁', '邓艾', '钟会', '庞德', '司马师', '司马昭', '司马炎', '曹仁', '曹纯', '曹昂', '刘氏', '超昂', '卞氏', '曹丕', '曹植'))

## 保存并载入邻接表 Adjacency List

```python
for line in nx.generate_adjlist(G):
    print(line)
```

    关羽 刘备 张飞
    刘备 诸葛亮 马超 徐庶 姜维 糜芳 糜竺 魏延 赵云 黄忠 庞统 法正 蒋琬 马良 孟获 沙摩柯
    张飞 刘备
    张苞 张飞
    关兴 关羽
    关平 张苞 关羽
    卢植 刘备
    公孙瓒 刘备
    甘氏 刘备
    刘禅 甘氏
    诸葛瞻 刘禅 诸葛亮
    诸葛亮 姜维
    姜维 诸葛亮
    黄月英 诸葛亮
    黄承彦 黄月英
    诸葛瑾 诸葛亮
    公孙越 公孙瓒
    马超
    马腾 马超
    韩遂 马腾
    徐庶
    曹操 徐庶 张辽 蒯越 蔡瑁 张绣 夏侯淳 夏侯渊 曹真 郭嘉 徐晃 乐进 张郃 许褚 典韦 荀彧 荀攸 贾诩 司马懿 程昱 于禁 邓艾 钟会 庞德
    刘胜 刘启
    刘启
    刘辩 刘启
    孙权 诸葛瑾 孙策 周瑜 陆逊 丁奉 徐盛 鲁肃 张昭 蒋钦 太史慈 周泰 凌统 吕蒙 甘宁 黄盖 韩当 程普
    孙尚香 刘备 吴国太
    糜氏 刘备
    糜芳 糜氏
    糜竺 糜芳
    魏延
    赵云
    黄忠
    庞统
    法正
    蒋琬
    马良
    孟获
    沙摩柯
    庞德公 庞统
    马谡 马良 诸葛亮 刘备
    祝融 孟获
    孙韶 孙策
    孙策 孙坚
    孙氏 陆逊 孙策
    陆逊
    刘协 刘辩
    董卓 刘协 吕布
    王允 刘协
    貂蝉 王允 吕布
    吕布 高顺 陈宫 张辽
    丁原 吕布
    高顺
    陈宫
    张辽
    刘表 刘协 黄祖 文聘
    蔡氏 刘表
    蔡瑁 蔡氏
    蒯越 蔡瑁
    黄祖
    文聘
    张宝 张角
    张角
    张梁 张宝
    袁绍 刘协
    袁术 袁绍
    袁谭 袁绍
    袁熙 袁谭
    袁尚 袁熙
    吴国太 孙坚 孙权
    孙坚 孙权
    大乔 孙策 陆逊
    小乔 大乔
    周瑜 小乔
    丁奉
    徐盛
    鲁肃
    张昭
    蒋钦
    太史慈
    周泰
    凌统
    吕蒙
    甘宁
    黄盖
    韩当
    程普
    曹嵩 曹操
    吕伯奢 曹嵩
    邹氏 曹操 张绣
    张绣
    清河公主 曹操
    夏侯楙 清河公主
    夏侯渊 夏侯楙 夏侯淳
    夏侯淳
    曹真
    曹爽 曹真
    郭嘉
    徐晃
    乐进
    张郃
    许褚
    典韦
    荀彧 荀攸
    荀攸
    贾诩
    司马懿
    程昱
    于禁
    邓艾
    钟会
    庞德
    司马师 司马懿
    司马昭 司马师 司马懿
    司马炎 司马昭
    曹仁 曹操
    曹纯 曹仁
    曹昂 曹操
    刘氏 曹操
    超昂 刘氏
    卞氏 曹操
    曹丕 卞氏
    曹植 曹丕

```python
# 将邻接表导出为本地文件 grid.edgelist
nx.write_edgelist(G, path="grid.edgelist", delimiter=":")
```

```python
# 从本地文件 grid.edgelist 读取邻接表
H = nx.read_edgelist(path="grid.edgelist", delimiter=":")
```

```python
# 可视化
plt.figure(figsize=(15,14))
pos = nx.spring_layout(H, iterations=3, seed=5)
nx.draw(H, pos, with_labels=True)
plt.show()
```

# 创建节点

## 创建无节点、无连接的空图

```python
G = nx.Graph()
```

```python
G
```

    <networkx.classes.graph.Graph at 0x7cea48aeb310>

```python
G.nodes
```

    NodeView(())

```python
# 可视化
nx.draw(G)
```

## 添加单个节点

```python
G.add_node('刘备')
```

```python
G.nodes
```

    NodeView(('刘备',))

```python
G.add_node('Tommy')
```

```python
G.nodes
```

    NodeView(('刘备', 'Tommy'))

```python
G.add_node(1)
```

```python
G.nodes
```

    NodeView(('刘备', 'Tommy', 1))

## 添加多个节点

```python
G.add_nodes_from(['诸葛亮', '曹操'])
```

```python
G.nodes
```

    NodeView(('刘备', 'Tommy', 1, '诸葛亮', '曹操'))

```python
G.add_nodes_from(range(100, 105))
```

```python
G.nodes
```

    NodeView(('刘备', 'Tommy', 1, '诸葛亮', '曹操', 100, 101, 102, 103, 104))

## 添加带属性特征的节点

```python
G.add_nodes_from([
    ('关羽',{'武器': '青龙偃月刀','武力值':90,'智力值':80}),
    ('张飞',{'武器': '丈八蛇矛','武力值':85,'智力值':75}),
    ('吕布',{'武器':'方天画戟','武力值':100,'智力值':70})
])
```

```python
G.nodes
```

    NodeView(('刘备', 'Tommy', 1, '诸葛亮', '曹操', 100, 101, 102, 103, 104, '关羽', '张飞', '吕布'))

```python
# 可视化
nx.draw(G)
```

## 创建另一个首尾相连成串的Path Graph

```python
H = nx.path_graph(10)
```

```python
# 可视化
nx.draw(H)
```

```python
H.nodes
```

    NodeView((0, 1, 2, 3, 4, 5, 6, 7, 8, 9))

## 将H的节点添加到G中

```python
G.add_nodes_from(H)
```

```python
G.nodes
```

    NodeView(('刘备', 'Tommy', 1, '诸葛亮', '曹操', 100, 101, 102, 103, 104, '关羽', '张飞', '吕布', 0, 2, 3, 4, 5, 6, 7, 8, 9))

```python
len(G)
```

    22

```python
nx.draw(G)
```

## 将H本身作为一个节点添加到G中

```python
G.add_node(H)
```

```python
G.nodes
```

    NodeView(('刘备', 'Tommy', 1, '诸葛亮', '曹操', 100, 101, 102, 103, 104, '关羽', '张飞', '吕布', 0, 2, 3, 4, 5, 6, 7, 8, 9, <networkx.classes.graph.Graph object at 0x7f5ad7283910>))

```python
len(G)
```

    23

```python
nx.draw(G)
```

## 小贴士

节点可以为任意[可哈希](https://docs.python.org/3/glossary.html#term-hashable)的对象，比如字符串、图像、XML对象，甚至另一个Graph、自定义的节点对象。

通过这种方式，你可以根据你的应用，自由灵活地构建：图为节点、文件为节点、函数为节点，等灵活的图形式。

# 创建图

NetworkX支持有向图（directed graph）、无向图（undirected graph）、带权重的图(weighte graph)、多路图（multigraph）。

文档：<https://networkx.org/documentation/stable/reference/classes/index.html>

```python
# 创建无向图
G = nx.Graph()
print(G.is_directed())
```

    False

```python
# 给整张图添加特征属性
G.graph['Name'] = 'HelloWorld'
print(G.graph)
```

    {'Name': 'HelloWorld'}

```python
# 创建有向图
H = nx.DiGraph()
print(H.is_directed())
```

    True

## 创建单个节点

特征属性的名字可以随便起

```python
# 创建0号节点，并添加特征属性
G.add_node(0, feature=5, label=0, suibianqide=2)
```

```python
G.nodes[0]
```

    {'feature': 5, 'label': 0, 'suibianqide': 2}

## 创建多个节点

```python
G.add_nodes_from([
  (1, {'feature': 1, 'label': 1, 'suibianqide':3}),
  (2, {'feature': 2, 'label': 2, 'suibianqide':4})
])
```

## 全图节点信息

```python
G.number_of_nodes()
```

    3

```python
G.nodes
```

    NodeView((0, 1, 2))

```python
G.nodes(data=True)
```

    NodeDataView({0: {'feature': 5, 'label': 0, 'suibianqide': 2}, 1: {'feature': 1, 'label': 1, 'suibianqide': 3}, 2: {'feature': 2, 'label': 2, 'suibianqide': 4}})

```python
# 遍历所有节点，data=True 表示输出节点特征属性信息

for node in G.nodes(data=True):
    print(node)
```

    (0, {'feature': 5, 'label': 0, 'suibianqide': 2})
    (1, {'feature': 1, 'label': 1, 'suibianqide': 3})
    (2, {'feature': 2, 'label': 2, 'suibianqide': 4})

## 创建单个连接，设置属性特征

特征属性的名字可以随便起

```python
G.add_edge(0, 1, weight=0.5, like=3)
```

## 创建多个连接

```python
G.add_edges_from([
  (1, 2, {'weight': 0.3, 'like':5}),
  (2, 0, {'weight': 0.1, 'like':8})
])
```

```python
G.edges[(0, 1)]
```

    {'weight': 0.5, 'like': 3}

## 可视化

```python
nx.draw(G, with_labels = True)
```

## 全图连接信息

```python
G.number_of_edges()
```

    3

```python
G.size()
```

    3

```python
G.edges()
```

    EdgeView([(0, 1), (0, 2), (1, 2)])

```python
G.edges(data=True)
```

    EdgeDataView([(0, 1, {'weight': 0.5, 'like': 3}), (0, 2, {'weight': 0.1, 'like': 8}), (1, 2, {'weight': 0.3, 'like': 5})])

```python
# 遍历所有连接，data=True 表示输出连接特征属性信息

for edge in G.edges(data=True):
    print(edge)
```

    (0, 1, {'weight': 0.5, 'like': 3})
    (0, 2, {'weight': 0.1, 'like': 8})
    (1, 2, {'weight': 0.3, 'like': 5})

## 节点的连接数（Node Degree）

```python
# 指定节点
node_id = 1
```

```python
G.degree[node_id]
```

    2

```python
# 指定节点的所有相邻节点
for neighbor in G.neighbors(node_id):
    print("Node {} has neighbor {}".format(node_id, neighbor))
```

    Node 1 has neighbor 0
    Node 1 has neighbor 2

# 后言

如果实在找不到机器跑，用现成的也行，如

<https://featurize.cn?s=ddfded217b7546b8b6f985a96fc1f7d7>

各种认证加起来应该能免费用好一会了(还是不推荐远程付费这种，最好本地跑)

