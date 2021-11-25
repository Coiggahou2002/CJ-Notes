# 栈

## 数组模拟

```cpp
int stk[MAXN], tt;

void push(int x) {
    stk[ ++ tt ] = x;
}

void pop() {
    tt--;
}

bool isEmpty() {
    return tt <= 0;
}

int query() {
    return stk[tt];
}
```

