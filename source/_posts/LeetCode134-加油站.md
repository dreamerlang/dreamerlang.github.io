---
title: LeetCode134. 加油站
date: 2020-09-18 23:56:11
tags:
- LeetCode题解
- 贪心
- 难度中等
---

在一条环路上有 N 个加油站，其中第 i 个加油站有汽油 gas[i] 升。你有一辆油箱容量无限的的汽车，从第 i 个加油站开往第 i+1 个加油站需要消耗汽油 cost[i] 升。你从其中的一个加油站出发，开始时油箱为空。如果你可以绕环路行驶一周，则返回出发时加油站的编号，否则返回 -1。

<!-- more -->

**说明:** 

如果题目有解，该答案即为唯一答案。
输入数组均为非空数组，且长度相同。
输入数组中的元素均为非负数。

**示例 1:**

**输入:** 
gas  = [1,2,3,4,5]
cost = [3,4,5,1,2]

**输出:** 3

**解释:**
从 3 号加油站(索引为 3 处)出发，可获得 4 升汽油。此时油箱有 = 0 + 4 = 4 升汽油
开往 4 号加油站，此时油箱有 4 - 1 + 5 = 8 升汽油
开往 0 号加油站，此时油箱有 8 - 2 + 1 = 7 升汽油
开往 1 号加油站，此时油箱有 7 - 3 + 2 = 6 升汽油
开往 2 号加油站，此时油箱有 6 - 4 + 3 = 5 升汽油
开往 3 号加油站，你需要消耗 5 升汽油，正好足够你返回到 3 号加油站。
因此，3 可为起始索引。

**示例 2:**

**输入:** 
gas  = [2,3,4]
cost = [3,4,3]

**输出:** -1

**解释:**
你不能从 0 号或 1 号加油站出发，因为没有足够的汽油可以让你行驶到下一个加油站。
我们从 2 号加油站出发，可以获得 4 升汽油。 此时油箱有 = 0 + 4 = 4 升汽油
开往 0 号加油站，此时油箱有 4 - 3 + 2 = 3 升汽油
开往 1 号加油站，此时油箱有 3 - 3 + 3 = 3 升汽油
你无法返回 2 号加油站，因为返程需要消耗 4 升汽油，但是你的油箱只有 3 升汽油。
因此，无论怎样，你都不可能绕环路行驶一周。



**方法一：**

首先车能够循环开一圈的充分条件是总的油量需要大于等于总的cost，所以关键的问题是怎么定位起点的问题，题目里说了答案是唯一解。所以肯定存在一个起点将左右两段路分割开来当然左边或者右边都有可能为空。

注：一个站的收益如果小于0，肯定不能作为起点；而连续的多个站也可以等效地看做一个站，如果其累积收益小于0，就跳过，寻找下一个。

**代码：**

```cpp
class Solution {
public:
    int canCompleteCircuit(vector<int>& gas, vector<int>& cost) {
        int sum=0;  //记录总的油量-总的cost
        int cur_sun=0;
        int start=0;
        for(int i=0;i<gas.size();i++){
            sum+=gas[i]-cost[i];
            cur_sun+=gas[i]-cost[i];
            if(cur_sun<0){ //若cur_sun<0，表示从0到i无法达到，于是从i+1开始
                start=i+1;
                cur_sun=0;
            }
        }
        return sum>=0?start:-1;
    }
};
```



**方法二：**

参考[采用画图的思想分析](https://leetcode-cn.com/problems/gas-station/solution/shi-yong-tu-de-si-xiang-fen-xi-gai-wen-ti-by-cyayc/)，采用数形结合的方式。具体如下图所示。依旧记录从索引idx开始的gas[i]-cost[i]的前缀和，然后从图可以看出，起点都在前缀和最小的那个位置的下一个位置。

![](/img/134.加油站_数形结合.jpeg)

**代码：**

```cpp
class Solution {
public:
    int canCompleteCircuit(vector<int>& gas, vector<int>& cost) {
        int sum=0;
        int min_remain=INT_MAX;
        int min_idx=0;
        for(int i=0;i<gas.size();i++){
            sum+=gas[i]-cost[i];
            if(sum<min_remain){
                min_idx=i;
                min_remain=sum;
            }
        }
        return sum>=0?((min_idx+1)%gas.size()):-1;
    }
};
```

