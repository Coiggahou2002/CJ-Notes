[toc]

# 剑指 Offer 53 - II. 0～n-1中缺失的数字

https://leetcode-cn.com/problems/que-shi-de-shu-zi-lcof/

如果 $O(n)$ 线性扫描，只要找到 $nums[i]\ != i$ 的位置就可以.

但要利用好条件，因为数组是递增排序的，应该要想到二分.

用二分去收缩区间：

+ 如果 $nums[mid] == mid$，说明左边不缺，找 $[mid+1,r]$
+ 如果 $nums[mid] > mid$，说明右边不缺，找 $[l,mid]$
+ $nums[mid] < mid$ 的情况不可能出现

二分法最后收缩到区间长度为 1 的那个数，必然是 $nums[i]$ 不等于 $i$ 的数.

上述情况只有一个例外，那就是：如果收缩到序列的最后一个数，如果序列缺的是 $i + 1$，那么最后收缩到的这个数 $nums[i]$ 是有可能等于 $i$ 的，这里要做一个特判.

具体例子：$[0,1,2,3,4]$ 与 $[0,1,2,3,5]$ 都会收缩到最后一个数，要区分这两种情况.

最后时间复杂度 $O(\log n)$

```cpp
class Solution {
public:
    int missingNumber(vector<int>& nums) {
        const int n = nums.size();
        int l = 0,  r = n - 1;
        while (l < r) {
            int mid = (l + r) >> 1;
            if (nums[mid] == mid) {
                l = mid + 1;
            } else {
                r = mid;
            }
        }
        int res = l;
        if (l == n - 1) {
            if (nums[l] == l) res = l + 1;
        }
        return res;
    }
};
```



#  剑指 Offer 11. 旋转数组的最小数字

https://leetcode-cn.com/problems/xuan-zhuan-shu-zu-de-zui-xiao-shu-zi-lcof/

按题目意思，原有序列满足：

$$a_1\le a_2\le...\le a_k \le a_{k+1}\le ...\le a_n$$

然后经过旋转（如果有旋转的话）被切分成 $[a_{k+1},...,a_n] + [a_1,...,a_k]$

我们对情况作如下分类：

+ 如果 $a_{k+1}>a_k$ ，说明有旋转，用二分法处理，取枢轴 $x = a_{k+1}$
  + 若 $a[mid] \ge x$，找右边 $[mid+1,r]$
  + 若 $a[mid] < x$，找左边 $[l,mid]$
+ 如果 $a_{k+1}<a_k$ ，与旋转的假设矛盾，说明并没有旋转，直接返回序列第一个元素
+ 如果 $a_{k+1}=a_k$ ，说明原序列中 $a_1\le a_2\le...\le a_k = a_{k+1}\le ...\le a_n$，并不与题设矛盾
  + 说明有旋转，用二分处理
  + 该情况也包含类似于 $[1,1,1,1,1]$ 的情况，也可以当作有旋转来处理.

```cpp
class Solution {
public:
    int minArray(vector<int>& numbers) {
        const int n = numbers.size();
        int l = 0,  r = n - 1;
        if (numbers[l] < numbers[r]) {
            return numbers[l];
        }
        int x = numbers[0];
        while (l < r) {
            int mid = (l + r) >> 1;
            if (numbers[mid] >= x) {
                l = mid + 1;
            } else {
                r = mid;
            }
        }
        return numbers[l];
    }
};
```