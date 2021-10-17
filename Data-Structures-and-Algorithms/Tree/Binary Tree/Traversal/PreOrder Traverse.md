# 二叉树前序遍历

```c
/*递归*/
void PreOrderRecursion(BiTree T) {
    BiTree p = T;
    if (p!=NULL) {
        visit(p);
        PreOrderRecursion(p->lchild);
        PreOrderRecursion(p->rchild);
    }
}

/*辅助栈迭代*/
vector<int> preorderTraversal(TreeNode* root) {
    vector<int> res;
    if (root == NULL) return res;
    stack<TreeNode*> stk;
    TreeNode* it = root;
    while (!stk.empty() || it != NULL) {
        while (it != NULL) {
            res.push_back(it->val);
            stk.push(it);
            it = it->left;
        }
        it = stk.top();
        stk.pop();
        it = it->right;
    }
    return res;
```