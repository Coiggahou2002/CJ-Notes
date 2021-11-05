# Vue | 原理

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