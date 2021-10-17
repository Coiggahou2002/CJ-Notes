# Top K 问题

## 一、快排 + 取第 k 个

略

## 二、最小/最大堆

方法二  维护一个大小为 k 的最小堆

```cpp
class Solution {
public:
    int findKthLargest(vector<int>& nums, int k) {
        priority_queue< int, vector<int>, greater<int> > heap;
        for (int num : nums) {
            if (heap.size() == k) {
                if (num > heap.top()) {
                    heap.pop();
                    heap.push(num);
                }
            } else {
                heap.push(num);
            }
        }
        return heap.top();
    }
};
```

## 三、快速选择算法

给出一个乱序序列，要求找出序列中第 k 小的数.

与快排思想类似，每一次双指针碰面，都会将序列分割成两块，不妨按升序排序考虑，一趟碰面之后，左半边区间的所有数，都小于等于右半边区间的所有数.

此时，设左半边区间的长度为 $lsize$，我们需要比较 $k$ 与 $lsize$

若 $k \le lsize$，就递归左半边区间寻找第 $k$ 小的数；

若 $k>lsize$，就递归右半边区间寻找第 $(k-lsize)$ 小的数.

![](https://cjpark-1304138896.cos.ap-guangzhou.myqcloud.com/note_img/20211016232650.png)

如此重复直到区间长度为 1，此时区间包含的数就是要找的目标.

时间复杂度

$$n+\frac{n}{2}+\frac{n}{4}+\frac{n}{8}+...<2n=O(n)$$

下面是一个“第 $k$ 小数”的 Demo.

```cpp
int quick_select(int* arr, int l, int r, int k) {
    if (l == r) return arr[l];
    int i = l - 1;
    int j = r + 1;
    int x = arr[(l + r) >> 1];
    while (i < j) {
        do i ++; while (arr[i] < x);
        do j --; while (arr[j] > x);
        if (i < j) swap(arr[i], arr[j]);
    }
    int lsize = j - l + 1;
    if (k <= lsize) return quick_select(arr, l, j, k);
    else return quick_select(arr, j + 1, r, k - lsize);
}
```

如果需要找第 $k$ 大数，只需要对快排指针的移动规则稍作更改，即改变两个 $while$ 处的不等号方向.

```cpp
int quick_select(int* arr, int l, int r, int k) {
    if (l == r) return arr[l];
    int i = l - 1;
    int j = r + 1;
    int x = arr[(l + r) >> 1];
    while (i < j) {
        do i ++; while (arr[i] > x); //改不等号方向
        do j --; while (arr[j] < x); //改不等号方向
        if (i < j) swap(arr[i], arr[j]);
    }
    int lsize = j - l + 1;
    if (k <= lsize) return quick_select(arr, l, j, k);
    else return quick_select(arr, j + 1, r, k - lsize);
}
```