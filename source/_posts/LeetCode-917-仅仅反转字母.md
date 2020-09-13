---
title: LeetDode 917. 仅仅反转字母
date: 2020-08-22 15:12:34
tags:
- LeetCode题解
- 字符串
---






 给定一个字符串 `S`，返回 “反转后的” 字符串，其中不是字母的字符都保留在原地，而所有字母的位置发生反转。 

<!-- more -->

示例 1：

输入："ab-cd"
输出："dc-ba"
示例 2：

输入："a-bC-dEf-ghIj"
输出："j-Ih-gfE-dCba"
示例 3：

输入："Test1ng-Leet=code-Q!"
输出："Qedo1ct-eeLg=ntse-T!"


提示：

S.length <= 100
33 <= S[i].ASCIIcode <= 122 
S 中不包含 \ or "



解法一：

利用双指针法，分别从字符串首尾开始遍历，当i，j所对应的字符都为字母时进行交换。

代码：

```cpp
class Solution {
public:
    string reverseOnlyLetters(string S) {
        int i=0;
        int size=S.size();
        int j=size-1;
        while(i<j){
            while(i<size&&!(S[i]>='a'&&S[i]<='z'||S[i]>='A'&&S[i]<='Z')){
                i++;
            }
            while(j>=0&&!(S[j]>='a'&&S[j]<='z'||S[j]>='A'&&S[j]<='Z')){
                j--;
            }
            if(i<j&&i<size&&j>=0){
                char tmp=S[i];
                S[i]=S[j];
                S[j]=tmp;
            }
            i++;
            j--;
        }
        return S;


    }
};
```



解法二：

先遍历一次S，将其中的字母字符存入栈，然后再遍历一次字符串S，将栈顶元素取出并替换。

代码：

```cpp
class Solution {
public:
    string reverseOnlyLetters(string S) {
        stack<char>st;
        for(auto c:S){
            if(c>='a'&&c<='z'||c>='A'&&c<='Z'){
                st.push(c);
            }
        }
        for(int i=0;i<S.size();i++){
            if(S[i]>='a'&&S[i]<='z'||S[i]>='A'&&S[i]<='Z'){
                S[i]=st.top();
                st.pop();
            }
        }
        return S;
    }
};
```

