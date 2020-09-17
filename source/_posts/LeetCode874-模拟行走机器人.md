---
title: LeetCode874. 模拟行走机器人
date: 2020-09-09 00:22:57
tags:
- LeetCode题解
- 模拟
- 难度简单
---

 机器人在一个无限大小的网格上行走，从点 (0, 0) 处开始出发，面向北方。

<!-- more -->

该机器人可以接收以下**三种类型**的命令：

-2：向左转 90 度
-1：向右转 90 度
1 <= x <= 9：向前移动 x 个单位长度
在网格上有一些格子被视为障碍物。

第 i 个障碍物位于网格点  (obstacles\[i][0], obstacles\[i][1])

机器人无法走到障碍物上，它将会停留在障碍物的前一个网格方块上，但仍然可以继续该路线的其余部分。

返回从原点到机器人所有经过的路径点（坐标为整数）的最大欧式距离的平方。

**示例 1：**

输入: commands = [4,-1,3], obstacles = []
输出: 25
解释: 机器人将会到达 (3, 4)

**示例 2：**

输入: commands = [4,-1,4,-2,4], obstacles = [[2,4]]
输出: 65
解释: 机器人在左转走到 (1, 8) 之前将被困在 (1, 4) 处

**提示：**

0 <= commands.length <= 10000
0 <= obstacles.length <= 10000
-30000 <= obstacle\[i][0] <= 30000
-30000 <= obstacle\[i][1] <= 30000
答案保证小于 2 ^ 31



**思路：**

记录机器人的当前方向和位置信息，然后对指令进行解析就行。具体见代码注释。

**代码：**

```cpp
class Solution {
public:
    int robotSim(vector<int>& commands, vector<vector<int>>& obstacles) {
        unordered_set<string> obs_set;
        for (auto obs : obstacles) {
            obs_set.insert(to_string(obs[0]) + "," + to_string(obs[1]));
        }
        //顺时针，初始向北即为{0,1}
        int dir[4][2] = { {0,1},{1,0},{0,-1},{-1,0} };
        //当前方向，即dir的一维下标
        int now_dir = 0;
        int ans = 0;
        int x = 0, y = 0;
        for (auto cmd : commands) {
            if (cmd == -1) {
                if (now_dir == 3) {
                    now_dir = 0;
                }
                else {
                    now_dir++;
                }
            }
            else if (cmd == -2) {
                if (now_dir == 0) {
                    now_dir = 3;
                }
                else {
                    now_dir--;
                }
            }
            else {
                for (int i = 0; i < cmd; i++) {
                    int new_x = x + dir[now_dir][0];
                    int new_y = y + dir[now_dir][1];
                    if(obs_set.find(to_string(new_x) + "," + to_string(new_y))!=obs_set.end()){ //如果在hashset中，则当前这个cmd无法继续执行
                        break;
                    }
                    x=new_x;
                    y=new_y;
                    ans = max(ans, x*x + y * y);
                }
            }
        }
        return ans;
    }
};

```

**续：**

最开始我的版本是这样的，即在做hashset的时候没有将obs[0]先转换为string再与","进行拼接，所以导致在LeetCode在线页面上编译不通过，但是我在vs2017上，是能够编译运行下面这份代码的。但是代码运行的结果并不正确，当测试用例为实例2的时候，运行结果是17，（目前还没有找到原因）。从测试结果来看，int+string的拼接并不会自动转换，结果是一个空串？测试结果：

```cpp
	int a = 1, b = 2;	
	//test1
	string s = a + ",";
	if (s == "") {
		cout << "yes" << endl;
	}
	cout << s << endl;
	//test2
	string ss = a + ","+ b;
	if (ss == "") {
		cout << "yes" << endl;
	}
	cout << ss << endl;
```

**结果：**

![](/img/leetcode874.png)



**错误的代码：**

```cpp
class Solution {
public:
    int robotSim(vector<int>& commands, vector<vector<int>>& obstacles) {
        unordered_set<string> obs_set;
        for (auto obs : obstacles) {
            obs_set.insert(obs[0] + "," + obs[1]);
        }
        //顺时针，初始向北即为{0,1}
        int dir[4][2] = { {0,1},{1,0},{0,-1},{-1,0} };
        //当前方向，即dir的一维下标
        int now_dir = 0;
        int ans = 0;
        int x = 0, y = 0;
        for (auto cmd : commands) {
            if (cmd == -1) {
                if (now_dir == 3) {
                    now_dir = 0;
                }
                else {
                    now_dir++;
                }
            }
            else if (cmd == -2) {
                if (now_dir == 0) {
                    now_dir = 3;
                }
                else {
                    now_dir--;
                }
            }
            else {
                for (int i = 0; i < cmd; i++) {
                    int new_x = x + dir[now_dir][0];
                    int new_y = y + dir[now_dir][1];
                    if(obs_set.find(new_x + "," + new_y)!=obs_set.end()){ //如果在hashset中，则当前这个cmd无法继续执行
                        break;
                    }
                    x=new_x;
                    y=new_y;
                    ans = max(ans, x*x + y * y);
                }
            }
        }
        return ans;
    }
};

```

**总结：**

c++里千万**不要直接将非string类型直接与string类型进行拼接**，否则会有意想不到的效果。（md找个错误找了一个多小时，官方给的题解也是不能编译通过的，辣鸡）。正常做法还是要先用to_string方法转为string。