---
title: LeetCode55.跳跃游戏
date: 2020-09-13 19:22:29
tags:
- 贪心
- LeetCode题解
---

给定一个非负整数数组，你最初位于数组的第一个位置。数组中的每个元素代表你在该位置可以跳跃的最大长度。判断你是否能够到达最后一个位置。

<!-- more -->

**示例 1:**

输入: [2,3,1,1,4]
输出: true
解释: 我们可以先跳 1 步，从位置 0 到达 位置 1, 然后再从位置 1 跳 3 步到达最后一个位置。

**示例 2:**

输入: [3,2,1,0,4]
输出: false
解释: 无论怎样，你总会到达索引为 3 的位置。但该位置的最大跳跃长度是 0 ， 所以你永远不可能到达最后一个位置。



**解法1:**

**dfs**，就是按照正常的思路进行递归的试验，若找到一条到终点的路径就返回。不过这样的解法会超时，时间复杂度和空间复杂度都很高。

**代码：**

```cpp
class Solution {
public:
    int len=0;
    bool dfs(int idx,vector<int>& nums){
        if(idx==len-1){
            return true;
        }
        int max_step=nums[idx];//如果搜索到当前下标对应的值为0，则表示不能抵达最后一个数
        if(max_step==0){
            return false;
        }   
        for(int i=idx+1;i<=idx+max_step;i++){
            if(i>=len){
                break;
            }
            if(dfs(i,nums)){ //若找到一条路径，则直接返回
                return true;
            }
        }
        return false;
    }

    bool canJump(vector<int>& nums) {
        len=nums.size();
        return dfs(0,nums);
    }
};
```



**解法2:**

**贪心**，遍历一次数组，每次维护当前都能到达的最远下标。官方题解有个地方可以优化，就是当遍历的时候下标max_idx小于i的时候，表示不能到达i，就更不可能到达最后一个数了，可以直接返回false。该解法时间复杂度O(n)，空间复杂度O(1)。

**代码：**

```cpp
class Solution {
public:
    bool canJump(vector<int>& nums) {
        int max_idx=0;
        int size=nums.size();
        for(int i=0;i<size;i++){
            if(i>max_idx){ //提前终止
                return false;
            }
            max_idx=max(max_idx,i+nums[i]);
            if(max_idx>=size-1){
                return true;
            }
        }
        return false;
    }
};
```

