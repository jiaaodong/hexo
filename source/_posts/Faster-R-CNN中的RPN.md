---
title: Faster R-CNN中的RPN
date: 2018-11-02 03:17:52
tags:
---
今天在读Faster R-CNN。

之前读过一次，当时正在看SSD，感觉Faster R-CNN的网络结构和SSD有一些相似，所以Faster R-CNN就只是粗略地看了一下。今天当我有时间仔细读了一下以后，突然被一个问题卡住了——下图中从feature map到256-d向量的intermediate networks是什么？
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181102033008862.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDk5NDkxMw==,size_16,color_FFFFFF,t_70)从图中可以看出，我们截取了CNN（比如VGG-16）的前半部分用于提取特征（常常是直觉用pre-trained model就行了），然后在输出的Feature Map中应用一个3 x 3的Sliding Window，每一个位置输出k个bounding boxes。但是，从这个Feature Map到bounding box的intermediate layer输出的是一个256-d的特征向量，原文中说是这个特征向量通过了全连接层得到的k个bounding boxes，那么这里的intermediate layer究竟是什么呢？

从[这部视频](https://www.youtube.com/watch?v=X3IlbjQs190)中我恍然大悟，从Feature Map到256-d特征向量是非常Straightforward的一件事（当我看懂了才觉得Straightforward以及我太菜了）所谓的Sliding Window，只是为了模型的原理更容易被解释，毕竟卷积层的本质就是一个kernel在一个feature map上面做sliding window。如果用Sliding window的方法来解释，这个过程变成了：一个3 x 3的窗口在输入RPN的Feature Map上面滑动（注意，另一个容易令人产生迷惑感的地方在于，虽然窗口只有 3 x 3，==但是需要 256 x 3 x 3 x 256个参数==才能把一个==3 x 3 x 256==的输入映射到一个==1 x 1 x 256==的vector），在滑动过程中，输入RPN的Feature Map的每一个像素值都由此得到了一个256-d的vector（或512-d，取决于用ZF还是用VGG-16），然后这个vector通过了一个全连接层得到了6k个输出，即，每一个 3 x 3的crop都会得到6k个输出，为k个bounding boxes的坐标、offset、objectiveness score（这里objectiveness score的意思是有object的可能性）。

下面我来说为什么作者又用了一定的篇幅来讲从卷积的角度如何理解刚才的sliding window：

实际上，前一层的Feature Map被256  x 3 x 3 x 256的kernel做卷积运算（[这篇知乎专栏](https://zhuanlan.zhihu.com/p/44599606)的图是非常好的一幅图），就可以得到W x H x 256的输出（W X H是输入RPN的feature map的resolution），这与sliding window在每一个位置都得到一个256-d vector的效果是一样的。关键就在于，这256-d vector如何能够直接在每一个位置都通过一个==相同==的全连接网络得到一个6k长的向量。这时候就要提到CNN的一个重要特性——通过卷积来实现空间上的参数共享。==一个256 x 1 x 1 x 6k 的kernel，本质上就是一个滑动的、将256个元素对应到6k个元素的小型全连接网络。== 这个观点来自于Fully Convolutional Networks那篇文章，感兴趣的读者可以看[Fully convolutional networks for semantic segmentation](https://people.eecs.berkeley.edu/~jonlong/long_shelhamer_fcn.pdf). 这个网络确实只有256 x 6k个参数，读者非常自豪地说，比MultiBox的参数少多了，但第4页的注释5提到了如果你考虑将输入RPN的Feature Map转为一个个256-d vector的那层（即feature projection layers），那一层的参数数量是3 x 3 x 512 x 512，整体参数就也不算少了。 

我们从头捋一遍，以VGG-16为例，根据视频中所讲，conv13的Feature Map是输入RPN的Feature Map（在该项目github的python实现中是conv5_3层），这个conv13的分辨率大约为40 x 60 ~ 2400。用一个3 x 3的Sliding Window（or, in fact, a kernel）走过Feature Map中的每一个像素点，将3 x 3 x 512的小块Feature Map映射到1 x 1 x 512 的Feature中，参数数量为512 x 3 x 3 x 512（我喜欢这样写一个卷积层的参数，因为正好是按照“上一层channel数”-本层的卷积大小-“本层的channel数”来排列，便于理解），接着通过一个全联结层，得到bounding box的坐标和分数。设k（生成的bounding box的数量）为9，则全联结层用一组参数将所有的512-d特征向量映射到9个数字，参数数量为512 x 9。总的参数为512 x 3 x 3 x 512 + 512 x 9。

这个Sliding Window的机制，使该全联结网络的参数对于Feature Map中的每一个像素点是共享的，也就是说，所有的512-d的vector都是通过同一组参数做推断，这组参数应用于所有位置的像素点，所以才能够让RPN做到translational invariance。

### 参考文献

Long, J., Shelhamer, E., & Darrell, T. (2015). Fully convolutional networks for semantic segmentation. Proceedings of the IEEE Computer Society Conference on Computer Vision and Pattern Recognition, 07–12–June, 3431–3440. https://doi.org/10.1109/CVPR.2015.7298965
