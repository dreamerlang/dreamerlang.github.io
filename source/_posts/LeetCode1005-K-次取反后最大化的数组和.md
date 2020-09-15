---
title: LeetCode1005. K 次取反后最大化的数组和
date: 2020-09-15 09:34:11
tags:
- 贪心
- LeetCode题解
- 难度简单
---

给定一个整数数组 A，我们只能用以下方法修改该数组：我们选择某个索引 `i` 并将 A[i] 替换为 -A[i]，然后总共重复这个过程 `K` 次。（我们可以多次选择同一个索引 `i`。）

<!-- more -->

**思路：**

先对数组进行一次快排，然后从左到右在小于等于k的情况下，对所有负数进行翻转并求和，如果负数的个数小于k，那么对（k-负数个数）的值进行判断，如果（k-负数个数）%2==0，则可以随便对一个非负数进行翻转，效果不变；而如果（k-负数个数）%2==1，需要对绝对值最小的那个数进行翻转，使其成为负数并求和。而数组是排好序的，所以只需要比较A[idx-1]（负数）与A[idx]（此时为非负数）的绝对值大小。注意如果数组里的数全部>=0的时候，idx为0，所以此时尽需将A[idx]翻转求和即可。时间复杂度为O(nlogn)。

**代码：**

```cpp
class Solution {
public:
    int largestSumAfterKNegations(vector<int>& A, int K) {
        sort(A.begin(),A.end());
        int idx=0;
        int ans=0;
        for(;idx<A.size()&&K>0;idx++){
            if(A[idx]<0){
                K--;
                ans-=A[idx];
            }else{
                break;
            }
        }
        if(idx==A.size()){
            return ans;
        }
        if(K%2==0){  //剩下需要翻转的数为偶数
            ans+=A[idx];
        }else{
            if(idx==0){  //当排序后的第一个数就为非负数
                ans-=A[idx];
            }
            else{
                if(-A[idx-1]<=A[idx]){ //因为之前已经减A[idx-1]一次，现在需要将之前那次减法撤销，并翻转一次，所以是加2*A[idx-1]，注意还要再加A[idx]，因为后面的for循环是从idx+1开始的
                    ans=ans+2*A[idx-1]+A[idx];
                }else{
                    ans=ans-A[idx];
                }
            }
        }
        for(int i=idx+1;i<A.size();i++){
            ans+=A[i];
        }
        return ans;
    }
};
```



评论区里还有利用小顶堆来做的，不过没有我这个解法快，因为建立堆的过程的时间就是O(nlogn)了，后面要对小顶堆进行k次取出重建并插入的过程。不过代码比较简洁，也贴在下面



```cpp
class Solution {
public:
    int largestSumAfterKNegations(vector<int>& A, int K) {
        int ans = 0;
        priority_queue<int,vector<int>,greater<int> >mypq;
        for(auto&a:A)mypq.push(a);
        while(K--){
            int temp = -mypq.top();mypq.pop();
            mypq.push(temp);
        }
        while(!mypq.empty()){
            ans+= mypq.top();mypq.pop();
        }
        return ans;
    }
};
```

