---
title: 'CS231n笔记2-Optimization: Stochastic Gradient Descent'
date: 2018-11-23 04:10:35
tags:
---
- 介绍了一种在高维空间可视化Loss Function的方法：随机挑选一个参数向量，如CIFAR-10的参数$W$为 $3072 * 10$。然后选定一个$\alpha$和一个向量$\vec u$，然后计算不同的$\alpha$值下参数$W+\alpha\vec u$得到的Loss Function。如果想要二维可视化，就选定$\alpha$ 、$\beta$和$\vec u$。
- 关于凸优化的问题——用只有一个Feature的数据举例子，找到某一个参数W与Loss的关系，是被0所截断的直线之和的平均。
- 但是NN是非凸的。
- ==补充复习一下Convex的定义==
- 斯坦福的课质量真的是高，贵在思路清晰：
>Strategy #1: A first very bad idea solution: Random search
>Strategy #2: Random Local Search
>Strategy #3: Following the Gradient
- 代码技巧：用`np.nditer`来遍历矩阵内部所有元素。
- Learning Rate 的重要性。
- 为什么可以用 mini-batch：因为数据是correlated的。粗略的证明：假设我们有1000个classes，每一个class中有1200张完全一样的图片，那么我们用120万张图片训练出来的结果与每个class各一张图片训练出来的结果是一样的。
- 使用2的倍数做为每个batch中数据的数量可以令向量化的运算更快。
- 具体用多少，还要考虑内存的限制。
