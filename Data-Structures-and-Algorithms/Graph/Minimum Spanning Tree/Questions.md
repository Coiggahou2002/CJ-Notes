# 最小生成树习题

## [模板题] P2820 局域网

![](https://cjpark-1304138896.cos.ap-guangzhou.myqcloud.com/note_img/20211016224852.png)

```cpp
#include <iostream>
#include <cstring>
#include <algorithm>
#include <cstdio>
#include <cstdlib>
#include <queue>
#include <vector>
using namespace std;

const int MAXN = 101;
const int INF = 0x3f3f3f3f;

int g[MAXN][MAXN]; //存图
int dist[MAXN]; //dist[i]表示i号点到集合的距离

/*集合的距离指的是集合之外的某个点u，到集合内点的距离的最小值*/

bool st[MAXN]; //st[i]表示i号点在不在集合内
int n, k;

int prim() {
    //首先dist设正无穷
    memset(dist, 0x3f, sizeof dist);

    //存函数返回值，即最小生成树的所有边权之和
    int res = 0; 

    //n次迭代
    //与单源最短路相似，但dji是迭代n-1次，因为源点已选定
    for (int i = 0; i < n; i ++ ) {
        //t标记到当前集合距离最短的点
        int t = -1;

        //于不在集合内的点中，寻找到集合距离最短的点
        for (int j = 1; j <= n; j ++ ) {
            if (!st[j]) {
                if (t == -1 || dist[j] < dist[t]) {
                    t = j;
                }
            }
        }

        //找到最短点，马上判断是不是INF
        //如果连最短的点都是INF，说明图不连通，必无最小生成树
        if (i != 0 && dist[t] == INF) return INF;

        //如果当前迭代到的点不是第一个点，就将找到的到集合距离最短的边权加入结果
        if (i != 0) res += dist[t];

        //将t号点加入集合
        st[t] = true;

        //马上用t号点去更新所有点到集合的距离
        //（每次集合距离改变必然是因为新点的加入，所以只用新点更新就好）
        for (int j = 1; j <= n; j ++ ) {
            dist[j] = min(dist[j], g[t][j]);
        }
    }
    return res;
}

int main() {
    //地图记得不可达初始化
    memset(g, 0x3f, sizeof g);

    scanf("%d%d", &n, &k);
    
    //edges_weight_sum，所有录入的边权之和
    int ews = 0;

    while ( k -- ) {
        int i, j, m;
        scanf("%d%d%d", &i, &j, &m);
        ews += m;
        if (m != 0) {
            //若有多重边，保留权最小的那条
            g[i][j] = min(g[i][j], m);
            g[j][i] = g[i][j];
        }
    }

    //总边权之和 - 最小生成树边权和 = 答案
    printf("%d", ews - prim());

}
```

## 力扣 1135. 最低成本联通所有城市

https://leetcode-cn.com/problems/connecting-cities-with-minimum-cost/

本题是稀疏图，适合用 Kruskal 算法

```cpp
class Solution {
public:
    static bool cmp(vector<int> a, vector<int> b) {
        return a[2] < b[2];
    }
    int find(int* fa, int x) {
        if (fa[x] != x) fa[x] = find(fa, fa[x]);
        return fa[x];
    }
    int minimumCost(int N, vector<vector<int>>& connections) {
        const int n = N;
        const int m = connections.size();
        if (m < n - 1) return -1;
        int fa[N + 1];
        for (int i = 1; i <= n; i ++ ) fa[i] = i;
        sort(connections.begin(), connections.end(), cmp);

        int res = 0;
        int cnt = 0;
        for (vector<int> e : connections) {
            int u = e[0],   v = e[1];
            u = find(fa, u);
            v = find(fa, v);
            if (u != v) {
                fa[u] = v;
                res += e[2];
                cnt ++;
            }
        }
        if (cnt < n - 1) return -1;
        else return res;
    }
};
```

## 力扣 1584. 连接所有点的最小费用

https://leetcode-cn.com/problems/min-cost-to-connect-all-points/

```cpp
class Solution {
public:

    typedef struct Edge {
        int u, v, w;
        bool operator< (const Edge &e) {
            return w < e.w;
        }
    }Edge;

    int find(int* fa, int x) {
        if (fa[x] != x) fa[x] = find(fa, fa[x]);
        return fa[x];
    }

    int minCostConnectPoints(vector<vector<int>>& points) {
        const int n = points.size();
        //建图，存边
        vector<Edge> edges;
        for (int i = 0; i < n; i ++ ) {
            for (int j = 0; j < n; j ++ ) {
                if (i == j) continue;
                Edge tmp;
                tmp.u = i,  tmp.v = j;
                tmp.w = abs(points[i][0] - points[j][0]) + abs(points[i][1] - points[j][1]);
                edges.push_back(tmp);
            }
        }
        //并查集数组，初始化
        int fa[n];
        for (int i = 0; i < n; i ++ ) fa[i] = i;

        //边权从小到大排序
        sort(edges.begin(), edges.end());

        const int m = edges.size();

        int res = 0, cnt = 0;
        for (Edge e : edges) {
            int u = e.u,  v = e.v,  w = e.w;
            u = find(fa, u);
            v = find(fa, v);
            if (u != v) {
                fa[u] = v;
                res += w;
                cnt ++;
            }
        }
        return res;

    }
};      
```