---
title: LeetCode1221. 分割平衡字符串
date: 2020-09-21 00:27:07
tags:
- LeetCode题解
- 贪心算法
- 难度简单
- 字符串
---

在一个「平衡字符串」中，'L' 和 'R' 字符的数量是相同的。给出一个平衡字符串 s，请你将它分割成尽可能多的平衡字符串。返回可以通过分割得到的平衡字符串的最大数量。

<!-- more -->

**示例 1：**

输入：s = "RLRRLLRLRL"
输出：4
解释：s 可以分割为 "RL", "RRLL", "RL", "RL", 每个子字符串中都包含相同数量的 'L' 和 'R'。

**示例 2：**

输入：s = "RLLLLRRRLR"
输出：3
解释：s 可以分割为 "RL", "LLLRRR", "LR", 每个子字符串中都包含相同数量的 'L' 和 'R'。

**示例 3：**

输入：s = "LLLLRRRR"
输出：1
解释：s 只能保持原样 "LLLLRRRR".

**提示：**

1 <= s.length <= 1000
s[i] = 'L' 或 'R'
分割得到的每个字符串都必须是平衡字符串。



**思路：**

遍历字符串，记录R和L的个数，当r_cnt==l_cnt时进行分割即可。

**代码：**

```cpp
class Solution {
public:
    int balancedStringSplit(string s) {
        int cnt=0;
        int l_cnt=0;
        int r_cnt=0;
        for(auto c:s){
            if(c=='R'){
                r_cnt++;
            }else{
                l_cnt++;
            }
            if(r_cnt==l_cnt){
                cnt++;
                r_cnt=0;
                l_cnt=0;
            }
        }
        return cnt;
    }
};
```

评论区里有个只需要两个额外变量的代码，贴在下面：

```cpp
class Solution {
public:
    int balancedStringSplit(string s) {
        int cnt=0;
        int ans=0;
        for(auto c:s){
            if(c=='R'){
                cnt++;
            }else{
                cnt--;
            }
            if(cnt==0){
                ans++;
            }
        }
        return ans;
    }
};
```

