---
title: Leetcode 62. Unique Paths
date: 2018-12-11 06:37:29
tags:
---
## Description:
A robot is located at the top-left corner of a m x n grid (marked 'Start' in the diagram below).

The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid (marked 'Finish' in the diagram below).

![在这里插入图片描述](https://assets.leetcode.com/uploads/2018/10/22/robot_maze.png)

How many possible unique paths are there?
## Examples 1:
Input: m = 3, n = 2
Output: 3
Explanation:
From the top-left corner, there are a total of 3 ways to reach the bottom-right corner:
1. Right -> Right -> Down
2. Right -> Down -> Right
3. Down -> Right -> Right
## Examples 2:
Input: m = 7, n = 3
Output: 28

## 思路：
又是一道非常典型的动态规划题目，还记得学机器学习中的强化学习时候，也是以动态规划开篇的。这道题和跳楼梯、整数求和的问题都是一样的。
跳楼梯：一次可以跨一步也可以跨两步，给定楼梯数量，有多少种走法。
整数求和：给定一个整数和一个数组，将整数用数组中的数目的和来表示，有多少种表示方法。
## 代码：
```c
class Solution {
public:
    int uniquePaths(int m, int n) {
        vector<vector<int>> path(m, vector<int>(n, 1));
        
        for(int i=1; i<m; i++){
            for(int j=1; j<n; j++){
                path[i][j] = path[i - 1][j] + path[i][j-1];
            }
        }
        return path[m-1][n-1];
    }
};
```

## 知识点：
1. 边界条件的确定：所有的值初始化为1，然后循环从i = 1, m = 1开始
2. 初始化二维vector的时候可以用`vector<vector<int>> path(m, vector<int>(n, 1))
`
