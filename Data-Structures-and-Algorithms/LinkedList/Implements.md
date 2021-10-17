# 单链表和双链表的代码实现

## 单链表

### 数组模拟

模板

```cpp
int MAXN = 1000001;

int head, idx, e[MAXN], ne[MAXN];
//head为头节点下标
//idx存放当前使用储存空间的位置
//e[]存放节点的val
//ne[]存放节点next的下标

//初始化
void init() {
    head = -1;
    idx = 0;
}

//在头节点之前加入一个值为x的新节点
void add_to_head(int x) {
    e[idx] = x;
    ne[idx] = head;
    head = idx ++;
}

//在下标为k的节点后面插入值为x的新节点
void add(int k, int x) {
    e[idx] = x;
    ne[idx] = ne[k];
    ne[k] = idx ++;
}

//删除下标为k的节点后面的节点
void remove(int k) {
    ne[k] = ne[ne[k]];
}

//头节点后移一位
void head_next() {
		head = ne[head];
}

//从头到尾打印链表
void iterate() {
    for (int i = head; i != -1; i = ne[i]) cout << e[i] << " ";
}
```

### 指针版

```cpp
#include <cstring>
#include <cstdlib>
#include <cstdio>
#include <iostream>
#include <algorithm>

using namespace std;

struct LNode
{
    int data;
    LNode* next;
};

typedef struct LNode LNode;
typedef struct LNode* LinkList;

//Initialize the list and return a pointer to the head_node of the list
LinkList init_linklist() {
    LNode* head = (LNode*)malloc(sizeof(LNode));
    head->next = NULL;
    return head;
}

//Insert a node at the front of the list
//Time Complexity: O(1)
void add_front(LinkList L, int x) {
    LNode* p = (LNode*)malloc(sizeof(LNode));
    p->data = x;
    p->next = L->next;
    L->next = p;
}

//Print the list from head to tail
//Time Complexity: O(n)
void print_list(LinkList L) {
    if (L == NULL) printf("NULLPTR_ERROR");
    if (L->next == NULL) printf("EMPTY_LIST");
    LNode* cur = L->next;
    while (cur != NULL) {
        printf("%d ",cur->data);
        cur = cur->next;
    }
}

//Find the val of a node with certain number, if it exists.
//The number given should starts from 1
//Time Complexity: O(n)
bool get_val_by_num(LinkList L, int num, int* res) {
    if (L == NULL || L->next == NULL) return false;
    if (num <= 0 || res == NULL) return false;
    LNode* cur = L->next;
    int cnt = 0;
    while (cur != NULL) {
        cnt ++;
        if (cnt == num) {
            *res = cur->data;
            return true;
        }
        cur = cur->next;
    }
    return false;
}

//Return the first node whose data equals val, or return NULL
//Time Complexity: O(n)
LNode* get_by_val(LinkList L, int val) {
    if (L == NULL) printf("NULLPTR_ERROR");
    if (L->next == NULL) printf("EMPTY_LIST");
    LNode* cur = L->next;
    while (cur != NULL) {
        if (cur->data == val) return cur;
        else cur = cur->next;
    }
    return NULL;
}

//Insert a new node with certain val after the given node p
//Then return a pointer pointed to the new node
//Time Complexity: O(1)
LNode* insert_after(LNode* p, int val) {
    if (p == NULL) return NULL;
    LNode* t = (LNode*)malloc(sizeof(LNode));
    t->data = val;
    t->next = p->next;
    p->next = t;
    return t;
}

//Insert a new node with certain val before the given node p
//Then return a pointer pointed to the new node
//Time Complexity: O(1)
LNode* insert_before(LNode* p, int val) {
    if (p == NULL) return NULL;
    LNode* t = insert_after(p, val);
    swap(p->data, t->data);
    return p;
}

//Delete the given node in the list, if it does exist
bool delete_node(LinkList L, LNode* p) {
    if (L == NULL || L->next == NULL) return false;
    if (p == NULL) return false;
    //This indicates that p is not the last node
    //We can swap the data of p and the node t after it, then delete t
    if (p->next != NULL) {
        LNode* t = p->next;
        swap(p->data, t->data);
        p->next = t->next;
        free(t);
        return true;
    } else {
        //This indicates that p is the last node
        //We need to iterate the list to find the prior node of p
        LNode* pr = L;
        bool found = false;
        while (!found) {
            if (pr->next == p) {
                pr->next = NULL;
                free(p);
                found = true;
            }
            pr = pr->next;
        }
        return found;
    }
}

//Return the length of the list
//If the list only has the head-node, its length gonna be 0
int get_length(LinkList L) {
    int cnt = 0;
    if (L == NULL || L->next == NULL) return cnt;
    LNode* cur = L;
    while (cur->next != NULL) {
        cur = cur->next;
        cnt ++;
    }
    return cnt;
}

int main() {

    /*---TESTING_CODE---*/

    LinkList lst = init_linklist();
    add_front(lst, 5);
    add_front(lst, 1);
    add_front(lst, 7);
    print_list(lst);

    LNode* t1 = get_by_val(lst, 8);
    if (t1 != NULL) printf("%d\\n",t1->data);
    else printf("t1_NULL\\n");

    LNode* t2 = get_by_val(lst, 5);
    if (t2 != NULL) printf("%d\\n",t2->data);
    else printf("t2_NULL\\n");

    int res1, res2, res3, res4;

    get_val_by_num(lst, 1, &res1);
    get_val_by_num(lst, 2, &res2);
    get_val_by_num(lst, 3, &res3);
    get_val_by_num(lst, 4, &res4);

    cout << res1 << " " << res2 << " " << res3 << " " << res4 << " " << endl;

    LNode* tar = get_by_val(lst, 5);
    insert_after(tar, 999);
    insert_before(tar, 666);
    print_list(lst);

    printf("\\n");

    LNode* toDel = get_by_val(lst, 999);
    delete_node(lst, toDel);
    print_list(lst);

    cout << endl;

    cout << get_length(lst) << endl;

    LinkList emptylst = init_linklist();

    add_front(emptylst, 789);

    print_list(emptylst);

    cout << get_length(emptylst) << endl;

    /*---END---*/

    return 0;
}
```

