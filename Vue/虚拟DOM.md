# Vue | 虚拟 DOM

## 概念

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

## 作用

真实 DOM 树元素非常庞大，直接操作 DOM 的代价是非常昂贵的

Vue 使用虚拟 DOM 来记录需要的最小信息，再通过 diff 算法得出需要修改的最小单位，再去操作 DOM 更新视图，提高了性能.

另外又例如，在一次性更新多个节点这件事情上，浏览器会执行 10 次更新 DOM 的流程，而虚拟 DOM 会将这多次更新的 diff 内容暂存，然后一次性 attach 到 DOM 树上，避免大量无谓的计算.