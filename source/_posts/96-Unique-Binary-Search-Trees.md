---
title: 96. Unique Binary Search Trees
date: 2018-12-13 05:06:24
tags:
---
## 题目：
Given n, how many structurally unique BST's (binary search trees) that store values 1 ... n?
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181213044919256.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDk5NDkxMw==,size_16,color_FFFFFF,t_70)
## 思路：
这题是有标准答案的，答案是用的动态规划的思路，但是这道题的动态规划思路并不是那么straightforward。首先我们想到的肯定是要递归，但递归到动态规划的差别是每一个sub problem是否重合。
从n个元素开始，拿出一个元素，看剩下的n-1个元素有多少种BST，这时候还要考虑，元素是不重复的（实际上在做BST的时候基本都假设元素是不重复的），然后在n-1个元素中，继续去掉一个，看其他的n-2个元素有多少种BST，这时候就可以发现在两种不同的n-1个元素中，其中的n-2个元素的情况是有重合的，所以是DP。
标准答案的思路不重不漏：把n个元素排序（其实只是在考虑的时候排序，真的计算时候，可以默认已经排序了），从第1个元素到第n个元素，每次考虑把第j个元素作为root，则BST是不重复的（因为元素是不重复的）。第j个元素左边都比他小，右边都比他大，左边的BST数目x右边的BST数目，就是以元素j为root的所有树的可能性。

## 代码：
```c
class Solution {
public:
    int numTrees(int n) {
        
        vector<int> ret(n+1, 0);
        ret[0] = 1;
        ret[1] = 1;
        for(int i = 2; i<n+1; i++ ){
            for(int j=1; j<i+1; j++){
                ret[i] += ret[j-1]*ret[i-j];
            }
        }
        return ret[n];
    }
};
```
