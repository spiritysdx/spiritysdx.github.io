# Actor-Critic Methods


## 前言

![https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230908/10.png?raw=true](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230908/10.png?raw=true)

Actor-Critic Methods 结合了价值学习和策略学习，同时训练了两个神经网络。

Actor 网络用于产生策略，Critic 网络用于评估策略。

![https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230908/11.png?raw=true](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230908/11.png?raw=true)

## 目标

① 更新策略网络Π的参数，是为了增大状态价值V的值，要用价值网络q进行打分来训练。

② 更新价值网络q的参数，是为了让评价网络更精准的评价动作，从而更精准的预测累计奖励。

![https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230908/12.png?raw=true](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230908/12.png?raw=true)

## 具体的可视化流程

![https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230908/13.png?raw=true](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230908/13.png?raw=true)

![https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230908/14.png?raw=true](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230908/14.png?raw=true)

![https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230908/15.png?raw=true](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230908/15.png?raw=true)

每个回合只执行一次动作，但预测两次动作，只更新一次参数。

## 流程总结

![https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230908/16.png?raw=true](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230908/16.png?raw=true)

![https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230908/17.png?raw=true](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230908/17.png?raw=true)

这里不用qt用deta t是使用了 Baseline 而不是原始的方法，不影响期望，但可以让方差降低减少误差。

实际任何qt附近的数都可以作为 Baseline 但它不能是动作 at 的函数。

![https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230908/18.png?raw=true](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20230908/18.png?raw=true)

