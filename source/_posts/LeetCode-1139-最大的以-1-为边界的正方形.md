---
title: LeetCode 1139. 最大的以 1 为边界的正方形
date: 2020-08-19 21:53:00
tags:
- LeetCode题解
- 动态规划
---

给你一个由若干 `0` 和 `1` 组成的二维网格 `grid`，请你找出边界全部由 `1` 组成的最大 **正方形** 子网格，并返回该子网格中的元素数量。如果不存在，则返回 `0`。 

<!-- more -->

示例 1：

输入：grid = [[1,1,1],[1,0,1],[1,1,1]]
输出：9
示例 2：

输入：grid = [[1,1,0,0]]
输出：1


提示：

1 <= grid.length <= 100
1 <= grid[0].length <= 100
grid\[i][j] 为 0 或 1



**思路：**

这题跟221.最大正方形不一样，因为这里面的正方形是边都为1就行，所以不能按照最大的全为1的正方形的思路。正确思路应该是先求出以i,j为右下角的顶点的正方形，以i，j结束的该列的最长连续的1串之和与以i，j结束的该行的最长连续的1串之和，然后再根据求得的可能的正方形的边长，再去检验左下角和右上角的最长连续1串是否符合要求。说起来有点晦涩，但是根据代码就好懂了。



**代码：**

```cpp
class Solution {
public:
    int x[105][105];  //以i，j结束的该列的最长连续的1串之和,i从1开始
    int y[105][105];  //以i，j结束的该行的最长连续的1串之和,j从1开始
    int largest1BorderedSquare(vector<vector<int>>& grid) {
        int ans=0;
        int row=grid.size();
        int col=grid[0].size();
        for(int i=0;i<row;i++){
            for(int j=0;j<col;j++){
                if(grid[i][j]==1){
                    x[i+1][j+1]=x[i][j+1]+1;
                    y[i+1][j+1]=y[i+1][j]+1;
                }
                int len=min(x[i+1][j+1],y[i+1][j+1]);
                for(int k=len;k>=1;k--){
                    if(x[i+1][j+1-k+1]>=k&&y[i+1-k+1][j+1]>=k){  //注意这里是k，不要写成len了，还有是要大于等于k，而不是==k
                        ans=max(ans,k);
                        break;
                    }
                }
            }
        }
        return ans*ans;
    }
};
```



221.最大正方形：<https://leetcode-cn.com/problems/maximal-square/> 