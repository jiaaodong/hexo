---
title: 'CS231n笔记1-Linear Classifier: Support Vector Machine, Softmax Classifier'
date: 2018-11-22 05:29:46
tags:
---
- Linear classifier is a template matching. 这里的线性分类器就是相当于给每一个Feature一个权值，这个权值其实是一个尺寸与图片一样大小的Filter。记得一个结论：Filter的样子往往就是你要Filter出的东西的样子，所以学习出的权值本身就是要学习的图片模板。
- Normalization. 归一化的作用要在反向传播的时候才能显现出来。
- Multi-class SVM loss. 目标是为了让正确的类别的分数比错误的类别的分数大出Delta值，如果已经大出Delta了，那就不管了。
- Regularization. 正则化的目的是防止过拟合，而L2 penalty的作用就是让权重更加diffuse。
- 其实hinge loss中的margin与正则化中的lambda是一个作用，都是表达的weights与loss的trade-off。因此hinge loss中的delta可以直接用1.
- 数据稳定性：在用softmax的时候要将所有的scores减去他们的最大值再取指数运算。
- cross-entropy与KL divergence是等效的（在one hot encoding的前提下），因为delta function的entropy是0。这里的Delta function就是1-hot encoding向量。
- 光看cross-entropy可以理解为概率中的MLE，如果是带有正则的cross-entropy可以理解为有gaussian distribution a posterier的MAP。
- cross-entropy loss从不会对已经训练出的分布“满意”，永远在找更加接近的。而SVM一旦对margin“满意”了（即所有样本正确类别的score都比错误类别的score至少大出delta值了），那么Hinge loss就不再增加了。
