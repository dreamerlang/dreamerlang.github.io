---
title: LeetCode402. 移掉K位数字
date: 2020-09-13 19:19:33
tags:
- 贪心
- 字符串
- LeetCode题解
---

 给定一个以字符串表示的非负整数 **num**，移除这个数中的 **k** 位数字，使得剩下的数字最小。

<!-- more -->

**注意:**

num 的长度小于 10002 且 ≥ k。
num 不会包含任何前导零。

**示例 1 :**

输入: num = "1432219", k = 3
输出: "1219"
解释: 移除掉三个数字 4, 3, 和 2 形成一个新的最小的数字 1219。
**示例 2 :**

输入: num = "10200", k = 1
输出: "200"
解释: 移掉首位的 1 剩下的数字为 200. 注意输出不能有任何前导零。

**示例 3 :**

输入: num = "10", k = 2
输出: "0"
解释: 从原数字移除所有的数字，剩余为空就是0。



**解法1:**

总的思路就是贪心，从左往右对字符串进行遍历，找到第一个递减的数字，然后将其删除。最简单的做法就是每次都从头遍历新删除后的字符串。这样时间复杂度为O(k*num.size())。

**代码：**

```cpp
class Solution {
public:
    string removeKdigits(string num, int k) {
        int len=num.size();
        if(len==k){
            return "0";
        }
        for(int i=0;i<k;i++){
            int flag=1;//flag标示本次遍历是否删除，若遍历后flag为1，表示该串是非递减串，应该删除最后一个数字
            for(int j=0;j<num.size()-1;j++){
                if(num[j]>num[j+1]){
                    num=num.substr(0,j)+num.substr(j+1);
                    flag=0;
                    break;
                }

            }
            if(flag){
                num=num.substr(0,num.size()-1);
            }
        }
        int idx=0;
        while(idx<num.size()&&num[idx]=='0'){ //删除前导0
            idx++;
        }
        if(idx==num.size()){
            return "0";
        }
        return num.substr(idx);
    }
};

```



**解法2:**

还是贪心，无需每一次都从开始对字符串进行遍历，记录下一次应该开始遍历的idx即可。时间复杂度为O(num.size())？

**代码：**

```cpp
class Solution {
public:
    string removeKdigits(string num, int k) {
        int len=num.size();
        if(len==k){
            return "0";
        }
        int idx=0;
        int i=0; //标示遍历过程中删除的数字个数
        for(;i<k;i++){
            while(idx<num.size()-1&&num[idx]<=num[idx+1]){
                idx++;
            }
            if(idx==num.size()-1){
                break;
            }
            num=num.substr(0,idx)+num.substr(idx+1);
            if(idx>0){  //注意，有可能删除的是第一个数字，所以要有这个判断
                idx--;
            }
            
        }
        num=num.substr(0,num.size()-(k-i));//此时字符串为非递减串，直接删除最后几个数即可
        idx=0;
        while(idx<num.size()&&num[idx]=='0'){//删除前导0
            idx++;
        }
        if(idx==num.size()){
            return "0";
        }
        return num.substr(idx);
    }
};
```



注：去年南大夏令营机试的第一题，当时用的第一种解法，没有考虑前导0，以至于第一题只有80分（不然100分就学硕了哎）。