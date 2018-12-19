---
title: 'CS231n笔记3-Backpropagation, Intuitions'
date: 2018-11-23 05:06:20
tags:
---
- 如果我们有了函数$f(W, \vec x)$，我们既关心$f$对$W$的导数，因为我们需要更新的就是$W$，也关心$f$对$\vec x$的导数，因为我们要反向传播。
> The derivative on each variable tells you the sensitivity of the whole expression on its value.
- 微分是完完全全的局部运算，每一个节点只需要计算该节点的输出对每一个输入的微分。只有当Forward Pass结束了，Backward Pass开始时候，每个节点的微分才被链式法则连接起来得到改节点对Loss Function的导数。
- Sigmoid函数也是把结果压缩到[0, 1]之间，其导数是$(1-\sigma(x))\sigma(x))$。
- Intuition of back-propagation：加法同等程度地传播梯度、max函数把梯度重新定向到更大的输入，乘法比较复杂，不太intuitive，还是不强行解释了。
- 因为乘法中的back propagation是和输入的数量级有关的，所以数据的预处理很重要，如果一个很大的数和一个很小的数相乘，反向传播时候，很大的输入得到很小的梯度，很小的输入得到很大的梯度。
