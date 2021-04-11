---
title: LeetCode46&47 全排列
date: 2021-04-11 14:48:59
tags:
- LeetCode题解
- 回溯
- dfs
---

**LeetCode.46**

给定一个 **没有重复** 数字的序列，返回其所有可能的全排列。

<!-- more -->

示例:

输入: [1,2,3]
输出:
[
  [1,2,3],
  [1,3,2],
  [2,1,3],
  [2,3,1],
  [3,1,2],
  [3,2,1]
]

**思路：**

正常的dfs，利用一个vis数组记录原数组里某个数是否被使用，递归结束的条件是tmp数组里的个数已经达到原数组的个数。

这里附上评论区里的常用回溯orDFS代码模板：

```python
result = [] 
def backtrack(路径, 选择列表):     
	if 满足结束条件:         
		result.add(路径)         
		return
  if 满足其他剪枝条件:
    return
	for 选择 in 选择列表:         
		做选择         
		backtrack(路径, 选择列表)         
		撤销选择
```



**代码：**

```cpp
class Solution {
public:
    vector<vector<int>> ans;
    void dfs(vector<int>& nums,vector<int>& vis,vector<int>& tmp){
        if(tmp.size()==nums.size()){
            ans.push_back(tmp);
            return;
        }
        for(int i=0;i<nums.size();i++){
            if(!vis[i]){
                tmp.push_back(nums[i]);
                vis[i]=1;
                dfs(nums,vis,tmp);
                tmp.pop_back();
                vis[i]=0;
            }
        }
    }
    vector<vector<int>> permute(vector<int>& nums) {
        int size=nums.size();
        vector<int>vis(size,0);
        vector<int>tmp;
        dfs(nums,vis,tmp);
        return ans;
    }
};
```



**LeetCode.47**

给定一个可包含重复数字的序列 `nums` ，**按任意顺序** 返回所有不重复的全排列。

**示例 1：**

输入：nums = [1,1,2]

输出：
[[1,1,2],
 [1,2,1],
 [2,1,1]]

**示例 2：**

输入：nums = [1,2,3]

输出：[[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]

**提示：**

- `1 <= nums.length <= 8`
- `-10 <= nums[i] <= 10`

**思路：**

跟46题不一样的地方是需要再额外判断：回溯时，在添加第i个数时，之前是否添加过相同的数字，即for循环里是否有重复挑选相同数字。注意事先对数组进行排序。（相对于46，其实就多了个**排序**加**额外判断**）

**代码：**

```cpp
class Solution {
public:
    vector<vector<int>> ans;
    void dfs(vector<int>& nums,vector<int>& vis,vector<int>& tmp){
        if(tmp.size()==nums.size()){
            ans.push_back(tmp);
            return;
        }
        for(int i=0;i<nums.size();i++){
            if(vis[i]){
                continue;
            }
            if(i>0&&nums[i]==nums[i-1]&&!vis[i-1]){ //！vis[i-1]表示在for循环里本次i之前已经选择过相同的数字了（因为vis[i-1]为重置为0）
                continue;
            }
            tmp.push_back(nums[i]);
            vis[i]=1;
            dfs(nums,vis,tmp);
            tmp.pop_back();
            vis[i]=0;
        }
    }
    vector<vector<int>> permuteUnique(vector<int>& nums) {
        int size=nums.size();
        vector<int>vis(size,0);
        sort(nums.begin(),nums.end());  //排序 
        vector<int>tmp;
        dfs(nums,vis,tmp);
        return ans;
    }
};
```



