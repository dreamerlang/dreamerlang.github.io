---
title: LeetCode16. 最接近的三数之和
date: 2020-09-23 00:25:28
tags:
- 数组
- 双指针
- 难度中等
---

给定一个包括 n 个整数的数组 nums 和 一个目标值 target。找出 nums 中的三个整数，使得它们的和与 target 最接近。返回这三个数的和。假定每组输入只存在唯一答案。 

<!-- more -->

示例：

输入：nums = [-1,2,1,-4], target = 1
输出：2
解释：与 target 最接近的和是 2 (-1 + 2 + 1 = 2) 。


提示：

- 3 <= nums.length <= 10^3
- -10^3 <= nums[i] <= 10^3
- -10^4 <= target <= 10^4



**思路：**

这题跟[Leetcode15.三数之和](https://leetcode-cn.com/problems/3sum/)的思路是一模一样的，首先对数组进行排序，先固定一个数nums[k]，然后利用双指针i,j在区间[k+1,nums.size()-1]区间遍历，根据每次nums[k]+nums[i]+nums[j]的和改变i，j的位置。两重循环，时间复杂度为O(n^2)。

**代码：**

```cpp
class Solution {
public:
    int threeSumClosest(vector<int>& nums, int target) {    
        int ans= 10e8;
        sort(nums.begin(), nums.end());
        for (int k = 0; k < nums.size(); ++k) {
            if (k > 0 && nums[k] == nums[k - 1]) continue; //优化，避免k索引对应的数重复遍历
            int i = k + 1, j = nums.size() - 1;
            while (i < j) {
                int tmp_sum=nums[k]+nums[i] + nums[j];
                if (tmp_sum == target) {
                    return target;
                } 
                if(abs(tmp_sum-target)<abs(ans-target)){
                    ans=tmp_sum;
                }
                if (tmp_sum < target){
                    while(i<j&&nums[i]==nums[i+1]){//优化，避免i索引对应的数重复遍历
                        i++;
                    }
                    i++;                   
                }
                else{
                    while(i<j&&nums[j]==nums[j-1]){//优化，避免j索引对应的数重复遍历
                        j--;
                    }
                    j--;
                }
            }
        }
        return ans;
    }
};
```

