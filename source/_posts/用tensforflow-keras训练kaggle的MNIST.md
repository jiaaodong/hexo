---
title: 用tensforflow.keras训练kaggle的MNIST
date: 2018-12-12 17:53:27
tags:
---
1. 将0-255的像素值normalize到0-1
2. 将categorical label转化为one-hot encoding
3. 选择合适的csv读取和写入包会加速Prototype的效率
4. Kaggle提交时候需要Header
5. Windows下生成csv会有空行，需要根据Python的版本来决定打开文件的方式及换行符
6. Kaggle的提交是有header的
