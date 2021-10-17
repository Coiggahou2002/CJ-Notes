# Kruskal 算法

## 基本流程

1. vector 存边
2. 将边集按权从小到大排序
3. 初始化并查集
4. 遍历每条边，如果不在当前生成树连通块，则加入
5. 用并查集描述和维护 MST 的连通信息
6. 最后遍历完再判断，若 MST 包含顶点数少于 n，说明无法生成 MST

## 时间复杂度

$O(mlogm)$，适合稀疏图

## 模板

```cpp
int n, m; //n为点数，m为边数
int fa[MAXN]; //并查集数组

//结构体存边，两点 + 边权，并自定义比较运算符
typedef struct Edge
{
    int u, v, w;
    bool operator< (const Edge &e) { return w < e.w };
}Edge;

//vector存边集
vector<Edge> edges;

//并查集算法
int find(int x) {
    if (fa[x] != x) fa[x] = find(fa[x]);
    return fa[x];
}

int kruskal() {
    
    //初始化并查集
    for (int i = 0; i < n; i ++ ) fa[i] = i;

    //将边按权由小到大排序
    sort(edges.begin(), edges.end());

    //res记录生成树的边权和，cnt记录生成树中的顶点数
    int res = 0, cnt = 0;

    //遍历每条边，如果它和生成树不在一个连通块中，就加入生成树
    //这种方法简单巧妙地避免了生成环，因为生成环的最后一条边的两端点必然在同一个连通块
    for (Edge e : edges) {
        int u = e.u,  v = e.v,  w = e.w;
        u = find(u);  v = find(v);
        if (u != v) {
            fa[u] = v;
            res += w;
            cnt ++;
        }
    }
    //如果遍历完成后，生成树顶点数仍少于n，说明无法找到MST
    if (cnt < n - 1) return INF;
    return res;
}
```