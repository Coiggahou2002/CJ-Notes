# 线段树

pbihao线段树模板

```cpp
#include<bits/stdc++.h>
#define ls u << 1, l, mid
#define rs u << 1 | 1, mid + 1, r
#define maxn 100021
#define LL long long 
using namespace std;
int n, a[maxn], sum[maxn * 4], lz[maxn * 4];

void push_up(int u){
	sum[u] = sum[u << 1] + sum[u << 1 | 1];
}
void push_down(int u, int l, int r){
	if(!lz[u])return;
	int mid = l + r >> 1, lc = u << 1, rc = u << 1 | 1;
	
	sum[lc] += (mid - l + 1) * lz[u];
	sum[rc] += (r - mid) * lz[u];
	
	lz[lc] += lz[u];
	lz[rc] += lz[u];
	
	lz[u] = 0;
}

void build(int u, int l, int r){
	lz[u] = 0;
	if(l == r){
		sum[u] = a[l];
		return;
	}
	int mid = l + r >> 1;
	build(ls);
	build(rs);
	push_up(u);
}

void update(int u, int l, int r, int x, int y, int z){
	if(l == x && y == r){
		sum[u] += z * (r - l + 1);
		lz[u] += z;
		return;
	}
	int mid = l + r >> 1;
	push_down(u, l, r);
	if(y <= mid)update(ls, x, y, z);
	else if(x > mid) update(rs, x, y, z);
	else{
		update(ls, x, mid, z);
		update(rs, mid + 1, y, z);
	}
	push_up(u);
}

int query(int u, int l, int r, int x, int y){
	if(x == l && r == y)return sum[u];
	int mid = l + r >> 1;
	push_down(u, l, r);
	if(y <= mid)return query(ls, x, y);
	else if (x > mid)return query(rs, x, y);
	else return query(ls, x, mid) + query(rs, mid + 1, y);
}

int main(){
	int m;
	scanf("%d%d", &n, &m);
	for(int i = 1; i <= n; i++)scanf("%d", a + i);
	build(1, 1, n);
	int op, l, r;
	for(int i = 1; i <= m; i++){
		scanf("%d%d%d", &op, &l, &r);
		if(op == 1){
			int x;
			scanf("%d", &x);
			update(1, 1, n, l, r, x);	
		}
		else printf("%d\\n", query(1, 1, n, l, r));
	}
	return 0;
}
// 3 3 6 4 3
```