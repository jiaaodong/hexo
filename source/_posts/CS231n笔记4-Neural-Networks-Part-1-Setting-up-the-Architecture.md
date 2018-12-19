---
title: 'CS231n笔记4-Neural Networks Part 1: Setting up the Architecture'
date: 2018-11-27 05:23:57
tags:
---
- Regularization 在神经网络中可以被解释为 Gradual Forgetting。
- Softmax是泛化版本的Sigmoid
- Softmax可以作为一个neuron的firing rate
- Sigmoid的问题：
	1. Saturation
	2. Non zero-centered
- tanh 和sigmoid 的关系：
	$tanh(x) = 2* \sigma(2x) - 1$
- ReLU 是会导致梯度消失的，从而大面积的神经元“死亡”，因此出现了Leaky ReLU。
- Maxout：是ReLU和Leaky ReLU的泛化，但参数增加了一倍
- 实践中怎么选择：用ReLU，但是注意Learning Rate，同时尽量监控网络中死掉的神经元。永远不用sigmoid。
- 有一层Hidden Layer的神经网络是Universal Approximator
- 与CNN相比，全连接网络对于层数的依赖没有那么明显（但3层一般比2层强）这有可能是因为CNN的分层体系
- 层数太多了 Capacity就增强了，但层数太多了就可能学到数据中的噪音，这就是过拟合。
- 实际上，用其他技巧来防止overfitting，而不是减少网络复杂度。（比如可以用L2 regularization, dropout, input noise）
- 小的神经网络的解容易被困在局部最优，而且那些局部最优都不是很好。每次得到的解的方差很大。
- 大的神经网络不好训练，一开始会有很多的解，但是当最优解被找到时候，是比较好的局部最优，而不是很依赖于随机初始化。
- 
