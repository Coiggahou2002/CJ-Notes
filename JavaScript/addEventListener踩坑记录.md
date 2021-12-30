# addEventListener 踩坑记录

## 场景

页面中有一块区域是一个 Vue 自定义组件（父亲），该自定义组件中用 iframe 套了一个外源的 html 页面（儿子），父子之间通过 `window.postMessage` 方法发送消息，通过为 message 事件添加 handler 来接收消息和处理后续逻辑

## bug

场景中有这样一个需求：儿子向父亲 postMessage，传递一些参数，父亲接收到 message 的时候带着这些参数发送 ajax 请求，接收到响应后，将响应数据传给儿子

但在调试时，发现儿子触发传递参数后，父亲带着这些参数发送了多个相同的 ajax 请求（本来发一次就足够了）

初步考虑可能原因：
1. 儿子多次触发 postMessage
2. 父亲多次接收到 message
3. 父亲发送 ajax 请求的代码有问题

经过简单的代码审查和断点调试，排除了 1 和 3，确认了现象级的原因为 2

查资料发现，postMessage 每调用一次只会传递一次信息，不会重复传递，排除儿子的问题，问题必然在父亲身上

最后发现呢，父亲监听 `message` 事件是通过 `window.addEventListener` 来实现的，该方法为指定事件添加 handler 函数，但可以多次、重复注册，导致可能会有多个功能相同的 handler 在同时处理 message 事件，就会导致 handler 中的发送 ajax 请求的代码被多次触发，最终发送多个相同的请求

## 解决方法

将父亲中为 `message` 添加 handler 的方法从 `addEventListener` 改成指定 `window.onmessage = function(e) {...}`

这样即可保证 handler 唯一

## 相关知识

可以通过 `addEventListener` 方式为特定的事件注册 handler

以消息监听为例：

```javascript
window.addEventListener('message', (event) => {
  // do sth in the handler
})
```

也可以通过指定 `onxxx` 来实现：

```javascript
window.onmessage = function (event) {
  // dos sth in the handler
}
```

两者的区别：
```js
//如其函数名，是为指定的事件“添加”handler
target.addEventListener('click', (e) => {...}) 

//覆写这个 target 原有的 onclick handler
target.onclick = function(e) {...} 
```

Stack Overflow 的例子

```javascript
var h = document.getElementById('a');
h.onclick = doThing_1;
h.onclick = doThing_2;

h.addEventListener('click', doThing_3);
h.addEventListener('click', doThing_4);

//最终 2、3、4 会生效，1 b
```

