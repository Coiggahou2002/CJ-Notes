# JavaScript | 对象

## 基本操作

```jsx
//所有属性字段都是字符串
//逗号换行
var cat = {
  "name": "Whiskers",
  "legs": 4,
  "tails": 1,
  "enemies": ["Water", "Dogs"]
};
```

属性读取

```jsx
//有空格就用下标法，没空格可以用.来访问
var myObj = {
  "Space Name": "Kirk",
  "More Space": "Spock",
  "NoSpace": "USS Enterprise"
};
myObj["Space Name"];
myObj['More Space'];
myObj["NoSpace"];  myObj.NoSpace;
```

属性添加和删除

```jsx
var myObj = {
  "Space Name": "Kirk",
  "More Space": "Spock",
  "NoSpace": "USS Enterprise"
};
myObj.newProperty = "newPropertyValue"; // 这样可以直接添加属性
delete myObj.newProperty;  // 这样直接删除属性
```

检查属性是否存在

```jsx
var myObj = {
  ...
}
myObj.hasOwnProperty(xxx)
```

## 对象属性 properties

如 `let user = { name: 'John' }` 中，`name` 就是 `user` 的对象属性

有四个属性：

+ `value`
+ `writable` 是否只读
+ `enumerable` 决定该属性是否显示在 `for props in ...` 中
+ `configurable` 决定它自己及以上 3 个属性能不能被修改

查询属性的信息：`Object.getOwnPropertyDescriptor(obj, propertyName)`

设置某属性：`Object.defineProperty(obj, propertyName, descriptor)`

## 构造函数

构造函数的两个约定：

+ 函数名以大写字母开头
+ 只能由 `new` 操作符来执行

当使用 `new 构造函数()` 时：

1. 一个新的空对象被创建，它被分配给函数中的 `this`
2. 函数体执行，通过 `this` 为空对象添加属性
3. 返回 `this`

```js
/* 注释部分是隐式执行的 */
function User(name, address) {
    // this = {};
    this.name = name;
    this.address = address;
    // return this
}
let user = new User('John', 'Beijing');
```

因为其实构造函数也是普通函数，所以，从理论上来讲，任何函数（箭头函数除外，它没有自己的 `this` ）只要前面加 `new` 都可以用作构造函数，首字母大写只是约定俗成的规矩——用来标识该方法是构造函数而不是普通函数.





![](https://cjpark-1304138896.cos.ap-guangzhou.myqcloud.com/note_img/20211122142402.png)

