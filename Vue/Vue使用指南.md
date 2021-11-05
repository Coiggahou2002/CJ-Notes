# Vue | 使用指南

## Vue 实例

### 基本的东西

`el` : 根元素挂载点 id

`data` : 数据域

`methods` : 方法域（所有被 Vue 管理的函数最好都不要写成箭头函数）

`computed` : 计算属性域

```javascript
var vm = new Vue({
    el: '#targetId',
    data: {
        /*...*/
    },
    methods: {
        // 此处所有方法不可以写箭头函数，否则用到的this都是window而不是vue实例
    },
    computed: {
        
    }
})
```

注意：

data 中的所有属性，最终都在 Vue 实例身上.

其实 Vue 实例中所有的属性，都可以在模板中直接使用，如 `<div> {{ $ref }} </div>` 也可以展示出它的值

### 关于 data

手动创建 Vue 实例 `new Vue(...)` 的时候，里面的 `data` 可以直接定义为对象

但在 Vue 组件中的 `data` 必须定义为一个返回 `Object` 的函数，如

```javascript
// 下面三种写法都是将d定义为函数，都可以用
data: function() {
  return {...}
}

data() {
  return {...}
}

data: () => {
  return {...}
}
```

源码 `/src/core/util/options.js` 中可以看到对 `data` 的类型判断和提示

![](https://cjpark-1304138896.cos.ap-guangzhou.myqcloud.com/note_img/20211105145136.png)

究其原因，我们分析一下：

但凡我们会封装一个 Vue 组件，就意味着我们需要在多个地方使用它，也就是需要复用这个组件，而多个被复用的组件必然是相互隔离的，其中自然也包括它们的数据，必须是相互隔离、不可见的.

如果我们在组件中将 `data` 定义为一个对象，这个对象就被写在了堆内存中，而该组件的多个实例都共用这个地址中的对象，那么就会导致组件的实例 A 改动了 `data`，实例 B 中的 `data` 也会跟着变化，因为它们其实操作的是同一个内存地址中的对象.

这样显然是不对的，理想的情况应该是每个组件实例被创建的时候，`data` 都会整个被复制一份.

如果我们在组件中将 `data` 定义为一个返回对象的函数，就能够达到这样的效果，因为由函数返回的对象，内存地址是不相同的，也就是同一组件的所有实例用的 `data` 都不相同.

## 常用指令

### 条件渲染

`v-if` 和 `v-show` 都可以控制条件渲染

两者的区别：

### 列表渲染

#### 基本用法

```vue
<ol>
    <li v-for="(item, index) in items" :key="index">
        {{ item.label }}
    </li>
</ol>
```

官方建议设置 `v-bind:key` 也就是 `key` 值，并确保它独一无二，这便于底层 diff 算法进行优化.

#### 为什么永远不要将 v-if 和 v-for 放在同一标签使用？

下面是 Vue 2.6 的部分源码截取

```javascript
// vue-2.6.14/src/compiler/codegen/index.js
export function genElement (el: ASTElement, state: CodegenState): string {
  if (el.parent) {
    el.pre = el.pre || el.parent.pre
  }

  if (el.staticRoot && !el.staticProcessed) {
    return genStatic(el, state)
  } else if (el.once && !el.onceProcessed) {
    return genOnce(el, state)
  } else if (el.for && !el.forProcessed) {
    return genFor(el, state)
  } else if (el.if && !el.ifProcessed) {
    return genIf(el, state)
  } else if (el.tag === 'template' && !el.slotTarget && !state.pre) {
    return genChildren(el, state) || 'void 0'
  } else if (el.tag === 'slot') {
    return genSlot(el, state)
  } else {
    // component or element
    // ...
  }
}
```

从源码中可见，在生成一个 element 的时候，`v-for` 比 `v-if` 先判断，即 `v-for` 优先级比 `v-if` 更高.

换句话说，如果你以类似下面的方式使用它们，就会导致每次渲染都会先执行循环再进行条件判断，造成性能的浪费

```vue
<template>
  <ol>
    <li v-if="isShow" v-for="(item, index) in items" :key="index">
      {{ item.content }}
    </li>
  </ol>
</template>
```

如果希望通过变量控制一个列表是否显示，正确做法是套一个 `<template>` 在外面，并把 `v-if` 写在 `<template>` 上

```vue
<template v-if="isShow">
  <ol>
    <li v-for="(item, index) in items" :key="index">
      {{ item.content }}
    </li>
  </ol>
</template>
```



### 属性和变量绑定

使用 `v-bind` 或直接用冒号 `:` 

### 事件绑定

使用 `v-on` 或者 `@`

### 双向数据绑定


## 组件分析

`/components`

`/views`

## 绑定

### 事件

`@click="函数名"`：给元素点击事件绑定 `methods` 域中的某个方法

`@scroll="函数名"`：滚动条滚动一个很小的单位就会触发（包含拖动滚动条、方向键、鼠标滚轮滚动）

`@wheel="函数名"`：鼠标滚轮每滚动一节就触发（只针对鼠标滚轮）

### 事件修饰符

`@click.stop="函数名"`：避免事件冒泡 (阻止父级元素的 click 事件被调用)

`@click.prevent="函数名"`：阻止默认事件

`@click.once="函数名"`：事件只触发一次

`@click.capture="函数名"` 使用事件的捕获模式

`@click.self="函数名"`：只有 `event.target` 是当前操作的元素时才触发函数

`@click.passive="函数名"`：无需等待函数执行完毕，立即执行回调（在移动端用得多，为了让用户直接看到滑动效果，而不是因为背后事件没执行完而造成滑动卡顿，导致用户体验下降）

### 各种简写

`:key`，`:disabled` 这种一个冒号的，都是 `v-bind:` 的缩写

`@click` 这种用 `@` 的都是 `v-on:` 的缩写

### methods

methods 中的所有函数，都会直接出现在 Vue 实例上，而且不做数据代理（因为函数作代理没有意义）

> 如果把函数写在了 data 中，也能使用，而且 Vue 实例会对这个函数做代理，但没必要，属于浪费资源