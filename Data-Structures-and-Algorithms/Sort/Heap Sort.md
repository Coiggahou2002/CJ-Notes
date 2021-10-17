# 堆排序 Heap Sort

建堆的操作次数是一个等差乘等比数列，操作次数粗略计算如下

$$
\frac{n}{4} \times 1 + \frac{n}{8} \times 2 + \frac{n}{16} \times 3 + ...=(An+B)q^n=O(n) 
$$

每次弹出一个数，都需要改变堆顶，然后 $down$ 堆顶元素，每次 $down$ 堆顶的时间复杂度为 $O(\log n)$，需要调整 $n$ 次，所以重建堆全过程复杂度 $O(n\log n)$

综上，堆排序总时间复杂度为 $O(n\log n)$.

