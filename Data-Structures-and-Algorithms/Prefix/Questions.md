# 典型例题

## P1387 最大正方形

![](https://cjpark-1304138896.cos.ap-guangzhou.myqcloud.com/note_img/20211016154336.png)

先用二维前缀和预处理地图，然后三重循环，最外层从 $min(m,n)$ 到 $1$ 枚举正方形边长 $r$ ，内层枚举坐标，从 $(r,r)$ 到 $(n,m)$，一旦这个区域的前缀和 $ans = r^2$ ，直接 $break$ 并返回边长 $r$.

```cpp
#include <iostream>
#include <cstdlib>
#include <cstdio>

using namespace std;

const int MAXN = 101;

short map[MAXN][MAXN];

int main() {
    int n, m;
    scanf("%d%d", &n, &m);
    for (int i = 1; i <= n; i ++ ) {
        for (int j = 1; j <= m; j ++ ) {
            scanf("%d", &map[i][j]);
        }
    }

    for (int i = 1; i <= n; i ++ ) {
        for (int j = 1; j <= m; j ++ ) {
            map[i][j] += map[i][j-1] + map[i-1][j] - map[i-1][j-1];
        }
    }

    int ans;

    for (int r = min(m,n); r >= 1; r -- ) {
        for (int i = r; i <= n; i ++ ) {
            for (int j = r; j <= m; j ++ ) {
                ans = map[i][j] - map[i-r][j] - map[i][j-r] + map[i-r][j-r];
                if (ans == r * r) {
                    printf("%d", r);
                    exit(0);
                }
            }
        }
    }
    return 0;
}
```

## [HNOI 2003\] 激光炸弹

https://www.luogu.com.cn/problem/P2280

### 分析

本题将地图看成网格，点的价值看作矩阵元素的值，抽象之后即：给定一个 $5000*5000$ 大小的矩阵，求 $R*R$ 子矩阵元素和的最大值.

因为计算前缀和过程中会出现 $[i-1][j-1]$，为防止越界，地图数据从 $(1,1)$ 开始记录.

输入矩阵后，预处理，做出二维前缀和，遍历前缀和，记录最大值即可.

### 注意

此题卡空间，空间最大限制 125 MB.

如果开两个 $5000*5000$ 的 int 型二维数组（一个记录原地图，一个做前缀和），要用 191 MB.

如果开一个  $5000*5000$ 的 int 型二维数组放前缀和，再开一个  $5000*5000$ 的 short 型二维数组放原矩阵（因为原矩阵元素值不超过 1000 ，可用short），总共要用 143 MB.

上述方案都不行.

考虑只开一个   $5000*5000$ 的 int 型二维数组，先输入原矩阵，然后一边计算前缀和一边覆盖，总共使用内存 95 MB，满足要求.

另外注意边界的处理，边界坐标之间加一减一的关系.

### 代码

```cpp
#include <iostream>
#include <algorithm>

using namespace std;

const int MAXN = 5010;
const int bd = 5001;

int N, R;
int map[MAXN][MAXN];
int res;

int main()
{
    cin >> N >> R;
    
    //Input the map (data = value)
    for (int i = 1; i <= N; i++)
    {
        int x, y, w;
        cin >> x >> y >> w;
        map[x + 1][y + 1] = w;
    }
    
    //Make the prefix of the map
    for (int i = 1; i <= bd; i++)
    {
        for (int j = 1; j <= bd; j++)
        {
            map[i][j] += map[i-1][j] + map[i][j-1] - map[i-1][j-1];
        }
    }
    
    //Iterate the prefix
    for (int i = 1; i <= bd - R + 1; i++)
    {
        for (int j = 1; j <= bd - R + 1; j++)
        {
            int temp;
            temp = map[i+R-1][j+R-1] - map[i-1][j+R-1] - map[i+R-1][j-1] + map[i-1][j-1];
            res = max(res,temp);
        }
    }
    
    cout << res;
    
    return 0;
}
```