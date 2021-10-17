# 查找 | 二叉排序树

## 定义

二叉排序树的递归定义：

+ 左子树所有节点的值都小于根节点值
+ 右子树所有节点的值都大于根节点值
+ 左、右子树也是二叉排序树

## 性质

1.对一棵 BST 进行中序遍历，可以得到升序的有序序列

2.中序遍历的第一个值，是原本序列中的最小值；中序遍历最后一个值，是原本序列中的最大值.

## 查找

略

## 插入节点

略

## 删除节点

迭代版本代码

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    TreeNode* deleteNode(TreeNode* root, int key) {

        TreeNode* fa = root;
        TreeNode* it = root;
        while (it != NULL && it->val != key) {
            if (key < it->val) {
                fa = it;
                it = it->left;
            }
            else if (key > it->val) {
                fa = it;
                it = it->right;
            }
            else break;
        }

        if (it == NULL) return root; //找不到，无需删除

        if (fa == it) {
            if (!it->left && !it->right) return NULL;
            else if (!it->left && it->right) return it->right;
            else if (it->left && !it->right) return it->left;
            else {
                TreeNode* lctr_fa = it;
                TreeNode* lctr = it->right;
                while (lctr->left != NULL) {
                    lctr_fa = lctr;
                    lctr = lctr->left;
                }
                swap(it->val, lctr->val);
                if (lctr_fa->left == lctr) {
                    lctr_fa->left = lctr->right;
                } else {
                    lctr_fa->right = lctr->right;
                }
            }
            return fa;
        }

        bool flag;
        if (fa->left == it) flag = 0;
        else flag = 1;

        //如果找到了，分类讨论
        if (it->left == NULL && it->right == NULL) {
            if (!flag) fa->left = NULL;
            else fa->right = NULL;
        } else if (it->left == NULL && it->right != NULL) {
            if (!flag) fa->left = it->right;
            else fa->right = it->right;
        } else if (it->left != NULL && it->right == NULL){
            if (!flag) fa->left = it->left;
            else fa->right = it->left;
        } else { //左右都有子树的情况
            TreeNode* lctr_fa = it;
            TreeNode* lctr = it->right;
            while (lctr->left != NULL) {
                lctr_fa = lctr;
                lctr = lctr->left;
            }
            swap(it->val, lctr->val);
            if (lctr_fa->left == lctr) {
                lctr_fa->left = lctr->right;
            } else {
                lctr_fa->right = lctr->right;
            }
        }
        
        return root;
    }
};
```