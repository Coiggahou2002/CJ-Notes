# 哈夫曼树习题

## [NOIP 2004 提高组] 合并果子

此题为求最优二叉树的典型例题

**手撸最小堆**

```cpp
#include <iostream>
#include <cstdio>
#include <algorithm>

#define ls(u) 2*u
#define rs(u) 2*u+1
#define fa(u) u/2

using namespace std;

const int MAXN = 10001;

int h[MAXN], hsize;
int res;

//最小堆

void down(int u) {
    int t = u;
    if (ls(u) <= hsize && h[ls(u)] < h[t]) t = ls(u);
    if (rs(u) <= hsize && h[rs(u)] < h[t]) t = rs(u);
    if (u != t) {
        swap(h[u], h[t]);
        down(t);
    }
}

void up(int u) {
    int t = u;
    if (fa(u) >= 1 && h[fa(u)] > h[t]) t = fa(u);
    if (u != t) {
        swap(h[u], h[t]);
        up(t);
    }
}

void build_heap() {
    for (int i = hsize / 2; i >= 1; i -- ) down(i);
}

void remove_min() {
    h[1] = h[hsize];
    hsize --;
    down(1);
}

void insert(int x) {
    h[ ++ hsize ] = x;
    up(hsize);
}

int main() {
    int n;
    scanf("%d", &n);

    hsize = n;

    for (int i = 1; i <= n; i ++ ) {
        scanf("%d", &h[i]);
    }

    build_heap();

    int a, b;

    while(hsize >= 2) {
        a = h[1],  remove_min();
        b = h[1],  remove_min();
        res += (a + b);
        insert(a + b);
    }

    printf("%d", res);
    return 0;
}
```

**优先队列最小堆版本**

```cpp
#include <iostream>
#include <cstdio>
#include <algorithm>
#include <queue>
#include <vector>

using namespace std;

int a[10001];

int main() {
    priority_queue<int, vector<int>, greater<int> > heap;

    int n;
    scanf("%d", &n);
    for (int i = 1; i <= n; i ++ ) {
        scanf("%d", &a[i]);
        heap.push(a[i]);
    }
    int ans = 0;
    while (heap.size() > 1) {
        int a = heap.top();
        heap.pop();
        int b = heap.top();
        heap.pop();
        ans += (a + b);
        heap.push(a + b);
    }
    printf("%d", ans);
    return 0;
}
```

## P1334 瑞瑞的木板

合并果子的逆向版本,

注意结果 $res$ 要开 $long \,long$ 来存否则 RE.

```cpp
#include <cstdio>
#include <algorithm>
#include <iostream>

#define ls(u) 2*u
#define rs(u) 2*u+1
#define fa(u) u/2

using namespace std;

const int MAXN = 1e5 + 10;

int h[MAXN], hsize;
long long res;

void down(int u) {
    int t = u;
    if (ls(u) <= hsize && h[ls(u)] < h[t]) t = ls(u);
    if (rs(u) <= hsize && h[rs(u)] < h[t]) t = rs(u);
    if (u != t) {
        swap(h[u], h[t]);
        down(t);
    }
}

void up(int u) {
    int t = u;
    if (fa(u) >= 1 && h[fa(u)] > h[t]) t = fa(u);
    if (u != t) {
        swap(h[u], h[t]);
        up(t);
    }
}

void build_heap() {
    for (int i = hsize / 2; i >= 1; i -- ) down(i);
}

void remove_min() {
    h[1] = h[hsize];
    hsize--;
    down(1);
}

void insert(int x) {
    h[ ++ hsize ] = x;
    up(hsize);
}

int main() {
    
    int n;
    
    scanf("%d", &n);
    hsize = n;

    long long sum = 0;
    long long lft = 0;

    for (int i = 1; i <= n; i ++ ) {
        scanf("%d", &h[i]);
        sum += h[i];
    }

    build_heap();

    while (hsize >= 2) {
        int a,b;
        a = h[1];
        remove_min();
        b = h[1];
        remove_min();
        res += (a + b);
        insert(a + b);
    }

    printf("%lld", res);

    return 0;
}
```