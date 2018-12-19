---
title: 如何令ros melodic能够兼容ros indigo的message
date: 2018-11-22 17:01:52
tags:
---
__问题描述__：从ros indigo记录下来的rosbag遇到ros melodic就会报以下错误：
``Client wants topic xxxxx to have datatype/md5sum [xxxxx], but our version has [xxxxx]. Dropping connection``

__问题分析__：这是因为indigo的message与melodic的message不兼容导致的，是ros的版本问题。我拿到的rosbag是indigo下记录的rosbag，而我的node运行环境是ros melodic。

__思路__：改所有的client，遇到一个改一个，因为rosbag是没办法改的了。

1. 从github下载Indigo的marker message
2. 改node的`#include`部分
3. 将`Marker.msg`与`MarkerArray.msg`放到`catkin_ws`的`src/[node name]/msg/`下
4. 改`CMakeLists.txt`
5. 删除`build` 和 `devel`，重新`catkin_make`

__注意__：
只有自己写的node可以这样改，强烈不推荐用同样的方法改rviz，因为rviz是用的std_msgs。但是我们又不能去改std_msgs，因为这样将会把今后所有用到std_msgs的node都改为indigo的msg。
