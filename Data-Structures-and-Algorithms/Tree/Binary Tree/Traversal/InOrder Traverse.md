# 二叉树中序遍历

```c
/*递归*/
void InOrderRecursion(BiTree T) {
    BiTree p = T;
    if (p!=NULL) {
        InOrderRecursion(p->lchild);
        visit(p);
        InOrderRecursion(p->rchild);
    }
}

/*辅助栈迭代*/
vector<int> inorderTraversal(TreeNode* root) {
    vector<int> res;
    stack<TreeNode*> stk;
    TreeNode* it = root;
    while (!stk.empty() || it != NULL) {
        while (it != NULL) {
            stk.push(it);
            it = it->left;
        }
        it = stk.top();
        stk.pop();
        res.push_back(it->val);
        it = it->right;
    }
    return res;
}
```