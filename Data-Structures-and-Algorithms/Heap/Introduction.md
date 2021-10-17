# 堆

## 模板

```cpp
//定义找父亲/找儿子的便捷写法
#define ls(u) 2*u
#define rs(u) 2*u+1
#define fa(u) u/2

const int MAXN = 1e6 + 10;

int h[MAXN], hsize;

//向下调整
void down(int u) {

		//t是指向当前子树最小节点位置的指针
    int t = u;

		//记得判左、右儿子是否存在
    if (ls(u) <= hsize && h[ls(u)] < h[t]) t = ls(u);
    if (rs(u) <= hsize && h[rs(u)] < h[t]) t = rs(u);

    if (u != t) {
        swap(h[u], h[t]);
        down(t); //记得递归继续down
    }
}

//向上调整
void up(int u) {

    int t = u;

		//记得判父亲下标不能小于1
    if (fa(u) >= 1 && h[fa(u)] > h[t]) t = fa(u);

    if (u != t) {
        swap(h[u], h[t]);
        up(t);
    }
}

//建堆，从最后一个内点开始down
void build_heap() {
    for (int i = hsize / 2; i >= 1; i -- ) down(i);
}

//插入新元素并维护
void insert(int x) {
    h[ ++ hsize ] = x;
    up(hsize);
}

//删除堆顶元素
void remove_min() {
    h[1] = h[hsize];
    hsize --;
    down(1);
}
```