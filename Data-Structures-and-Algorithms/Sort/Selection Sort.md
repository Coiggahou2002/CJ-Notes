# 选择排序 Select Sort

外层循环指针 $i$  遍历区间 $[0,len-1]$

内层循环指针 $j$，遍历区间 $[i+1,len]$ 寻找比 $a[i]$ 小的最小的数，然后与 $a[i]$ 交换.

时间复杂度 $O(n^2)$，空间复杂度 $O(1)$

与冒泡类似，如果在选 min 的时候用的是 $<$ 就是稳定的，如果是 $\le$ 则是不稳定的.

默认简单选择排序是不稳定的

```cpp
void select_sort(int* arr, int len) {
    for (int i = 0; i < len - 1; i ++ ) {
        int min_idx = i;
        for (int j = i + 1; j < len; j ++ ) {
            if (arr[j] < arr[min_idx]) min_idx = j;
        }
        swap(arr[i], arr[min_idx]);
    }
}
```