---
title: SSD的使用
date: 2018-11-08 20:43:41
tags:
---
### 1. Preface
最近需要用到SSD的目标检测来做一个demo，因此今天在配置SSD。

### 2. Setup
使用一个现成的Github项目，第一步肯定就是按照人家的README乖乖配置环境。我的笔记本啥都干不了，硬盘空间放不下一个数据集，CPU是低压版，没有GPU，所以我都是习惯性用Google Cloud Platfom（GCP），我的Setup也都是基于我使用的Ubuntu 16.04 镜像来实现的。不过这个镜像和一般的Ubuntu16也没什么不同。

#### 2.1 克隆 SSD 远程仓库
- 从[SSD](https://github.com/weiliu89/caffe/tree/ssd)的github中找到Installation。然后把他们的repository给clone过来，接下来开始配置Caffe。
#### 2.2 配置Caffe
- [Caffe 的安装](http://caffe.berkeleyvision.org/installation.html#compilation)
- Caffe的编译需要根据本地的Python 环境来改写Makefile.config文件，主要需要修改的是Python的路径和numpy的路径，Makefile.config里面的注释写得比较清楚，我的Python环境是用Anaconda配置的，所以我就把Python的路径改成了我的Anaconda路径，其实文件里已经写好了，只需要把Python的版本改成3.7就行了。我暂时不想用GPU（因为谷歌的GPU还是挺贵的，虽然是按照使用时间收费，但是我还不清楚是按照instance的使用时间收费还是按照GPU的使用时间收费），所以把`cpu_only`设置为1。把`BLAS`改成自己用的，我用的是`atlas`，这个取决于在上一步安装的时候究竟安装了哪个pkg。
- 然后开始编译，`make all`，或者`make -j8`多线程快速编译（我对j8还没怎么理解）结果就遇到了[在编译Caffe时候的常见问题](https://github.com/BVLC/caffe/wiki/Commonly-encountered-build-issues)中提到的1-4，根据网页上说的解决方法把问题解决（主要就因为缺少一些Debian的pkg，毕竟我每次都会新创建一个服务器instance，里面什么都没有，只有必备的Ubuntu16，连Anaconda都是自己下载的）。
- 然后再次编译（记得次重新编译前用一下`make clean`），又出现了以下的错误:
```
CXX/LD -o .build_release/tools/create_label_map.bin
.build_release/lib/libcaffe.so: undefined reference to `boost::re_detail::cpp_regex_traits_implementation<char>::tr
ansform_primary(char const*, char const*) const'
```
这个错误主要是boost的错误，在[这个issue](https://github.com/BVLC/caffe/issues/6043)里面有相关解答和[这个](https://github.com/rbgirshick/fast-rcnn/issues/52)

