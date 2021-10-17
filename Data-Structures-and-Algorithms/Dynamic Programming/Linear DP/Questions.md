# 线性 DP 习题

## 力扣 70. 爬楼梯

滚动数组

```cpp
class Solution {
public:
    int climbStairs(int n) {
        const int len = n;
        if (len == 0) return 1;
        if (len == 1) return 1;
        if (len == 2) return 2;
        int opt[3];
        opt[0] = 1;  opt[1] = 1;  opt[2] = 2;
        for (int i = 3; i <= len; i ++ ) {
            opt[0] = opt[1];
            opt[1] = opt[2];
            opt[2] = opt[0] + opt[1];
        }
        return opt[2];
    }
};
```

## 力扣 121. 买卖股票的最佳时机

DP法（也可用单调deque）

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int lowest = INT_MAX;
        int profit = 0;
        for (int price : prices) {
            lowest = min(lowest, price);
            profit = max(profit, price - lowest);
        }
        return profit;
    }
};
```

## 剑指 Offer 46. 把数字翻译成字符串

https://leetcode-cn.com/problems/ba-shu-zi-fan-yi-cheng-zi-fu-chuan-lcof/

本题是线性 DP.

首先预处理一下数据，to_string 转一下字符串，再转成 int 数组 $arr[]$，方便提取每一位数字.

**状态设计**

$f[i]$ 表示以第 $i$ 位数结尾的字符串的分割方法总数

初始状态 $f[0]=1$，而 $f[1]$ 要看 $arr[0]$ 与 $arr[1]$ 能否组成 26 以内的两位数，若能，$f[1]=2$，否则 $f[1]=1$.

**状态转移**

+ 如果 $arr[i-1]$ 与 $arr[i]$ 无法组成 26 以内的数字
  + $f[i]=f[i-1]$
  + 相当于在前面已经确定的 $f[i-1]$ 种分法后面全部加上一个 $arr[i]$，分割方法个数不变，直接继承.
+ 如果 $arr[i-1]$ 与 $arr[i]$ 能够组成 26 以内的数字
  + $f[i]=f[i-1]+f[i-2]$
  + 直接继承 $f[i-1]$ 的同时，在所有 $f[i-2]$ 种分法后面加上这个两位数，也是直接继承，所以最终是两者相加
  + 注意！该种情况下特别要求，$arr[i-1]$ 不能为 0，例如 $506$ 中的 $06$ 并不能看作合法的分割方法。

**无空间优化版 DP 写法**

```cpp
class Solution {
public:
    int translateNum(int num) {
        string str = to_string(num);
        const int n = str.length();
        vector<int> arr(n);
        if (n == 1) return 1;
        for (int i = 0; i < n; i ++ ) arr[i] = str[i] - '0';

        int dp[n];

        dp[0] = 1;
        if (arr[0]*10 + arr[1] <= 25) dp[1] = 2;
        else dp[1] = 1;

        for (int i = 2; i < n; i ++ ) {
            dp[i] = dp[i-1];
            if (arr[i-1] != 0 && arr[i-1]*10 + arr[i] <= 25) {
                dp[i] += dp[i-2];
            }
        }

        return dp[n-1];
    }
};
```