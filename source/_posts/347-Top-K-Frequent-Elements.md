---
title: 347. Top K Frequent Elements
date: 2018-12-14 04:12:12
tags:
---
## 题目：
Given a non-empty array of integers, return the k most frequent elements.
## 例子：
Input: nums = [1,1,1,2,2,3], k = 2
Output: [1,2]
Input: nums = [1], k = 1
Output: [1]
## 注意：
- You may assume k is always valid, 1 ≤ k ≤ number of unique elements.
- Your algorithm's time complexity must be better than O(n log n), where n is the array's size.
## 思路：
统计出现次数就可以想到哈希表，但是这里不要忽略了排序的问题，这个就是个哈希+排序。这里其实都是整数，所以可以用线性排序，比如桶排序。
## 代码
```c
class Solution {
public:
    vector<int> topKFrequent(vector<int>& nums, int k) {
        unordered_map<int, int> counts;
        for(auto num:nums){
            counts[num]++;
        }
        priority_queue<int, vector<int>, greater<int>> heap;
        for(auto count:counts){
            heap.push(count.second);
            if(heap.size() > k){
                heap.pop();
            }
        }
        vector<int> ret;
        for(auto i:counts){
            if(i.second >= heap.top()){
                ret.push_back(i.first);
            }
        }
        return ret;
    }
};
```
## 知识点：
1. C++自己带的heap，在库`<queue>`里，[Doc](https://en.cppreference.com/w/cpp/container/priority_queue)
2. 复习了`unordered_map<type, type>`，用iterator时候，索引为`i.first`，数值为`i.second`。
3. 散列表复杂度$n$，二叉堆插入元素复杂度是$log(k)$，$k$为堆的高度。插入了$n$个元素，所以总的复杂度是$nlog(k)$。
4. 堆的高度只需要$k$，如果堆高于$k$了，就扔掉最后的元素。
5. c++的iterator：`for(auto i:nums)`
