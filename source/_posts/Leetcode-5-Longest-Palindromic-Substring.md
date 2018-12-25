---
title: Leetcode-5. Longest Palindromic Substring
date: 2018-12-22 23:59:23
tags:
---

# 题目要求:
Given a string s, find the longest palindromic substring in s. You may assume that the maximum length of s is 1000.
# 例子:
Input: "babad"
Output: "bab"
Note: "aba" is also a valid answer.

Input: "cbbd"
Output: "bb"

# 思路：
答案里面两个比较常规的思路，第一个是把字符串倒过来，然后变成了找最大公共子数组的问题，但是要注意的是，如果字符串中恰好有两个子数组是互为倒序，也会被当成回文数列，要多加一个条件检验一下检测出的数组首末是否相同。第二个思路是直接应用动态规划，判断i,j之间的子数组是不是回文，就要判断i+1,j-1之间的子数组是不是回文，因此可以构造一个2维表格。需要注意的是，迭代的过程中，不能按照一般的顺序循环表格，要先循环2个字符的数组，再循环3个字符的数组。

# 代码：
```c
class Solution {
public:
    string longestPalindrome(string s) {
        const int len_s = s.length();
        vector<vector<bool>> table;
        int max_palind=1;
        int start=0, end=0;
        table.resize(len_s);
        for(int i = 0; i< len_s; i++){
            table[i].resize(len_s);
            table[i][i] = true;
            table[i][i+1] = s[i] == s[i+1];
            if(table[i][i+1]){
                max_palind = 2;
                start = i;
                end = i+1;
            }
        }

        for(int i=0; i<len_s; i++){
            for(int j=i+2; j<len_s; j++){
                table[i][j] = (table[i+1][j-1] && s[i]==s[j]);
                if(table[i][j] == true){
                    if(j-i+1>max_palind){
                        max_palind = j-i+1;
                        start = i;
                        end=j;
                    }
                }
            }
        }
        return s.substr(start, max_palind);
        
        
    }
};
```

