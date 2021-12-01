## 元素

`text` 普通文字

`view`

`block` 包装块

`button` 按钮

attributes 对大小写敏感

标签严格闭合

## 数据绑定

声明式模板语法与 Vue 相似

`{{ something }}`

props 中也可以用模板 `<text data="{{sth}}"> hello </text>`

data 与 Vue 不同，不具有双向绑定，只用于初始化

```javascript
Page({
  data: {
    // 初始数据，无双向绑定
  }
})
```

|      | Miniprogram                                                  | Vue                             |
| ---- | ------------------------------------------------------------ | ------------------------------- |
|      | `wx:if="{{判断条件变量}}"`                                   | `v-if="判断条件变量"`           |
|      | `wx:for="{{items}}"` ，`wx:for-index="idx"`，`wx:for-item="itemName"` | `v-for="(item,index) in items"` |
|      | `wx:key="xxxxx"`                                             | `v-bind:key="xxxx"`             |
|      |                                                              |                                 |

## 事件

`bindtap="方法名"`

方法名可以传 `e`，默认提供事件回调

## 模板

有点像 Vue 中的组件，但不具备完全的组件特征，相当于简便复用代码块的工具

```vue
<!--
item: {
  index: 0,
  msg: 'this is a template',
  time: '2016-06-18'
}
-->


<template name="msgItem">
  <view>
    <text> {{index}}: {{msg}} </text>
    <text> Time: {{time}} </text>
  </view>
</template>


<template is="msgItem" data="{{...item}}"/>

<!-- 输出
0: this is a template Time: 2016-06-18
-->
```

## 引用

 import 有作用域的概念，即只会 import 目标文件中定义的 template，而不会 import 目标文件中 import 的 template，简言之就是 import 不具有递归的特性

include 可以将目标文件中除了 `<template/> <wxs/>` 外的整个代码引入，相当于是将除了 `<template/>` 和 `<wxs/>` 外的代码都拷贝到 include 的位置