---
title: 557. 反转字符串中的单词 III
date: 2020-08-20 12:11:36
tags:
- LeetCode题解
- 字符串
---

给定一个字符串，你需要反转字符串中每个单词的字符顺序，同时仍保留空格和单词的初始顺序。 

<!-- more -->

示例 1:

输入: "Let's take LeetCode contest"
输出: "s'teL ekat edoCteeL tsetnoc" 
注意：在字符串中，每个单词由单个空格分隔，并且字符串中不会有任何额外的空格。



**方法一：**

利用stringstream将句子拆分为不含空格的单词，然后再利用algorithm头文件中的reverse函数对每个单词进行翻转。

```cpp
class Solution {
public:
    string reverseWords(string s) {
        stringstream ss(s);
        string x;
        string ans="";
        ss >> ans;
        reverse(ans.begin(),ans.end());
        while (ss >> x){
            ans=ans+" ";
            reverse(x.begin(),x.end());
            ans=ans+x;
        }
        return ans;
    }
};
```





**方法二：**

手写split函数与reverse函数。

```cpp
class Solution {
public:

    vector<string> split(string s){
        vector<string> strs;
        int len=s.size();
        int start=0;
        for(int i=0;i<len;i++){
            if(s[i]==' '){
                strs.push_back(s.substr(start,i-start));
                start=i+1;
            }
        }
        strs.push_back(s.substr(start));

        return strs;
        
    }

    string reverseStr(string s){
        char tmp;
        int i=0;
        int j=s.size()-1;
        for(int i=0;i<j;i++,j--){
            tmp=s[i];
            s[i]=s[j];
            s[j]=tmp;
        }
        return s;
    } 

    string reverseWords(string s) {
        if(s.size()==0){
            return s;
        }
         vector<string> strs=split(s);
         string ans=reverseStr(strs[0]);
         for(int i=1;i<strs.size();i++){
            ans=ans+" ";
            ans=ans+reverseStr(strs[i]);
         }
         return ans;
         

    }
};
```



