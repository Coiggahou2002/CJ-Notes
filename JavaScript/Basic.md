# JavaScript | 基础语法

## 变量

```jsx
var myVariable;    //所有局部变量都用var声明
myGlobalVariable;  //全局变量不写var
```

**局部变量优先级高于全局变量（当遇到重名时）**

八大类型

+ undefined
+ null
+ boolean
+ bigint
+ number
+ object
+ symbol
+ string
  + 单双引号都可以
  + 单双互套就可以不用\转义符号
  + 可以用 + 号连接
  + 有 length 属性
  + 支持下标 str[idx] 访问字符
  + 一旦 assign，不可更改个别字符，但可以重新赋值

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0a1e21dd-7085-41aa-9236-76f7c7694a11/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0a1e21dd-7085-41aa-9236-76f7c7694a11/Untitled.png)

## 数组

类似于 Python 中的列表，可以是不同元素类型

原生的双端队列，两头都支持插入删除

```jsx
myArr.push(x)                    //末尾添加元素，类似push_back()
var last_rmv = myArr.pop();      //弹出末尾元素，有返回值，类似pop_back()
var first_rmv = myArr.shift();   //弹出首个元素，有返回值，类似pop_front()
var first_add = myArr.unshift(x); //在数组头部插入元素，类似push_front()
```

## 函数

```jsx
function functionName() {
	//hahah...
}
```

没有返回值的函数，返回 undefined

## 分支结构

if 写法和 C++ 完全一样

switch 写法也和 C++ 完全一样

## 运算符

相等判断

+ == 会执行数据类型转换
+ === 不执行数据类型转换

不相等判断

+ ! = 是模糊不等判断，如 1 ! = "1" 是假的
+ ! ==是严格不等判断