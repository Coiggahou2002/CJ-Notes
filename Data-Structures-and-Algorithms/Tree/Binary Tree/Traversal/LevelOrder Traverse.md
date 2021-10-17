# 二叉树层序遍历

## 模板

### 不分层遍历

```java
class Solution {
    public List<Integer> levelOrder(TreeNode root) {
        if (root == null) return new LinkedList<List<Integer>>();
        Queue<TreeNode> q = new LinkedList<>();
        List<Integer> res_list = new LinkedList<>();
        TreeNode tmp = null;
        q.add(root);
        while (!q.isEmpty()) {
            tmp = q.poll();
            res_list.add(tmp.val);
            if (tmp.left != null) q.add(tmp.left);
            if (tmp.right != null) q.add(tmp.right);
        }
        return res_list;
    }
}
```

### 分层遍历

```java
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        if (root == null) return new LinkedList<List<Integer>>();
        Queue<TreeNode> q = new LinkedList<>();
        List<List<Integer>> res_list = new LinkedList<>();
        TreeNode tmp = null;
        q.add(root);

        while (!q.isEmpty()) {
            List<Integer> tmp_list = new LinkedList<>();
            int len = q.size();
            for (int i = len; i >= 1; i--) {
                tmp = q.poll();
                tmp_list.add(tmp.val);
                if (tmp.left != null) q.add(tmp.left);
                if (tmp.right != null) q.add(tmp.right);
            }
            res_list.add(tmp_list);
        }
        return res_list;
    }
}
```