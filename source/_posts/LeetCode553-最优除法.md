---
title: LeetCode553. 最优除法
date: 2020-08-29 17:19:12
tags:
- LeetCode
- 字符串
- 回溯
---




给定一组**正整数，**相邻的整数之间将会进行浮点除法操作。例如， [2,3,4] -> 2 / 3 / 4 。 

<!-- more -->

但是，你可以在任意位置添加任意数目的括号，来改变算数的优先级。你需要找出怎么添加括号，才能得到最大的结果，并且返回相应的字符串格式的表达式。你的表达式不应该含有冗余的括号。

**示例：**

输入: [1000,100,10,2]
输出: "1000/(100/10/2)"

**解释:**
1000/(100/10/2) = 1000/((100/10)/2) = 200
但是，以下加粗的括号 "1000/((100/10)/2)" 是冗余的，
因为他们并不影响操作的优先级，所以你需要返回 "1000/(100/10/2)"。

**其他用例:**
1000/(100/10)/2 = 50
1000/(100/(10/2)) = 50
1000/100/10/2 = 0.5
1000/100/(10/2) = 2

**说明:**

输入数组的长度在 [1, 10] 之间。
数组中每个元素的大小都在 [2, 1000] 之间。
每个测试用例只有一个最优除法解。



**解法一：**

因为a，b，c，d都为正整数（即大于等于1）简单推导可知max(a/b/c/d/e)=a/(b/c/d/e)=a/b\*c\*d*e。所以即第一个数后面的所有数加上括号即可，注意数组长度为1和2时无需加括号。

**代码：**

```cpp
class Solution {
public:
    string optimalDivision(vector<int>& nums) {
        int size=nums.size();
        if(size==1){
            return to_string(nums[0]);
        }
        if(size==2){
            return to_string(nums[0])+"/"+to_string(nums[1]);
        }
        string ans=to_string(nums[0])+"/(";
        for(int i=1;i<size-1;i++){
            ans=ans+to_string(nums[i])+"/";
        }
        return ans+to_string(nums[size-1])+")";

    }
};
```



**解法二：回溯**

这题的暴力方法是将列表分成 left和 right 两部分并分别对它们调用函数。我们从 start 到 end 遍历 i并得到 left=(start,i)和 right=(i+1,end)。

left 和 right 分别返回它们对应部分的最大和最小值和它们对应的字符串。

最小值可以通过左边部分的最小值除以右边部分的最大值得到，也就是 minVal=left.min/right.maxminVal=left.min/right.max。

类似的，最大值可以通过左边部分的最大值除以右边部分的最小值得到，也就是 maxVal=left.max/right.minmaxVal=left.max/right.min。

现在，怎么添加括号呢？由于出发运算是从左到右的，也就是最左边的除法默认先执行，所以我们不需要给左边部分添加括号，但我们需要给右边部分添加括号。

比方假设左边部分是 "2" ，右边部分是 "3/4"，那么结果字符串 "2/(3/4)" 对应的是 左边部分+"/"+"("+右边部分+")"。

还有一点，如果右边部分只有一个数字，我们也不需要添加括号。

也就是说，如果左边部分是 "2" 且右边部分是 "3" （只包含单个数字），那么答案应该是 "2/3" 而不是 "2/(3)"。

```java
public class Solution {
    public String optimalDivision(int[] nums) {
        T t = optimal(nums, 0, nums.length - 1, "");
        return t.max_str;
    }
    class T {
        float max_val, min_val;
        String min_str, max_str;
    }
    public T optimal(int[] nums, int start, int end, String res) {
        T t = new T();
        if (start == end) {
            t.max_val = nums[start];
            t.min_val = nums[start];
            t.min_str = "" + nums[start];
            t.max_str = "" + nums[start];
            return t;
        }
        t.min_val = Float.MAX_VALUE;
        t.max_val = Float.MIN_VALUE;
        t.min_str = t.max_str = "";
        for (int i = start; i < end; i++) {
            T left = optimal(nums, start, i, "");
            T right = optimal(nums, i + 1, end, "");
            if (t.min_val > left.min_val / right.max_val) {
                t.min_val = left.min_val / right.max_val;
                t.min_str = left.min_str + "/" + (i + 1 != end ? "(" : "") + right.max_str + (i + 1 != end ? ")" : "");
            }
            if (t.max_val < left.max_val / right.min_val) {
                t.max_val = left.max_val / right.min_val;
                t.max_str = left.max_str + "/" + (i + 1 != end ? "(" : "") + right.min_str + (i + 1 != end ? ")" : "");
            }
        }
        return t;
    }
}

作者：LeetCode
链接：https://leetcode-cn.com/problems/optimal-division/solution/zui-you-chu-fa-by-leetcode/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

复杂度分析

时间复杂度： O(n!)。添加了括号以后所有表达式的数目为 O(n!)，其中 nn 是列表中元素的数目。

空间复杂度： O(n^2)。递归树的深度为 O(n) 且每一个节点都最多包含 O(n) 长度的字符串。

作者：LeetCode
链接：https://leetcode-cn.com/problems/optimal-division/solution/zui-you-chu-fa-by-leetcode/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。