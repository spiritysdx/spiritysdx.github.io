# Deep Q-Network (DQN) (Value-Based learning)


## 寻找最佳的Q值函数

实际并不知道最佳的Q值函数，需要使用神经网络 Q(s,a;w) 来近似最佳的Q值函数。

实际流程大致为当前状态转换为矩阵后，通过卷积层提取特征向量，再通过全连接层得到Q值向量，此时的Q值向量每一个元素代表某一个动作的得分。

![https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230907/4.png?raw=true](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230907/4.png?raw=true)

![https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230907/3.png?raw=true](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230907/3.png?raw=true)

## TD算法训练DQN

![https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230907/5.png?raw=true](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230907/5.png?raw=true)

实际就是在总任务中，预测任务的一部分，然后实际运行这部分任务，看看二者之间的TD误差，然后通过梯度下降不断调整参数，使得TD误差最小(最好的当然是0，但实际大都不可能达到)，然后继续执行这部分任务后续部分，循环往复。

任务执行前：

![https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230907/6.png?raw=true](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230907/6.png?raw=true)

任务执行后：

![https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230907/7.png?raw=true](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230907/7.png?raw=true)

完整流程：

![https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230907/8.png?raw=true](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230907/8.png?raw=true)

在环境输出预测的Q值时，会使用贪心策略，即选择Q值最大的动作，实际在实际运行时，会使用epsilon-greedy策略，即有一定概率随机选择动作，以一定的概率选择Q值最大的动作。

每次得到reward和state时，都会去进行梯度下降计算，更新参数。
