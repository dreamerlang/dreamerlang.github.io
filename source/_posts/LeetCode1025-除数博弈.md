---
title: LeetCode1025. 除数博弈
date: 2020-08-19 17:06:46
tags:
- LeetCode题解
- 动态规划
---

该题有两种解法。

<!-- more -->

**题目：**

![](http://qfax2tvfl.hn-bkt.clouddn.com/img/20200819170951.png)



**解法一：**
dp，仔细想易知dp[i]的结果是由dp[1]到dp[i-1]的结果来决定的，即如果dp[i]为true，那么dp[j]（0<j<i）必然为false，并且i和j满足i%(i-j)==0。

代码：

```cpp
class Solution {
public:
    int dp[1005];
    bool divisorGame(int N) {
        for(int i=2;i<=N;i++){
            for(int j=i-1;j>0;j--){
                int tmp=i-j;
                if(i%tmp==0&&dp[j]==false){
                    dp[i]=true;
                    break;
                }
            }
        }
        return dp[N];
    }
};
```
然后可以优化的是在遍历j的时候不需要都遍历完，因为当j<i/2时，i%tmp必然不能整除，所以有如下：

```cpp
class Solution {
public:
    int dp[1005];
    bool divisorGame(int N) {
        for(int i=2;i<=N;i++){
            for(int j=i-1;j>=i/2;j--){
                int tmp=i-j;
                if(i%tmp==0&&dp[j]==false){
                    dp[i]=true;
                    break;
                }
            }
        }
        return dp[N];
    }
};
```

**解法二：**
看的题解，直接上图

![](http://qfax2tvfl.hn-bkt.clouddn.com/img/20200819171237.png)

```cpp
class Solution {
public:
    bool divisorGame(int N) {
        return N % 2 == 0;
    }
};

作者：LeetCode-Solution
链接：https://leetcode-cn.com/problems/divisor-game/solution/chu-shu-bo-yi-by-leetcode-solution/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```