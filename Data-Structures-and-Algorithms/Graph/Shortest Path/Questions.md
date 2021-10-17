# 最短路习题

## 力扣 743. 网络延迟时间

https://leetcode-cn.com/problems/network-delay-time/

正权边最短路模板题.

看数据范围更接近稠密图，所以使用邻接矩阵存图.

以给出的点 k 作为源点，全图跑一遍堆优化版 Dijkstra，最后遍历一次 $dist$ 数组，如果有 $INF$，说明信号无法从源点成功传到所有节点，否则找出以源点为起点的最长路长度 $max\_ cost$.

```cpp
class Solution {
public:
    int networkDelayTime(vector<vector<int>>& times, int n, int k) {
        priority_queue<pair<int,int>, vector<pair<int,int> >, greater<pair<int,int> > > heap;
        int g[n+1][n+1];   memset(g, 0x3f, sizeof(g));
        int dist[n+1];     memset(dist, 0x3f, sizeof(dist));
        bool st[n+1];      memset(st, 0, sizeof(st));
        
        //录入图
        for (auto e : times) {
            g[e[0]][e[1]] = e[2];
        }

        //原点设置0，入堆
        dist[k] = 0;
        heap.push({0,k});

        while (!heap.empty()) {
            //堆中存pair，first存点到原点距离，second存点编号（pair优先排序first）
            int vertex = heap.top().second;
            int distance = heap.top().first;
            heap.pop();
            //如果访问过，说明时冗余点，跳过
            if (st[vertex]) continue;
            st[vertex] = true; //标记进入set
            //遍历vertex的后继节点
            for (int j = 1; j <= n; j ++ ) {
                if (g[vertex][j] == 0x3f3f3f3f) continue;
                //更新dist[j]并入堆
                if (g[vertex][j] + distance < dist[j]) {
                    dist[j] = g[vertex][j] + distance;
                    heap.push({dist[j],j});
                }
            }
        }
        //最后跑一遍dist，如果有INF说明信号无法传到所有节点，否则找出以原点位起点的最长路长度
        int max_cost = 0;
        for (int i = 1; i <= n; i ++ ) {
            if (dist[i] == 0x3f3f3f3f) return -1;
            max_cost = max(max_cost, dist[i]);
        }
        return max_cost;
    }
};
```

## 洛谷 P1629 邮递员送信

https://www.luogu.com.cn/problem/P1629

此题为有向图，所有道路都是单向的，而每送一封邮件又必须回到邮局，所以必不可能原路返回，从点 $v_1$ 到点 $v_k$ 的最短路是去时的最短路程长度，而返程的最短路则是 $v_k$ 到 $v_1$ 的最短路.

这里不考虑跑多源最短路 $Floyd$，因为 $O(n^3)$ 对于 $n=1000$ 容易爆.

考虑跑一次 $Dijkstra$ 后，将所有边反向，再以 $v_1$ 为起点跑一次 $Dijkstra$.

注意：

+ 建图输入时，记得提防多重边和自环，由于本题跑 dij, 所以环可以存在，不影响
+ 第二次跑最短路算法之前，记得重置状态数组 $st[]$
+ 反向建图只需要 $swap(g[i][j],g[j][i])$，但注意是对**半矩阵内的** $i,j$ 执行，如果对整个矩阵走一遍 $swap$ 就又全部换回原样了.

```cpp
#include <algorithm>
#include <iostream>
#include <climits>
#include <vector>
#include <queue>
#include <cstring>

using namespace std;

#define pii pair<int,int>

const int MAXN = 1010;

int n, m;
bool st[MAXN];
long long dist[MAXN];
int g[MAXN][MAXN];

void dijkstra() {
    memset(dist, 0x3f, sizeof(dist));
    priority_queue<pii, vector<pii >, greater<pii > > heap;
    dist[1] = 0;
    heap.push({0,1});
    while (!heap.empty()) {
        int vertex = heap.top().second;
        int distance = heap.top().first;
        heap.pop();
        if (st[vertex]) continue;
        st[vertex] = true;
        for (int i = 1; i <= n; i ++ ) {
            if (distance + g[vertex][i] < dist[i]) {
                dist[i] = distance + g[vertex][i];
                heap.push({dist[i], i});
            }
        }
    }
}

int main() {
    
    memset(g, 0x3f, sizeof(g));
    cin >> n >> m;
    for (int i = 1; i <= m ; i ++ ) {
        int u, v, w;
        cin >> u >> v >> w;
        //注意提防多重边，只保留权最小的那一条
        g[u][v] = min(g[u][v], w);
    }
    
    dijkstra();
    
    long long res = 0;
    for (int i = 1; i <= n; i ++ ) {
        res += dist[i];
    }

    //重置set
    memset(st, 0, sizeof(st));
    //将所有边逆向
    for (int i = 1; i < n; i ++ ) {
        for (int j = i + 1; j <= n; j ++) {
            swap(g[i][j], g[j][i]);
        }
    }
    //再跑一遍dij
    dijkstra();

    for (int i = 1; i <= n; i ++ ) {
        res += dist[i];
    }

    cout << res;
    return 0;

}
```