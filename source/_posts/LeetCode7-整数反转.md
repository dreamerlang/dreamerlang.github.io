---
title: 7. 整数反转
date: 2020-10-13 22:32:47
tags:
- LeetCode题解
- 难度简单
---

给出一个 32 位的有符号整数，你需要将这个整数中每位上的数字进行反转。

<!-- more -->

**示例 1:**

输入: 123
输出: 321

**示例 2:**

输入: -123
输出: -321

**示例 3:**

输入: 120
输出: 21

**注意:**

假设我们的环境只能存储得下 32 位的有符号整数，则其数值范围为 [−231,  231 − 1]。请根据这个假设，如果反转后整数溢出那么就返回 0。



我的代码：

```cpp
class Solution {
public:
    int reverse(int x) {
        int flag=1;
        if(x==0){
            return 0;
        }else if(x<0){
            flag=-1;
        }
        stack<int>st;
        x=abs(x);
        while(x!=0){
            st.push(x%10);
            x/=10;
        }
        long long reverse_num=0;
        int ws=0;
        while(!st.empty()){
            reverse_num+=st.top()*pow(10,ws);
            st.pop();
            ws++;
        }
        reverse_num=flag*reverse_num;
        if(reverse_num>INT_MAX||reverse_num<INT_MIN){
            return 0;
        }
        return reverse_num;
    }
};
```



别人的代码：

```java
    public int reverse(int x) {
        long n = 0;
        while(x != 0) {
            n = n*10 + x%10;
            x = x/10;
        }
        return (int)n==n? (int)n:0;
    }
```



...