---
title: LeetCode31. 下一个排列
date: 2020-09-29 23:21:35
tags:
- LeetCode题解
- 数组
- 难度中等
---

实现获取下一个排列的函数，算法需要将给定数字序列重新排列成字典序中下一个更大的排列。

<!-- more -->

如果不存在下一个更大的排列，则将数字重新排列成最小的排列（即升序排列）。

必须原地修改，只允许使用额外常数空间。

以下是一些例子，输入位于左侧列，其相应输出位于右侧列。
1,2,3 → 1,3,2
3,2,1 → 1,2,3
1,1,5 → 1,5,1

**思路：**

从右往左找到第一个降序的数nums[numIndex]，然后与右边第一个大于它的数进行交换，然后对数组下标[numIndex+1，size()-1]逆序。如下图所示：

![](/img/leetcode31_1.png)

**代码：**

```cpp
class Solution {
public:

    void reverse(vector<int>& nums,int left,int right){
        while(left<right){
            int tmp=nums[left];
            nums[left]=nums[right];
            nums[right]=tmp;
            left++;
            right--;
        }
    }

    int findFirstDecresedNumber(vector<int>& nums){  //从右往左找到第一个降序的数
        int idx=nums.size()-1;
        while(idx>0&&nums[idx]<=nums[idx-1]){
            idx--;
        }
        return idx-1;
    }

    void nextPermutation(vector<int>& nums) {
        int numIndex=findFirstDecresedNumber(nums);
        if(numIndex==-1){
            reverse(nums,0,nums.size()-1);
        }else{
            int idx=numIndex+1;
            while(idx<nums.size()&&nums[idx]>nums[numIndex]){  //注意这里是大于，不是大于等于
                idx++;
            }
            int tmp=nums[numIndex];
            nums[numIndex]=nums[idx-1];
            nums[idx-1]=tmp;
            reverse(nums,numIndex+1,nums.size()-1);
        }
    }
};
```

