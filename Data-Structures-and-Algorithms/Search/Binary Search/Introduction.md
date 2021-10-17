## 基本知识

时间复杂度：$O(\log n)$

空间复杂度：$O(1)$

## 递归实现
```java
public static int rank(int key, int[] a, int lo, int hi)
{
    if (lo > hi) return -1;
    int mid = lo + (hi-lo) / 2;
    if      (key < a[mid]) return rank(key, a, lo, mid-1);
    else if (key > a[mid]) return rank(key, a, mid, hi);
    else                   return mid;
}
```


## 非递归实现
```java
public static int BinarySearch(int[] nums, int target)
{
    if (nums == NULL || nums.length == 0) return -1;
    int left = 0;
    int right = nums.length - 1; //要减1，搜索区间左闭右闭
    while (left <= right) { //小于等于
        int mid = left + (right-left) / 2; //防溢出
        if (nums[mid] < target) {
            left = mid + 1;
        } else if (nums[mid] > target) {
            right = mid - 1;
        } else if (nums[mid] == target) {
            return mid;
        }
    }
    return -1;
}
```

## 常见注意事项

+ 是否需要保证搜索区间大于某个长度？（在 `while` 处修改循环条件）