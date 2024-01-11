# 图优化问题经常拿来比较的一些模型


## 前言

两篇文章的主体解析没有涉及作者进行模型比较的部分，这里主要解决一下该部分

由于两篇文章都涉及该方法的比较，所以重头戏是```DQN```以及其衍生的一些变体，还有部分别的模型，是需要提前了解的。

大致的模型解决思路图

![https://pic1.zhimg.com/80/v2-6eaafed73d5507e3c462aa54ae270990_720w.webp](https://pic1.zhimg.com/80/v2-6eaafed73d5507e3c462aa54ae270990_720w.webp)

## RL-GNN

### DQN

Human-level control through deep reinforcement learning

<https://www.nature.com/articles/nature14236>

使用```CNN```提取状态特征，状态表示的预处理

使用神经网络作为函数逼近器学习价值网络，得到高维空间下的近似Q值函数

使用```off-policy```框架，并把过去的经验```transition```保存下来重用

### S2V-DQN

Learning Combinatorial Optimization Algorithms over Graphs

<https://proceedings.neurips.cc/paper_files/paper/2017/file/d9896106ca98d3d05b8cbdf4fd8b13a1-Paper.pdf>

```S2V-DQN```是```DQN```的一个变体。

该方法利用元算法（贪心算法），逐步构建问题的解决方案。

在每个贪心步骤中，利用图嵌入技术（```structure2vec```，即```S2V```）来将图中的节点编码为向量表示，这种编码方式在每一步中都会得到更新，以反映当前状态下的局部信息。这种图嵌入方法有助于捕捉图中节点的特征。

在每个贪心步骤中，通过结合部分解来更新图嵌入，以将新的收益信息反映到最终的目标值中。为了实现这种延迟回报的学习，采用强化学习中的```n-step Q-learning```方法，以调整图嵌入网络的参数。

### ECO-DQN

Exploratory combinatorial optimization with reinforcement learning

<https://ojs.aaai.org/index.php/AAAI/article/download/5723/5579>

基于```DQN```，修改```action```改为可以移除某一个点，

```reward```改为```normalized incremental reward```，具体来说，在一个```episode```中，每次找到更低的局部最小值，就会获得一个小的```reward```。

允许```agent```在测试时通过探索解空间来不断改进解决方案，可以在解决方案子集中添加或删除顶点，并在测试时寻找不断改进的解决方案。

### M-DQN

Munchausen Reinforcement Learning

<https://proceedings.neurips.cc//paper/2020/file/2c6a0bae0f071cbbf0bb3d5b11d90a82-Paper.pdf>

利用当前策略去引导```bootstrapping```，因为当前策略能够影响下一步做的决策。

其实质就是在任何时间差异算法中(文章列举了DQN和IQN的修改)，将缩放的对数策略添加到即时奖励中。

## ReduMIS

Finding Near-Optimal Independent Sets at Scale

<https://www.researchgate.net/publication/281487239_Finding_Near-Optimal_Independent_Sets_at_Scale>

使用约简规则来提高进化算法的性能，并选择可能在大型独立集中的顶点以实现进一步的约简。

这个过程递归地重复进行，从而发现大型独立集。

该算法属于进化算法，结合核化技术，在巨大稀疏网络中计算大规模独立集。通过递归选择可能属于大规模独立集的顶点，并进一步核化图形，比现有的局部搜索算法更快，并且能够在比以前文献报道的更大实例上计算高质量的独立集。

## Classic

Combinatorial Optimization: Algorithms and Complexity

<https://www.researchgate.net/publication/220695898_Combinatorial_Optimization_Algorithms_and_Complexity>

经典强化学习方法包括动态规划、蒙特卡洛方法和时序差分学习等，但目前好像没看到直接用来解决问题的，大多数是嵌入到某些模型中，优化某一个步奏。
