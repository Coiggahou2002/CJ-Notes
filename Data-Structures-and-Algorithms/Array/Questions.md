# 数组杂题

## 力扣 1. 两数之和

https://leetcode-cn.com/problems/two-sum

最朴素的方法是 $O(n^2)$ 遍历，每次都在区间 $[i+1,len-1]$ 查找 $(target-a_i)$，找到就返回，空间复杂度 $O(1)$

```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        const int len = nums.size();
        vector<int> ans;
        for (int i = 0; i < len; i ++ ) {
            for (int j = i + 1; j < len; j ++ ) {
                if (nums[i] + nums[j] == target) {
                    ans.push_back(i);
                    ans.push_back(j);
                    return ans;
                }
            }
        }
        return ans;
    }
};
```

**空间换时间的做法**

使用键值对为 $<nums[i],i>$ 的哈希表，可以在 $O(1)$ 时间内根据值查找下标。

对于每个 $nums[i]$，先在哈希表中查找 $(target-nums[i])$，找到就返回，找不到就插入键值对 $<nums[i],i>$，这样可以避免自己和自己匹配.

```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int, int> map;
        unordered_map<int,int>::iterator it;
        vector<int> ans;
        for (int i = 0; i < nums.size(); i ++ ) {
            int delta = target - nums[i];
            it = map.find(delta);
            if (it == map.end()) {
                map.emplace(nums[i], i);
                continue;
            }
            else {
                ans.push_back(i);
                ans.push_back(it->second);
                return ans;
            }
        }
        return ans;
    }
};
```

## 力扣 406. 根据身高重建队列

**基本思想：**

题目给出一个乱序队列，我们需要做的，是根据每个人的 $k_i$，将这个人调整到队列中合适的位置，使得这个人的前面有且仅有 $k_i$ 个人比它高，至于它前面站了多少个比它矮的人，没有任何影响。

**思路：**

首先按照身高给这些人降序排列，排列完成后，每个人在当前序列中的下标 $idx$，就正好代表这个人的前面有 $idx$ 个比它高的人，然后我们再比较这个人的 $idx$ 与 $k_i$ 值，如果 $k_i<idx$，说明这个人需要向前调整，我们采用类似冒泡排序的交换方法，将这个人向前交换 $t$ 次，即可完成调整，此处的 $t = idx - k_i$.

**隐藏的坑：**

我们的排序规则其实是不完善的。

为什么？设想下面的情况：

如果仅按身高给这些人降序排列，那么排列后，可能存在某两个人 $p_1$ 和 $p_2$，它们身高相等，其中 $p_1$ 排在  $p_2$ 的前面，但 $k_1 > k_2$，那么就会导致一个问题——$p_1$ 会先于 $p_2$ 向前调整，但是轮到 $p_2$ 调整的时候，$p_2$ 会跨越到 $p_1$ 的前面，由于 $p_2$ 的身高和 $p_1$ 相等，那么 $p_2$ 站在 $p_1$ 前面就会对 $p_1$ 造成影响，导致 $p_1$ 的位置又变得不符合 $k_1$.

所以我们应该确保，对于具有相同身高的两个人，$k$ 值小的人应该在初始排序时就被排在前面.

所以对排序规则作调整即可，加上“当两人身高相等时，$k$ 值小的排在前面”，其他照旧即可.

```cpp
class Solution {
public:
    static bool cmp(vector<int> a, vector<int> b) {
        if (a[0] > b[0]) {
            return true;
        } else if (a[0] == b[0]) {
            if (a[1] < b[1]) return true;
            else return false;
        } else {
            return false;
        }
    }

    vector<vector<int>> reconstructQueue(vector<vector<int>>& people) {
        sort(people.begin(), people.end(), cmp);
        for (int i = 0, len = people.size(); i < len; i ++ ) {
            if (people[i][1] < i) {
                int swap_times = i - people[i][1];
                for (int j = i; swap_times > 0; swap_times--, j--) {
                    swap(people[j], people[j-1]);
                }
            }
        }
        return people;
    }
};
```