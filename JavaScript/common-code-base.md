# JavaScript | 常用代码

## 字符串处理

翻转字符串

```javas
myStr.split('').reverse().join()
```

## 随机数

```jsx
Math.random() //返回[0,1)的随机数
Math.floor(Math.random() * k)  //返回[0,k-1]之间的随机整数

/**
 * 
 * @param {随机数下界(包括} low 
 * @param {随机数上界(包括)} high 
 * @returns 区间[low,high]之间的随机整数
 */
function myRandom(low, high) {
    return Math.floor(Math.random() * (high - low + 1)) + low;
}
```

## 字符串转整数

```jsx
return parseInt(str);     //将str以十进制解析成整数
return parseInt(str, 2);  //将str以二进制解析成整数
parseInt("11", 2); //返回值为3
```
