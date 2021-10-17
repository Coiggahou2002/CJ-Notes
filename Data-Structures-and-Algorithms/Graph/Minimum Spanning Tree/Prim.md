# Prim 算法

与 $Djikstra$ 算法有些相似。

单源最短路用集合 $set$ 记录已经确定最短路的点，然后每次找出不在 $set$ 中的、距离源点最短的点，用它更新余下的 $dist[]$ ，直到图中所有点都在 $set$ 中.

$Dijkstra$ 最后的返回结果是包含了所有点到源点最短距离的 $dist[]$ 数组.

$Prim$ 算法使用集合 $set$ 记录已经确定到集合 $set$ 距离最短的点（一个点到一个点集的距离，定义为这个点到点集中所有点的距离中，最短的那个距离），然后每次找出不在 $set$ 中的、距离集合最短的点，用它更新所有点到 $set$ 的距离，直到所有点都在 $set$ 中.

$Prim$ 算法返回的结果是最小生成树的所有边权之和.

两者时间复杂度都为 $O(n^2+m)$

## 代码模板

```cpp
const int MAXN = 101;
const int INF = 0x3f3f3f3f;

int g[MAXN][MAXN]; //存图
int dist[MAXN]; //dist[i]表示i号点到集合的距离

/*集合的距离指的是集合之外的某个点u，到集合内点的距离的最小值*/

bool st[MAXN]; //st[i]表示i号点在不在集合内
int n; //点数

int prim() {
    //首先dist设正无穷
    memset(dist, 0x3f, sizeof dist);

    //存函数返回值，即最小生成树的所有边权之和
    int res = 0; 

    //n次迭代
    //与单源最短路相似，但dij是迭代n-1次，因为源点已选定
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
```