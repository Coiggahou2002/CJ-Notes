# 二叉搜索树习题

## 力扣-700. 二叉搜索树中的搜索

https://leetcode-cn.com/problems/search-in-a-binary-search-tree/

在给定的二叉搜索树中找到值等于给定值的节点并返回，简单的二分搜索。

```java
class Solution {
    public TreeNode searchBST(TreeNode root, int val) {
        if (root == null) return null;
        if (val < root.val) return searchBST(root.left,val);
        if (val > root.val) return searchBST(root.right,val);
        else {return root;}
    }
}
```

## 力扣-938. 二叉搜索树的范围和

https://leetcode-cn.com/problems/range-sum-of-bst/

现在几乎已经形成习惯了，见到二叉树问题，先想递归和遍历方式。

此题与二分查找的思想非常类似。

我们用递归书写，主要调整好 Base Case，剩下的交给递归调用栈就好了。

此题是规范的二叉搜索树（即中序遍历展开为升序序列），只需要分类讨论清楚，什么时候去找左子树，什么时候找右子树，什么时候两者都找，什么时候要将自己的节点值加进结果中，以及边界 case 考虑好，即可。

+ 如果 $root.val < [lo..hi]$  ，找右子树
+ 如果 $lo \le root.val \le hi$，将节点值加入结果，然后查找左、右子树
+ 如果 $[lo...hi] < root.val$ ，找左子树

```java
class Solution {
    public int rangeSumBST(TreeNode root, int low, int high) {
        if (root == null) return 0;
        if (root.val < low) return rangeSumBST(root.right,low,high);
        if (root.val > high) return rangeSumBST(root.left,low,high);
        else {
            return root.val + rangeSumBST(root.left,low,high) + rangeSumBST(root.right,low,high);
        }
    }
}
```

## 力扣-701. 二叉搜索树中的插入操作

https://leetcode-cn.com/problems/insert-into-a-binary-search-tree/

题目说了所给数据与二叉树已有的任何一个节点值都不相同，不需要考虑同值的问题。

仍然是递归的思想，当我通过比较得知所给值应该放在节点的哪一边时，就转换到了将所给值插入其中一边的子树的问题，父问题和子问题的结构相同，递归到底即可，关键在 Base Case.

这题我刚开始的 Base Case 写法是 `if (p == null) p = new TreeNode(val);`，这样写是不对的，不能等到一直挖到 `null` 节点了，才建立节点并赋给这个 `null` 节点，这样是行不通的，会导致不断地建立新节点导致爆内存。

正确的 Base Case 应该是，当一直挖到**该插入的地方**的父节点时（比如说，要插入的值比父节点的值要大，而且这个父节点右孩子刚好是空的，有位置放，那么这个父节点就是所谓的“该插入的地方”），就将父节点的左孩子或者右孩子赋值为一个新节点，这样才有上下依附关系，才不会导致新插的节点被孤立。

![image-20211017000308184](C:/Users/Techn/AppData/Roaming/Typora/typora-user-images/image-20211017000308184.png)

之所以另外开一个函数写，是因为题目要求原函数返回整个二叉搜索树的根节点，如果直接在原函数里写递归会很不方便。

```java
class Solution {
    public TreeNode insertIntoBST(TreeNode root, int val) {
        if (root == null) return new TreeNode(val);
        insertNode(root,val);
        return root;
    }
    public void insertNode(TreeNode p, int val) {
        if (val < p.val) {
            if (p.left == null) {
                p.left = new TreeNode(val);
            } else {
                insertNode(p.left,val);
            }
        } else {
            if (p.right == null) {
                p.right = new TreeNode(val);
            } else {
                insertNode(p.right,val);
            }
        }
    }
}
```

## 力扣-108. 将有序数组转换为二叉搜索树

https://leetcode-cn.com/problems/convert-sorted-array-to-binary-search-tree/

给定有序数组，要求构造**平衡的**二叉搜索树.

本质上还是二分查找的模板，只不过与建树结合了起来（二分的性质保证了该方法建出的二叉搜索树一定是平衡的）.

这里采用的划分中点的方式是 `mid = lo + (hi - lo) / 2` ，也就是**中点偏左**。

![](https://cjpark-1304138896.cos.ap-guangzhou.myqcloud.com/note_img/20211017000239.png)

如果是中点偏右，建出来的二叉搜索树是不一样的。

注意，推 Base Case 的过程中，在白纸上老老实实手推出边界条件，不要靠脑子想，容易错。

```java
class Solution {
    public TreeNode sortedArrayToBST(int[] nums) {
        if (nums.length == 0) return null;
        else {
            return arrayToTree(nums,0,nums.length-1);
        }
    }
    public TreeNode arrayToTree(int[] nums, int lo, int hi) {
        if (lo > hi) return null;
        int mid = lo + (hi-lo)/2; 
        TreeNode p = new TreeNode(nums[mid]);
        p.left = arrayToTree(nums,lo,mid-1);
        p.right = arrayToTree(nums,mid+1,hi);
        return p;
    }
}
```