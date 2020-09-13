---
title: LeetCode227. 基本计算器 II
date: 2020-08-24 14:47:13
tags:
- LeetCode题解
- 字符串
- 栈
---




实现一个基本的计算器来计算一个简单的字符串表达式的值。 

 

<!-- more -->

字符串表达式仅包含非负整数，`+`， `-` ，`*`，`/` 四种运算符和空格 ` `。 整数除法仅保留整数部分。 

**示例 1:**

输入: "3+2*2"
输出: 7

**示例 2：**

输入: " 3/2 "
输出: 1

**示例 3：**

输入: " 3+5 / 2 "
输出: 5

**说明：**

你可以假设所给定的表达式都是有效的。
请不要使用内置的库函数 eval。

**思路：**

一般的字符串运算是需要使用数字栈和符号栈的，但是因为这题只有+,-,/,*，遇到/和\*的时候就可以直接与后面一个数进行运算了，所以这里只需要设立一个标志位来记录待计算的符号。当该计算时，若上一个运算符是+或者-，直接进行一元运算入栈。遍历字符串结束后，再将栈中元素弹出求和即可。

**代码：**

```cpp
class Solution {
public:
    int calculate(string s) {
        stack<int>nums;
        int num=0;
        char lastsign='+';
        for(int i=0;i<s.size();i++){
            if(s[i]>='0'&&s[i]<='9'){
                num=num*10+(s[i]-'0'); //避免num溢出
            }
            if((s[i]<'0'||s[i]>'9')&&s[i]!=' '||i==s.size()-1){//计算，注意这里是if，不能用else if
                if(lastsign=='+'){  //即遇到运算符或者最后一个数字时计算
                    nums.push(num);
                }else if(lastsign=='-'){
                    nums.push(-num);
                }else if(lastsign=='*'||lastsign=='/'){
                    int top=nums.top();
                    nums.pop();
                    nums.push(lastsign=='*'?top*num:top/num);
                }
                lastsign=s[i]; //记录运算符，等待下一次计算
                num=0; //重新记录数字
            }  
        }
        int ans=0;
        while(!nums.empty()){
            ans+=nums.top();
            nums.pop();
        }
        return ans;

    }
};
```

