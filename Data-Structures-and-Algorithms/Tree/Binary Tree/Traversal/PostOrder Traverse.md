# 二叉树后序遍历

**递归写法**

```c
void postOrder(TreeNode* root, vector<int>& res) {
    if (root == NULL) return;
    postOrder(root->left, res);
    postOrder(root->right, res);
    res.push_back(root->val);
}
vector<int> postorderTraversal(TreeNode* root) {
    vector<int> res;
    postOrder(root, res);
    return res;
}
```

**迭代写法**

基本思路是：后序遍历是“左右根”，$reverse$ 一下就是 "根右左"，就转化为与 "根左右" 类似的写法.

将迭代版前序遍历稍作改动，$left$ 和 $right$ 的语句互换位置，并且在遍历过程中使用一个栈 $rev\_res$ 存输出序列，最后再把栈的元素全部倒入作为 $res$ 的 $vector$，起到一个 $reverse$ 的作用》

```cpp
vector<int> postorderTraversal(TreeNode* root) {
    vector<int> res;
    stack<int> rev_res;
    stack<TreeNode*> stk;
    TreeNode* it = root;
    while (!stk.empty() || it != NULL) {
        while (it != NULL) {
            rev_res.push(it->val);
            stk.push(it);
            it = it->right;
        }
        it = stk.top();
        stk.pop();
        it = it->left;
    }
    while (!rev_res.empty()) {
        res.push_back(rev_res.top());
        rev_res.pop();
    }
    return res;
}
```