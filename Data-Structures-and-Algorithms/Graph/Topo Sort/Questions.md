# 拓扑排序习题

## 力扣 207. 课程表

https://leetcode-cn.com/problems/course-schedule/

```cpp
class Solution {
public:
    bool canFinish(int numCourses, vector<vector<int>>& prerequisites) {
        vector<int> degin(numCourses); //入度列
        vector< vector<int> > g(numCourses); //初始化邻接表存图
        
        //将图的数据存入邻接表与入度列
        for (vector<int> e: prerequisites) {
            g[e[1]].push_back(e[0]);
            degin[e[0]] ++;
        }

        //遍历入度列，将所有入度为 0 的点先入队
        queue<int> q;
        for (int i = 0; i < numCourses; i ++ ) {
            if (degin[i] == 0) q.push(i);
        }
				
				//先手判断，若无元素入队，说明不存在拓扑序列
        if (q.empty()) return false;

				//ans存放排出的拓扑序列
        vector<int> ans;

        while (!q.empty()) {
            int u = q.front();
            ans.push_back(u);
            q.pop();
						//遍历与当前点u关联的边<u,v>，删边并维护各个v的入度
            for (int v : g[u]) {
                if (v == -1) continue;
                degin[v] --;
                if (degin[v] == 0) q.push(v); //删的同时将入度为0的v入队
                v = -1;
            }
        }
		
				//如果拓扑序列长度等于顶点数，说明存在拓扑序列
        if (ans.size() == numCourses) return true;
        else return false;
        
    }
};
```

## AcWing 1191. 家谱树

https://www.acwing.com/problem/content/description/1193

拓扑排序模板题，所给图是连通的，套板子

```cpp
#include <iostream>
#include <vector>
#include <queue>

using namespace std;

int n;

int main() {
    
    cin >> n;
    
    vector<int> d(n + 1);
    vector< vector<int> > g(n + 1);
    
    //输入图
    for (int i = 1; i <= n; i ++ ) {
        int u;
        while (true) {
            cin >> u;
            if (u == 0) break;
            g[i].push_back(u);
            d[u] ++;
        }
    }
    
    queue<int> q;
    for (int i = 1; i <= n; i ++ ) {
        if (d[i] == 0) q.push(i);
    }
    
    vector<int> ans;
    
    while (!q.empty()) {
        int u = q.front();
        ans.push_back(u);
        q.pop();
        for (int v : g[u]) {
            if (v == -1) continue;
            d[v] --;
            if (d[v] == 0) q.push(v);
            v = -1;
        }
    }
    
    for (int a : ans) {
        cout << a << " ";
    }
    
    return 0;
}
```

## AcWing 1192. 奖金

https://www.acwing.com/problem/content/1194/

员工为点，代表意见为边，建有向图.

u 的奖金应比 v 高，则建加边 <v, u>（奖金低的放前面）

改造入度列，让入度列顺便携带点的奖金信息，初始所有入度为 0 的点奖金初始化 100 元.

每次删边<i, j>时，将 i 的奖金加一块钱赋给 j 的奖金.

最终拓扑序列不存员工号，只存奖金，最后输出奖金总和.

```cpp
#include <queue>
#include <iostream>
#include <vector>
#define PII pair<int,int>

using namespace std;

int n, m;

int main() {
    cin >> n >> m;

	//度数列d放pair，first存点的入度，second存该点的奖金
    vector<PII> d(n + 1); 

    vector< vector<int> > g(n + 1); //员工编号从1开始，容器多开一位

	//邻接表 + 度数列，录入图
    for (int i = 1; i <= m; i ++ ) {
        int u, v;
        cin >> u >> v;
        g[v].push_back(u);
        d[u].first ++;
    }
    
    queue<int> q;
    
	//将所有初始入度为0的点入队，同时初始化它们的奖金为100元
    for (int i = 1; i <= n; i ++ ) {
        d[i].second = 100;
        if (d[i].first == 0) q.push(i);
    }
    
    if (q.empty()) cout << "Poor Xed";
    
    vector<int> bonus; //拓扑序列，不存员工编号，存奖金

    while (!q.empty()) {
        int u = q.front();
        bonus.push_back(d[u].second);
        q.pop();
        for (int v : g[u]) {
            if (v == -1) continue;
            d[v].first --;
			//删边<u,v>的同时，给予v比u多一块钱的奖金
            d[v].second = d[u].second + 1;
            if (d[v].first == 0) q.push(v);
            v = -1;
        }
    }
    //最后记得比较拓扑序列的长度和顶点数是否相同
    if (bonus.size() != n) cout << "Poor Xed";
    else {
        int res = 0;
        for (int money : bonus) res += money;
        cout << res;
    }
    
    return 0;
}
```

## 力扣 1136. 平行课程

https://leetcode-cn.com/problems/parallel-courses/

此题虽然求的不是所给图的拓扑序列，但依然要走拓扑排序的流程，唯一要改动的地方是，**记录遍历的层数**，类似于二叉树的层序遍历，记录队列的轮数.

