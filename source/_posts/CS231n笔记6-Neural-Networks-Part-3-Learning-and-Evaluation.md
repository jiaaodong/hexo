---
title: 'CS231n笔记6-Neural Networks Part 3: Learning and Evaluation'
date: 2018-12-03 05:06:03
tags:
---
- Centered-formula 是二阶近似的导数，可以用$f(x+h)$与$f(x-h)$的傅里叶级数证明
- 用双精度来做gradient checking
- 还是关于双精度的问题：有的时候数据太小了，很容易产生数值方面的问题，$1e^{-10}$以下就很不安全了。所以在计算Loss Function的时候可以考虑不处以Batch Size。如果能够放缩的话，$1e^{0}$是浮点数比较密集的地方
- 反复强调Gradient Check的重要性与 各种方法，以至于只用少数的样本在非常初级的时候去检验，都不一定可靠。
- Sanity Check：
	1. 观察最初的Loss Function是不是符合常理，比如10个类别随机给标签，那么每个类别是0.1，cross-entropy应该为$-log(0.1) = 2.302$
	2. 更强的正则化会令损失函数增加
	3. 过拟合小的子数据集
-  Batch Size小会令Loss Function的降低过程噪音更大
-  momentum 的优化方法是在固定的learning rate的基础上，把learning rate作用到一个量上，然后每次累积这个量，也就是说，每一个batch的update不仅受到当前batch的影响，还受到上一个batch的影响：
> $v = \mu \times v - learning rate \times grad$
> $x = x + v$
- 每一次考虑梯度下降的时候就考虑3维空间：参数是2维，Loss Function是第三维，每次更新就是在2维地图选择一个“方向”前进。
- 关于物理的动能、动量的联想也是很有帮助的。
- Learning Rate 有点像速度，如果一开始在距离最优很远的地方，就要有很大的速度去到达最优，但是如果已经距离最优很近了，太大的速度就会在最优周围跳来跳去
- 二阶矩是用Hessian Matrix代表曲率，曲率越大的地方，就用更小的Learning Rate。问题是，Hessian Matrix的计算量太大，所以就用Quasi-Newton，而不用真正的Newton Method
- 我们也可以对不同的参数设置不同的Learning Rate
- 这是Adam方法和Momentum方法很大的不同
- Adam主要就是同时考虑均值和方差，分子是均值的moving average，分母是方差的moving average，均值的方向是靠谱方向，方差大的方向是不靠谱的方向。
- 当梯度初始化为0时候，用了normalization
- 用一个worker程序来取样hyper-parameters，并将每一个epoch的结果、参数记录在一个checkpoint文件中。用一个master程序来启动、关闭worker，并查看不同参数下的表现。
- 只用一个validation fold来做交叉验证
- 用`10**uniform(-6,1)`来做learning rate和regularization strength
- 一开始用小epoch，from coarse to fine。
- Model Ensembling可以得到更好的效果，不过要考虑成本。
