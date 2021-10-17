# 双指针习题

## 力扣 283. 移动零

因为要保持数的原有相对顺序，所以只能用具有先后关系的快慢指针，slow找0，fast找非0

```cpp
void moveZeroes(int* nums, int numsSize){
    int slow = 0;
    int fast = 0;
    int tmp;

		//先让slow找到第一个0，然后让fast跟上
    while (nums[slow] && slow < numsSize - 1) slow ++;
    fast = slow;
		//只要在一开始就确保fast位置大于等于slow，后面slow不可能穿过fast
    
    while (fast < numsSize - 1) {
        while (nums[slow] && slow < numsSize - 1) slow ++; //slow找0
        while (nums[fast] == 0 && fast < numsSize - 1) fast ++; //fast找非0
        tmp = nums[fast], nums[fast] = nums[slow], nums[slow] = tmp;
    }
}
```

## 剑指 Offer 57. 和为s的两个数字

https://leetcode-cn.com/problems/he-wei-sde-liang-ge-shu-zi-lcof/

如果此题给出的数组是无序的，那就是 $Two\ Sums$ 的做法，开个哈希表，边查边emplace.

但此题给出的数组是有序的，需要利用好这个性质.

使用双指针 $head$ 和 $tail$

+ 先将 $tail$ 向前移动到不大于 $target$ 的第一个数字
+ 对于现在检索的区间 $[head,tail]$
  + $nums[head]+nums[tail]>target$
    + 说明需要其中一个变小一些，那么 $tail$ 前移一位
  + $nums[head]+nums[tail]<target$
    + 说明需要其中一个变大一些，那么 $tail$ 后移一位

```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        vector<int> res(2);
        const int n = nums.size();
        int head = 0;
        int tail = n - 1;
        while (nums[tail] >= target) tail--;
        while (head < tail) {
            if (nums[head] + nums[tail] < target) {
                head ++;
            } else if (nums[head] + nums[tail] > target) {
                tail --;
            } else {
                res[0] = nums[head];
                res[1] = nums[tail];
                break;
            }
        }
        return res;
    }
};
```

## 剑指 Offer 21. 调整数组顺序使奇数位于偶数前面

https://leetcode-cn.com/problems/diao-zheng-shu-zu-shun-xu-shi-qi-shu-wei-yu-ou-shu-qian-mian-lcof/

如果使用辅助空间，就类似于 Merge 操作，奇数放一个数组，偶数放一个数组，最后接在一起。

如果不开空间，就是典型的头尾双指针，遇到 $<even,odd>$ 数对就交换，有点类似于逆序对.

```cpp
class Solution {
public:
    vector<int> exchange(vector<int>& nums) {
        const int n = nums.size();
        int odd = 0;
        int even = n - 1;
        while (odd < even) {
            while (odd < n && (nums[odd] & 1) == 1) odd ++;
            while (even > 0 && (nums[even] & 1) == 0) even --;
            if (odd < even) swap(nums[odd], nums[even]);
        }
        return nums;
    }
};
```