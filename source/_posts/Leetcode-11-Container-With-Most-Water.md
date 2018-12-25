---
title: Leetcode 11. Container With Most Water
date: 2018-12-23 21:39:47
tags: Leetcode
---

# 题目：
Given n non-negative integers a1, a2, ..., an , where each represents a point at coordinate (i, ai). n vertical lines are drawn such that the two endpoints of line i is at (i, ai) and (i, 0). Find two lines, which together with x-axis forms a container, such that the container contains the most water.

Note: You may not slant the container and n is at least 2.

![Alt](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/07/17/question_11.jpg)

# 例子：
Input: [1,8,6,2,5,4,8,3,7]
Output: 49

# 思路：
暴力，复杂度$n^2$
两边缩小到中间，每次都留下比较高的那边，缩短比较矮的那边。

# 代码（暴力）：
```c
class Solution {
public:
    int maxArea(vector<int>& height) {
        int max= 0;
        int tank_h, tank_w;
        for(int i = 0; i < height.size(); i++){
            
            for(int j=i+1; j<height.size(); j++){
                    
                tank_h = height[i]>height[j]? height[j]: height[i];
                tank_w = j-i;
                max = tank_h*tank_w > max? tank_h*tank_w:max;
                
            }
        }
        return max;
    }
};
```
# 代码（两点法）：




