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

## 排序

元素类型为基本数据类型：

```javascript
const arr = [1,8,4,2,3];
arr.sort();  //默认升序排序，[1,2,3,4,8]
arr.sort((a,b) => a - b); //升序排序，[1,2,3,4,8]
arr.sort((a,b) => b - a); //降序排序，[8,4,3,2,1]
```

元素类型为对象，希望根据对象某个属性来排序：

```javascript
const arr = [
  { id: "123", name: "sfjsdf" },
  { id: "67", name: "we" },
  { id: "144", name: "ggg" },
  { id: "22", name: "124fddfs" },
  { id: "199", name: "iului" },
]
//根据id属性升序排序（先从字符串转数字）
arr.sort((a,b) => parseInt(a.id) - parseInt(b.id));
```

传入的自定义比较器函数中，返回值最好不要用 `>`, `<` 这种产出布尔值的表达式（曾经出现过场景 bug，踩过坑），因为比较器不是根据 `true` 和 `false` 去执行比较行为的，而是根据正、负、零返回值，可以参考 MDN docs 关于自定义比较器函数的返回值的说明：

![](https://cjpark-1304138896.cos.ap-guangzhou.myqcloud.com/note_img/20211231165735.png)