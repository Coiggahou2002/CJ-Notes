# jQuery

## 本质

就是一个 JavaScript 库，提供了大量操作 DOM 的简介写法来替代繁琐的原生 js 写法.

## 基本概念

### DOM 对象与 jQuery 对象的关系

用原生写法如 `document.getElementById("xxx")` 得到的都是 DOM 对象，可以访问 DOM 对象拥有的属性和方法.

用 jQuery 写法如 `$("#xxx")` 获取到的则都是 jQuery 对象，只能执行 jQuery 提供的方法.

大部分 jQuery 对象其实就是 DOM 对象经过的简单的包装形成的，常见的 jQuery 对象是 DOM 对象组成的数组.

DOM 对象只要在外边套上 `$()` 就可以转成 jQuery 对象.

jQuery 对象通过取下标如 `$("#mydiv")[0]` 可以获取到 DOM 对象.

> 一般来说，通过 jQuery 获取的对象，变量名由美元符号 `$` 开头.

## 常见用法

### 基本格式

`$(selector).action()`

### 文档就绪事件

```javascript
$(document).ready(function(){
 
   // 开始写 jQuery 代码...
 
});
```

简写如下：

```javascript
$(function(){
 
   // 开始写 jQuery 代码...
 
});
```

### 选择器

#### 元素选择器

`$("tag_name")` 通过标签元素类型选择，返回该类型所有元素组成的数组

`$("#id_name")` 通过 id 来选择

`$(".class_name")` 通过 class 选择

`$("tag_name.class_name")` 选择特定 class 的特定标签

`$("[prop_name]")` 通过属性来选择，如 `$("[href]")` 选中带有 href 属性的所有元素

#### 层级选择器

### 事件

例子

```javascript
$("p").click(function () {
    $(this).hide();
});
```

`.click(function(){ ... })`

`.dbclick(function(){ ... })`

`.mouseenter(function(){ ... })`

`.mouseleave(function(){ ... })`

`.mousedown(function(){ ... })`

`.mouseup(function(){ ... })`

`.mousehover(function(){ ... })`

`.focus(function(){ ... })` 获得焦点

`.blur(function(){ ... })` 失去焦点

与原生 JS 操作 DOM 绑定事件略有不同，jQuery 中的写法要求将执行函数填在事件函数括号里面.