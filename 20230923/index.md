# DeepWalk实战-维基百科词条图嵌入可视化


## 参考资料

<https://www.analyticsvidhya.com/blog/2019/11/graph-feature-extraction-deepwalk/>

<https://github.com/prateekjoshi565/DeepWalk>

## 安装工具包

```python
!pip install networkx gensim pandas numpy tqdm scikit-learn matplotlib
```

    Collecting networkx
      Downloading networkx-3.1-py3-none-any.whl (2.1 MB)
    [2K     [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m2.1/2.1 MB[0m [31m50.1 MB/s[0m eta [36m0:00:00[0ma [36m0:00:01[0m
    [?25hCollecting gensim
      Obtaining dependency information for gensim from https://files.pythonhosted.org/packages/22/40/7d2cce3ad4ad5d02aa68e253e6ea5f0acc381f02f594e235fe00a274faff/gensim-4.3.2-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata
      Downloading gensim-4.3.2-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (8.3 kB)
    Collecting pandas
      Obtaining dependency information for pandas from https://files.pythonhosted.org/packages/de/ce/b5d9c7ce1aaf9023b823c81932a50cd5e8f407198a696b0d1c6025a40b03/pandas-2.1.1-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata
      Downloading pandas-2.1.1-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (18 kB)
    Collecting numpy
      Obtaining dependency information for numpy from https://files.pythonhosted.org/packages/c4/36/161e2f8110f8c49e59f6107bd6da4257d30aff9f06373d0471811f73dcc5/numpy-1.26.0-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata
      Downloading numpy-1.26.0-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (58 kB)
    [2K     [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m58.5/58.5 kB[0m [31m4.8 MB/s[0m eta [36m0:00:00[0m
    [?25hCollecting tqdm
      Obtaining dependency information for tqdm from https://files.pythonhosted.org/packages/00/e5/f12a80907d0884e6dff9c16d0c0114d81b8cd07dc3ae54c5e962cc83037e/tqdm-4.66.1-py3-none-any.whl.metadata
      Downloading tqdm-4.66.1-py3-none-any.whl.metadata (57 kB)
    [2K     [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m57.6/57.6 kB[0m [31m19.5 MB/s[0m eta [36m0:00:00[0m
    [?25hCollecting scikit-learn
      Obtaining dependency information for scikit-learn from https://files.pythonhosted.org/packages/8f/87/5969092159207f583481ad80a03f09e2d4af1ebd197f4530ca4e906c947e/scikit_learn-1.3.1-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata
      Downloading scikit_learn-1.3.1-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (11 kB)
    Collecting matplotlib
      Obtaining dependency information for matplotlib from https://files.pythonhosted.org/packages/65/5b/3b8fd7d66043f0638a35fa650570cbe69efd42fe169e5024f9307598b47e/matplotlib-3.8.0-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata
      Downloading matplotlib-3.8.0-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (5.8 kB)
    Collecting scipy>=1.7.0 (from gensim)
      Obtaining dependency information for scipy>=1.7.0 from https://files.pythonhosted.org/packages/ef/1b/7538792254aec6850657d5b940fd05fe60582af829ffe40d6c054f065f34/scipy-1.11.3-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata
      Downloading scipy-1.11.3-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (60 kB)
    [2K     [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m60.4/60.4 kB[0m [31m23.3 MB/s[0m eta [36m0:00:00[0m
    [?25hCollecting smart-open>=1.8.1 (from gensim)
      Obtaining dependency information for smart-open>=1.8.1 from https://files.pythonhosted.org/packages/fc/d9/d97f1db64b09278aba64e8c81b5d322d436132df5741c518f3823824fae0/smart_open-6.4.0-py3-none-any.whl.metadata
      Downloading smart_open-6.4.0-py3-none-any.whl.metadata (21 kB)
    Requirement already satisfied: python-dateutil>=2.8.2 in /root/miniconda3/envs/jupyter-env/lib/python3.11/site-packages (from pandas) (2.8.2)
    Requirement already satisfied: pytz>=2020.1 in /root/miniconda3/envs/jupyter-env/lib/python3.11/site-packages (from pandas) (2023.3.post1)
    Collecting tzdata>=2022.1 (from pandas)
      Downloading tzdata-2023.3-py2.py3-none-any.whl (341 kB)
    [2K     [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m341.8/341.8 kB[0m [31m66.1 MB/s[0m eta [36m0:00:00[0m
    [?25hCollecting joblib>=1.1.1 (from scikit-learn)
      Obtaining dependency information for joblib>=1.1.1 from https://files.pythonhosted.org/packages/10/40/d551139c85db202f1f384ba8bcf96aca2f329440a844f924c8a0040b6d02/joblib-1.3.2-py3-none-any.whl.metadata
      Downloading joblib-1.3.2-py3-none-any.whl.metadata (5.4 kB)
    Collecting threadpoolctl>=2.0.0 (from scikit-learn)
      Obtaining dependency information for threadpoolctl>=2.0.0 from https://files.pythonhosted.org/packages/81/12/fd4dea011af9d69e1cad05c75f3f7202cdcbeac9b712eea58ca779a72865/threadpoolctl-3.2.0-py3-none-any.whl.metadata
      Downloading threadpoolctl-3.2.0-py3-none-any.whl.metadata (10.0 kB)
    Collecting contourpy>=1.0.1 (from matplotlib)
      Obtaining dependency information for contourpy>=1.0.1 from https://files.pythonhosted.org/packages/b7/f6/78f60fa0b6ae64971178e2542e8b3ad3ba5f4f379b918ab7b18038a3f897/contourpy-1.1.1-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata
      Downloading contourpy-1.1.1-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (5.9 kB)
    Collecting cycler>=0.10 (from matplotlib)
      Obtaining dependency information for cycler>=0.10 from https://files.pythonhosted.org/packages/e7/05/c19819d5e3d95294a6f5947fb9b9629efb316b96de511b418c53d245aae6/cycler-0.12.1-py3-none-any.whl.metadata
      Downloading cycler-0.12.1-py3-none-any.whl.metadata (3.8 kB)
    Collecting fonttools>=4.22.0 (from matplotlib)
      Obtaining dependency information for fonttools>=4.22.0 from https://files.pythonhosted.org/packages/72/2c/7634a6c16b29d0c31cf54051beefab796abdfe8f52abead6d09e5403696e/fonttools-4.43.1-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata
      Downloading fonttools-4.43.1-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (152 kB)
    [2K     [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m152.4/152.4 kB[0m [31m68.2 MB/s[0m eta [36m0:00:00[0m
    [?25hCollecting kiwisolver>=1.0.1 (from matplotlib)
      Obtaining dependency information for kiwisolver>=1.0.1 from https://files.pythonhosted.org/packages/17/ba/17a706b232308e65f57deeccae503c268292e6a091313f6ce833a23093ea/kiwisolver-1.4.5-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata
      Downloading kiwisolver-1.4.5-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (6.4 kB)
    Requirement already satisfied: packaging>=20.0 in /root/miniconda3/envs/jupyter-env/lib/python3.11/site-packages (from matplotlib) (23.1)
    Collecting pillow>=6.2.0 (from matplotlib)
      Obtaining dependency information for pillow>=6.2.0 from https://files.pythonhosted.org/packages/3c/49/f87cecbdec4b00cc1187f01196d48c08828204cd861915fab44972dc705c/Pillow-10.0.1-cp311-cp311-manylinux_2_28_x86_64.whl.metadata
      Downloading Pillow-10.0.1-cp311-cp311-manylinux_2_28_x86_64.whl.metadata (9.5 kB)
    Collecting pyparsing>=2.3.1 (from matplotlib)
      Obtaining dependency information for pyparsing>=2.3.1 from https://files.pythonhosted.org/packages/39/92/8486ede85fcc088f1b3dba4ce92dd29d126fd96b0008ea213167940a2475/pyparsing-3.1.1-py3-none-any.whl.metadata
      Downloading pyparsing-3.1.1-py3-none-any.whl.metadata (5.1 kB)
    Requirement already satisfied: six>=1.5 in /root/miniconda3/envs/jupyter-env/lib/python3.11/site-packages (from python-dateutil>=2.8.2->pandas) (1.16.0)
    Downloading gensim-4.3.2-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (26.7 MB)
    [2K   [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m26.7/26.7 MB[0m [31m11.2 MB/s[0m eta [36m0:00:00[0m00:01[0m00:01[0m
    [?25hDownloading pandas-2.1.1-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (12.2 MB)
    [2K   [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m12.2/12.2 MB[0m [31m227.0 MB/s[0m eta [36m0:00:00[0m00:01[0m
    [?25hDownloading numpy-1.26.0-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (18.2 MB)
    [2K   [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m18.2/18.2 MB[0m [31m215.8 MB/s[0m eta [36m0:00:00[0ma [36m0:00:01[0m
    [?25hDownloading tqdm-4.66.1-py3-none-any.whl (78 kB)
    [2K   [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m78.3/78.3 kB[0m [31m40.2 MB/s[0m eta [36m0:00:00[0m
    [?25hDownloading scikit_learn-1.3.1-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (10.9 MB)
    [2K   [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m10.9/10.9 MB[0m [31m258.3 MB/s[0m eta [36m0:00:00[0m00:01[0m
    [?25hDownloading matplotlib-3.8.0-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (11.6 MB)
    [2K   [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m11.6/11.6 MB[0m [31m261.5 MB/s[0m eta [36m0:00:00[0m00:01[0m
    [?25hDownloading contourpy-1.1.1-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (302 kB)
    [2K   [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m302.8/302.8 kB[0m [31m121.8 MB/s[0m eta [36m0:00:00[0m
    [?25hDownloading cycler-0.12.1-py3-none-any.whl (8.3 kB)
    Downloading fonttools-4.43.1-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (4.8 MB)
    [2K   [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m4.8/4.8 MB[0m [31m268.2 MB/s[0m eta [36m0:00:00[0m
    [?25hDownloading joblib-1.3.2-py3-none-any.whl (302 kB)
    [2K   [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m302.2/302.2 kB[0m [31m124.7 MB/s[0m eta [36m0:00:00[0m
    [?25hDownloading kiwisolver-1.4.5-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (1.4 MB)
    [2K   [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m1.4/1.4 MB[0m [31m240.8 MB/s[0m eta [36m0:00:00[0m
    [?25hDownloading Pillow-10.0.1-cp311-cp311-manylinux_2_28_x86_64.whl (3.6 MB)
    [2K   [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m3.6/3.6 MB[0m [31m271.2 MB/s[0m eta [36m0:00:00[0m
    [?25hDownloading pyparsing-3.1.1-py3-none-any.whl (103 kB)
    [2K   [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m103.1/103.1 kB[0m [31m51.8 MB/s[0m eta [36m0:00:00[0m
    [?25hDownloading scipy-1.11.3-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (36.4 MB)
    [2K   [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m36.4/36.4 MB[0m [31m64.6 MB/s[0m eta [36m0:00:00[0m:00:01[0m00:01[0m
    [?25hDownloading smart_open-6.4.0-py3-none-any.whl (57 kB)
    [2K   [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m57.0/57.0 kB[0m [31m17.1 MB/s[0m eta [36m0:00:00[0m
    [?25hDownloading threadpoolctl-3.2.0-py3-none-any.whl (15 kB)
    Installing collected packages: tzdata, tqdm, threadpoolctl, smart-open, pyparsing, pillow, numpy, networkx, kiwisolver, joblib, fonttools, cycler, scipy, pandas, contourpy, scikit-learn, matplotlib, gensim
    Successfully installed contourpy-1.1.1 cycler-0.12.1 fonttools-4.43.1 gensim-4.3.2 joblib-1.3.2 kiwisolver-1.4.5 matplotlib-3.8.0 networkx-3.1 numpy-1.26.0 pandas-2.1.1 pillow-10.0.1 pyparsing-3.1.1 scikit-learn-1.3.1 scipy-1.11.3 smart-open-6.4.0 threadpoolctl-3.2.0 tqdm-4.66.1 tzdata-2023.3
    [33mWARNING: Running pip as the 'root' user can result in broken permissions and conflicting behaviour with the system package manager. It is recommended to use a virtual environment instead: https://pip.pypa.io/warnings/venv[0m[33m
    [0m

## 导入工具包

```python
import networkx as nx # 图数据挖掘

# 数据分析
import pandas as pd
import numpy as np

import random # 随机数
from tqdm import tqdm # 进度条

# 数据可视化
import matplotlib.pyplot as plt
%matplotlib inline

plt.rcParams['font.sans-serif']=['SimHei']  # 用来正常显示中文标签  
plt.rcParams['axes.unicode_minus']=False  # 用来正常显示负号
```

## 获取维基百科网页引用关联数据

1.打开网站`https://densitydesign.github.io/strumentalia-seealsology`

2.Distance设置为`4`

3.输入以下链接

<https://en.wikipedia.org/wiki/Computer_vision>

<https://en.wikipedia.org/wiki/Deep_learning>

<https://en.wikipedia.org/wiki/Convolutional_neural_network>

<https://en.wikipedia.org/wiki/Decision_tree>

<https://en.wikipedia.org/wiki/Support-vector_machine>

4.点击`START CRAWLING`，爬取1000个网页之后，点击`STOP & CLEAR QUEUE`

5.Download-下载TSV文件，保存至代码相同目录，命名为`seealsology-data.tsv`

```python
df = pd.read_csv("seealsology-data.tsv", sep = "\t")
```

```python
df.head()
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
      <th>source</th>
      <th>target</th>
      <th>depth</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>support-vector machine</td>
      <td>in situ adaptive tabulation</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>support-vector machine</td>
      <td>kernel machines</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>support-vector machine</td>
      <td>fisher kernel</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>support-vector machine</td>
      <td>platt scaling</td>
      <td>1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>support-vector machine</td>
      <td>polynomial kernel</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>

```python
df.shape
```

    (13211, 3)

## 构建无向图

```python
G = nx.from_pandas_edgelist(df, "source", "target", edge_attr=True, create_using=nx.Graph())
```

```python
# 节点个数
len(G)
```

    8560

## 因节点数过多，省去可视化步骤

```python
# # 可视化
# plt.figure(figsize=(15,14))
# nx.draw(G)
# plt.show()
```

## 生成随机游走节点序列的函数

```python
def get_randomwalk(node, path_length):
    '''
    输入起始节点和路径长度，生成随机游走节点序列
    '''
    
    random_walk = [node]
    
    for i in range(path_length-1):
        # 汇总邻接节点
        temp = list(G.neighbors(node))
        temp = list(set(temp) - set(random_walk))    
        if len(temp) == 0:
            break
        # 从邻接节点中随机选择下一个节点
        random_node = random.choice(temp)
        random_walk.append(random_node)
        node = random_node
        
    return random_walk
```

```python
all_nodes = list(G.nodes())
```

```python
all_nodes
```

    ['support-vector machine',
     'in situ adaptive tabulation',
     'kernel machines',
     'fisher kernel',
     'platt scaling',
     'polynomial kernel',
     'predictive analytics',
     'regularization perspectives on support-vector machines',
     'relevance vector machine',
     'sequential minimal optimization',
     'space mapping',
     'winnow (algorithm)',
     'decision tree',
     'behavior tree (artificial intelligence, robotics and control)',
     'boosting (machine learning)',
     'decision cycle',
     'decision list',
     'decision matrix',
     'decision table',
     'decision tree model',
     'design rationale',
     'drakon',
     'markov chain',
     'random forest',
     'odds algorithm',
     'topological combinatorics',
     'truth table',
     'convolutional neural network',
     'attention (machine learning)',
     'convolution',
     'deep learning',
     'natural-language processing',
     'neocognitron',
     'scale-invariant feature transform',
     'time delay neural network',
     'vision processing unit',
     'applications of artificial intelligence',
     'comparison of deep learning software',
     'compressed sensing',
     'differentiable programming',
     'echo state network',
     'liquid state machine',
     'reservoir computing',
     'scale space',
     'sparse coding',
     'computer vision',
     'computational imaging',
     'computational photography',
     'computer audition',
     'egocentric vision',
     'machine vision glossary',
     'teknomo–fernandez algorithm',
     'vision science',
     'visual agnosia',
     'visual perception',
     'visual system',
     'achromatopsia',
     'akinetopsia',
     'apperceptive agnosia',
     'associative visual agnosia',
     'asthenopia',
     'astigmatism (eye)',
     'color blindness',
     'human echolocation',
     'helmholtz–kohlrausch effect',
     'color balance',
     'magnocellular cell',
     'memory-prediction framework',
     'prosopagnosia',
     'scotopic sensitivity syndrome',
     'recovery from blindness',
     'visual modularity',
     'visual processing',
     'color vision',
     'depth perception',
     'entoptic phenomenon',
     'gestalt psychology',
     'lateral masking',
     'looming',
     'naked eye',
     'machine vision',
     'motion perception',
     'multisensory integration',
     'interpretation (philosophy)',
     'spatial frequency',
     'visual illusion',
     'wikt:sensation',
     'hallucinogen persisting perception disorder',
     'illusory palinopsia',
     'refractive error',
     'visual snow',
     'cognitive science',
     'neuroscience',
     'ophthalmology',
     'optometry',
     'psychophysics',
     'agnosia',
     'blindness',
     'color agnosia',
     'gestaltzerfall',
     'riddoch syndrome',
     'topographical disorientation',
     'brain',
     'color constancy',
     'eye',
     'linguistic relativity and the color naming debate',
     'neuropsychology',
     'optical illusion',
     'primary colors',
     'visual cortex',
     'visual neuroscience',
     'adaptive control',
     'cognitive model',
     'computational electromagnetics',
     'computer-aided design',
     'engineering optimization',
     'finite element method',
     'kriging',
     'linear approximation',
     'machine learning',
     'mental model',
     'mental rotation',
     'mirror neuron',
     'model-dependent realism',
     'multiphysics',
     'performance tuning',
     'response surface methodology',
     'semiconductor device modeling',
     'spatial cognition',
     'spatial memory',
     'support vector machine',
     'theory of mind',
     'attribution bias',
     'cephalopod intelligence',
     'cetacean intelligence',
     'eliminative materialism',
     'empathy',
     'grounding in communication',
     'intentional stance',
     'joint attention',
     'mental body',
     'mentalization',
     'mini-sea',
     'origin of language',
     'perspective-taking',
     'quantum mind',
     'relational frame theory',
     'self-awareness',
     'social neuroscience',
     'embodied cognition',
     'the mind of an ape',
     'turing test',
     'type physicalism',
     'interpersonal accuracy',
     'cognitive map',
     'dissociation (neuropsychology)',
     'method of loci',
     'spatial ability',
     'visual memory',
     'mind mapping',
     'spatial contextual awareness',
     'sense of direction',
     'cognition',
     'diode modelling',
     'transistor models',
     'compact model coalition',
     'technology cad',
     'electronic circuit simulation',
     'bipolar junction transistor',
     'safe operating area',
     'electronic design automation',
     'cognitive biology',
     'cognitive musicology',
     'cognitive computing',
     'cognitive psychology',
     'cognitivism (psychology)',
     'comparative cognition',
     'information processing technology and aging',
     'mental chronometry',
     'nootropic',
     'cognitive abilities screening instrument',
     'affective science',
     'cognitive anthropology',
     'cognitive ethology',
     'cognitive linguistics',
     'cognitive neuropsychology',
     'cognitive neuroscience',
     'cognitive science of religion',
     'computational neuroscience',
     'computational-representational understanding of mind',
     'concept mining',
     'decision field theory',
     'decision theory',
     'dynamicism',
     'educational neuroscience',
     'educational psychology',
     'embodied cognitive science',
     'enactivism',
     'epistemology',
     'folk psychology',
     'heterophenomenology',
     'human cognome project',
     'human–computer interaction',
     'indiana archives of cognitive science',
     'informatics (academic field)',
     'malleable intelligence',
     'neural darwinism',
     'personal information management',
     'qualia',
     'quantum cognition',
     'simulated consciousness',
     'situated cognition',
     'society of mind theory',
     'speech–language pathology',
     'personal relative direction',
     'spatial disorientation',
     'augmented reality',
     'context awareness',
     'context-aware pervasive systems',
     'fiduciary marker',
     'human-computer interaction',
     'location awareness',
     'location-based service',
     'situational awareness',
     'topology',
     'ubiquitous computing',
     'mechanical aptitude',
     'motor imagery',
     "raven's progressive matrices",
     'exquisite corpse',
     'graph (discrete mathematics)',
     'idea',
     'mental literacy',
     'nodal organizational structure',
     'personal wiki',
     'rhizome (philosophy)',
     'social map',
     'spider mapping',
     'cognitive geography',
     'fuzzy cognitive map',
     'repertory grid',
     'mind map',
     'kernel perceptron',
     'kernel trick',
     'actuarial science',
     'artificial intelligence in healthcare',
     'analytical procedures (finance auditing)',
     'computational sociology',
     'criminal reduction utilising statistical history',
     'disease surveillance',
     'learning analytics',
     'pattern recognition',
     'predictive inference',
     'predictive policing',
     'social media analytics',
     'fisher information metric',
     'kernel methods for vector output',
     'kernel density estimation',
     'representer theorem',
     'similarity learning',
     "cover's theorem",
     'radial basis function network',
     'recurrent neural networks',
     'tensor product network',
     'consciousness',
     'biological naturalism',
     'emergent materialism',
     'materialism',
     'mind–body problem',
     'monism',
     'philosophy of mind',
     'physicalism',
     'presentism (philosophy of time)',
     'natural language processing',
     'artificial intelligence in fiction',
     'blindsight',
     'causality',
     'computer game bot turing test',
     'explanation',
     'explanatory gap',
     'functionalism (philosophy of mind)',
     'graphics turing test',
     'ex machina (film)',
     'hard problem of consciousness',
     'mark v. shaney',
     'mind-body problem',
     'philosophical zombie',
     'problem of other minds',
     'reverse engineering',
     'sentience',
     'simulated reality',
     'social bot',
     'technological singularity',
     'uncanny valley',
     'voight-kampff machine',
     'winograd schema challenge',
     'shrdlu',
     'primate cognition',
     'thomas nagel',
     'animal cognition',
     'alex (parrot)',
     'action-specific perception',
     'active inference',
     'blue brain project',
     'conceptual blending',
     'conceptual metaphor',
     'ecological psychology',
     'embodied bilingual language',
     'embodied embedded cognition',
     'embodied music cognition',
     'embodied phenomenology',
     'extended cognition',
     'extended mind thesis',
     'externalism',
     'feeling',
     'image schema',
     'metaphors we live by',
     "moravec's paradox",
     'motor cognition',
     'neuroconstructivism',
     'neurophenomenology',
     'plant cognition',
     'where mathematics comes from',
     'women, fire, and dangerous things',
     'biocultural evolution',
     'emotion',
     'social cognition',
     'social cognitive neuroscience',
     'social cognitive and affective neuroscience',
     'social psychology',
     'sociobiology',
     'neural synchrony',
     'animal consciousness',
     'bicameral mentality',
     'boltzmann brain',
     'cartesian theater',
     'childhood amnesia',
     'confidence',
     'deindividuation',
     'dunning–kruger effect',
     'feldenkrais method',
     'higher consciousness',
     'human self-reflection',
     'intentionality',
     'julian jaynes',
     'lucid dream',
     'memory inhibition',
     'mental noting',
     'mindfulness',
     'mirror test',
     'modesty',
     'the origin of consciousness in the breakdown of the bicameral mind',
     'psychological mindedness',
     'robert langs',
     'autonomic computing',
     'self-consciousness',
     'self-knowledge (psychology)',
     'situation awareness',
     'vedanta',
     'vipassanā',
     'yoga nidra',
     'cognitive grammar',
     'construction grammar',
     'constructivism (psychological school)',
     'operant conditioning',
     'personal construct theory',
     'schema (psychology)',
     'artificial consciousness',
     'bohm interpretation',
     'coincidence detection in neurobiology',
     'electromagnetic theories of consciousness',
     'evolutionary neuroscience',
     'many-minds interpretation',
     'orch-or',
     'holonomic brain theory',
     'mechanism (philosophy)',
     'quantum neural network',
     'role reversal',
     'role-taking theory',
     'abiogenesis',
     'biolinguistics',
     'bouba/kiki effect',
     'bow-wow theory',
     'digital infinity',
     'essay on the origin of languages',
     'evolutionary psychology of language',
     'foxp2 and human evolution',
     'generative anthropology',
     'historical linguistics',
     "man's first word",
     'neurobiological origins of language',
     'origins of society',
     'origin of speech',
     'proto-language',
     'theory of language',
     'ambit (adolescent mentalization-based integrative treatment)',
     'mentalization based treatment',
     'metacognition',
     'psychic equivalence',
     'esoteric',
     'esoteric cosmology',
     'etheric body',
     'spiritual evolution',
     'subtle body',
     'thoughtform',
     'science',
     'mind',
     'asperger syndrome',
     'cooperative eye hypothesis',
     'vocabulary development',
     'four causes',
     'telos',
     'teleology',
     'high- and low-level',
     'instrumentalism',
     'intention',
     'level of analysis',
     'life stance',
     'david marr (psychologist)',
     'naturalization of intentionality',
     'operationalism',
     'philosophical realism',
     'stance (linguistics)',
     "the philosophy of 'as if'",
     'communication',
     'common knowledge (logic)',
     'media richness theory',
     'against empathy',
     'artificial empathy',
     'attribution (psychology)',
     'digital empathy',
     'emotional intelligence',
     'emotional literacy',
     'empathic concern',
     'empathizing–systemizing theory',
     'empathy in chickens',
     'empathy in literature',
     'empathy in online communities',
     'empathism',
     'ethnocultural empathy',
     'highly sensitive person',
     'humanistic coefficient',
     'identification (psychology)',
     'life skills',
     'mimpathy',
     'moral emotions',
     'oxytocin',
     'people skills',
     'philip k. dick',
     'do androids dream of electric sheep?',
     'rapport',
     'self-conscious emotions',
     'sensibility',
     'simulation theory of empathy',
     'social emotions',
     'soft skills',
     'theory of mind in animals',
     'vicarious embarrassment',
     'attention schema theory',
     'constructivist epistemology',
     'cotard delusion',
     'deconstructivism',
     'epiphenomenalism',
     'nihilism',
     'phenomenology (philosophy)',
     'principle of locality',
     'property dualism',
     'reductionism',
     'scientism',
     'substance dualism',
     "morgan's canon",
     'john c. lilly',
     'louis herman',
     'animal language',
     'vocal learning',
     'spindle neuron',
     'military dolphin',
     'u.s. navy marine mammal program',
     'so long, and thanks for all the fish',
     'uplift universe',
     'box–behnken design',
     'central composite design',
     'gradient-enhanced kriging',
     'ioso',
     'optimal design',
     'plackett–burman design',
     'polynomial and rational function modeling',
     'polynomial regression',
     'probabilistic design',
     'surrogate model',
     'performance application programming interface',
     'profiling (computer programming)',
     'radial basis function',
     'optiy',
     'surrogate endpoint',
     'surrogate data',
     'fitness approximation',
     'computer experiment',
     'conceptual model',
     'bayesian regression',
     'bayesian model selection',
     'interval finite element',
     'curve fitting',
     'line regression',
     'local polynomial regression',
     'polynomial interpolation',
     'smoothing spline',
     'padé approximant',
     'bayesian experimental design',
     'blocking (statistics)',
     'convex function',
     'convex optimization',
     'design of experiments',
     'efficiency (statistics)',
     'entropy (information theory)',
     'fisher information',
     'glossary of experimental design',
     "hadamard's maximal determinant problem",
     'information theory',
     'jack kiefer (mathematician)',
     'replication (statistics)',
     'statistical model',
     'abraham wald',
     'jacob wolfowitz',
     'finite difference time-domain method',
     'all models are wrong',
     'commensurability (philosophy of science)',
     'conceptualist realism',
     'fallibilism',
     'internal realism',
     'models of scientific inquiry',
     'ontological pluralism',
     'pragmatism',
     'scientific realism',
     'associative sequence learning',
     'common coding theory',
     'emotional contagion',
     'mirror-touch synesthesia',
     'mirroring (psychology)',
     'motor theory of speech perception',
     'on intelligence',
     'positron emission tomography',
     'speech repetition',
     'mental event',
     'mental function',
     'mental operations',
     'functional neuroimaging',
     'noumenon',
     'psychedelic experience',
     'synchronicity',
     'alan baddeley',
     'auditory processing disorder',
     "baddeley's model of working memory",
     'conduction aphasia',
     'developmental verbal dyspraxia',
     'echoic memory',
     'echolalia',
     'language development',
     'language acquisition',
     'language-based learning disability',
     'mirror neurons',
     'passive speakers (language)',
     'phonological development',
     'pronunciation',
     'second-language acquisition',
     'short-term memory',
     'speech perception',
     'thematic coherence',
     'transcortical motor aphasia',
     'transcortical sensory aphasia',
     'vocabulary',
     'diffuse optical imaging',
     'hot cell',
     'molecular imaging',
     'hierarchical temporal memory',
     'cohort model',
     'speech recognition',
     'auditory phonetics',
     'trace (psycholinguistics)',
     'haskins laboratories',
     'procedural memory',
     'kinesthetic learning',
     'nonverbal communication',
     'affective neuroscience',
     'autism',
     'behavioral contagion',
     'emotional detachment',
     'emotional competence',
     'folie à deux',
     'groupthink',
     'limbic resonance',
     'limbic regulation',
     'microexpression',
     'projective identification',
     'viral phenomenon',
     'lawrence w. barsalou',
     'mental practice of action',
     'predictive coding',
     'roger sperry',
     'vittorio guidano',
     'william james',
     'anti-realism',
     'critical realism (philosophy of perception)',
     'dialectical materialism',
     "musgrave's scientific realism",
     'naïve realism',
     'pessimistic induction',
     'scientific materialism',
     'scientific perspectivism',
     'social constructionism',
     'american philosophy',
     'charles sanders peirce bibliography',
     'doctrine of internal relations',
     'holistic pragmatism',
     'new legal realism',
     'communication theory as a field',
     'scientific method',
     'realpolitik',
     'dialectical realism',
     'epistemological realism',
     'extended modal realism',
     'legal realism',
     'modal realism',
     'objectivism',
     'philosophy of social science',
     'principle of bivalence',
     'problem of future contingents',
     'truth-value link realism',
     'speculative realism',
     'direct and indirect realism',
     'anekantavada',
     'legal pluralism',
     'nelson goodman',
     'panarchy (political philosophy)',
     'pantheism',
     'pluralism (political philosophy)',
     'pluralism (political theory)',
     'postmodernism',
     'quantifier variance',
     'religious pluralism',
     'value pluralism',
     'deductive-nomological',
     'explanandum',
     'hypothetico-deductive method',
     'inquiry',
     'instrumental and value rationality',
     'instrumental and intrinsic value',
     'natural kind',
     'fact–value distinction',
     'inductive reasoning',
     'john dewey',
     'richard rorty',
     'is logic empirical?',
     'logical holism',
     'pancritical rationalism',
     'perspectivism',
     'probabilism',
     'defeasible reasoning',
     'autopoiesis',
     'consensus reality',
     'constructivism in international relations',
     'cultural pluralism',
     'epistemological pluralism',
     'tinkerbell effect',
     'map–territory relation',
     'meaning making',
     'personal construct psychology',
     'post-rationalist cognitive therapy',
     'eigenmode expansion',
     'beam propagation method',
     'finite-difference frequency-domain',
     'scattering-matrix method',
     'discrete dipole approximation',
     'internal model (motor control)',
     'knowledge representation',
     'lovemap',
     'macrocognition',
     'neuro-linguistic programming',
     'neuroeconomics',
     'neuroplasticity',
     'ooda loop',
     'psyche (psychology)',
     'self-stereotyping',
     'social intuitionism',
     'system dynamics',
     'text and conversation theory',
     'binomial approximation',
     "euler's method",
     'finite differences',
     'finite difference methods',
     "newton's method",
     'power series',
     'taylor series',
     'bayes linear statistics',
     'gaussian process',
     'multivariate interpolation',
     'nonparametric regression',
     'radial basis function interpolation',
     'spatial dependence',
     'variogram',
     'information field theory',
     'bayesian inference',
     'bayesian hierarchical modeling',
     'statistical inference',
     'buffer analysis',
     'cartography',
     'complete spatial randomness',
     'cost distance analysis',
     'geocomputation',
     'geospatial intelligence',
     'geospatial predictive modeling',
     'de-9im',
     'geographic information science',
     'mathematical statistics',
     'modifiable areal unit problem',
     'point process',
     'proximity analysis',
     'spatial autocorrelation',
     'spatial descriptive statistics',
     'spatial relation',
     'terrain analysis',
     'boundary problem (in spatial analysis)',
     'extrapolation domain analysis',
     'fuzzy architectural spatial analysis',
     'geodemographic segmentation',
     'geographic information systems',
     'geoinformatics',
     'geostatistics',
     'permeability (spatial and transport planning)',
     'spatial econometrics',
     'spatial epidemiology',
     'suitability analysis',
     'viewshed analysis',
     'lasso (statistics)',
     'local regression',
     'non-parametric statistics',
     'semiparametric regression',
     'isotonic regression',
     'multivariate adaptive regression splines',
     'surface fitting',
     'smoothing',
     'bayesian interpretation of regularization',
     'gaussian free field',
     'gauss–markov process',
     "student's t-distribution",
     'imprecise probability',
     'asymptotic expansion',
     'generating function',
     'madhava series',
     'newton polynomial',
     'puiseux series',
     'shift operator',
     "aitken's delta-squared process",
     'bisection method',
     'euler method',
     'fast inverse square root',
     'scoring algorithm',
     'gradient descent',
     'integer square root',
     'kantorovich theorem',
     "laguerre's method",
     'methods of computing square roots',
     "newton's method in optimization",
     'richardson extrapolation',
     'root-finding algorithm',
     'secant method',
     "steffensen's method",
     'subgradient method',
     'finite difference',
     'finite difference time domain',
     'infinite difference method',
     'stencil (numerical analysis)',
     'finite difference coefficients',
     'five-point stencil',
     'lax–richtmyer theorem',
     'finite difference methods for option pricing',
     'upwind differencing scheme for convection',
     'central differencing scheme',
     'discrete poisson equation',
     'discrete laplace operator',
     'discrete calculus',
     'divided differences',
     'finite-difference time-domain method',
     'finite volume method',
     'ftcs scheme',
     "gilbreath's conjecture",
     'sheffer sequence',
     'summation by parts',
     'time scale calculus',
     'crank–nicolson method',
     'linear multistep method',
     'numerical integration',
     'numerical methods for ordinary differential equations',
     'tacit knowledge',
     'explicit knowledge',
     'conversation theory',
     'organizational structure',
     'organizational culture',
     'organizational communication',
     'sensemaking',
     'structuration theory',
     'causal loop diagram',
     'comparison of system dynamics software',
     'ecosystem model',
     'plateau principle',
     'system archetype',
     'system dynamics society',
     'twelve leverage points',
     'wicked problem',
     'world3',
     'population dynamics',
     'predator-prey interaction',
     'dynamical systems theory',
     'grey box model',
     'operations research',
     'social dynamics',
     'system identification',
     'systems theory',
     'systems thinking',
     'triz',
     'jay forrester',
     'dennis meadows',
     'donella meadows',
     'peter senge',
     'graeme snooks',
     'john sterman',
     'contagion heuristic',
     'dual process theory (moral psychology)',
     'ethical intuitionism',
     'moral foundations theory',
     'moral sense theory',
     'social identity theory',
     'social identity approach',
     'ego death',
     'human spirit',
     'inscape (visual art)',
     'motivation',
     'nafs',
     'persona',
     'persona (psychology)',
     'reincarnation',
     'psychosis',
     'control theory',
     'double-loop learning',
     'dmaic',
     'pdca',
     'learning cycle',
     'maneuver warfare',
     'nursing process',
     'problem solving',
     'swot analysis',
     'united states army strategist',
     'activity-dependent plasticity',
     'brain training',
     'environmental enrichment (neural)',
     'neural backpropagation',
     'neuroplastic effects of pollution',
     'psychedelic drug',
     'kinesiology',
     'journal of neuroscience, psychology, and economics',
     'emotional freedom technique',
     'milton h. erickson',
     'family systems therapy',
     'frank farrelly',
     'avatar course',
     'steve andreas',
     'richard bandler',
     'john grinder',
     'paul mckenna',
     'allegory of the cave',
     'blind men and an elephant',
     'emic and etic',
     'fallacy of misplaced concreteness',
     'good regulator',
     'ludic fallacy',
     "mary's room",
     'mind projection fallacy',
     'nominalism',
     'non-aristotelian logic',
     'on the content and object of presentations',
     'philosophy of perception',
     'reification (fallacy)',
     'structural differential',
     'surrogation',
     'unintended consequences',
     'use–mention distinction',
     'when a white horse is not a horse',
     'naturalistic decision-making',
     'karol g. ross',
     'sexual script theory',
     'alphabet of human thought',
     'belief revision',
     'chunking (psychology)',
     'commonsense knowledge (artificial intelligence)',
     'conceptual graph',
     'datr',
     'fo(.)',
     'first-order logic',
     'logico-linguistic modeling',
     'knowledge graph',
     'knowledge management',
     'semantic technology',
     'valuation-based system',
     'repetitive control',
     'common sense',
     'concept',
     'concept mapping',
     'conceptual framework',
     'conceptual model (computer science)',
     'conceptual schema',
     'conceptual system',
     'information model',
     'international conference on conceptual modeling',
     'interpretation (logic)',
     'isolated system',
     'ontology (computer science)',
     'paradigm',
     'physical model',
     'process of concept formation',
     'scientific modeling',
     'theory',
     'cognitive bias',
     'cognitive description',
     'cognitive development',
     'cognitive interventions',
     'cognitive module',
     'cognitive poetics',
     'cognitive robotics',
     'connectionism',
     'discursive psychology',
     'evolutionary psychology',
     'fuzzy-trace theory',
     'genetic epistemology',
     'artificial intelligence',
     'intertrial priming',
     'models of abnormality',
     'neurocognitive',
     'perceptual control theory',
     'psychological adaptation',
     'rubicon model (psychology)',
     'water-level task',
     'applied element method',
     'boundary element method',
     "céa's lemma",
     'direct stiffness method',
     'discontinuity layout optimization',
     'discrete element method',
     'finite difference method',
     'finite element machine',
     'finite element method in structural mechanics',
     'finite volume method for unsteady flow',
     'infinite element method',
     'isogeometric analysis',
     'lattice boltzmann methods',
     'meshfree methods',
     'movable cellular automaton',
     'multidisciplinary design optimization',
     'patch test (finite elements)',
     'rayleigh–ritz method',
     'tessellation (computer graphics)',
     'weakened weak form',
     '3d computer graphics',
     '3d printing',
     'additive manufacturing file format',
     'algorithmic art',
     'cad standards',
     'coarse space (numerical analysis)',
     'comparison of 3d computer graphics software',
     'comparison of cad, cam, and cae file viewers',
     'comparison of computer-aided design software',
     'comparison of eda software',
     'computer-aided industrial design',
     'digital architecture',
     'iso 128',
     'iso 10303',
     'model-based definition',
     'molecular design software',
     'open-source hardware',
     'rapid prototyping',
     'responsive computer-aided design',
     'system integration',
     'virtual prototyping',
     'virtual reality',
     'em simulation software',
     'analytical regularization',
     'computational physics',
     'electromagnetic field solver',
     'electromagnetic wave equation',
     'mie theory',
     'physical optics',
     'rigorous coupled-wave analysis',
     'uniform theory of diffraction',
     'shooting and bouncing rays',
     'computational cognition',
     'computational models of language acquisition',
     'mindmodeling@home',
     'vernon mountcastle',
     'adaptive resonance theory',
     'stephen grossberg',
     'predictive learning',
     ...]

```python
get_randomwalk('random forest', 5)
```

    ['random forest',
     'bootstrap aggregating',
     'cascading classifiers',
     'boosting (meta-algorithm)',
     'margin classifier']

## 生成随机游走序列

```python
gamma = 10 # 每个节点作为起始点生成随机游走序列个数
walk_length = 5 # 随机游走序列最大长度
```

```python
random_walks = []

for n in tqdm(all_nodes): # 遍历每个节点
    for i in range(gamma): # 每个节点作为起始点生成gamma个随机游走序列
        random_walks.append(get_randomwalk(n, walk_length))
```

    100%|██████████| 8560/8560 [00:00<00:00, 29750.07it/s]

```python
# 生成随机游走序列个数
len(random_walks)
```

    85600

```python
random_walks[1]
```

    ['support-vector machine',
     'in situ adaptive tabulation',
     'support vector machine',
     'kernel machines',
     'representer theorem']

## 训练Word2Vec模型

```python
from gensim.models import Word2Vec # 自然语言处理
```

```python
model = Word2Vec(vector_size=256, # Embedding维数
                 window=4, # 窗口宽度
                 sg=1, # Skip-Gram
                 hs=0, # 不加分层softmax
                 negative=10, # 负采样
                 alpha=0.03,  # 初始学习率
                 min_alpha=0.0007, # 最小学习率
                 seed=14 # 随机数种子
                )
```

```python
# 用随机游走序列构建词汇表
model.build_vocab(random_walks, progress_per=2)
```

```python
# 训练（耗时1分钟左右）
model.train(random_walks, total_examples=model.corpus_count, epochs=50, report_delay=1)
```

    (16577429, 16582850)

## 分析Word2Vec结果

```python
# 查看某个节点的Embedding
model.wv.get_vector('random forest').shape
```

    (256,)

```python
model.wv.get_vector('random forest')
```

    array([ 0.23334791, -0.00155042, -0.33077806,  0.17064342, -0.1081422 ,
            0.0355994 ,  0.24897015, -0.04796787, -0.75819224,  0.25839478,
            0.6178689 ,  0.0565461 , -0.06071352, -0.04134927, -0.04617707,
            0.103586  , -0.36969525,  0.15610352, -0.37987182,  0.4583097 ,
           -0.33232433, -0.15699145,  0.21371919,  0.28104368,  0.10022977,
           -0.37963775, -0.03466332,  0.53544474,  0.1582788 , -0.23802406,
            0.31076565, -0.26209658, -0.5055485 , -0.09460515,  0.16454849,
           -0.34730902,  0.23466834, -0.20844714,  0.00270234, -0.3288512 ,
           -0.06863198, -0.20678101, -0.24483734,  0.10853583,  0.6155114 ,
            0.1928504 , -0.7044639 ,  0.6126048 ,  0.17559509, -0.19937247,
            0.4083251 , -0.2327426 , -0.04890826,  0.21921228, -0.11285872,
           -0.13407706, -0.4656657 , -0.2325841 , -0.3509935 ,  0.44738993,
            0.34505916,  0.0984872 , -0.30333465, -0.3040225 , -0.15608855,
           -0.22470486, -0.14090897,  0.32624975,  0.02556773, -0.17766297,
           -0.03282307, -0.06009924, -0.8818592 ,  0.25421733,  0.03796612,
            0.15704152,  1.0321482 , -0.2573714 , -0.40552002, -0.1648857 ,
           -0.22849622, -0.15332152,  0.11458287,  0.0682546 , -0.41684514,
           -0.1707345 , -0.4403224 , -0.28976214, -0.4716022 ,  0.50526184,
           -0.09183959,  0.17879933, -0.2811215 , -0.03059806,  0.25511235,
            0.6181534 ,  0.29505378, -0.22633915, -0.03940143,  0.06210095,
            0.21721607, -0.0526966 , -0.37159044,  0.35864654, -0.17438811,
           -0.22126454, -0.4025731 ,  0.58789897, -0.41082326,  0.17033327,
            0.34257182,  0.6182118 , -0.23921025, -0.21790318,  0.30060363,
           -0.20056939, -0.36237326,  0.36493126,  0.11915668,  0.4602384 ,
           -0.08704186, -0.30845055, -0.23525943, -0.11308478, -0.10704424,
           -0.55574316, -0.20751908,  0.31260073,  0.21971068, -0.07155439,
           -0.07340603, -0.17634007, -0.0146748 , -0.28767866,  0.17059626,
           -0.35025132,  0.18504645, -0.38653925,  0.01171829, -0.9433046 ,
           -0.38610327,  0.32028997, -0.3758984 ,  0.16362464, -0.0357476 ,
           -0.40415367,  0.09392213,  0.13586934, -0.18233965,  0.08914205,
            0.25584528,  0.05338218,  0.3587864 , -0.01985494,  0.17005035,
           -0.03306824, -0.04700813, -0.26085326, -0.78282183,  0.44279492,
            0.17237511, -0.08858976, -0.30706018,  0.09871512, -0.3005835 ,
            0.04916183, -0.05475043, -0.04583912, -0.11915255,  0.25339088,
           -0.37361142, -0.25820956, -0.16901338,  0.06111354, -0.22381096,
           -0.67695826, -0.03763117,  0.28279755, -0.07734507, -0.09612124,
           -0.25081888,  0.08107558,  0.3038904 ,  0.23102888, -0.14562789,
           -0.169836  , -0.18778533, -0.06876044,  0.0312659 ,  0.52690244,
            0.11245615, -0.19194236, -0.09534905, -0.354454  ,  0.44036543,
           -0.443105  ,  0.19732304,  0.31750274,  0.5105512 , -0.03289589,
           -0.1481061 ,  0.38620627, -0.42837468,  0.10600033, -0.09797872,
           -0.01391832, -0.2905861 ,  0.06858752, -0.31003636,  0.52351254,
            0.13377245, -0.70264685, -0.09665483, -0.16441043, -0.04715606,
           -0.1264535 , -0.11374191,  0.422727  , -0.16340947,  0.1502349 ,
           -0.07239548, -0.17635728, -0.06277203,  0.08725803,  1.0166868 ,
            0.10296601,  0.08663206,  0.21920188,  0.04908649,  0.4252364 ,
            0.17294641,  0.58820933,  1.0219544 , -0.37107244, -0.37064922,
            0.3717883 ,  0.47950497, -0.37201375, -0.25528926,  0.45519346,
            0.07936895, -0.18259984,  0.05479478,  0.01196737,  0.02831526,
           -0.17895861,  0.35639855,  0.16127516, -0.59905547,  0.20060547,
           -0.18414594,  0.4863781 ,  0.74616545, -0.21859436, -0.31797457,
            0.10649797], dtype=float32)

```python
# 找相似词语
model.wv.similar_by_word('decision tree')
```

    [('drakon', 0.7287265658378601),
     ('comparison sort', 0.69761723279953),
     ('decision list', 0.6575663685798645),
     ('behavior trees (artificial intelligence, robotics and control)',
      0.6542879343032837),
     ('behavior tree (artificial intelligence, robotics and control)',
      0.653127908706665),
     ('minimum spanning tree', 0.6494240164756775),
     ('decision tree model', 0.6384998559951782),
     ('decision matrix', 0.627086877822876),
     ('decision stump', 0.5921448469161987),
     ('pseudocode', 0.5691863298416138)]

## PCA降维可视化

### 可视化全部词条的二维Embedding

```python
X = model.wv.vectors
```

```python
# 将Embedding用PCA降维到2维
from sklearn.decomposition import PCA
pca = PCA(n_components=2)
embed_2d = pca.fit_transform(X)
```

```python
embed_2d.shape
```

    (8560, 2)

```python
# import os
# import shutil
# import urllib.request
# import matplotlib

# # 设置字体文件的URL和目标文件路径
# font_url = "https://github.com/StellarCN/scp_zh/raw/master/fonts/SimHei.ttf"
# target_font_path = os.path.join(matplotlib.get_data_path(), "fonts/ttf/SimHei.ttf")

# # 下载字体文件
# urllib.request.urlretrieve(font_url, target_font_path)

# # 找到matplotlibrc文件的路径
# matplotlibrc_path = matplotlib.matplotlib_fname()
# # 获取matplotlibrc文件所在的文件夹路径
# matplotlibrc_dir = os.path.dirname(matplotlibrc_path)

# # 找到fonts/ttf目录的路径
# fonts_dir = os.path.join(matplotlib.get_data_path(), "fonts/ttf")

# # 删除缓存文件夹中的内容
# cache_dir = matplotlib.get_cachedir()
# shutil.rmtree(cache_dir, ignore_errors=True)

# print("字体文件已下载到:", target_font_path)
# print("matplotlibrc文件所在目录:", matplotlibrc_dir)
# print("删除缓存文件夹:", cache_dir)

```

```python
plt.figure(figsize=(14,14))
plt.scatter(embed_2d[:, 0], embed_2d[:, 1])
plt.show()
```

### 可视化某个词条的二维Embedding

```python
term = 'computer vision'
```

```python
term_256d = model.wv[term].reshape(1,-1)
```

```python
term_256d.shape
```

    (1, 256)

```python
term_2d = pca.transform(term_256d)
```

```python
term_2d
```

    array([[-0.31159213, -0.55424124]], dtype=float32)

```python
plt.figure(figsize=(14,14))
plt.scatter(embed_2d[:,0], embed_2d[:,1])
plt.scatter(term_2d[:,0],term_2d[:,1],c='r',s=200)
plt.show()
```

### 可视化某些词条的二维Embedding

```python
# 计算PageRank重要度
pagerank = nx.pagerank(G)
# 从高到低排序
node_importance = sorted(pagerank.items(), key=lambda x:x[1], reverse=True)
```

```python
# 取最高的前n个节点
n = 30
terms_chosen = []
for each in node_importance[:n]:
    terms_chosen.append(each[0])
```

```python
# 手动补充新节点
terms_chosen.extend(['computer vision','deep learning','convolutional neural network','convolution','natural-language processing','attention (machine learning)','support-vector machine','decision tree','random forest','computational imaging','machine vision','cognitive science','neuroscience','psychophysics','brain','visual cortex','visual neuroscience','cognitive model','finite difference','finite difference time domain','finite difference coefficients','finite difference methods for option pricing','iso 128','iso 10303'])

```

```python
terms_chosen
```

    ['graph theory',
     'chaos theory',
     'claude shannon',
     'information theory',
     'operations research',
     'quantum logic gate',
     'data mining',
     'remote sensing',
     'electromagnetic wave equation',
     'collective intelligence',
     'control theory',
     'fourier transform',
     'empathy',
     'false dilemma',
     'simulated reality',
     'superlens',
     'digital preservation',
     'wearable computer',
     'low-density parity-check code',
     'analytics',
     'spatial dependence',
     'constructed language',
     'correlation',
     'semantic web',
     'psychoacoustics',
     'collaborative software',
     'philosophy of perception',
     'cognitive science',
     'transhumanism',
     'self-awareness',
     'computer vision',
     'deep learning',
     'convolutional neural network',
     'convolution',
     'natural-language processing',
     'attention (machine learning)',
     'support-vector machine',
     'decision tree',
     'random forest',
     'computational imaging',
     'machine vision',
     'cognitive science',
     'neuroscience',
     'psychophysics',
     'brain',
     'visual cortex',
     'visual neuroscience',
     'cognitive model',
     'finite difference',
     'finite difference time domain',
     'finite difference coefficients',
     'finite difference methods for option pricing',
     'iso 128',
     'iso 10303']

```python
# 输入词条，输出词典中的索引号
term2index = model.wv.key_to_index
```

```python
# index2term = model.wv.index_to_key
# term_index = np.array(term2index.values())
```

```python
# 可视化全部词条和关键词条的二维Embedding
plt.figure(figsize=(14,14))
plt.scatter(embed_2d[:,0], embed_2d[:,1])

for item in terms_chosen:
    idx = term2index[item]
    plt.scatter(embed_2d[idx,0], embed_2d[idx,1],c='r',s=50)
    plt.annotate(item, xy=(embed_2d[idx,0], embed_2d[idx,1]),c='k',fontsize=12)
plt.show()
```

## TSNE降维可视化

### 可视化全部词条的二维Embedding¶

```python
# 将Embedding用TSNE降维到2维
from sklearn.manifold import TSNE
tsne = TSNE(n_components=2, n_iter=1000)
embed_2d = tsne.fit_transform(X)
```

```python
plt.figure(figsize=(14,14))
plt.scatter(embed_2d[:, 0], embed_2d[:, 1])
plt.show()
```

### 可视化全部词条和关键词条的二维Embedding

```python
plt.figure(figsize=(14,14))
plt.scatter(embed_2d[:,0], embed_2d[:,1])

for item in terms_chosen:
    idx = term2index[item]
    plt.scatter(embed_2d[idx,0], embed_2d[idx,1],c='r',s=50)
    plt.annotate(item, xy=(embed_2d[idx,0], embed_2d[idx,1]),c='k',fontsize=12)
plt.show()
```

```python
embed_2d.shape
```

    (8560, 2)

### 导出TSNE降维到二维之后的Embedding

```python
terms_chosen_mask = np.zeros(X.shape[0])
for item in terms_chosen:
    idx = term2index[item]
    terms_chosen_mask[idx] = 1
```

```python
df = pd.DataFrame()
df['X'] = embed_2d[:,0]
df['Y'] = embed_2d[:,1]
df['item'] = model.wv.index_to_key
df['pagerank'] = pagerank.values()
df['chosen'] = terms_chosen_mask
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
      <th>X</th>
      <th>Y</th>
      <th>item</th>
      <th>pagerank</th>
      <th>chosen</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>86.368530</td>
      <td>19.423891</td>
      <td>graph theory</td>
      <td>0.000289</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>37.059536</td>
      <td>-4.361592</td>
      <td>information theory</td>
      <td>0.000185</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>49.873173</td>
      <td>-24.942081</td>
      <td>claude shannon</td>
      <td>0.000173</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>-0.228913</td>
      <td>-9.877577</td>
      <td>data mining</td>
      <td>0.000091</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>-27.195446</td>
      <td>-0.748093</td>
      <td>chaos theory</td>
      <td>0.000083</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>8555</th>
      <td>-28.879446</td>
      <td>54.118038</td>
      <td>clive wearing</td>
      <td>0.000050</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>8556</th>
      <td>-37.767330</td>
      <td>20.450050</td>
      <td>shy tory factor</td>
      <td>0.000051</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>8557</th>
      <td>0.282963</td>
      <td>60.908173</td>
      <td>collective intentionality</td>
      <td>0.000051</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>8558</th>
      <td>-13.359755</td>
      <td>-6.086845</td>
      <td>variational bayesian methods</td>
      <td>0.000047</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>8559</th>
      <td>22.884514</td>
      <td>82.094078</td>
      <td>vocabulary learning</td>
      <td>0.000047</td>
      <td>0.0</td>
    </tr>
  </tbody>
</table>
<p>8560 rows × 5 columns</p>
</div>

```python
df.to_csv('tsne_vis_2d.csv',index=False)
```

### 可视化全部词条的三维Embedding

```python
# 将Embedding用TSNE降维到2维
from sklearn.manifold import TSNE
tsne = TSNE(n_components=3, n_iter=1000)
embed_3d = tsne.fit_transform(X)
```

### 导出TSNE降维到三维之后的Embedding

```python
df = pd.DataFrame()
df['X'] = embed_3d[:,0]
df['Y'] = embed_3d[:,1]
df['Z'] = embed_3d[:,2]
df['item'] = model.wv.index_to_key
df['pagerank'] = pagerank.values()
df['chosen'] = terms_chosen_mask
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
      <th>X</th>
      <th>Y</th>
      <th>Z</th>
      <th>item</th>
      <th>pagerank</th>
      <th>chosen</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>25.126606</td>
      <td>-1.095677</td>
      <td>-5.323648</td>
      <td>graph theory</td>
      <td>0.000289</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>12.123558</td>
      <td>-8.145174</td>
      <td>-4.209531</td>
      <td>information theory</td>
      <td>0.000185</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>12.928997</td>
      <td>-6.321793</td>
      <td>2.291451</td>
      <td>claude shannon</td>
      <td>0.000173</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>-7.868805</td>
      <td>-13.286612</td>
      <td>-14.298657</td>
      <td>data mining</td>
      <td>0.000091</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1.674853</td>
      <td>-2.698683</td>
      <td>8.963208</td>
      <td>chaos theory</td>
      <td>0.000083</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>8555</th>
      <td>-5.377722</td>
      <td>20.571009</td>
      <td>3.298291</td>
      <td>clive wearing</td>
      <td>0.000050</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>8556</th>
      <td>-12.325991</td>
      <td>19.030151</td>
      <td>-8.670228</td>
      <td>shy tory factor</td>
      <td>0.000051</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>8557</th>
      <td>4.694840</td>
      <td>23.837486</td>
      <td>-1.217674</td>
      <td>collective intentionality</td>
      <td>0.000051</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>8558</th>
      <td>-11.294335</td>
      <td>-2.773937</td>
      <td>-14.848171</td>
      <td>variational bayesian methods</td>
      <td>0.000047</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>8559</th>
      <td>-2.179960</td>
      <td>17.069071</td>
      <td>-16.326900</td>
      <td>vocabulary learning</td>
      <td>0.000047</td>
      <td>0.0</td>
    </tr>
  </tbody>
</table>
<p>8560 rows × 6 columns</p>
</div>

```python
df.to_csv('tsne_vis_3d.csv',index=False)
```

## 作业

用`tsne_vis_2d.csv`和`tsne_vis_3d.csv`做可视化

参考代码：<https://echarts.apache.org/examples/zh/editor.html?c=scatter3d&gl=1&theme=dark>

