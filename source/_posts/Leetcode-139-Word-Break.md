---
title: Leetcode 139. Word Break
date: 2018-12-10 06:42:08
tags:
---
## Description:
- Given a non-empty string s and a dictionary wordDict containing a list of non-empty words, determine if s can be segmented into a space-separated sequence of one or more dictionary words.

## Note:

 - The same word in the dictionary may be reused multiple times in the segmentation.
  - You may assume the dictionary does not contain duplicate words.

### Example 1:

Input: s = "leetcode", wordDict = ["leet", "code"]
Output: true
Explanation: Return true because "leetcode" can be segmented as "leet code".

### Example 2:

Input: s = "applepenapple", wordDict = ["apple", "pen"]
Output: true
Explanation: Return true because "applepenapple" can be segmented as "apple pen apple".
             Note that you are allowed to reuse a dictionary word.

### Example 3:

Input: s = "catsandog", wordDict = ["cats", "dog", "sand", "and", "cat"]
Output: false

## 思路：
最简单的思路是，检查`s`的所有子字符串是否在`dict`中，如果不在就跳过检查下一个，如果在就检查剩下的部分`s'`的所有子字符串是否在`dict`中，因此，每一个分割后的小问题之间互相包含，实际上这个题和《算法导论》中讲解动态规划的切木头的问题如出一辙，是典型的动态规划问题。
动态规划的核心思想在于“将已经解决的小问题记录在table里”，这个题中，需要记录的是数组的哪一部分已经满足`Dict`中的字符串组合，当检验未知部分时，可以跳过这些已知部分的检验。
## 代码：
```c++
class Solution {
public:
    bool wordBreak(string s, vector<string>& wordDict) 
    {
     vector<bool>  dp_record(s.size() + 1, false);
        dp_record[0] = true;
        
        for(int i=1; i<=s.size(); i++)
        {
            for(int j=i - 1; j>=0; j--)
            {
                if(dp_record[j])
                {
                    string word_to_check = s.substr(j, i-j);
                    if(one_dimensional_search(wordDict, word_to_check))
                    {
                        dp_record[i] = true;
                        break;
                        
                    }
                }
            }    
         
        }
        return dp_record[s.size()];
    }
        bool one_dimensional_search(vector<string> dict, string word)
    {
        for(int i = 0; i<dict.size(); i++){
            if (dict[i] == word){
                return true;
            }
        }
        return false;
        
    }

    

};
```
## 知识点：
1. `s.substr()`是`string`类的方法，截取从起点`pos`开始的长度`n`的子数组。
2. `vector`声明的时候初始化:
```c++
vector<typename> variable_name(num_element, element_value)
```
3. 记录结果的数组比要检测的数组多一个元素，因为需要初始化位置有一个true才能开始第一轮判断。
