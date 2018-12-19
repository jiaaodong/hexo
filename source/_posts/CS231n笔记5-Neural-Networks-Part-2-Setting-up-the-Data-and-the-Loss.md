---
title: 'CS231n笔记5-Neural Networks Part 2: Setting up the Data and the Loss'
date: 2018-11-28  05:01:33
tags:
---
- Mean subtraction 是一般会做的，相当于把整个点云平移到原点附近
- Normalization 仅当不同的Feature是不同的Scale时候更加必要，但是从今天白天对雷达多普勒响应的分类中看，SVM需要Normalize。
- PCA 方法常常用来降维，`np.dot(X, X.T)` 就是矩阵X的covariance matrix。这个矩阵一定是对称的、半正定的，因此可以对其应用奇异值分解。分解后得到了协方差矩阵的特征向量（并且`np.linalg.svd`所得到的特征向量已经按照对应的特征值由大到小的顺序拍好），所以原来的特征空间可以投影到任意比Feature维度低的空间里。
- variance 小的维度是噪声更大的维度。
- Whitening 就是把所有维度按照特征值方向投影后，再把每个维度处以该维度的特征值。
- PCA和Whitening在DL里基本不需要，但是Normalization和centering很重要。
- 将PCA和Whitening转换回去？可以通过乘Transormation Matrix的转置
- ==所有的预处理都要基于训练集的数据，而不是测试集的数据。比如：mean-subtraction 要减去训练集的mean，pca要拿训练集来计算特征向量==
- ==不能将权值初始化为0！！否则每一个Neuron的输出就成了一样的，反向传播时候函数的更新也变成了一样的。但是实际上神经元是不对称的==。这叫做Asymmetry breaking
- 可以将权值初始化为标准正态分布与一个很小的数的乘积，但是特别小的初始化值会令权重更新时候的值也变小，在深度学习时候导致收敛变慢。
- 初始化为标准正太分布后，每个neuron是一个`M x N`大小的矩阵，共`MN`个元素，每个元素都满足标准正态分布，但该neuron的输出会因此而拥有更大标准差的分布，所以我们在初始化neuron的时候要根据输入的数量改变初始化正态分布的标准差。主要的原因是因为输入和输出之间的相乘与相加。
- 三种形式的初始化：`w = np.random.randn(n) / sqrt(n)`，`w = np.random.rand(n) * 1 / (n1 + n2)` `w = np.random.randn(n) * sqrt(2.0/n)`。第三种是为ReLU专门设置的。
- bias通常都直接初始化为0
- Batch Normalization 可以有效地缓解不良的初始化，而且batch normalization 是一个可微分的过程。这个过程在神经元之后，Non-linearity 之前
- 从L2-norm regularization的求导可以得出结论：权值在训练过程中缓慢趋近于0
- L1 regularization 是显性的特征值挑选
- 在测试的时候不用Dropout但是要把所有的输出乘以Dropout的系数。因为之前有Dropout的时候，实际上每一层的Activation信号增强了，否则难以达到与没有Dropout时候相同的信号强度
- 但这个放缩通常被加在training中，除以probability
- Cross-entropy 在类别标签非常多的时候不好用，可以用一棵树来实现层级式的判断
- 如果每个样本的label不是一个单一的数字，而是一个向量，我们可以把损失函数写成两种形式，一种是$max(1 - \delta * label)$，另一种是把问题看成回归问题，将scores向量回归到label向量。
- 尽量不要构建L2-norm损失函数，因为在差距较大的时候会得到非常大的更新值，而且这个时候要求具体值的精确度，而不像softmax——只要求数量级之间的关系对了就行。
- 在遇到回归问题时候，先考虑能不能离散化为分类问题。
