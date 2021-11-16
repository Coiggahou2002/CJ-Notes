# 典型例题

[toc]

## 力扣 1290. 二进制链表转整数

https://leetcode-cn.com/problems/convert-binary-number-in-a-linked-list-to-integer/

**辅助栈法**

用辅助栈将链表数据反向取出，按转换算法转成十进制。

```java
class Solution {
    public int getDecimalValue(ListNode head) {
        if (head.next == null) {
            return head.val;
        }
        //以下处理的链表长度大于1
        int p = 0;
        int count = 0;
        Stack<Integer> stk = new Stack<Integer>();
        ListNode x;
        for (x = head; x != null; x = x.next) {
            stk.push(x.val);
            count++;
        }
        int sum = 0;
        for (int i = 0; i < count; i++) {
            sum += stk.pop() * Math.pow(2,p);
            p++;
        }
        return sum;
    }
}
```
**移位法**

顺序遍历链表，每次将 `sum` 左移一位，再加上 `x.val`.

```java
class Solution {
    public int getDecimalValue(ListNode head) {
        if (head.next == null) return head.val;
        int sum = 0;
        ListNode x;
        for (x = head; x != null; x = x.next) {
            sum = (sum << 1) + x.val;
        }
        return sum;
    }
}
```

## 力扣 876. 链表的中间节点

https://leetcode-cn.com/problems/middle-of-the-linked-list/

**操作方法**

此题是典型的快慢指针题，快指针 `fast` 和慢指针 `slow` 从同一起点出发，`fast` 每次走2步，`slow` 每次走1步，当 `fast` 走到链表尾时，`slow` 正好处于链表中点，剩余的细节用 `corner case` 微调即可。


**代码实现**

```java
class Solution {
    public ListNode middleNode(ListNode head) {
        if (head.next == null) return head; //单元素链表，返回自己
        if (head.next.next == null) return head.next;
        //以下处理的链表长度至少为3
        ListNode slow, fast;
        slow = head;
        fast = head;
        while (fast != null && fast.next != null) {
            fast = fast.next.next;
            slow = slow.next;
        }
        return slow;
    }
}
```

**拓展**

如果对快慢指针步长的倍数关系进行调整，应该还能够实现返回链表的几分之一位置的节点，具体实现待补充。

## 剑指Offer 22. 返回链表倒数第k个节点

https://leetcode-cn.com/problems/lian-biao-zhong-dao-shu-di-kge-jie-dian-lcof/

**两次遍历法**

先扫描整个链表获取长度 `count` ，重新扫描一次到 `count - k` 位置即可获取倒数第k个节点。

```java
class Solution {
    public ListNode getKthFromEnd(ListNode head, int k) {
        if (head == null) return null;
        if (head.next == null) return head;

        ListNode x;
        int count = 0;
        for (x = head; x.next != null; x = x.next) {
            count++;
        }
        x = head;
        for (int i = 0; i <= count - k; i++) {
            x = x.next;
        }
        return x;
    }
}
```
**定距同步双指针**

开局先定义两个指针 `p1` 和 `p2`，都指向 `head` ，然后控制它们起始距离为 `k`，然后同步前进，当领先的指针走到表尾时，后面的指针恰好到达第k个节点。

```java
class Solution {
    public ListNode getKthFromEnd(ListNode head, int k) {
        if (head == null) return null;
        if (head.next == null) return head;

        ListNode p1;
        ListNode p2;
        p1 = head;
        p2 = head;
        for (int i = 0; i < k; i++) {
            p2 = p2.next;
        }
        while (p2 != null) {
            p1 = p1.next;
            p2 = p2.next;
        }
        return p1;
    }
}
```

## 力扣 237. 删除链表中的给定节点

https://leetcode-cn.com/problems/delete-node-in-a-linked-list/

**操作方法**

此题传入参数为要删除的节点，这意味着我们**无法访问之前的节点**，也就无法通过修改前面节点的指向来实现删除。

对于链表这种不连续存储的数据结构来讲，**我们只关心它存储的值**，所以我们可以有如下操作。

不妨记传入的欲删除节点为 `x` ，它后面的两个节点依次为 `y` , `z`。

先将 `y` 的 `val` 赋给 `x`，再将 `x` 指向 `z` ，即可删除 `y` 。

**代码实现**

```java
class Solution {
    public void deleteNode(ListNode node) {
        node.val = node.next.val;
        node.next = node.next.next;
    }
}
```

## 力扣 83. 删除排序链表中的重复元素

https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list/

