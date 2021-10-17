# 递归

## 力扣-100. 相同的树

https://leetcode-cn.com/problems/same-tree/

典型的递归，两棵树相同当且仅当值相同且左右子树相同，Base Case 写全面即可。

【单节点递归】

```java
class Solution {
    public boolean isSameTree(TreeNode p, TreeNode q) {
        if (p == null && q == null) return true;
        else if (p != null && q == null) return false;
        else if (p == null && q != null) return false;
        else if (p.val == q.val) {
            return isSameTree(p.left,q.left) && isSameTree(p.right,q.right);
        } else {
            return false;
        }
    }
}
```

## 剑指 Offer 27. 二叉树的镜像

https://leetcode-cn.com/problems/er-cha-shu-de-jing-xiang-lcof/

稍作分析可知，可用递归来写——对于二叉树的每个**内点** Node ，将它的左子节点和右子节点交换位置，自顶向下，对每个内点都执行一次这样的操作即可，完美符合递归的思维，用递归书写最方便.

【单节点递归】

![](https://cjpark-1304138896.cos.ap-guangzhou.myqcloud.com/note_img/20211016235650.png)

**递归写法**

```java
class Solution {
    public TreeNode mirrorTree(TreeNode root) {
        if (root == null) return null; //特判截停空参
        FlipNode(root);
        return root;
    }
    public void FlipNode(TreeNode p) {
        if (p == null) {
            return;
        }
        TreeNode tmp = null;
        tmp = p.left;
        p.left = p.right;
        p.right = tmp;
        FlipNode(p.left);
        FlipNode(p.right);
    }
}
```

2021-03-28 写的C语言版本

```cpp
struct TreeNode* invertTree(struct TreeNode* root){

    if (root == NULL) return NULL;
    if (root->left == NULL && root->right == NULL) return root;
		
		//先分别翻转左、右子树，再交换它们（翻过来也写）
    root->left = invertTree(root->left);
    root->right = invertTree(root->right);

		//服了，swap也要自己写，注意这里不能用函数写，改不了实参
    struct TreeNode* tmp;
    tmp = root->left,  root->left = root->right,  root->right = tmp;

    return root;

}
```

**非递归写法（辅助栈或者队列）**

首先初始化，将根节点入栈。约定在每次弹出栈顶元素时对其进行处理，具体处理指——若弹出的节点有孩子，那么将它的左、右孩子入栈，然后交换它左、右孩子（子树）的位置，然后继续弹出、处理......重复上述步骤直到栈空。

其实用队列也是可以的，两者的区别在于：如果用栈，实际操作过程类似于 DFS；如果用队列，实际操作过程类似于 BFS.

![](https://cjpark-1304138896.cos.ap-guangzhou.myqcloud.com/note_img/20211016235727.png)

```java
class Solution {
    public TreeNode mirrorTree(TreeNode root) {
        if (root == null) return null;
        Stack<TreeNode> s = new Stack<TreeNode>();
        s.push(root);
        TreeNode p = root;
        TreeNode tmp;
        while (!s.isEmpty()) {
            p = s.pop();
            if (p != null) {
                s.push(p.left);
                s.push(p.right);
                tmp = p.left;
                p.left = p.right;
                p.right = tmp;
            }
        }
        return root;
    }
}
```

## 剑指 Offer 55 - I. 二叉树的深度

https://leetcode-cn.com/problems/er-cha-shu-de-shen-du-lcof/

$$
maxDepth(root) = \left\{ maxDepth(root.left),\ maxDepth(root.right) \right\}_{max} + 1
$$

由此可得出简洁明了的递归写法，如下

```java
class Solution {
    public int maxDepth(TreeNode root) {
        if (root == null) return 0;
        if (root.left == null && root.right == null) { //识别叶子节点
            return 1;
        }
        return Math.max(maxDepth(root.left),maxDepth(root.right))+1;
    }
}
```

## 力扣-111. 二叉树的最小深度

https://leetcode-cn.com/problems/minimum-depth-of-binary-tree/

此题与上题类似，也是深度最值的问题，同样是用递归解决，很容易想到基本思路，即一棵树的最小高度，等于它的左子树、右子树的最小高度中更小的一个再加一。

但是由于是求最小高度，就会出现一些细节问题。

例如，当一棵子树左儿子为 `null`，右儿子不为空时，如果我们把递归的 Base Case 中 `null` 节点的高度设为返回 0 的话，得到的答案就是错误的。（如下图所示）

![](https://cjpark-1304138896.cos.ap-guangzhou.myqcloud.com/note_img/20211017000021.png)

所以此题的关键在于递归的 Base Case 书写.

**Base Case**

检查当前节点

+ 节点为 `null`，返回 `Integer.MAX_VALUE`
+ 节点无孩子，返回 1
+ 节点有孩子，返回 $\left\{ minDepth(lchild),\, minDepth(rchild) \right\}_{min} + 1$

（写无穷大是为了在 `min{}` 函数选最小的时候不取 `null` 的那一边）

```java
class Solution {
    public int minDepth(TreeNode root) {
        if (root == null) return 0;
        return minDepth2(root);
    }
		//另外开一个函数是为了避开root为null的情况，该情况由原函数拦截，新函数中不考虑
    public int minDepth2(TreeNode p) {
        if (p == null) return Integer.MAX_VALUE;
        if (p.left == null && p.right == null) return 1;
        return Math.min(minDepth2(p.left),minDepth2(p.right)) + 1;
    }
}
```

**补充（广度优先搜索）**

上述解法是 DFS 的思路，下面补充 BFS 的做法。

即用队列逐层遍历节点，一旦找到叶节点，马上返回当前层的深度。

## 剑指 Offer 28. 对称的二叉树

https://leetcode-cn.com/problems/dui-cheng-de-er-cha-shu-lcof/

此题要求：判断给定二叉树是否镜面对称

**原思路**

不妨先将根的右子树反转，再判断根的左子树和右子树是否相同。

这样就将问题拆解成两个已知子问题——反转二叉树 + 判断两二叉树是否相同

```java
class Solution {
    public boolean isSymmetric(TreeNode root) {
        if (root == null) return true;
        if (root.left == null && root.right == null) return true;
        if (root.left == null && root.right != null) return false;
        if (root.right == null && root.left != null) return false;
        reverseTree(root.right);
        return isSame(root.left, root.right);
    }

		/*反转二叉树*/
    public void reverseTree(TreeNode p) {
        if (p == null) return;
        TreeNode tmp = null;
        tmp = p.left;
        p.left = p.right;
        p.right = tmp;
        reverseTree(p.left);
        reverseTree(p.right);
    }

		/*判断给定的两个二叉树是否完全相同*/
    public boolean isSame(TreeNode t1, TreeNode t2) {
        if (t1 != null && t2 != null) {
            if (t1.val == t2.val) {
                return isSame(t1.left, t2.left) && isSame(t1.right, t2.right);
            } else {
                return false;
            }
        } else if (t1 == null && t2 == null) {
            return true;
        } else {
            return false;
        }
    }
}
```

**优化思路——优雅的递归**

如何判断给定两二叉树是否镜面对称？

+ 如果两者都为空，对称
+ 如果一个空一个不空，不对称
+ 如果两者都有左右孩子
  + 两者值相等
    + 左树的左儿子与右树右儿子镜面对称 $\And \And$ 左树右儿子与右树左儿子镜面对称，则对称
    + 否则不对称
  + 两者值不等，不对称

上述为递归函数的文字描述版.

```java
class Solution {
    public boolean isSymmetric(TreeNode root) {
        if (root == null) return true;
        return checkPairSym(root.left, root.right);
    }

    public boolean checkPairSym(TreeNode t1, TreeNode t2) {
        if (t1 == null && t2 == null) return true;
        if (t1 == null && t2 != null) return false;
        if (t1 != null && t2 == null) return false;
        if (t1.val == t2.val) {
            return checkPairSym(t1.left, t2.right) && checkPairSym(t1.right, t2.left);
        } else {
            return false;
        }
    }
}
```

2021-03-29 重刷 C语言版

递归版

```cpp
bool sym(struct TreeNode* t1, struct TreeNode* t2) {
    if (t1 == NULL && t2 == NULL) return true;
    if (t1 == NULL || t2 == NULL) return false;
    return (t1->val == t2->val) && (sym(t1->left, t2->right) && sym(t1->right, t2->left));
}

bool isSymmetric(struct TreeNode* root){
    return sym(root->left, root->right);
}
```

队列迭代版

开一个队列 $Q$，首先将 $root$ 入队 2 次.

队列每轮操作如下：

1. 取出 2 个结点 $u,v$
2. 判断比较
   + 若 $u,v$ 都是 $NULL$，进入下一轮
   + 若 $u,v$ 只有其中一个是 $NULL$，截断，返回 $false$
   + 若 $u,v$ 都不为 $NULL$
     + $u,v$ 的 $val$ 相等
       + $push(u左,v右)$
       + $push(u右,v左)$
       + 进入下一轮
     + $u,v$ 的 $val$ 不相等，截断，返回 $false$

时间复杂度 $O(2n)$，空间复杂度 $O(2n)$，因为底层 $NULL$ 结点也要入队一次.

```cpp
class Solution {
public:
    bool isSymmetric(TreeNode* root) {
        queue<TreeNode*> q;
        q.push(root);
        q.push(root);

        while (!q.empty()) {
            TreeNode *u, *v;
            u = q.front(); q.pop();
            v = q.front(); q.pop();
            if (u == NULL && v == NULL) continue;
            if (u == NULL || v == NULL) return false;
            if (u->val == v->val) {
                q.push(u->left);  q.push(v->right);
                q.push(u->right); q.push(v->left);
            } else {
                return false;
            }
            
        }
        return true;
    }
};
```

## 力扣-617. 合并二叉树

https://leetcode-cn.com/problems/merge-two-binary-trees/

看到二叉树先想递归和 Base-Case ！！！

这道题会卡住我，主要是因为我认为“新建节点并接到父节点上”这件事情和递归不能共存，导致思维混乱，实际上是可以的，本质上还是因为没有理解清楚递归。

递归就像领导下命令干活，从最高级领导逐级往下去下达指示和命令，命令一层一层向下传达，只有基层要老老实实把工作落实好，其他层的领导都只需要干两件事——下达命令以及汇报成果，自顶向下“递”的过程就是逐级下达命令的过程，只有“触底”了，命令下到基层了，真正干实事的人才开始动起来（也就是 Base-Case），基层的活干完了，就会向上汇报，然后逐级向上汇报，一直报到最高领导，领导得到反馈，这整个工作流程才算走完。

```java
class Solution {
    public TreeNode mergeTrees(TreeNode t1, TreeNode t2) {
        if (t1 == null && t2 == null) return null;
        if (t1 != null && t2 == null) return t1;
        if (t1 == null && t2 != null) return t2;
        else {
            TreeNode n = new TreeNode(t1.val + t2.val);
            n.left = mergeTrees(t1.left, t2.left);
            n.right = mergeTrees(t1.right, t2.right);
            return n;
        }
    }
}
```

2021-03-28 写的C版

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     struct TreeNode *left;
 *     struct TreeNode *right;
 * };
 */

struct TreeNode* mergeTrees(struct TreeNode* root1, struct TreeNode* root2){
    if (root1 == NULL && root2 == NULL) return NULL;
    if (root1 == NULL) return root2;
    if (root2 == NULL) return root1;
    root1->val += root2->val;
    root1->left = mergeTrees(root1->left, root2->left);
    root1->right = mergeTrees(root1->right, root2->right);
    return root1;
}
```

## 力扣-112. 路径总和

https://leetcode-cn.com/problems/path-sum/

此题很容易想到用递归解决。难点在于**对所谓“路径”的识别**，也就是必须是一直往下**查到叶结点**，刚好经过的所有节点值之和等于 `sum`，才能叫“路径总和”，也就是说，不能够查到一半发现经过的节点和等于 `sum` 就停止。就像下面的用例。

![](https://cjpark-1304138896.cos.ap-guangzhou.myqcloud.com/note_img/20211017000047.png)

```java
class Solution {
    public boolean hasPathSum(TreeNode root, int sum) {
        if (root == null) return false; //拦住空用例和非法路径
        if (root.left == null && root.right == null) { //判断叶子节点
            if (sum == root.val) return true;
            else return false;
        } else {
            return hasPathSum(root.left, sum - root.val) || hasPathSum(root.right, sum - root.val);
        }
    }
}
```



# 中序遍历

## 剑指 Offer 54. 二叉搜索树的第k大节点

https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-di-kda-jie-dian-lcof/

我们知道，二叉搜索树的中序遍历序列为升序有序序列，那么我们考虑**倒中序**序列遍历（即：右、根、左），即可得到降序序列，遍历序列用数组存储后，返回数组下标为 $k-1$ 的元素值即可.

```java
class Solution {
    public int kthLargest(TreeNode root, int k) {
        LinkedList<Integer> order_list = new LinkedList<>();
        InOrderTraverse(root,order_list);
        return order_list.get(k-1);
    }
    public void InOrderTraverse(TreeNode root, List order_list) {
        if (root != null) {
            InOrderTraverse(root.right,order_list);
            order_list.add(root.val);
            InOrderTraverse(root.left,order_list);
        }
    }
}
```



# 层次遍历

## 力扣-102. 二叉树的层序遍历

https://leetcode-cn.com/problems/binary-tree-level-order-traversal/

```java
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        if (root == null) {
            return new LinkedList<List<Integer>>();
        }
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

## 剑指 Offer 32 - I. 从上到下打印二叉树

https://leetcode-cn.com/problems/cong-shang-dao-xia-da-yin-er-cha-shu-lcof/

此题就是标准的二叉树中序遍历模板题，要求按自顶向下、每层从左到右遍历后，输出整个二叉树结点值的序列，但是有个小细节值得注意。

一般来讲，层序遍历都使用队列来安排节点的遍历顺序，这是没有问题的。

但是，我们习惯于用 `ArrayList` 或者 `LinkedList` 来存放遍历得到的序列，但此题要求函数返回值为 `int[]` 型数组，本来我想使用 `ArrayList` 的 `toArray()` 方法来将其转换成数组，却发现转换出来的是 `Object[]` 型或者 `Integer[]` 型数组，无法转换成 `int[]` 型数组，如果按照题目给的范围，弃用 ArrayList，直接开一个 `int[1001]` 数组来存放结果，返回时又会出现大量冗余 0 导致不通过，所以只能用最笨的办法——先用 `ArrayList` 存储结果序列，再开一个 `int[array_list.size()]` 数组，再循环遍历赋值。

```java
class Solution {
    public int[] levelOrder(TreeNode root) {
        /*Special*/
        ArrayList<Integer> res_list = new ArrayList<>();
        if (root == null) {
            return new int[0];
        }
        
        Queue<TreeNode> q = new LinkedList<>();
        q.add(root);
        TreeNode tmp = null;
        int i = 0;
        while (!q.isEmpty()) {
            tmp = q.poll();
            res_list.add(tmp.val);
            if (tmp.left != null) q.add(tmp.left);
            if (tmp.right != null) q.add(tmp.right);
        }
        int[] res = new int[res_list.size()];
        for (int k = 0; k < res_list.size(); k++) {
            res[k] = res_list.get(k);
        }
        return res;
    }
}
```

## 剑指 Offer 32 - II. 从上到下打印二叉树 II

https://leetcode-cn.com/problems/cong-shang-dao-xia-da-yin-er-cha-shu-ii-lcof/

此题依然是二叉树的层序遍历，不同的是，需要将打印结果分层输出，每一层的节点值序列对应一个数组，最终输出一个数组列表。

此题的难点在于：BFS 层序遍历的过程中使用的是队列，**如何在队列中区分不同层的节点？**

**原思路**

使用两个队列 `q1` 和 `q2`，在处理 `q1` 队中元素时，将这些元素的孩子都添加到 `q2`，然后处理 `q2`，将 `q2` 队中元素的孩子都添加到 `q1`，如此循环直到两个队列都为空，则树已遍历完成。

```java
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        //特判
        if (root == null) return new LinkedList<List<Integer>>();

        List<List<Integer>> res_list = new LinkedList<>();
        Queue<TreeNode> q1 = new LinkedList<>();
        Queue<TreeNode> q2 = new LinkedList<>();
        q1.add(root);
        TreeNode tmp = null;
        while (!q1.isEmpty() || !q2.isEmpty()) {
            List<Integer> tmp_list1 = new LinkedList<>();
            List<Integer> tmp_list2 = new LinkedList<>();
            while (!q1.isEmpty()) {
                tmp = q1.peek();
                q1.poll();
                tmp_list1.add(tmp.val);
                if (tmp.left != null) q2.add(tmp.left);
                if (tmp.right != null) q2.add(tmp.right);
            }
            if (tmp_list1.size()!=0) res_list.add(tmp_list1);
            while (!q2.isEmpty()) {
                tmp = q2.peek();
                q2.poll();
                tmp_list2.add(tmp.val);
                if (tmp.left != null) q1.add(tmp.left);
                if (tmp.right != null) q1.add(tmp.right);
            }
            if (tmp_list2.size()!=0) res_list.add(tmp_list2);
        }
        return res_list;
    }
}
```

**改进思路**

使用**长度**这个量来区分位于同一队列中的不同层次节点，在当前层刚要开始处理时，获取队列长度并储存到 `size` 中，用 `size` 限定 for 循环次数，那么在弹出节点、添加节点的过程中，虽然队列的长度会动态变化，但 for 循环的次数在一开始就规定了，所以当前层有多少个节点，就会处理多少个，后面新添加的，留给下一个 for 循环，下一个 for 循环初始化时，再获取一次 `size` ，以此类推...

```java
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        //特判
        if (root == null) return new LinkedList<List<Integer>>();

        Queue<TreeNode> q = new LinkedList<>();
        List<List<Integer>> res_list = new LinkedList<>();
        q.add(root);
        TreeNode tmp = null;

        while (!q.isEmpty()) {
            List<Integer> tmp_list = new LinkedList<>(); //每行节点值序列
            int len = q.size(); //获取当前层节点数，固定for循环次数
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

## 剑指 Offer 32 - III. 从上到下打印二叉树 III

https://leetcode-cn.com/problems/cong-shang-dao-xia-da-yin-er-cha-shu-iii-lcof/

略

## 力扣-103. 二叉树的锯齿形层序遍历

https://leetcode-cn.com/problems/binary-tree-zigzag-level-order-traversal/

仍然是层序遍历，但此题要求奇数层从左到右、偶数层从右到左输出每层节点值序列。

一开始的想法是在对遍历过程稍作手脚，但发现不太好实现。

后来发现，遍历时仍然按照分层中序遍历的模板来写即可，只需要对最终的结果序列集中的偶数行作翻转操作即可，可以使用 `Collections` 工具类的 `Collections.reverse(list)` 来完成

再后来，发现这个操作很慢...

改进的方法是：谁说遍历的时候一定要在 `tmp_list` 的末尾添加元素呢？奇数层要求从左到右，那我存储序列时就每次都在 `tmp_list` 的末尾添加元素；偶数层要求从右到左，那我就每次在 `tmp_list` 的头部添加元素，这样按顺序遍历时得出来的就是反向序列。

毕竟 `addFirst()` 和 `addLast()` 都是 `LinkedList` 的**特有方法**，也是**链表的优势**，我们不能将其浪费啊。

```java
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        if (root == null) return new LinkedList<List<Integer>>();

        Queue<TreeNode> q = new LinkedList<>();
        List<List<Integer>> res_list = new LinkedList<>();
        TreeNode tmp = null;
        q.add(root);
        int count = 1;
        while (!q.isEmpty()) {
            LinkedList<Integer> tmp_list = new LinkedList<Integer>();
            int len = q.size();
            for (int i = len; i >= 1; i--) {
                tmp = q.poll();

								/*---------添加部分--------*/
                if (count % 2 == 0) {
                    tmp_list.addFirst(tmp.val);
                } else {
                    tmp_list.addLast(tmp.val);
                }
								/*-------------------------*/

                if (tmp.left != null) q.add(tmp.left);
                if (tmp.right != null) q.add(tmp.right);
            }
            count++;
            res_list.add(tmp_list);
        }
        return res_list;
    }
}
```

## 力扣-637. 二叉树的层平均值

https://leetcode-cn.com/problems/average-of-levels-in-binary-tree/

此题仍然是标准层序遍历，把每层放入 `sum` 中算平均值即可.

```java
class Solution {
    public List<Double> averageOfLevels(TreeNode root) {
        /*Special Judge*/
        if (root == null) return new LinkedList<Double>();

        List<Double> res = new LinkedList<>();
        Queue<TreeNode> q = new LinkedList<>();
        q.add(root);
        TreeNode tmp = null;
        double sum = 0;
        while (!q.isEmpty()) {
            int size = q.size();
            for (int i = size; i >= 1; i--) {
                tmp = q.poll();
                sum += tmp.val;
                if (tmp.left != null) q.add(tmp.left);
                if (tmp.right != null) q.add(tmp.right);
            }
            res.add(sum/size);
            sum = 0;
        }
        return res;
    }
}
```

## 力扣-515. 在每个树行中找最大值

https://leetcode-cn.com/problems/find-largest-value-in-each-tree-row/

层序遍历 + 记录 `max`

```java
class Solution {
    public List<Integer> largestValues(TreeNode root) {
        if (root == null) return new LinkedList<Integer>();
        List<Integer> res = new LinkedList<>();
        Queue<TreeNode> q = new LinkedList<>();
        q.add(root);
        TreeNode tmp = null;
        while (!q.isEmpty()) {
            int size = q.size();
            int max = Integer.MIN_VALUE;
            for (int i = size; i >= 1; i--) {
                tmp = q.poll();
                max = Math.max(max,tmp.val);
                if (tmp.left != null) q.add(tmp.left);
                if (tmp.right != null) q.add(tmp.right);
            }
            res.add(max);
            max = 0;
        }
        return res;
    }
}
```

## 力扣-1161. 最大层内元素和

https://leetcode-cn.com/problems/maximum-level-sum-of-a-binary-tree/

层序遍历包装题。唯一需要注意的一点是，不需要开一个 List 来存放每层的和，因为只需要知道最大的那个，使用两个变量，一个存最大，一个迭代覆盖即可。

```java
class Solution {
    public int maxLevelSum(TreeNode root) {
        if (root == null) return 0;
        Queue<TreeNode> q = new LinkedList<>();

        TreeNode tmp = null;
        int level_sum = 0;      //暂存每层元素和
        int max_index = 1;      //暂存和最大的层编号
        int max_val = root.val; //暂存层最大和
        int level = 0;          //层遍历计数器
        
        q.add(root);

        while (!q.isEmpty()) {
            int size = q.size();
            for (int i = 1; i <= size; i++) {
                tmp = q.poll();
                level_sum += tmp.val;
                if (tmp.left != null) q.add(tmp.left);
                if (tmp.right != null) q.add(tmp.right);
            }
            level++;
            if (level_sum > max_val) {
                max_val = level_sum;
                max_index = level;
            }
            //每遍历一层，level_num被覆盖一次，不需要开数组来存每层的和
            level_sum = 0;
        }
        return max_index;
    }
}
```

## 力扣-199. 二叉树的右视图

https://leetcode-cn.com/problems/binary-tree-right-side-view/

给定一棵二叉树，想象你站在它的右侧，返回你能看到的节点序列。

此题乍一看，难道不是对树的右儿子一直挖到底就可以了吗？

但仔细一想，如果右子树深度很浅，左子树很深，那么人从右边就会看到左子树的树叶。

![](https://cjpark-1304138896.cos.ap-guangzhou.myqcloud.com/note_img/20211016235047.png)

所以，此题还是得用层序遍历来解决。但我们可以对层序遍历稍作修改，**比如说把添加左、右孩子的顺序倒过来，就可以实现二叉树每层从右到左的层序遍历**，然后我们将每层遍历到的第一个节点值加入结果集，即可。

```java
class Solution {
    public List<Integer> rightSideView(TreeNode root) {
        if (root == null) return new LinkedList<Integer>();
        List<Integer> res = new LinkedList<>();
        Queue<TreeNode> q = new LinkedList<>();
        TreeNode tmp = null;
        q.add(root);
        while (!q.isEmpty()) {
            int size = q.size();

            /*每层处理队头元素（即最右边）*/
						tmp = q.peek();
            res.add(tmp.val);
						/*---------------------------*/

            for (int i = size; i >= 1; i--) {
                tmp = q.poll();
								//BFS中，添加左右孩子顺序调换，实现反向层序遍历
                if (tmp.right != null) q.add(tmp.right);
                if (tmp.left != null) q.add(tmp.left);
            }
        }
        return res;
    }
}
```

## 力扣-107. 二叉树的层序遍历 II

https://leetcode-cn.com/problems/binary-tree-level-order-traversal-ii/

要求返回层序遍历的列表数组，但要求顺序是自底向上。

其实很简单，原层序遍历模板的队列不需要变，只需要对结果集做手脚，每当遍历完一层，将该层节点值序列加入结果集链表的头部而不是尾部，即 `addFirst()` 而不是 `addLast()` ，每次都将位于更底层的序列放在结果集头部，最终呈现的就是自底向上。

```java
class Solution {
    public List<List<Integer>> levelOrderBottom(TreeNode root) {
        if (root == null) return new LinkedList<List<Integer>>();

        LinkedList<List<Integer>> res_list = new LinkedList<>();

        Queue<TreeNode> q = new LinkedList<>();
        TreeNode tmp = null;

        q.add(root);
        while (!q.isEmpty()) {
            int size = q.size();
            List<Integer> tmp_list = new LinkedList<>();
            for (int i = size; i >= 1; i--) {
                tmp = q.poll();
                tmp_list.add(tmp.val);
                if (tmp.left != null) q.add(tmp.left);
                if (tmp.right != null) q.add(tmp.right);
            }
            res_list.addFirst(tmp_list);
        }
        return res_list;
    }
}
```

## 力扣-116. 填充每个节点的下一个右侧节点指针

https://leetcode-cn.com/problems/populating-next-right-pointers-in-each-node/

## 力扣-117. 填充每个节点的下一个右侧节点指针 II

https://leetcode-cn.com/problems/populating-next-right-pointers-in-each-node-ii/

两题的要求是一样的，只不过第一题给的二叉树是满二叉树，第二题是普通情形的二叉树，而我们使用的层序遍历，是不受上述条件限制的。所以层次遍历的解法对这两题都适用。
![image-20211016235114953](C:/Users/Techn/AppData/Roaming/Typora/typora-user-images/image-20211016235114953.png)

显然 BFS 分层遍历，对于每一层的节点队列，取一个节点，出队，将其 `next` 设为下一个节点（队头节点），如果是当前层最后一个节点，则连接 `null`

```java
class Solution {
    public Node connect(Node root) {
        if (root == null) return null;
        Queue<Node> q = new LinkedList<>();
        q.add(root);
        Node tmp = null;
        while (!q.isEmpty()) {
            int size = q.size();
            for (int i = 1; i <= size; i++) {
                tmp = q.poll();
                if (i != size) {
                    tmp.next = q.peek();
                } else {
                    tmp.next = null;
                }
                if (tmp.left != null) q.add(tmp.left);
                if (tmp.right != null) q.add(tmp.right);
            }
        }
        return root;
    }
}
```