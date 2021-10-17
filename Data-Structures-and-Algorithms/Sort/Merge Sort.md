# 归并排序 Merge Sort

![](https://cjpark-1304138896.cos.ap-guangzhou.myqcloud.com/note_img/merge_sort.gif)

**关于 merge：**

+ 如果不开辟额外空间，原地归并，需要实现“数组中的插入操作”，时间复杂度加 $O(n)$
+ 利用辅助空间和双指针归并，需要开辟额外空间，有两种方法
  + 每次 merge 时开辟
  + 一开始就开辟一个与原数组等长的 tmp 数组

数组拆分 $\log n$ 次，每次 merge 平均 $O(n)$，总时间复杂度 $O(n\log n)$

空间复杂度 $O(n)$

注意：归并排序是否具有稳定性，依然取决于具体函数中不等号的写法。

## 模板

```cpp
void merge_sort(int q[], int l, int r) {
    if (l >= r) return;

    int mid = l + r >> 1; //偏左的mid计算方法

    //为什么递归要写在前面？因为是分割到底才进行归并
    //越是外层的归并，归并语句越迟执行
    merge_sort(q, l, mid);
    merge_sort(q, mid + 1, r);

    int k = 0;

    //将序列分成两段[l, mid]与[mid+1, r]，分别以i,j为起点
    //注意两段不一定等长，可能长度相差1到2
    int i = l;
    int j = mid + 1;

    //按顺序归并到tmp中
    while (i <= mid && j <= r) {
        if (q[i] <= q[j]) tmp[ k ++ ] = q[ i ++ ];
        else tmp[ k ++ ] = q[ j ++ ];
    }

    //由于两段序列不一定等长，可能有一个序列有剩余未归并元素
    //此处不判断哪个序列剩余，直接两个都归并一遍就好
    //因为经历了上面之后，必然只有一个序列多余，下面两个while只会执行一个
    while (i <= mid) tmp[ k ++ ] = q[ i ++ ];
    while (j <= r) tmp[ k ++ ] = q[ j ++ ];

    //归并完，也就是合并了两个小序列之后，再用tmp[]覆盖q[]
    for (i = l, j = 0; i <= r; i ++, j ++ ) q[i] = tmp[j];
}
```