快慢指针，与一维数组的快慢指针操作非常相似，只需要在处理完之后，将 `slow` 所指节点的 `next` 设为 `null` ，将后面的多余部分链表丢弃即可。
```java
class Solution {
    public ListNode deleteDuplicates(ListNode head) {
        //针对长度0、1、2的链表写特判
        if (head == null) return head;
        if (head.next == null) return head;
        if (head.next.next == null) {
            if (head.val == head.next.val) {
                head.next = null;
                return head;
            } else {
                return head;
            }
        }
        //快慢指针
        ListNode slow, fast;
        slow = head;
        fast = head;
        while (fast.next != null) {
            while (slow.val == fast.val && fast.next != null) {
                fast = fast.next;
            }
            if (fast.next != null) {
                slow = slow.next;
                slow.val = fast.val;
            }
        }
        if (fast.val > slow.val) { 
            //注意，此处只有写大于号才能准确判定还有未出现的数需要前移
            slow = slow.next;
            slow.val = fast.val;
        }
        slow.next = null;
        return head;
    }
}
```

## 剑指Offer 06. 从尾到头打印链表

https://leetcode-cn.com/problems/cong-wei-dao-tou-da-yin-lian-biao-lcof/

使用辅助栈，遍历链表，全部压入再逐个弹出。

```java
class Solution {
    public int[] reversePrint(ListNode head) {
        Stack<Integer> stk = new Stack<Integer>();
        ListNode x;
        int len = 0;
        for (x = head; x != null; x = x.next) {
            stk.push(x.val);
            len++;
        }
        int[] rec = new int[len];
        for (int i = 0; i < len; i++) {
            rec[i] = stk.pop();
        }
        return rec;
    }
}
```

## 力扣 206. 反转链表

**三指针**

```cpp
struct ListNode* reverseList(struct ListNode* head){
    //判空
    if (head == NULL) return NULL;

    struct ListNode *pre, *cur, *ne;
    pre = NULL;
    cur = head;
    ne = head->next;

    while (ne) {
        cur->next = pre;
        pre = cur;
        cur = ne;
        ne = ne->next;
    }

    //最后节点指向还没改，补丁
    cur->next = pre;

    return cur;
}
```

**辅助栈法**
```java
class Solution {
    public ListNode reverseList(ListNode head) {
        if (head == null) return null;
        if (head.next == null) return head;
        Stack<Integer> stk = new Stack<Integer>();
        ListNode x,y;
        for (x = head; x != null; x = x.next) {
            stk.push(x.val);
        }
        for (y = head; y != null; y = y.next) {
            y.val = stk.pop();
        }
        return head;
    }
}
```

递归写法

```cpp
struct ListNode* reverseList(struct ListNode* head){\
    if (head == NULL || head->next == NULL) return head;
    struct ListNode* tmp = reverseList(head->next);
    head->next->next = head;
    head->next = NULL;
    return tmp;
}
```

## 力扣 141. 环形链表

判断链表是否有环，双指针.

```cpp
bool hasCycle(struct ListNode *head) {
    if (head == NULL) return false;
    struct ListNode* slow = head;
    struct ListNode* fast = head->next;
    while (fast && fast->next) {
        fast = fast->next->next;
        slow = slow->next;
        if (fast == slow) return true;
    }
    return false;
}
```

## 力扣 160. 相交链表

判断两个链表是否相交，若相交，返回第一个相交节点的引用，否则返回 NULL

方法一  哈希表 `unordered_map`

先遍历链表 A，记录 A 的所有结点的地址到哈希表中，然后遍历 B，边遍历边检索哈希表，一旦发现某个结点地址被记录过，那么它必然是第一个相交结点，返回该节点即可.

```cpp
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        if (headA == NULL || headB == NULL) return NULL;

        unordered_map<ListNode*, bool> recmap;

        ListNode* it = headA;
        while (it) {
            recmap.emplace(it, true);
            it = it->next;
        }

        it = headB;
        while (it) {
            if (recmap[it]) return it;
            it = it->next;
        }

        return NULL;
    }
};
```

方法二  双指针相遇法

初始时令双指针 $itA,itB$ 分别指向 $A,B$ 的头结点，然后同步前进，一旦其中任何一个指针（比如指针 $A$）到达 $null$，就让它回到对方链表的头结点（如 $A$ 回到 $B$ 的头结点），然后继续两者同步前进.

如此循环，若链表有相交，两者必然会在某时刻相遇于第一个相交结点.

若链表不相交，两者必然相遇于 $null$.

