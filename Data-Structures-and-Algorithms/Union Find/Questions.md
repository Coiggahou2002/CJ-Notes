# 并查集习题

## P3367 并查集模板

## P1551 亲戚

## P2078 朋友

![](https://cjpark-1304138896.cos.ap-guangzhou.myqcloud.com/note_img/20211016222530.png)

本题抽象一下，两个公司分别是无向图 $G_1$ 和 $G_2$，题目给出 $p$ 次和 $q$ 次操作，分别在两个图中添加无向边 $(u,v)$，另外，给出两个固定点小明和小红，即 $u^*$ 和 $v^*$，要求取

$$
\{u^*所在连通分支的点数，v^*所在连通分支的点数\}_{min}
$$

得到的就是最多能配成情侣的对数。

**注意细节：**

1. 开 2 个数组分别处理两间公司
2. 女公司下标记得预处理，取反
3. 最后需要连通分支数 $size$ 的时候，要找 $size[find(1)]$ 而不是 $size[1]$ ，因为在合并集合的过程中，最终谁是祖先取决于题目数据的输入顺序，所以，小明所在连通分支的祖先不一定是小明（很有可能不是），所以应该套一个 $find()$

代码：

```cpp
#include <iostream>
#include <algorithm>

using namespace std;

const int MAXN = 10010;

int bf[MAXN], bsize[MAXN];
int gf[MAXN], gsize[MAXN];

int find_m(int x) {
    if (bf[x] != x) bf[x] = find_m(bf[x]);
    return bf[x];
}

int find_h(int x) {
    if (gf[x] != x) gf[x] = find_h(gf[x]);
    return gf[x];
}

int main() {
    int n, m, p, q;
    cin >> n >> m >> p >> q;
    
    for (int i = 1; i <= n; i ++ ) bf[i] = i, bsize[i] = 1;
    for (int j = 1; j <= m; j ++ ) gf[j] = j, gsize[j] = 1;

    int x, y;
    while (p -- ) {
        cin >> x >> y;
        if (find_m(x) == find_m(y)) continue;
        bsize[find_m(y)] += bsize[find_m(x)];
        bf[find_m(x)] = find_m(y);
    }

    while (q -- ) {
        cin >> x >> y;
        x = -x;
        y = -y;
        if (find_h(x) == find_h(y)) continue;
        gsize[find_h(y)] += gsize[find_h(x)];
        gf[find_h(x)] = find_h(y);
    }

    cout << min(bsize[find_m(1)], gsize[find_h(1)]);
}
```

## P1536 村村通

![](https://cjpark-1304138896.cos.ap-guangzhou.myqcloud.com/note_img/20211016222604.png)

**分析**

每组数据相当于一个无向图，给出点的个数和边集，要求录入数据后判断图的连通分支个数，算出后连通分支数减去 1 就是要修的公路条数。

开一个 $fa[MAXN]$，初始化时让 $fa[i]=i$ ，然后再根据每组数据的点的个数为其界定每次使用的范围。

每组数据的边录入完之后，在点数范围内遍历 $fa[i]$，只要 $find(i) == i$ ，说明 $i$ 是某个集合的老大，统计有多少个这样的老大即可。

另外，每组数据搞定后，别忘了恢复现场，即让 $fa[i]=i$ ，这项工作可以在上述遍历找老大的时候顺便完成。

**代码**

```cpp
#include <iostream>
#include <cstring>
#include <algorithm>
#include <cstdio>
#include <cstdlib>
#include <queue>
#include <vector>
using namespace std;

const int MAXN = 1001;

int fa[MAXN];

int find(int x) {
    if (fa[x] != x) fa[x] = find(fa[x]);
    return fa[x];
}

int main() {
    int  n, m;

    //初始化
    for (int i = 1; i <= MAXN; i ++ ) fa[i] = i;

    while(true) {
        scanf("%d%d", &n, &m);

        //由于不知道有多少组数据，检测输入0个点时结束
        if (n == 0) break; 

        //录入边，同时合并连通块
        while ( m -- ) {
            int i, j;
            scanf("%d%d", &i, &j);
            fa[find(i)] = find(j);
        }

        int cnt = 0;

        for (int i = 1; i <= n; i ++) {
            //如果父亲是自己，说明是老大
            if (find(i) == i) {
                cnt ++;
            } else { //否则恢复现场
                fa[i] = i;
            }
        }

        printf("%d\\n", cnt - 1);
    }

    return 0;
}
```

## 力扣 323. 无向图中连通分量的数目

https://leetcode-cn.com/problems/number-of-connected-components-in-an-undirected-graph/

模板题，求连通分量个数

```cpp
class Solution {
public:
    int find(int* fa, int x) {
        if (fa[x] != x) fa[x] = find(fa, fa[x]);
        return fa[x];
    }
    int countComponents(int n, vector<vector<int>>& edges) {
        int fa[n];
        for (int i = 0; i < n; i ++ ) fa[i] = i;
        for (vector<int> e : edges) {
            fa[find(fa, e[1])] = find(fa, e[0]);
        }
        int cnt = 0;
        for (int i = 0; i < n; i ++ ) {
            if (find(fa, i) == i) cnt ++;
        }
        return cnt;
    }
};
```

## 力扣 547. 省份数量

https://leetcode-cn.com/problems/number-of-provinces/

模板题，求连通块个数

```cpp
class Solution {
public:
    int find(int* fa, int x) {
        if (fa[x] != x) fa[x] = find(fa, fa[x]);
        return fa[x];
    }
    int findCircleNum(vector<vector<int>>& isConnected) {
        const int n = isConnected.size();
        int fa[n];
        for (int i = 0; i < n; i ++ ) fa[i] = i;
        for (int u = 0; u < n; u ++ ) {
            for (int v = u + 1; v < n; v ++ ) {
                if (isConnected[u][v]) {
                    fa[find(fa,u)] = find(fa, v);
                }
            }
        }
        int res = 0;
        for (int i = 0; i < n; i ++ ) {
            if (fa[i] == i) {
                res ++;
            }
        }
        return res;
    }
};
```

## 力扣 261. 以图判树

https://leetcode-cn.com/problems/graph-valid-tree/

必要条件 $m = n-1$，不满足直接踢掉，满足则用并查集检查最后连通块个数是否为 1

```cpp
class Solution {
public:
    int find(int* fa, int x) {
        if (fa[x] != x) fa[x] = find(fa, fa[x]);
        return fa[x];
    }
    bool validTree(int n, vector<vector<int>>& edges) {
        const int m = edges.size();
        if (m != n - 1) return false;
        int cnt = n; //初始连通块个数
        int fa[n];
        for (int i = 0; i < n; i ++ ) fa[i] = i;

        for (auto e : edges) {
            int u = e[0],    v = e[1];
            u = find(fa, u); v = find(fa, v);
            if (u != v) {
                fa[u] = v;
                cnt --;
            }
        }
        if (cnt == 1) return true;
        else return false;
    }
};
```

## 力扣 1101. 彼此熟识的最早时间

https://leetcode-cn.com/problems/the-earliest-moment-when-everyone-become-friends/

先按时间戳升序排序，再用并查集处理，一旦 $size=n$，说明只有一个连通块，返回时间戳

```cpp
class Solution {
public:
    static bool cmp(vector<int> a, vector<int> b) {
        return a[0] < b[0];
    }
    int find(int* fa, int x) {
        if (fa[x] != x) fa[x] = find(fa, fa[x]);
        return fa[x];
    }
    int earliestAcq(vector<vector<int>>& logs, int N) {
        if (N == 1) return -1;
        
        //需要一个维护size的并查集，初始化并查集
        const int n = N;
        int size[n];
        int fa[n];
        for (int i = 0; i < n; i ++ ) fa[i] = i,  size[i] = 1;

        //按时间戳排序logs
        sort(logs.begin(), logs.end(), cmp);

        //按顺序检索，合并
        for (vector<int> pair : logs) {
            int u = pair[1],  v = pair[2];
            u = find(fa, u);  v = find(fa, v);
            if (u != v) {
                fa[u] = v;
                size[v] += size[u];
            }
            if (size[v] == n) return pair[0];
        }
        return -1;
    }
};
```

## 力扣 684. 冗余连接

https://leetcode-cn.com/problems/redundant-connection/

按照所给信息逐步建图，返回最早出现的使图成环的边即可

如何检测？对于即将连接的两个顶点，若 $find$ 的结果相同，说明它们在同一个连通块，若连接，则成环.

```cpp
class Solution {
public:
    int find(int* fa, int x) {
        if (fa[x] != x) fa[x] = find(fa, fa[x]);
        return fa[x];
    }
    vector<int> findRedundantConnection(vector<vector<int>>& edges) {
        const int n = edges.size();
        int fa[n + 1];
        vector<int> ans(2);
        for (int i = 1; i <= n; i ++ ) fa[i] = i;
        for (vector<int> e : edges) {
            int u = e[0],  v = e[1];
            int a = find(fa, u), b = find(fa, v);
            if (a == b) {
                ans[0] = u;
                ans[1] = v;
                return ans;
            }
            else {
                fa[a] = b;
            }
        }
        return ans;
    }
};
```

## 力扣 1319. 连通网络的操作次数

https://leetcode-cn.com/problems/number-of-operations-to-make-network-connected/

$n$ 台电脑能用 $m$ 条边连通的必要条件是 $m \ge n-1$，所以开始就应该把不符合该条件的 case 过掉

必要条件满足后，按照所给出的边集去构建图，最后判断连通块个数.

$p$ 个连通块需要 $p-1$ 条边来消除

```cpp
class Solution {
public:
    int find(int* fa, int x) {
        if (fa[x] != x) fa[x] = find(fa, fa[x]);
        return fa[x];
    }
    int makeConnected(int n, vector<vector<int>>& connections) {\\
        const int m = connections.size();
        if (m < n - 1) return -1;
        int fa[n];
        for (int i = 0; i < n; i ++ ) fa[i] = i;
        for (vector<int> e : connections) {
            int u = e[0],  v = e[1];
            u = find(fa, u);
            v = find(fa, v);
            fa[u] = v;
        }
        int cnt = 0;
        for (int i = 0; i < n; i ++ ) {
            if (fa[i] == i) cnt ++;
        }
        return cnt - 1;
    }
};
```

## 力扣 200. 岛屿数量

https://leetcode-cn.com/problems/number-of-islands/

**方法一  并查集**

本题实际上是要统计连通分量个数，搞清楚集合的合并规则即可.

+ 并查集数组是一维的，需要建立二维索引到一维索引的映射，$(i,j)\rightarrow i*n+j$
+ $fa[]$ 数组初始化，陆地初始化为对应一维索引值，水区初始化为 -1
+ 为了防止下标越界，矩阵地图对首行首列预处理
+ 集合合并规则：
  + 若 $grid[i][j]=grid[i-1][j]=1$，合并 $i*n+j$ 与 $(i-1)*n+j$
  + $grid[i][j]=grid[i][j-1]=1$，合并 $i*n+j$ 与 $i*n+j-1$

时间复杂度 $O(mn)$，空间复杂度 $O(mn)$

```cpp
class Solution {
public:
    int find(int* fa, int x) {
        if (fa[x] != x) fa[x] = find(fa, fa[x]);
        return fa[x];
    }
    int numIslands(vector<vector<char>>& grid) {
        const int m = grid.size();
        const int n = grid[0].size();

        //初始化并查集
        //注意使用并查集要建立二维索引到一维索引的映射，[i][j]-->[i*n+j]
        int fa[m * n];
        for (int i = 0; i < m; i ++ ) {
            for (int j = 0; j < n; j ++ ) {
                if (grid[i][j] == '1') {
                    fa[i * n + j] = i * n + j;
                } else {
                    fa[i * n + j] = -1; //-1标记水区
                } 
            }
        }

        //首行首列预处理
        for (int i = 1; i < m; i ++ ) {
            if (grid[i][0] == '1' && grid[i-1][0] == '1') {
                fa[find(fa, i*n)] = find(fa, (i-1)*n);
            }  
        }
        for (int j = 1; j < n; j ++ ) {
            if (grid[0][j] == '1' && grid[0][j-1] == '1') {
                fa[find(fa, j)] = find(fa, j-1);
            }
        }

        //对于一个格子，如果它和左边或上方格子都是1，就合并两集合
        for (int i = 1; i < m; i ++ ) {
            for (int j = 1; j < n; j ++ ) {
                if (grid[i][j] == '1' && grid[i-1][j] == '1') {
                    fa[find(fa, i*n+j)] = find(fa, (i-1)*n+j);
                } 
                if (grid[i][j] == '1' && grid[i][j-1] == '1') {
                    fa[find(fa, i*n+j)] = find(fa, i*n+j-1);
                }
            }
        }

        //遍历并查集数组，统计集合个数
        int res = 0;
        for (int i = 0; i < m * n; i ++ ) {
            if (fa[i] == i) res ++;
        }
        return res;
    }
};
```

方法二  DFS

```cpp
class Solution {
public:

    //给出点坐标，判断是否在合法地图范围内
    bool inArea(int i, int j, int m, int n) {
        return i >=0 && i < m && j >= 0 && j < n;
    }

    //标记上下左右四个格子
    void mark_neighbor(vector<vector<char>>& grid, int i, int j) {
        
        //如果传进来的格子不是陆地，直接return，否则标记为'2'
        //注意，为了与水区作区分，访问过的标记为'2'而不是'0'
        if (grid[i][j] != '1') return;
        else grid[i][j] = '2';

        //分别判断上下左右四个格子，如果在合法范围内，就去探测
        if (inArea(i, j-1, grid.size(), grid[0].size())) mark_neighbor(grid, i, j-1);
        if (inArea(i-1, j, grid.size(), grid[0].size())) mark_neighbor(grid, i-1, j);
        if (inArea(i+1, j, grid.size(), grid[0].size())) mark_neighbor(grid, i+1, j);
        if (inArea(i, j+1, grid.size(), grid[0].size())) mark_neighbor(grid, i, j+1);
    }

    int numIslands(vector<vector<char>>& grid) {
        const int m = grid.size();
        const int n = grid[0].size();
        int res = 0;

        for (int i = 0; i < m; i ++ ) {
            for (int j = 0; j < n; j ++ ) {
                if (grid[i][j] == '1') {
                    mark_neighbor(grid, i, j);
                    res ++;
                }
            }
        }
        return res;
    }
};
```

方法三  BFS

```cpp
class Solution {
public:

    //给出点坐标，判断是否在合法地图范围内
    bool inArea(int i, int j, int m, int n) {
        return i >=0 && i < m && j >= 0 && j < n;
    }

    //标记上下左右四个格子
    void bfs(vector<vector<char>>& grid, int x, int y) {
        queue<pair<int,int> > q;
        q.push({x,y});
        while (!q.empty()) {
            pair<int,int> p = q.front();
            q.pop();
            int i = p.first,  j = p.second;
            if (inArea(i, j, grid.size(), grid[0].size()) && grid[i][j] == '1') {
                grid[i][j] = '2';
                q.push({i, j-1});
                q.push({i-1, j});
                q.push({i, j+1});
                q.push({i+1,j});
            }
        }
    }

    int numIslands(vector<vector<char>>& grid) {
        const int m = grid.size();
        const int n = grid[0].size();
        int res = 0;

        for (int i = 0; i < m; i ++ ) {
            for (int j = 0; j < n; j ++ ) {
                if (grid[i][j] == '1') {
                    bfs(grid, i, j);
                    res ++;
                }
            }
        }
        return res;
    }
};
```