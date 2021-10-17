# 快速排序 Quick Sort

一般选中间数作枢轴.

若选取序列头或序列尾作枢轴，在序列原本有序时，达到最坏时间复杂度 $O(n^2)$

注意：快排是否具有稳定性，依然取决于具体函数中不等号的写法。

## 模板

```cpp
void quick_sort(int q[], int l, int r) {
    if (l >= r) return;

    int i = l - 1;
    int r = r + 1;
    int x = q[l + r >> 1]; //取中间数做基准数
    while (i < j) {
        do i ++; while (q[i] < x);
        do j --; while (q[j] > x);
        if (i < j) swap(q[i], q[j]);
    }
    quick_sort(q, l, j);
    quick_sort(q, j + 1, r);
}
```