```cpp
struct ListNode *getIntersectionNode(struct ListNode *headA, struct ListNode *headB) {
    struct ListNode *itA, *itB;
    itA = headA;
    itB = headB;
		
    while (itA != itB) {
        //两者都不为null且不相等时，同步前进
        while (itA && itB && itA != itB) {
            itA = itA->next;
            itB = itB->next;
        }
        //每次同步前进到有其中一个指针为null，则重定向该指针
        if (itA == NULL && itB != NULL) itA = headB;
        if (itB == NULL && itA != NULL) itB = headA;
    }
    //itA与itB相等时退出while，判断itA或itB，null则不交，非null则交
    return itA;
}
```

## 力扣 138. 复制带随机指针的链表

[https://leetcode-cn.com/problems/copy-list-with-random-pointer/](https://leetcode-cn.com/problems/copy-list-with-random-pointer/)

**我的做法**

此题如果只是复制链表，那就是大水题，所以随机指针域的复制是难点所在，难就难在——新链表的每个节点的随机指针域指向的也是新链表中的节点，只不过节点与节点之间的随机指针指向关系要跟原链表一致.

考虑用双指针同步迭代两次链表，第一次拷贝 $val$ 并建立整个链表框架，同时用哈希表建立原链表和新链表对应节点的一一映射，第二次迭代链表时，利用建立的映射，拷贝 $random$ 域.

时间复杂度 $O(n)$，空间复杂度 $O(n)$

```cpp
/*
// Definition for a Node.
class Node {
public:
    int val;
    Node* next;
    Node* random;
    
    Node(int _val) {
        val = _val;
        next = NULL;
        random = NULL;
    }
};
*/

class Solution {
public:
    Node* copyRandomList(Node* head) {
        if (head == NULL) return NULL;
        
        //存储原链表每个结点到新链表每个结点的地址映射
        unordered_map<Node*, Node*> address;
        
        //新链表头节点
        Node* new_head = new Node(head->val);
        
        //双指针同步迭代
        Node *it1 = head,  *it2 = new_head;
        while (it1 && it1->next) {
            address.emplace(it1, it2); //添加地址映射
            it2->next = new Node(it1->next->val); //复制下个节点值
            it1 = it1->next;
            it2 = it2->next;
        }
        //最后一个，补丁
        address.emplace(it1, it2);
        it2->next = NULL; //结尾记得接NULL

        //重置两个指针
        it1 = head;   it2 = new_head;
        
        //再次双指针迭代
        while (it1) {
            if (it1->random == NULL) it2->random = NULL;
            else it2->random = address.at(it1->random);
            it1 = it1->next;
            it2 = it2->next;
        }

        return new_head;
    }
};
```

**更优的解法**

第一次遍历链表，将每个拷贝好 $val$ 的新节点插在旧链表对应节点的旁边（右边）.

第二次遍历链表，拷贝 $random$ 域，此处已经无须映射，因为**新旧节点的相邻关系**，假若旧节点 $A.random = C$，则新节点 $A'.random = C$，而 $A$ 就是 $A.next$， $C'$ 就是 $C.next$，相当于用这样一种结构形式来模拟了哈希表的映射功能。

最后，拷贝完 $random$ 域后，记得**将新旧链表脱钩**.

时间复杂度 $O(n)$ ，空间复杂度 $O(1)$

![](https://cjpark-1304138896.cos.ap-guangzhou.myqcloud.com/note_img/image-20211016151819939.png)

```cpp
class Solution {
public:
    Node* copyRandomList(Node* head) {
        if (head == NULL) return NULL;

        //第一次遍历，拷贝节点放原节点旁边
        Node* it = head;
        while (it) {
            Node* tmp = new Node(it->val);
            tmp->next = it->next;
            it->next = tmp;
            it = tmp->next;
        }

        //初次拷贝完成，记录新链表头节点位置
        Node* new_head = head->next;

        //重置指针，重新遍历，拷贝random域
        it = head; 
        while (it && it->next) {
            if (it->random == NULL) it->next->random = NULL;
            else it->next->random = it->random->next;
            it = it->next->next;
        }

        //双指针实现新旧链表脱钩
        Node* it1 = head,  *it2 = new_head;
        while (it1->next && it2->next) {
            it1->next = it2->next;
            it1 = it1->next;
            it2->next = it1->next;
            it2 = it2->next;
        }
        //结尾补丁
        it1->next = NULL;
        it2->next = NULL;

        return new_head;
    }
};
```