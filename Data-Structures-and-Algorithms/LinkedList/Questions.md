# 典型例题

[toc]

## 力扣 206. 反转链表

三指针

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

方法一  哈希表 $unordered\_map$

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