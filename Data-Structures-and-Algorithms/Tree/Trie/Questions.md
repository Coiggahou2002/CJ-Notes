# 前缀树习题

## 力扣 208. 实现 Trie 树

https://leetcode-cn.com/problems/implement-trie-prefix-tree/

## P2580 于是他错误的点名开始了

![](https://cjpark-1304138896.cos.ap-guangzhou.myqcloud.com/note_img/20211016232041.png)

**分析**

先给出花名册，用花名册建一个 $Trie$ 树，然后再将 $query()$ 函数稍作改造，每次查询的串走到底（也就是找到）之后，将 $cnt$ 加一，相当于模拟“点名次数”，然后主程序中根据 $cnt$ 来输出答案即可。

**代码**

```cpp
#include <iostream>
#include <cstdio>
#define LL long long

using namespace std;

const LL MAXN = 1e7 + 10;

LL son[MAXN][26];
int cnt[MAXN];
LL idx;
char input[55];

void insert(char* str) {
    LL p = 0;
    for (int i = 0; str[i]; i ++ ) {
        int u = str[i] - 'a';
        if (!son[p][u]) son[p][u] = ++ idx;
        p = son[p][u];
    }
    cnt[p] ++;
}

int query(char* str) {
    LL p = 0;
    for (int i = 0; str[i]; i ++ ) {
        int u = str[i] - 'a';
        if (!son[p][u]) return 0;
        p = son[p][u];
    }

		//此处对模板稍改动，如果单词能查到，那么给它的cnt加一
		//这样可以避免点到重复名之后为了+1而再去insert一个已有的串
    cnt[p] ++;  

    return cnt[p] - 1;
}

int main() {
    int n, m;
    scanf("%d", &n);
    while( n -- ) {
        scanf("%s", input);
        insert(input);
    }
    scanf("%d", &m);
    while( m -- ) {
        scanf("%s", input);
        int t = query(input);
        if (t == 1) printf("OK\\n");
        else if (t == 0) printf("WRONG\\n");
        else printf("REPEAT\\n");
    }
    return 0;
}
```