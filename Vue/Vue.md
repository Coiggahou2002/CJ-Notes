# Vue

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

## Vue CLI

启动网页 UI 界面：`vue ui`

创建项目：`vue create <app-name>`

创建项目很慢的解决方法：`gedit ~/.vuerc`，将 `useTaobaoRegistry` 改为 true

解决办法！！！

用 `vue ui` 创建 Vue 2.x 项目的时候：

+ 不要开梯子
+ 修改 `~/.vuerc` 将使用淘宝镜像设置为 true
+ 怀有一颗虔诚的心

## 数据代理

### 基本原理

```javascript
// 通过该方法指定的对象属性，默认是不可枚举、不可修改、不可删除的（除非人为设置）
Object.defineProperty(object, key, {
    value: 'xxxx',
    enumerale: false,
    writable: true,
    configurable: true，
    get() {
        // 可以劫持getter进行一系列操作
	},
    set(value) {
        // 可以劫持setter进行一系列操作
    }
})
```

### 最简单的数据代理

 通过 `Object.defineProperty()` 劫持代理方的 getter 和 setter，在其中修改源数据方

```javascript
let originObject = { x : 100 };
let proxyObject = { y : 200};

Object.defineProperty(proxyObject, 'x', {
    get() {
        return originObject.x;
    },
    set(value) {
        originObject.x = value;
    }
});

proxyObject.x = 20323;

console.log(proxyObject.x); // 20323
```

我们创建 Vue 实例时，在 options 中传入的 data 中的所有属性，最终会出现在 vm 身上，即被 vm 代理

```js
var vm = new Vue({
    el: '#app',
    data: {
        name: 'John',
        address: 'Kk'
    }
})

vm.name // John
vm.address // Kk
```

具体的实现方式其实大体思路很显然，这些属性其实原本是在 `vm._data` 里面的，在此过程中， `vm` 充当了 `vm._data` 的代理对象，每当 DOM 模板中取 `vm.name` 时，会触发通过人为设置 `Object.defineProperty()` 来实现的 vm 对 name 的 getter，在此 getter 中取 `vm._data.name` . 

类似地，人为去修改 `vm.name` 时，会触发 setter，此 setter 中会去更新 `vm._data.name` 的值

```js
Object.defineProperty(vm, 'name', {
    get() {
        return vm._data.name;
    },
    set(value) {
        vm._data.name = value;
    }
})
```

### 监视实例对象

```js
function Observer(obj) {
    const keys = Object.keys(obj);
    keys.forEach((key) => {
        Object.defineProperty(this, key, {
            get() {
                return obj[key];
            },
            set(newVal) {
                obj[key] = newVal;
            }
        })
    })
}

const obs = new Observer(data);
```



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

## 组件通信

### 父传子

**父组件通过给 `props` 给子组件传值.**

通过一个简单例子来说明，场景如下：

父组件是一个显示文章的页面视图，子组件叫 `MarkdownContent`，能将 Markdown 字符串渲染成 HTML 字符串，每次用户打开一篇文章，父组件都需要向服务端发请求，拿到 Markdown 字符串格式的文章，然后交给子组件渲染，所以产生了向子组件传值的需求.

父组件中需要通过 `v-bind:子组件属性名` 传入数据，如下

```vue
<!--父组件中通过:attribute形式给子组件传值-->

<template>
  <MarkdownContent :markdownContent="comment.content"/>
</template>

<script>
import MarkdownContent from "./MarkdownContent";
export default {
  components: {
    MarkdownContent  
  },
  data() {
    return {
      comment: {
        author: '',
        content: ''
      }
    }
  }
}
</script>
```

子组件中需要设置相应的 `props`，并设置好数据绑定

```vue
<template>
  <mavon-editor v-model="markdownContent">
  </mavon-editor>
</template>

<script>
export default {
  props: {
    markdownContent: {
      type: String,
        default: '',
        required: true
    }
  }
}
</script>
```

### 子传父

子组件通过 `this.$emit('向外暴露事件名'，携带数据)` 来向父组件传值.

举例说明，场景如下：

父组件是一个评论区页面，子组件是一个简单的 Markdown 文本编辑框加上一个“提交”按钮，现在我们希望，当用户输入了评论并点击了子组件里的“提交”按钮时，父组件能够收到通知并拿到这份评论文本，通过 HTTP 请求发给服务端.

我们可以这么做：

给子组件的提交按钮绑定一个处理事件，在该事件中 `emit`

```vue
<template>
  <mavon-editor v-model="content"/>
  <v-btn @click="handleCommit">
    发表评论
  </v-btn>
</template>
<script>
export default {
  data() {
    return {
      content: '',
    }
  }
  methods: {
    handleCommit() {
      this.$emit('onCommit', this.content)
    }
  }
}
</script>
```

同时，在父组件中通过 `v-on:子组件emit的函数名="父组件中某个处理函数(接收emit的数据参数)"`来感知

```vue
<template>
  <Comments @onCommit="commitToServer"/>
</template>
<script>
export default {
  methods: {
    commitToServer(contentOfComment) {
      HttpService({
        url: '...',
        method: 'post',
        data: {
          comment: contentOfComment
        }
      })
    }
  }
}
</script>
```

### 兄弟组件之间传

待补充

### 父组件从子组件取数据

如果并不需要“子组件触发某事件时父组件收到通知”的效果，而仅仅只需要让父组件取出子组件的数据，可以通过 `this.$refs` 来实现.

例如，子组件是一个文本框，父组件希望点击按钮时，能够直接取出子组件文本框的内容

如下，子组件做自己就好，不需要做任何特殊的准备

```vue
<!--子组件-->
<template>
  <v-text-field v-model="text"></v-text-field>
</template>
<script>
export default {
  name: 'ChildComponent',
  data() {
    return {
      text: ''
    }
  },
}
</script>
```

父组件中，需要给子组件加一个 `ref` 属性，也就是引用名，加上就可以取数据了

```vue
<!--父组件-->
<template>
  <ChildComponent ref="myChild"/>
  <v-btn @click="getText"></v-btn>
</template>
<script>
import ChildComponent from '...'
export default {
  components: {
    ChildComponent
  },
  methods: {
    getText() {
      console.log(this.$refs.myChild.text);
    }
  }
}
</script>
```

