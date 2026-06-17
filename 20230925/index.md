# 用图卷积和树搜索解决组合优化问题(含笔记)


## 基于原文作的个人笔记，在此展出

建议按住ctrl然后滑动鼠标滑轮放大查看，笔记做的很小，图片分辨率很高，放大也不会糊

原文：Combinatorial Optimization with Graph Convolutional Networks and Guided Tree Search

原文链接：<https://arxiv.org/abs/1810.10659>

![https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20231019/0.png?raw=true](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20231019/0.png?raw=true)

![https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20231019/1.png?raw=true](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20231019/1.png?raw=true)

![https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20231019/2.png?raw=true](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20231019/2.png?raw=true)

![https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20231019/3.png?raw=true](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20231019/3.png?raw=true)

![https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20231019/4.png?raw=true](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20231019/4.png?raw=true)

![https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20231019/5.png?raw=true](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20231019/5.png?raw=true)

![https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20231019/6.png?raw=true](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20231019/6.png?raw=true)

![https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20231019/7.png?raw=true](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20231019/7.png?raw=true)

![https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20231019/8.png?raw=true](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20231019/8.png?raw=true)

![https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20231019/9.png?raw=true](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20231019/9.png?raw=true)

![https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20231019/10.png?raw=true](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20231019/10.png?raw=true)

![https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20231019/11.png?raw=true](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20231019/11.png?raw=true)

![https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20231019/12.png?raw=true](https://cdn.spiritlhl.net/https://github.com/spiritysdx/images/blob/main/20231019/12.png?raw=true)

