# 广搜习题

## 力扣 1730. 获取食物的最短路径

https://leetcode-cn.com/problems/shortest-path-to-get-food/

```cpp
class Solution {
public:
		//判断点坐标是否可走
    bool validPoint(int i, int j, vector<vector<char> > & grid) {
        return i >= 0 && i < grid.size() && j >= 0 && j < grid[0].size() && grid[i][j] != 'X';
    }
    int bfs(vector<vector<char> > & grid) {
				//广搜队列，存坐标
        queue<pair<int,int> > q;

				//找起点
        for (int i = 0; i < grid.size(); i ++)
            for (int j = 0; j < grid[0].size(); j ++)
                if (grid[i][j] == '*')  q.push({i, j});

				//记录BFS层数，层数即为步数
        int route_cnt = -1;

        while (!q.empty()) {
						//最外面套个for，控制BFS的层数，每走一层，路径长度加一
            for (int k = q.size(); k; k --) {
                int x = q.front().first;
                int y = q.front().second;
                q.pop();
								//一旦搜到食物，必然是最短路，截断返回
                if (validPoint(x, y, grid) && grid[x][y] == '#') {
                    return ++route_cnt;
                }
								//坐标不管合不合法，先入队，合法性判断留给出队的时候再判
                if (validPoint(x, y, grid) && (grid[x][y] == 'O' || grid[x][y] == '*')) {
                    grid[x][y] = '&'; //一定要记得打上已访问标记
                    q.push({x - 1, y});
                    q.push({x, y + 1});
                    q.push({x, y - 1});
                    q.push({x + 1, y}); 
                }
            }
            route_cnt++;
        }
        return -1;
    }
    int getFood(vector<vector<char>>& grid) {
        return bfs(grid);
    }
};
```

## 面试题 16.19. 水域大小

https://leetcode-cn.com/problems/pond-sizes-lcci/

此题用并查集会更快，这里给出BFS代码

```cpp
class Solution {
public:
    bool validPoint(int i, int j, vector<vector<int>>& land) {
        return i >= 0 && i < land.size() && j >= 0 && j < land[0].size() && land[i][j] == 0;
    }
    int bfs(vector<vector<int>>& land, int i, int j) {
        queue<pair<int,int> > q;
        q.push({i, j});
        int pool_size = 0;
        while (!q.empty()) {
            int x = q.front().first;
            int y = q.front().second;
            q.pop();
            if (validPoint(x, y, land)) {
                pool_size++;
                land[x][y] = -1; //已访问标记
                q.push({x - 1, y - 1});
                q.push({x - 1, y});
                q.push({x - 1, y + 1});
                q.push({x, y - 1});
                q.push({x, y + 1});
                q.push({x + 1, y - 1});
                q.push({x + 1, y});
                q.push({x + 1, y + 1});
            }
        }
        return pool_size;
    }
    vector<int> pondSizes(vector<vector<int>>& land) {
        vector<int> ans;
        for (int i = 0; i < land.size(); i ++ )
            for (int j = 0; j < land[0].size(); j ++)
                if (land[i][j] == 0) ans.push_back(bfs(land, i, j));

        sort(ans.begin(), ans.end());
        return ans; 
    }
};
```

## 力扣 200. 岛屿数量

https://leetcode-cn.com/problems/number-of-islands/

此题可并查集、DFS，此处给出BFS写法

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