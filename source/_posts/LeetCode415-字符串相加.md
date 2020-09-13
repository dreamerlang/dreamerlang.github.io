---
title: LeetCode415. 字符串相加
date: 2020-08-21 10:17:50
tags:
- LeetCode
- 字符串
---

 给定两个字符串形式的非负整数 `num1` 和`num2` ，计算它们的和。 

<!-- more -->

**提示：**

num1 和num2 的长度都小于 5100
num1 和num2 都只包含数字 0-9
num1 和num2 都不包含任何前导零
你不能使用任何內建 BigInteger 库， 也不能直接将输入的字符串转换为整数形式



模拟整数的加法就行了，我写的时候在长度短的字符串添加了前置0。

```cpp
class Solution {
public:
    string addStrings(string num1, string num2) {
        int len1=num1.size();
        int len2=num2.size();
        int len=max(len1,len2);
        string leadingz="";
        if(len1>len2){
            for(int i=0;i<len1-len2;i++){
                leadingz+="0";
            }
            num2=leadingz+num2;
        }
        else if(len1<len2){
            for(int i=0;i<len2-len1;i++){
                leadingz+="0";
            }
            num1=leadingz+num1;            
        }
        int jw=0;  //表示进位
        for(int i=len-1;i>=0;i--){
            int sum=num1[i]-'0'+num2[i]-'0'+jw;
            if(sum>=10){
                jw=1;
            }else{
                jw=0;
            }
            sum=sum%10;
            num1[i]=sum+'0';
        }
        if(jw){
            num1="1"+num1;
        }
        return num1;
    }
};
```



看了评论后有更简洁的写法，代码如下：

```cpp
class Solution {
public:
    string addStrings(string num1, string num2) {
        string str;
        int cur = 0, i = num1.size()-1, j = num2.size()-1;
        while (i >= 0 || j >= 0 || cur != 0) {
            if (i >= 0) cur += num1[i--] - '0';
            if (j >= 0) cur += num2[j--] - '0';
            str = to_string (cur % 10)+str;
            cur /= 10;
        }
        return str;
    }
};
```

