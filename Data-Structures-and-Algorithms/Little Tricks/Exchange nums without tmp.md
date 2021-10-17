# 不使用中间变量交换两数

增量转移法（画图模拟就知道什么意思了）

```cpp
b = a - b;
a = a - b;
b = a + b;
//数组去重，返回去重后的长度
int remove_duplicates(int* nums, int numsSize) {
    if (numsSize == 0) {
        return 0;
    }
    int fast = 1, slow = 1;
    while (fast < numsSize) {
        if (nums[fast] != nums[fast - 1]) {
            nums[slow] = nums[fast];
            ++slow;
        }
        ++fast;
    }
    return slow;
}
```