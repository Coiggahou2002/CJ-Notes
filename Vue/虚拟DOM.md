# Vue | 虚拟 DOM

> Vue 2.x 与虚拟 DOM 相关的核心源码位于 /src/core/vdom 下

## 概念

### 声明式渲染

过去的 jQuery 是命令式操作 DOM，也就是直接操作 DOM.

现在的 Vue，Angular，React 都是声明式操作 DOM，也就是说，我们只需要描述状态和 DOM 之间的映射关系，框架帮我们将状态渲染成视图.

### 虚拟 DOM

虚拟 DOM 简单来说就是用 JavaScript 对象来描述 DOM，例如

真实 DOM

```html
<div id="app">
    <p class="p">节点内容</p>
    <h3>{{ foo }}</h3>
</div>
```

 render 函数中看虚拟 DOM 的大概模样

```javascript
(function anonymous() {
  with (this) {
    return _c("div", { attrs: { id: "app" } }, [
      _c("p", { staticClass: "p" }, [_v("节点内容")]),
      _v(" "),
      _c("h3", [_v(_s(foo))]),
    ]);
  }
});
```

## 细节

### 状态变化时如何重新渲染

最简单粗暴的方式是：每当状态更新，暴力重新渲染整个 DOM

最理想节能的方式是：当某个状态发生变化时，只更新与这个状态相关联的 DOM 节点

虚拟 DOM 的解决方式是：通过状态生成虚拟节点树，在渲染之前，用新生成的虚拟节点树与旧的虚拟节点树对比，找出不同的部分，只渲染那一部分.

### 变化侦测的粒度

Vue 1.0 使用的是最细粒度的侦测，每一个绑定都有一个 Watcher 来观察状态的变化，导致内存开销很大.

Vue 2.x 换成了中等粒度的侦测方案，即变化侦测只通知到组件级别，然后由组件内部通过虚拟 DOM 去进行对比和渲染，属于一个折中方案.

## 虚拟 DOM 做了什么

+ 提供与真实 DOM 节点对应的虚拟节点 VNode
+ 将虚拟节点 VNode 与旧的虚拟节点 oldVNode 比较，然后更新视图

> 一句话概括虚拟 DOM 的本质：用 JavaScript 的计算成本换取 DOM 的操作成本

## 模板转换成视图的过程

