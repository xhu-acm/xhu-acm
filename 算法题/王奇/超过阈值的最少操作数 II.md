[超过阈值的最少操作数](https://leetcode.cn/problems/minimum-operations-to-exceed-threshold-value-ii?
envType=problem-list-v2&envId=sPXSXw4N)
自己做题时的思考：
想的是先用vector数组进行排序，因为vector容器可以动态添加和删除元素，然后每次删除添加完成后进行一次排序，在运行调试时没问题，但是在提交的时候发现给的测试案例过大导致超时，因为sort函数时间复杂度是O(n log n)，但是为了保证顺序，每次循环都会运行一次，就导致超时，然后只能迫于无奈向ai问有没有能减少时间复杂度的方法，ai推荐了堆，然后去b站上学了一下，知道了一些堆的用法与算法，再根据堆的特性延续一下思路，把代码写出来

题解：
先利用堆的特性将nums数组降序排列到q堆中
然后进入循环，定义两个变量存储前两个元素的值，然后将他们弹出，之后将运算后的值存储进去利用堆的特点，维持顺序，反复进行，当q的最小值也大于目标值时返回操作次数

最后的代码
class Solution {
public:
    int minOperations(vector<int> &nums, int k) {
        int res = 0;
        priority_queue<long long, vector<long long>, greater<long long>> q(nums.begin(), nums.end());
        while (q.top() < k) {
            long long x = q.top(); q.pop();
            long long y = q.top(); q.pop();
            q.push(x + x + y);
            res++;
        }
        return res;
    }
};
```

代码内容
class Solution {
public:
    int minOperations(vector<int> &nums, int k) {
        int res = 0;
        priority_queue<long long, vector<long long>, greater<long long>> q(nums.begin(), nums.end());
        while (q.top() < k) {
            long long x = q.top(); q.pop();
            long long y = q.top(); q.pop();
            q.push(x + x + y);
            res++;
        }
        return res;
    }
};
```