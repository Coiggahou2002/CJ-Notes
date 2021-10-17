# 位运算

本文总结位运算常用技巧和题型。

## 判断奇偶

```cpp
bool isOdd(int x) {  return (x & 1) == 1;  }  //奇数返回真
bool isEven(int x) {  return (x & 1) == 0;  }  //偶数返回真
```

## 快速乘除

```cpp
x >> 1;   //除以2的1次幂
x << 1;   //乘2的1次幂
x << 2;   //乘2的2次幂
```

## 查看二进制数的第 k 位

```cpp
n >> k & 1
```

## 返回 x 的最后一位 1

```cpp
int lowbit(int x) {
		return x & -x;
}
```

## 统计二进制数中 1 的个数

方法一  $lowbit(x)$

```cpp
while (x != 0) {
		x -= lowbit(x);
		res++;
}
return res;
```

方法二 $remove\_lastone(x)$

```cpp
int remove_lastone(int x) {
		return x & (x - 1);
}

while (x != 0) {
		x = remove_lastone(x);
		res ++;
}

return res;
```