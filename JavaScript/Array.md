# JavaScript | 数组

## 基本操作

```jsx
arr.length(); //获取长度
arr.concat(append_arr); //连接两个数组
arr.fill(x); //用x填充数组
arr.indexOf(x); //找到x在数组中的位置

/*头尾增删*/
arr.push(x);            //末尾插入元素x
arr.pop();              //末尾删除元素，返回删除的元素
arr.unshift(x);         //队头插入元素x
arr.shift();            //删除并返回队头元素

/*重置位置*/
arr.sort();             //排序
arr.reverse();          //反转 
```

## 常用方法

### reduce

reduce() 方法接收一个函数作为累加器，数组中的每个值（从左到右）开始缩减，最终计算为一个值。

例子

```jsx
//定义sum()函数，传入不定长序列args数组，返回数组所有元素的和
const sum(...args) => {
	//此处reduce作为参数数组args的一个子方法来调用
	//reduce(function(), initialValue);
	//往reduce里面传入的函数，描述遍历数组每个元素时做的操作（如累加）
	return args.reduce((a, b) => a + b, 0);
}
```

### 展开

```jsx
arr = [1, 89, 20, 15];

/*旧写法*/
var maximum = Math.max.apply(null, arr); //maximum = 89
var maximum = Math.max(arr) //返回NaN，因为Math.max()只接受参数列表，不接受数组

/*新写法*/
let maximum = Math.max(...arr) //将数组展开成参数列表，可以求出max

/*例子：拷贝数组*/
const arr1 = ['JAN', 'FEB', 'MAR', 'APR', 'MAY'];
let arr2 = [...arr1]; //这样就完成了拷贝，三个点相当于拆散数组
```