# 内置类型

> 动态语言中，所谓类型指的不是变量类型，而是值的类型

![](https://cjpark-1304138896.cos.ap-guangzhou.myqcloud.com/note_img/buit-in-types.jpg)

## 基础

**基本类型（保存到栈区）**

+ undefined
+ null
+ boolean
+ string
+ number

**引用类型（保存到堆区）**

+ object

**typeof 判断类型**

是一个运算符，作用于基本类型，返回一个字符串.

`typeof null` 会返回 `'object'`，**也就是说，我们无法通过该方式判断一个值是不是 null**

换句话说，typeof 对于基本类型，除了 null 都可以返回正确答案

```javascript
console.log(typeof 123)                     // number
console.log(typeof 'shit')                  // string
console.log(typeof false)                   // boolean

console.log(typeof undefined)               // undefined

console.log(typeof null)                    // object
console.log(typeof [1,2,false])             // object
console.log(typeof {a: 123, b: 'shit'})     // object

console.log(typeof (()=>{return null}))     // function
```

那怎么判断呢，我们知道所有 object 都是真，只有 null 是假 object，所以可以

```js
function isNull(something) {
    return !something && typeof something === 'object'
}
```

## Array

是容器类型，可以容纳任何类型，包括自己.

类数组转数组：`Array.from(alikeArray)`

## Number

JS 里没有真正的整数，所有数字都是双精度浮点数

下面表达式的值都是假

```js
console.log(0.1 + 0.2 == 0.3)   //false
console.log(0.1 + 0.2 === 0.3)  //false
```

所以我们需要 `Number.EPSILON` 作为比较容差

```js
function numbersCloseEnoughToEqual(n1,n2) {
    return Math.abs( n1 - n2 ) < Number.EPSILON;
}
```
