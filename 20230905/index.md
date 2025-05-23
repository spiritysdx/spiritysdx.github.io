# 强化学习术语翻译


## State

状态，即状态空间，表示环境中的当前状态。

## Action && Agent

动作，即动作空间，表示在当前状态下，执行的动作。

动作由谁做的就是```Agent```，即智能体。

## Policy Π

策略，即策略空间，表示在当前状态下，智能体可以采取的动作。

数学上表示为概率密度函数```Π```，即执行动作a在当前状态s下概率为p(a|s)。

强化学习实际就是学习这个函数，执行的动作最好是随机抽样得到的，要有随机性，如果策略固定那么动作也就固定了。

## Reward R

奖励，即奖励函数，表示在当前状态下执行动作a后，环境给智能体的奖励。

这个函数是需要自己定义的，智能体通过学习策略，来最大化奖励。

## state transition

状态转移，即状态转移函数，表示在当前状态下执行动作a后，环境转移到下一个状态s的概率，是一个条件概率密度函数。

状态转移可以是确定或者随机的，一般是随机的。


## 随机性来源

① 动作带来的随机性，policy对输入状态s给予随机的动作。

② 状态转移带来的随机性，环境对输入的状态s和动作a，输出下一个状态s'，这个s'是随机的。

以上都基于随机抽样得到的随机性。

## trajectory 轨迹

(状态、动作、奖励) 的序列，即轨迹。

## Return 回报

从开始到结束的累计奖励，即回报。

Ut = Rt + Rt+1 + Rt+2 + ... + Rt+n

未来的奖励应当比当前的奖励低，所以 Rt+1 应当小于当前的 Rt。

因此 Discounted return 折扣回报 应运而生

γ 是折扣率，介于0到1之间，属于超参数需要调。

Ut = Rt + γ*Rt+1 + γ^2*Rt+2 + ... + γ^n*Rt+n

由于当前的回报U取决于奖励R，所以未结束时Ut是未知的，只有当结束时，Ut才确定，因而Ut也是随机的。

当前时刻的Rt取决于当前时刻的状态st和动作at，所以Rt是随机的。

## Action-Value Function Q(s, a)

动作价值函数Q

在策略Π下，在状态s执行动作a的期望回报，即

QΠ(st, at) = E(Ut|St=st, At=at)

这里是对非当前时刻t下的状态s和a积分(通过策略Π)，由于当前时刻的状态s和动作t给定是数值，所以是一个数值。

直观意义下，就是使用策略 Π 在状态 s 下执行动作 a 是好还是坏，评估动作的分数(当前状态下所有可能的动作的得分)。

由于用不同的策略 Π 会有不同的Q函数，所以如果对QΠ求最大化，得到的就是最优策略Π下的Q函数。

## State-Value Function V(s)

状态价值函数S

离散动作求和，连续动作积分。

![https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230907/1.png?raw=true](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230907/1.png?raw=true)

VΠ可以告诉我们当前的局势好不好，具体而言

① 使用策略 Π ，VΠ 可以告诉我们状态 S 下当前局势的好坏。

② 评价策略 Π 的好坏，不同的策略 Π 会有不同的 VΠ 值，策略 Π 越好，VΠ 的平均值越大。

## 如何训练智能体 agent

![https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230907/2.png?raw=true](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230907/2.png?raw=true)

Policy-Based learning 和 Value-Based learning

也即是 策略学习 和 价值学习，前者不言自明就是学习Π函数，后者是学习最优动作价值函数，强化学习只需要学习其中之一即可。

## 常用的测试集 (gym)

内含各种常见的测试强化学习算法的问题。


