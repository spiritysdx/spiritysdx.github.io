# 通过GCN生成概率图引导树搜索解决图的组合优化问题


## 前言

老板下指示复现两篇文章，这是其中一篇

https://arxiv.org/pdf/1810.10659.pdf

文章的原理什么的已经大致明白了但仍然有小部分不懂，故而做下记录，以备后续复现或深入了解

## 原始数据

### Training Data

https://www.cs.ubc.ca/~hoos/SATLIB/benchm.html

### Testing Data

#### SAT Competition 2017

https://helda.helsinki.fi/bitstream/handle/10138/224324/sc2017-proceedings.pdf

#### BUAA-MC

https://www.sciencedirect.com/science/article/pii/S0004370207000653

#### SNAP Social Networks

http://snap.stanford.edu/data/#socnets

#### Citation networks

https://ojs.aaai.org/aimagazine/index.php/aimagazine/article/view/2157

## 相关Github仓库

原作者仓库的脚本年久失修且数据需要自己生成，但作者又没给生成方式，只给了一个mat文件，仓库issues有不少对此的疑问

https://github.com/isl-org/NPHard

另一位瑞士博士将原作者的代码修复后，又使用了```dgl```库进行优化，不用自己写基础代码构建了，同时还给出了原始数据如何标记的代码和生成的一些随机图实例，可以说是拿到就能跑了，这些在第二个仓库中都有

https://github.com/MaxiBoether/mis-benchmark-framework

但暂时由于第二个仓库作者的数据集的链接失效无法跑通，已发issue询问，已经和他联系上了，待他有空补一下数据集就能跑通了

## 大体思路

分了几步走，我精简的总结一下

### 一、根据最基础的GCN结构，做了以下的改进

- 输入层不变，激活函数是ReLU
- 在隐藏层加入```drop out```在激活函数前，激活函数是ReLU
- 输出层不变，激活函数是sigmod
- 二元交叉熵损失函数输出值不舍入为0或1，保持为[0, 1]上的值

层数作者实验过了，最好的层数是20层，输入层大小32，隐藏层大小都是32，输出层大小也是32

由于数据集 SATLIB 包含合成的 SAT 实例，知道其真实的指派情况。通过切换子句中的自由变量的开启和关闭，可以生成相应图的多个标记。

(上面这句话就看不懂了，生成标记卡住我了，原始的数据是一堆.cnf文件，第二个仓库作者用的```Gurobi```生成的标记，准备按第二个仓库作者的思路来)

使用Adam进行单图小批量训练，并使用学习率 10−4，训练进行200个epochs。

### 二、为了生成多个概率图但又保持准确性和多样性，做了以下改进

```
为了使网络能够区分不同的模式，扩展了函数 \(f\) 的结构以生成多个概率图。给定输入图 \(G\)，改进后的网络 \(f\) 生成 \(M\) 个概率图：

\[ f_1(G_i; \theta), \ldots, f_M(G_i; \theta) \]

为了训练 \(f\) 生成多样且高质量的概率图，采用了反思损失：

\[ L(D, \theta) = \sum_i \min_m \ell(l_i, f_m(G_i; \theta)) \]

其中，\(\ell(\cdot, \cdot)\) 是在公式 (2) 中定义的二元交叉熵损失函数。

```

大白话就是对于每个样本，计算 M 个概率图中的每个图与实际标签之间的损失，然后选择其中最小的损失作为训练损失。这样设计促使网络在生成多个概率图时，更加青睐于与实际标签最为接近的概率图，从而提高了生成结果的质量和多样性。

### 三、树搜索算法改进概率图的选择，尽可能快速的遍历到所有可能的解，做了以下操作

维护一个不完整解决方案的队列， 并在每一步中随机选择一个进行扩展。

当扩展一个不完整的解决方案时， 使用原有的M个概率图生成M个新的更完整的解决方案，并将它们加入到队列后面。

这有点类似于广度优先搜索(BSF)，而不是深度优先搜索(DSF)。因为如果以深度优先的方式扩展树，解决方案的多样性将会受到影响，因为它们的大多数原始的解决方案是相同的。通过以广度优先的方式扩展树，可以获得更高的多样性。

扩展后的树节点被保留在一个队列中，并且每次迭代中随机选择一个节点进行扩展。

( 这块作者说是使用了多线程，个人感觉上还能再进一步使用 多进程+多线程，但感觉进程之间的通信可能会影响解决的速度，得有阻塞的选择最优解，很麻烦且开销大，可能这就是没使用该方法的原因吧 )

### 四、用了局部搜索和图缩减技术进一步优化算法

老实说这一步我没看太懂，因为作者说的很简洁，有空我再补补对应的知识，暂时先就文章字面的描述总结和翻译一下。

#### 局部搜索(LS)

通过简单地插入、删除和交换节点，在局部范围内修改解决方案，以使解决方案的质量只能提高。

使用这种方法来优化树搜索产生的候选解

https://pubsonline.informs.org/doi/abs/10.1287/opre.42.5.860

对于局部搜索，采用了一种2-改进局部搜索算法。

https://dl.acm.org/doi/10.1145/3447548.3467232

该算法遍历图中的所有顶点，并尝试将一个标记为1的顶点vi替换为两个标记为1的顶点vj和vk。在最大独立集（MIS）中，vj和vk必须是vi的邻居，并且1-紧密而且彼此不邻接。这里，如果一个顶点的邻居中只有一个被标记为1，我们称该顶点为1-紧密。换句话说，在图中vj和vk的唯一一个标记为1的邻居是vi。

通过使用一种数据结构，使得插入和删除节点的时间与它们的度成正比，这个局部搜索算法可以在O(E)的时间内找到一个有效的2-改进（如果存在）。该算法的一种更快的增量版本维护了一个包含参与2-改进的候选节点的列表。它确保只有当节点的邻居发生了某些变化时才会反复检查一个节点。

#### 图缩减(GR)

还有一些图缩减技术可以快速将图缩减为一个更小的图，同时保持最优独立集的大小不变。

通过仅将f应用于图的「复杂」部分来加速计算。

https://arxiv.org/abs/1411.2680

https://arxiv.org/abs/1509.00764

## 实验比较

最后测试了四个经典的NP-hard问题：可满足性（SAT）、最大独立集（MIS）、最小顶点覆盖（MVC）和最大团（MC）。

(其他三种问题都可转换为MIS问题解决)

结果表明，与基于强化学习的方法相比，该方法在SATLIB基准测试中解决了所有问题，而强化学习方法没有解决任何问题。

此外，该方法在性能上与基于传统启发式方法的高度优化的求解器相当或更好。

## 后言

总体读下来感觉自己对离散数学的图论部分基础不够扎实，看后面的树优化吃力

关于GCN的部分还是很好理解的，图嵌入树部分也还行，但到后面树优化部分就开始抓瞎了

看了项目仓库是conda的env做环境的约束，但我想我以后做的项目最好还是打包成docker镜像比较好，环境以及依赖版本的约束更有效一些

conda的依赖导出的yml文件好多都是没版本号的，属实坑后来者复现，docker的话一般导出都有版本号约束，几乎不需要改什么依赖就能跑了

这里其实还缺少了两个模型评价部分互相比较的别的模型，这些东西由于两篇文章都有点关系，所以准备整合到一起总结，不单独每篇文章都拉出来说一遍了。
