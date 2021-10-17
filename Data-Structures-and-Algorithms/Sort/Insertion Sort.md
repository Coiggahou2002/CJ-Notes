# 直接插入排序 Insert Sort

就像打牌开局理牌序一样，整个序列分为两个区间 $[0,i-1]$ 和 $[i, len-1]$ 两部分，每次取第二个区间的第一个数 $x$，然后与第一个区间作比较，找到 $x$ 在第一个区间中的正确位置，然后插入进去，这里的“插入”一般用“逐个交换”形式来替代。

![](https://cjpark-1304138896.cos.ap-guangzhou.myqcloud.com/note_img/insertion_sort.gif)

时间复杂度 $O(n^2)$，空间复杂度 $O(1)$

此处代码中，若是 $<$ 则是稳定排序，$\le$ 则是不稳定排序.

```jsx
public static void insertSort(int[] arr) {
    // 从第二个数开始，往前插入数字
    for (int i = 1; i < arr.length; i++) {
        // j 记录当前数字下标
        int j = i;
        // 当前数字比前一个数字小，则将当前数字与前一个数字交换
        while (j >= 1 && arr[j] < arr[j - 1]) {
            swap(arr[j], arr[j - 1]);
            // 更新当前数字下标
            j--;
        }
    }
}
```