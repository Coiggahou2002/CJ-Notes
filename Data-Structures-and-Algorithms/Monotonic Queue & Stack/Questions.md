# 单调栈和单调队列习题

## 模板题 P5788 单调栈

![](https://cjpark-1304138896.cos.ap-guangzhou.myqcloud.com/note_img/20211017101915.png)

注意，原单调栈模板是“找到数列中第 $i$ 个元素**之前**第一个比它大的数”。

这里要求“找到数列中第 $i$ 个元素**之后**第一个比它大的数”，所以我们**将原数列逐个进栈的时候，应该倒序**，也就是从 $a[n]$ 开始，那么问题就被转换成熟悉的问题（找之前的数）.

剩下的与单调栈模板操作类似，根据题意确定栈的具体单调性，然后每次新元素进栈都维护单调性，同时输出需要的答案即可。

另外一点特别注意，按照我们这个思路，打出来的答案序列也是倒序的，需要 $reverse$ 一下.

(这里用的是栈来 $reverse$ )

```cpp
#include <iostream>
#include <cstring>
#include <algorithm>
#include <cstdio>
#include <cstdlib>
#include <queue>
#include <vector>
#include <stack>

using namespace std;

const int MAXN = 3000010;

long long a[MAXN];

int main() {
    int n;
    cin >> n;
    for (int i = 1; i <= n; i ++ ) scanf("%lld", &a[i]);

    stack<int> ms; //mono_stack，单调栈
    stack<int> rev; //用来反转答案

		//从数列末尾开始进栈
    for (int i = n; i >= 1; i -- ) {

        while (!ms.empty() && a[ms.top()] <= a[i]) {
            ms.pop();
        }

				//维护单调性后，若发现栈不空，说明要找的数找到了
        if (!ms.empty()) rev.push(ms.top());
				//若栈空了，说明不存在符合要求的数，输出0
        else rev.push(0);
				
				//维护好单调性之后，进栈
        ms.push(i);

    }

    while(!rev.empty()) printf("%d ", rev.top()), rev.pop();

    return 0;

}
```

## 模板题 P1886 滑动窗口

![](https://cjpark-1304138896.cos.ap-guangzhou.myqcloud.com/note_img/20211017101946.png)

分析见代码注释

```cpp
#include <iostream>
#include <cstring>
#include <algorithm>
#include <cstdio>
#include <cstdlib>
#include <queue>
#include <vector>
using namespace std;

const int MAXN = 1e6 + 10;

int q[MAXN];
int a[MAXN];
int hh, tt;

void init_queue() {
    hh = 0;
    tt = -1;
}

void push_back(int x) {
    q[ ++ tt ] = x;
}

int front() {
    return q[hh];
}

int back() {
    return q[tt];
}

void remove_front() {
    hh ++;
}

void remove_back() {
    tt --;
}

bool empty() {
    return hh > tt;
}

int main() {
    init_queue();

    int n, k;

    scanf("%d%d", &n, &k);
    for (int i = 1; i <= n; i ++ ) scanf("%d", &a[i]);

    //队列存储经过单调处理的滑动窗口（注意为了定位非窗口内元素，队列中存的是下标）
    //每次循环代表窗口向右移动一个单位
    for (int i = 1; i <= n; i ++ ) {

        //维护窗口，不允许队列中有窗口外的元素
        //为什么是while？因为最坏情况是维护单调性时一个都没删，那么此时队头就是多的
        //每轮最多只会在队头多出一个不在窗口内的元素
        if (!empty() && front() < i - k + 1) remove_front();

        //队尾加入元素之前，将队尾所有比它大的元素删掉，然后再将它放进队尾
        while (!empty() && a[back()] >= a[i]) remove_back();
        push_back(i);

        //(i-k+1)是窗口头部的下标，当窗口头部移动到原序列头部时，才开始输出答案
        if (i - k + 1 >= 1) printf("%d ", a[front()]);
    }

    //清空队列，再写一遍求最大值
    memset(q, 0, sizeof q);

    printf("\\n");

    //以下同上，改不等号方向即可
    for (int i = 1; i <= n; i ++ ) {
        if (!empty() && front() < i - k + 1) remove_front();
        while (!empty() && a[back()] <= a[i]) remove_back();
        push_back(i);
        if (i - k + 1 >= 1) printf("%d ", a[front()]);
    }
    return 0;
}
```

## 力扣 121. 买卖股票的最佳时机

https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock

给定一个数组 prices ，它的第 i 个元素 prices[i] 表示一支给定股票第 i 天的价格。

你只能选择 某一天 买入这只股票，并选择在 未来的某一个不同的日子 卖出该股票。设计一个算法来计算你所能获取的最大利润。

返回你可以从这笔交易中获取的最大利润。如果你不能获取任何利润，返回 0 。

**单调双端队列**

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        deque<int> sdq;
        int profit = 0;
        for (int price : prices) {
            while (!sdq.empty() && sdq.back() >= price) {
                sdq.pop_back();
            }
            sdq.push_back(price);
            profit = max(profit, sdq.back() - sdq.front());
        }
        return profit;
    }
};
```