## 双链表

### 数组模拟

```cpp
#include <iostream>
#include <cstring>

using namespace std;

const int MAXN = 1e6 + 10;

int h, t, idx, e[MAXN], l[MAXN], r[MAXN];
//h为头节点下标，一般置0
//t为尾节点下表，一般置1
//用0和1做双链表的头尾节点，idx从2开始

//初始化
//单链表以head和-1作为左、右边界
//双链表以h,t作左、右边界（即头插和尾插都不会越过h和t）
void init() {
    h = 0;
    t = 1;
    r[h] = t;
    l[t] = h;
    idx = 2; //idx从2开始，因此第k个插入的数下表为k+1
}

//在下标为k的节点右侧插入值x的节点
void add_right(int k, int x) {
    e[idx] = x;
    l[idx] = k;
    r[idx] = r[k];
    l[r[k]] = idx;
    r[k] = idx;
    idx ++;
}

//在下标为k的节点左侧插入值为x的节点
void add_left(int k, int x) {
    add_right(l[k], x);
}

//删掉下标为k的节点
void remove(int k) {
    r[l[k]] = r[k];
    l[r[k]] = l[k];
    
}

int main()
{
    int M;
    cin >> M;
    init();
    string op;
    while (M--)
    {
        int k, x;
        cin >> op;
        if (op == "L") {
            cin >> x;
            add_right(0, x); //在链表最左端插入，相当于在h右侧插入
        } else if (op == "R") {
            cin >> x;
            add_left(1, x); //在链表最右端插入，相当于在t左侧插入
        } else if (op == "D") {
            cin >> k;
            remove(k + 1); //注意下标k+1
        } else if (op == "IL") {
            cin >> k >> x;
            add_left(k + 1, x);
        } else {
            cin >> k >> x;
            add_right(k + 1, x);
        }
    }
    
    //从左到右遍历输出
    //记住遍历起点和终点是 r[h] 与 t
    for (int i = r[h]; i != t; i = r[i]) cout << e[i] << " ";
    
    return 0;
}
```