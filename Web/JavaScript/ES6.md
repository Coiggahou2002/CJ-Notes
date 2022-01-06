# JavaScript | ES6 特性

## var 与 let 的区别

| var                                                    | let                          |
| ------------------------------------------------------ | ---------------------------- |
| 允许重复声明，覆盖，不报错                             | 不允许重复声明，会报错       |
| 在函数内声明，创建局部变量；在函数外声明，创建全局变量 | 作用域会被限制在当前代码块中 |

## const 声明

对于用 const 声明的普通变量，只能声明一次，且不可以被再次赋值（相当于就是声明了一个不可修改的常量）

但如果是对于对象（包括数组和函数）使用 const 声明，那么对象仍然是可以修改的，只是不能重新赋值而已.

```jsx
const MY_CONST = "unmodified_string";
const arr = [1,2,3];
arr = [4,5,6]; //不可以重新赋值
arr[0] = 999;  //可以修改
arr.push(999); //可以修改
```

## 对象冻结 Object.freeze()

对对象调用冻结函数之后，对象将不可修改

```jsx
let obj = {
  name:"FreeCodeCamp",
  review:"Awesome"
};
Object.freeze(obj); //冻结后，下面的修改都无效
obj.review = "bad";
obj.newProp = "Test";
```

## 箭头函数

```jsx
// ES5的写法
const myFunc = function() {
	const myVar = "value";
	return myVar;

// 需要函数体时的写法
const myFunc = () => {
  const myVar = "value";
  return myVar;
}

// 不需要函数体，只返回值的写法
const myFunc = () => "value";

// 其他例子
const plus = (a,b) => a + b;
const multiply = (a,b) => a * b;
const concatArr = (arr1, arr2) => arr1.concat(arr2); 

//带默认参数值
const greeting = (name = "Anonymous") => "Hello " + name;
console.log(greeting("John"));  //Hello John
console.log(greeting());        //Hello Anonymous
```

## 解构赋值

参考：

[踩坑指南：JavaScript解构赋值](https://zhuanlan.zhihu.com/p/158670265)

### 对象解构

其实就是用若干变量提取对象若干个属性的简洁写法

```jsx
/*例1*/
const HIGH_TEMPERATURES = {
  yesterday: 75,
  today: 77,
  tomorrow: 80
};
/**
 * 自动创建两个变量highToday和highTomorrow
 * 提取HIGH_TEMPERATURES的today和tomorrow属性分别赋值给他们
 */
const { today : highToday, tomorrow : highTomorrow} = HIGH_TEMPERATURES;

/*例2*/
//传一个对象，只要它的max和min属性，然后用这两个属性来运算
const stats = {
  max: 56.78,
  standard_deviation: 4.34,
  median: 34.54,
  mode: 23.87,
  min: -0.75,
  average: 35.85
};
const half = ({max, min}) => (max + min) / 2.0;
```

下面的例子不是很明白

```jsx
const LOCAL_FORECAST = {
  yesterday: { low: 61, high: 75 },
  today: { low: 64, high: 77 },
  tomorrow: { low: 68, high: 80 }
};

const lowToday = LOCAL_FORECAST.today.low;
const highToday = LOCAL_FORECAST.today.high;

const {lowToday: {low}, highToday:{high}} = LOCAL_FORECAST;
```

### 数组解构

```jsx
//交换两数的例子
let a = 8, b = 6;
[a, b] = [b, a];

//除去数组前几个元素
const source = [1,2,3,4,5,6,7,8,9,10];
const [a, b, ...arr] = source;  //arr = [3,4,5,6,7,8,9]
```

## 模板字符串

```jsx
//使用反引号括住总串，中间可以用${}插入变量
arr = ["shit", "fuck"];
const str = `<li class="text-warning">${arr[0]}</li>`;
console.log(str);
```

## Promise

待补充