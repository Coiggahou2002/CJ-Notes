# 位运算习题

## 力扣 191. 位 1 的个数

编写一个函数，输入是一个无符号整数（以二进制串的形式），返回其二进制表达式中数字位数为 '1' 的个数（也被称为汉明重量）。

此处不能用 $lowbit(x)$，因为是无符号这整型，而 $lowbit(x)$ 需要用到 $x \& -x$.

可以用另外一种方法，$remove\_lastone(x)$，该函数每执行一次都会将 $x$ 的最低位的 1 变成 0.

```cpp
int remove_lastone(int x) {
		return x & (x - 1);
}
```

## 力扣 461. 汉明距离

两个整数之间的[汉明距离](https://baike.baidu.com/item/汉明距离)指的是这两个数字对应二进制位不同的位置的数目。

给出两个整数 $x$ 和 $y$，计算它们之间的汉明距离。

先将给出的两个数进行**”异或“运算（比较两个数，如果他们相同就返回 0，不同就返回 1）**，得到的结果 $cmp$ 是长度与 $x,y$ 长度相同的一个二进制数，$cmp$ 有多少个 1，就代表 $x$ 和 $y$ 有多少位是不同的，接下来只要统计 $cmp$ 中有多少个 1 即可.

```cpp
int hammingDistance(int x, int y){
    int cmp = x ^ y;
    int res = 0;
    while (cmp != 0) {
        cmp -= lowbit(cmp);
        res ++;
    }
    return res;
}

int lowbit(int x) {
    return x & -x;
}
```