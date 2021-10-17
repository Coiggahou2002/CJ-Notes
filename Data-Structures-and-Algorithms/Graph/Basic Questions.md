# 图论基础题

## 力扣-997. 找到小镇的法官

https://leetcode-cn.com/problems/find-the-town-judge/

**题意抽象**

由题意和附加条件可知，本题可以抽象为一幅有向简单图（没有环且没有平行边），需要找到一个入度为 $N-1$ 且出度为 $0$ 的顶点，并且这样的顶点唯一，则符合要求.

**原思路**

先遍历图，找到所有入度为 $N-1$ 的顶点（此过程需要 $O(n^2)$ 的复杂度），放入链表 `judge_list`，再判断链表中元素是否只有 1 个，若不是则不存在法官；若只有 1 个元素，则再遍历一次图，判断该顶点的出度是否为 0.

总的复杂度为 $O(n^2)$

**改进思路**

实际上，只需要遍历一次 `trust[i]` 数组，就可以掌握所有顶点的入度和出度信息，可以开两个数组 `in[]` 和 `out[]` 分别记录入度列和出度列，接下来只要线性查找，每找到一个入度为 $N-1$ 且出度为 $0$ 的顶点，就加入链表，后续操作同原思路.

总时间复杂度为 $O(n)$

**总结**

核心优化点在于——图的遍历方式，获取图特定信息的方式

**代码**

```java
class Solution {
    public int findJudge(int N, int[][] trust) {
        /*Special Judge*/
        if (N == 1) return 1;

        //分别记录编号为下标的入度和出度，下标0的位置不用
        int[] in = new int[1001];
        int[] out = new int[1001];

        //一次遍历统计所有点的入度与出度信息
        for (int k = 0; k < trust.length; k++) {
            out[trust[k][0]] += 1;
            in[trust[k][1]] += 1;
        }

        //查找入度为N-1且出度为0的顶点并加入链表
        LinkedList<Integer> judge_list = new LinkedList<>();
        for (int k = 1; k <= N; k++) {
            if (in[k] == N-1 && out[k] == 0) {
                judge_list.add(k);
            }
        }

				//如果链表只有一个元素，说明法官只有一个，符合要求，否则不符合
        if (judge_list.size() == 1) {
            return judge_list.getFirst();
        }
        return -1;
    }
}
```