![](https://cjpark-1304138896.cos.ap-guangzhou.myqcloud.com/note_img/20211106112225.png)

## VNode

VNode 可以直接理解为：用来描述一个 DOM 节点的 JavaScript 对象

下面是源码中 VNode 的结构

```typescript
export default class VNode {
  tag: string | void;
  data: VNodeData | void;
  children: ?Array<VNode>;
  text: string | void;
  elm: Node | void;
  ns: string | void;
  context: Component | void; // rendered in this component's scope
  key: string | number | void;
  componentOptions: VNodeComponentOptions | void;
  componentInstance: Component | void; // component instance
  parent: VNode | void; // component placeholder node

  // strictly internal
  raw: boolean; // contains raw HTML? (server only)
  isStatic: boolean; // hoisted static node
  isRootInsert: boolean; // necessary for enter transition check
  isComment: boolean; // empty comment placeholder?
  isCloned: boolean; // is a cloned node?
  isOnce: boolean; // is a v-once node?
  asyncFactory: Function | void; // async component factory function
  asyncMeta: Object | void;
  isAsyncPlaceholder: boolean;
  ssrContext: Object | void;
  fnContext: Component | void; // real context vm for functional nodes
  fnOptions: ?ComponentOptions; // for SSR caching
  devtoolsMeta: ?Object; // used to store functional render context for devtools
  fnScopeId: ?string; // functional scope id support

  constructor (
    tag?: string,
    data?: VNodeData,
    children?: ?Array<VNode>,
    text?: string,
    elm?: Node,
    context?: Component,
    componentOptions?: VNodeComponentOptions,
    asyncFactory?: Function
  ) {
    this.tag = tag
    this.data = data
    this.children = children
    this.text = text
    this.elm = elm
    this.ns = undefined
    this.context = context
    this.fnContext = undefined
    this.fnOptions = undefined
    this.fnScopeId = undefined
    this.key = data && data.key
    this.componentOptions = componentOptions
    this.componentInstance = undefined
    this.parent = undefined
    this.raw = false
    this.isStatic = false
    this.isRootInsert = true
    this.isComment = false
    this.isCloned = false
    this.isOnce = false
    this.asyncFactory = asyncFactory
    this.asyncMeta = undefined
    this.isAsyncPlaceholder = false
  }
}
```

### 类型

VNode 是一个类，Vue 通过给一个 VNode 设置不同的属性来对其作类型上的区分，如 `tag` 属性一定不为空就是元素节点，`isComment` 属性为真就说明是注释节点，等等.

#### 注释节点

注释节点就是形如 `<!--这里是注释内容-->` 的节点

对应的的 VNode 长这样

```javascript
{
  text: '这里是注释内容',
  isComment: true,
}
```

源码中创建注释节点的代码：

```typescript
export const createEmptyVNode = (text: string = '') => {
  const node = new VNode()
  node.text = text
  node.isComment = true
  return node
}
```

#### 文本节点

创建代码

```typescript
export function createTextVNode (val: string | number) {
  // 4个参数对应tag,data,children,text
  return new VNode(undefined, undefined, undefined, String(val))
}
```

#### 元素节点

有效属性：

+ tag，节点名称（如 p，ol，li）
+ data，包含了节点上的数据（如 attrs，class，style）
+ children，子节点列表
+ context

如 `<p><span>Hello</span><span>World</span></p>` 的 p 元素对应的 VNode 结构如下

```javascript
{
  tag: 'p',
  data: {...},
  children: [VNode, VNode],
  context: {...}
  // ...
}
```

#### 组件节点

组件节点的独有属性：

+ componentOptions，组件节点的选项参数
  + propsData
  + tag
  + children
  + ...
+ componentInstance，组件的实例（也是 Vue 实例）

#### 克隆节点

#### 函数式组件节点

独有属性：

+ functionalContext
+ functionalOptions

> 只有元素节点、注释节点、文本节点会被插入到 DOM 中

## 核心算法 patch

patch 算法做了什么：

+ 对比新旧两个 VNode 之间有哪些不同
+ 根据对比结果，找出需要更新的节点进行更新

整体流程：

##### <img src="https://cjpark-1304138896.cos.ap-guangzhou.myqcloud.com/note_img/20211106132914.png" style="zoom:50%;" />

对现有 DOM 进行修改，无非就是做以下三件事之一：

+ 创建新增的节点
+ 删除已经废弃的节点
+ 修改需要更新的节点

### 新增节点

当 VNode 与 oldVNode 完全不是同一个东西或者 oldVNode 为空时，就需要使用 VNode 生成新 DOM 节点

<img src="https://cjpark-1304138896.cos.ap-guangzhou.myqcloud.com/note_img/20211106133510.png" style="zoom:67%;" />

### 删除节点

当 VNode 与 oldVNode 完全不是同一个东西或 VNode 为空时，就需要删除旧的 DOM 节点

### 更新节点

当 VNode 与 oldVNode 是同一个节点，就需要进一步比对，找出不相同的最小部分

## 作用

真实 DOM 树元素非常庞大，直接操作 DOM 的代价是非常昂贵的

Vue 使用虚拟 DOM 来记录需要的最小信息，再通过 diff 算法得出需要修改的最小单位，再去操作 DOM 更新视图，提高了性能.

另外又例如，在一次性更新多个节点这件事情上，浏览器会执行 10 次更新 DOM 的流程，而虚拟 DOM 会将这多次更新的 diff 内容暂存，然后一次性 attach 到 DOM 树上，避免大量无谓的计算.