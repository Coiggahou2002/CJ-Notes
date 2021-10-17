# 容器

## 变长数组

```cpp
#include <vector>
vector<int> v;
v[i]
v.push_back(int val);
v.size();
```

## 栈

```cpp
stack<int> stk;
stk.push(int val);
stk.pop();
stk.top();
stk.empty();
stk.size();
```

## 队列

```cpp
#include <queue>
queue<int> q;
q.size();
q.front();
q.push_back(int val);
q.pop();
q.empty();
```

## 优先队列

```cpp
#include <queue>
priority_queue<int, vector<int>, greater<int> > min_heap;
priority_queue<int, vector<int>, less<int> > max_heap;
heap.push(int val);
heap.top();
heap.pop();
heap.size();
```

## 无序集合

元素唯一

```cpp
#include <unordered_set>
unordered_set<char, int> myset;
myset.emplace(char key, int val);
myset.size();
```

## 哈希表

```cpp
#include <unordered_map>

unordered_map<char, int> mymap;
mymap.emplace('a', 1);          //插入键值对
mymap[char key]                 //按key访问value
```

## 无序对

```cpp
pair<int, int> mypair;
mypair.first = 1;
mypair.second = 0;
//表示(1,0)
```

# 排序

适用于容器，对于结构体或自定义比较规则，可传入静态比较函数

```cpp
vector<int> arr;
static bool cmp(int a, int b) {
		return a > b;
}
sort(arr.begin(), arr.end(), cmp);
```

# 字符串

string 类

```cpp
string str;
str.append(string str_to_add);
str.push_back(char char_to_push);
str.length();
str.substr(int begin_index, int substr_len);  //提取子串，返回子串
```

正负数转字符串

```cpp
string tmp = to_string(int num);
```