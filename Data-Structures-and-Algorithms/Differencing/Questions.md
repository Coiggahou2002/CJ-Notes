# 例题

## AcWing 101. 最高的牛

https://www.acwing.com/problem/content/103/

### 基本思路

+ `h[i]` 存放每头牛的最大高度，全部初始化为最大高度 $H$.
+ 给出的每一对关系之间不可能存在交叉，只有可能是相互嵌套，否则可以推出矛盾.
+ 相邻的牛之间可以相互看见，牛与自身也可以相互看见.
+ 给出的关系可能存在重复，所以首先要对给出的所有关系去重，这里选择使用 `set<pair<int,int>>` 来存储不重复的有序对.
+ 对于每一对给出的有序对，将位于他们之间的牛的高度全部减 1

### 常规写法

```cpp
#include <iostream>
#include <cstdlib>
#include <set>

using namespace std;

const int MAXN = 1e4 + 10;

int N,P,H,M;
int h[MAXN]; //用于记录每头牛的最大高度

int main()
{
    cin >> N >> P >> H >> M;
    
    //每头牛最大高度都初始化为所给最大值
    for (int i = 1; i <= N; i++) h[i] = H; 
    
    set<pair<int, int>> existed;
    
    for (int i = 1; i <= M; i++)
    {
        int a, b;
        cin >> a >> b;

        //保证有序对左小右大，方便处理
        if (a > b) swap(a,b); 
        
        if (existed.count({a,b}) == 0)
        {   //集合里没有才添加
            existed.insert({a,b});
            
            //自己或者相邻，不处理
            if (b - a == 1 || b - a == 0) {}
            else {
                for (int j = a + 1; j <= b - 1; j++)
                {
                    h[j] -= 1;
                }
            }
        }
    }
    
    for (int i = 1; i <= N; i++) printf("%d\\n", h[i]);
    return 0;
}
```

### 差分思路

在本题中，每头牛实际上有多高并没有太大的关系，牛与牛之间能否相互看见，取决于它们之间的高度差，抽象化之后，实际上就是对序列 $h_i$ 的某个区间 $[l,r]$ 上每一个数都进行减去 1 的操作，所以考虑用差分优化.

另外，从差分的角度来说，本题的关键在于各头牛高度之间的相对值，所以，理解的思维也可以是相对的，我们可以反过来理解——每当给出一个关系, $A$ 和 $B$ 可以相互看见，视作保持 $A$, $B$ 之间的所有牛高度不变，将这些高度不变的牛之外的所有牛，全部抬升一个高度。

### 差分代码

```cpp
#include <iostream>
#include <cstdlib>
#include <set>

using namespace std;

const int MAXN = 1e4 + 10;

int N,P,H,M;
int h[MAXN]; //用于记录每头牛的最大高度
int difh[MAXN]; //高度的差分

int main()
{
    cin >> N >> P >> H >> M;
    
    //每头牛最大高度都初始化为所给最大值
    //只需要将差分第一个设为H即可
    difh[1] = H;
    
    set<pair<int, int>> existed;
    
    for (int i = 1; i <= M; i++)
    {
        int a, b;
        cin >> a >> b;

        //保证有序对左小右大，方便处理
        if (a > b) swap(a,b); 
        
        if (existed.count({a,b}) == 0)
        {   //集合里没有才添加
            existed.insert({a,b});
            
            //自己或者相邻，不处理
            if (b - a == 1 || b - a == 0) {}
            else {
                //对差分端点操作，相当于原高度每个减去一
                difh[a + 1] += -1;
                difh[b] -= -1; 
            }
        }
    }
    
    //最后再做前缀和得出h[i]
    for (int i = 1; i <= N; i++)
    {
        h[i] = h[i - 1] + difh[i];
    }
    
    for (int i = 1; i <= N; i++) printf("%d\\n", h[i]);
    return 0;
}
```