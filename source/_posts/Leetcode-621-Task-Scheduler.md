---
title: Leetcode 621. Task Scheduler
date: 2018-12-14 05:46:24
tags:
---
## 题目：
Given a char array representing tasks CPU need to do. It contains capital letters A to Z where different letters represent different tasks.Tasks could be done without original order. Each task could be done in one interval. For each interval, CPU could finish one task or just be idle.

However, there is a non-negative cooling interval n that means between two same tasks, there must be at least n intervals that CPU are doing different tasks or just be idle.

You need to return the least number of intervals the CPU will take to finish all the given tasks.
## 例子：
Input: tasks = ["A","A","A","B","B","B"], n = 2
Output: 8
Explanation: A -> B -> idle -> A -> B -> idle -> A -> B.
## 思路：
出现最多的任务决定了总的时间，标准答案的第三种思路最直接：![在这里插入图片描述](https://leetcode.com/problems/task-scheduler/Figures/621_Task_Scheduler_new.PNG)
## 代码（抄的）：
```c
class Solution {
public:
    int leastInterval(vector<char>& tasks, int n) {
        unordered_map<char,int>mp;
        int count = 0;
        for(auto e : tasks)
        {
            mp[e]++;
            count = max(count, mp[e]);
        }
        
        int ans = (count-1)*(n+1);
        for(auto e : mp) if(e.second == count) ans++;
        return max((int)tasks.size(), ans);
    }
};
```
## 知识点：
1. 如果要用max，只能比较两个数
2. 如果要用priority_queue，插入用push，弹出堆顶用top
