---
title: LeetCode522. 最长特殊序列 II
date: 2020-08-28 11:47:34
tags:
- 字符串
- LeetCode
---




给定字符串列表，你需要从它们中找出最长的特殊序列。最长特殊序列定义如下：该序列为某字符串独有的最长子序列（即不能是其他字符串的子序列）。 

<!-- more -->

子序列可以通过删去字符串中的某些字符实现，但不能改变剩余字符的相对顺序。空序列为所有字符串的子序列，任何字符串为其自身的子序列。

输入将是一个字符串列表，输出是最长特殊序列的长度。如果最长特殊序列不存在，返回 -1 。

**示例：**

输入: "aba", "cdc", "eae"
输出: 3



输入: "aaa", "aaa", "aa"
输出: -1




**提示：**

所有给定的字符串长度不会超过 10 。
给定字符串列表的长度将在 [2, 50 ] 之间。



**思路：**

可以先对字符串数组按长度从大到小进行排序，然后遍历每个字符串，判断该字符串是否为其他字符串的子序列，如果都不是，则返回该字符串的长度，如果不存在这样的字符串，则返回-1。这里在判断该字符串是否为其他字符串的子序列时可以进行剪枝，具体见代码。



**代码：**

```cpp
class Solution {
public:
    static bool cmp_func(string &a, string &b)  //注意加上static
    {
        if(a.size() > b.size())
            return true;
        return false;
    }

    bool judge(string &a, string &b){ //判断a是否为b的子序列
        int lena=a.size();
        int lenb=b.size();
        int straIndex = 0;
        for(int strbIndex=0;straIndex<lena&&strbIndex<lenb;strbIndex++){
            if(a[straIndex]==b[strbIndex]){
                straIndex++;
            }
        }
        return straIndex==lena;

    }

    int findLUSlength(vector<string>& strs) {
        sort(strs.begin(),strs.end(),cmp_func);
        int size=strs.size();
        for(int i=0;i<size;i++){
            bool flag=true;
            for(int j=0;j<size;j++){
                if(strs[j].size()<strs[i].size()){  //剪枝，如果strs[j]的长度小于strs[i]，那么strs[i]肯定不会是strs[j]的子序列
                    break;
                }
                if(i==j){   //跳过自身
                    continue;
                }
                if(judge(strs[i],strs[j])){
                    flag=false;
                    break; 
                }
            }
            if(flag){
                return strs[i].size();
            }
        }
        return -1;
    }
};
```



题目链接：<https://leetcode-cn.com/problems/longest-uncommon-subsequence-ii/> 




