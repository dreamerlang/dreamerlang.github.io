---
title: LeetCode1025. 除数博弈
date: 2020-08-19 17:06:46
tags:
- LeetCode题解
- 动态规划
- 难度简单
---

爱丽丝和鲍勃一起玩游戏，他们轮流行动。爱丽丝先手开局。

<!-- more -->

最初，黑板上有一个数字 N 。在每个玩家的回合，玩家需要执行以下操作：

选出任一 x，满足 0 < x < N 且 N % x == 0 。
用 N - x 替换黑板上的数字 N 。
如果玩家无法执行这些操作，就会输掉游戏。

只有在爱丽丝在游戏中取得胜利时才返回 True，否则返回 False。假设两个玩家都以最佳状态参与游戏。

**示例 1：**

输入：2
输出：true
解释：爱丽丝选择 1，鲍勃无法进行操作。

**示例 2：**

输入：3
输出：false
解释：爱丽丝选择 1，鲍勃也选择 1，然后爱丽丝无法进行操作。


**提示：**

1 <= N <= 1000

该题有两种解法。

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

![](/img/leetcode1025.png)

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