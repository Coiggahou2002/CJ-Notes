# 概念

用数列观点来理解，$S_n$ 是 $a_n$ 的前缀和，而 $a_n$ 是 $S_n$ 的差分，两者互为逆运算.

构造方法如下

$$
\begin{cases} a_1=1\\ a_i=S_i-S_{i-1}\left( i\ge 2 \right)\\ \end{cases}
$$

# 应用场景

现有序列 ${Sn}$ ，需要多次对区间 $[l, r]$ 上的每个 $S_n$ 加上常数 $c$，若线性扫描需要 $O(n)$ ，若用差分只需要 $O(1)$ 的时间复杂度.

# 具体流程

1. 读入序列 $\{S_n\}$
2. 读入完毕后，构造差分序列 $\{a_n\}$
3. 当需要对区间 $[l, r]$ 上的每个 $S_n$ 加上常数 $c$ 时，等价于执行

$$
\begin{cases} a_l+=c\\ a_{r+1}-=c\\ \end{cases}
$$

1. 大量执行上述操作的过程中，只修改 $\{a_n\}$，不修改 $\{S_n\}$
2. 所有操作结束后，再对 $\{a_n\}$ 做一次前缀和，得到 $\{S_n\}$

# 模板

```cpp
#include <iostream>
using namespace std;

const int MAXN = 1e6 + 10;

int a[MAXN]; 
int difa[MAXN]; //a[i]的差分

void diff(int l, int r, int c)
{
    difa[l] += c;
    difa[r + 1] -= c;
}

int main()
{
    cin >> n; //原始序列长度
    
    //读入原始序列
    for (int i = 1; i <= n; i++) cin >> a[i];

    //根据原始序列构造差分序列
    for (int i = 1; i <= n; i++) diff(i,i,a[i]);

    /*
     此处执行一系列区间加减操作
     */

    //对最终修改完毕的差分序列做前缀和，得到原序列
    for (int i = 1; i <= n; i++)
    {
        a[i] = a[i - 1] + difa[i];
    }

    return 0;
}
```