```jsx
class Solution {
public:
    int minimumSemesters(int n, vector<vector<int>>& relations) {
        vector<int> d(n + 1);
        vector< vector<int> > g(n + 1);
        for (vector<int> e : relations) {
            g[e[0]].push_back(e[1]);
            d[e[1]] ++;
        }
        queue<int> q;
        for (int i = 1; i <= n; i ++ ) {
            if (d[i] == 0) q.push(i);
        }
        if (q.empty()) return -1;
        int res = 0;
        vector<int> ans;
        while (!q.empty()) {
            for (int i = q.size(); i; i -- ) {
                int u = q.front();
                ans.push_back(u);
                q.pop();
                for (int v : g[u]) {
                    if (v == -1) continue;
                    d[v] --;
                    if (d[v] == 0) q.push(v);
                    v = -1;
                }
            }
            res ++;
        }
        if (ans.size() == n) return res;
        else return -1;

    }
};
```

## 力扣 269. 火星词典

https://leetcode-cn.com/problems/alien-dictionary/

**基本问题框架：**

基本模型是一个有向无环图.

给出的字典中所有出现过的字母，是图的点集；

我们需要根据字典中给出的串与串之间的偏序关系，通过字典序的比较规则，从中获取字母之间的偏序关系，建立有向边.

然后给出该图的一个拓扑序列.

**需要注意的点：**

+ 串与串之间的偏序关系具有传递性，所以用双指针 $O(n)$ 扫描 $words$ 即可，无需 $O(n^2)$。但注意由于双指针要求 $words$ 长度至少为 2，所以要针对长度 1 的情况写特判避开.
+ 用字母建图，必然需要作 $char \rightarrow int$ 的映射，而且为了映射和下标处理的方便性，建图时必然直接建 26 个字母的空位，那么后续遍历点的过程中就需要区分出“无效点”，这里采取 $g[u].empty()$ 来判断，即如果字母 $u$ 的邻接表为空，说明它是无效点。但是，图中可能存在孤立点，孤立点的邻接表也是空的，又需要将“无效点”与“孤立点”作区分，所以录入顶点时给孤立点的邻接表加上一个值为 -1 的结点，以作区分.
+ 由于拓扑排序模板在最后输出的时候，需要判定顶点个数与拓扑序列长度是否相等，所以本题还需要统计出现过的字母的个数，所以使用了 $unordered_\_set$ 存字母，获取其 $size$ 即可.
+ $words$ 中每两个相邻串，至多只能给出一条有向边的信息.
  + 如果在两串结束之前能找到第一个不同的字母对 $c_i,c_j$，则加边 $<c_i,c_j>$，剩余信息无用
  + 如果在找到不同字母之前，其中一个串已经结束
    + 若是排在前面的串先结束，则这两个串无法提供有效的有向边信息，跳过
    + 若是排在后面的串先结束，说明这两个串在 $words$ 中本就违背字典序，无法给出合法拓扑序列，程序结束.

```jsx
class Solution {
public:
    string alienOrder(vector<string>& words) {
        vector< vector<int> > g(26);
        vector<int> d(26);
        unordered_set<int> chars;

        const int size = words.size(); //字典中串的个数

        //因为遍历words使用双指针，所以要求words长度至少为2，长度1要写特判避开
        if (size == 1) {
            return words[0];
        }

        //先走一遍所有串，记下出现过的所有字母，先建成图中的孤立点
        for (string s : words) {
            for (char c : s) {
                int u = c - 'a';
                g[u].push_back(-1);
                chars.emplace(u);
            }
        }

        //按顺序比较words中相邻的两个串，由于偏序关系的传递性，只需要O(n)
        for (int a = 0, b = 1; b < size; a ++, b ++ ) {
            int i = 0,  j = 0;
            int la = words[a].length(),  lb = words[b].length();
            //i,j一起前进，直到找到第一个不同的字母
            while (i < la && j < lb && words[a][i] == words[b][j]) {
                i ++; j ++;
            }
            //如果找到第一个不同字母之前，其中某个串已经结束
            if (i == la || j == lb) {
                //如果是a串先结束，说明a,b这对串不能提供有效的边信息
                if (i == la) continue;
                //如果是b串先结束，说明这两个串的字典序是不合理的，答案不存在
                if (j == lb) return "";
            }
            //如果不是上面的情况，说明a,b这对串能提供有效边信息，建边
            int u = words[a][i] - 'a';
            int v = words[b][j] - 'a';
            g[u].push_back(v);
            d[v] ++;
        }
        
        queue<int> q;

        //把所有入度为0的点入队，注意包含孤立点并去掉无效点
        for (int i = 0; i < 26; i ++ ) {
            if (d[i] == 0 && !g[i].empty()) {
                q.push(i);
            }
        }

        if (q.empty()) return "";

        string res; //结果串，存答案

        while (!q.empty()) {
            int u = q.front();
            res.push_back('a' + u);
            q.pop();
            for (int v : g[u]) {
                if (v == -1) continue;
                d[v] --;
                if (d[v] == 0) q.push(v);
                v = -1;
            }
        }
        if (res.length() == chars.size()) return res;
        else return "";
    }
};
```