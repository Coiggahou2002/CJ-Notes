# 回溯习题

## 力扣 46. 全排列

```cpp
class Solution {
public:
    vector<vector<int>> permute(vector<int>& nums) {
        vector<bool> visited(nums.size(), false);
        vector<int> ans;
        vector<vector<int>> res;
        dfs(nums, visited, res, ans);
        return res;
    }
    void dfs(vector<int>& nums, vector<bool>& visited, vector<vector<int>>& res, vector<int>& ans) {
        if (ans.size() == nums.size()) {
            res.push_back(ans);
            return;
        }
        for (int i = 0; i < nums.size(); i ++ ) {
            if (visited[i]) continue;
            visited[i] = true;
            ans.push_back(nums[i]);
            dfs(nums, visited, res, ans);
            visited[i] = false;
            ans.pop_back();
        }
    }
};
```