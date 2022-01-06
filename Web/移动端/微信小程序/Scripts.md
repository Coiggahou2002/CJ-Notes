小程序中的 JavaScript同浏览器中的 JavaScript 以及 NodeJS 中的 JavaScript 是不相同的

## 与普通 Web 脚本区别

浏览器中的JavaScript 是由 ECMAScript 和 BOM（浏览器对象模型）以及 DOM（文档对象模型）组成的

![](https://cjpark-1304138896.cos.ap-guangzhou.myqcloud.com/note_img/20211201103622.png)

NodeJS中的JavaScript 是由 ECMAScript 和 NPM以及Native模块组成，NodeJS的开发者会非常熟悉 NPM  的包管理系统，通过各种拓展包来快速的实现一些功能，同时通过使用一些原生的模块例如 FS、HTTP、OS等等来拥有一些语言本身所不具有的能力。

![](https://cjpark-1304138896.cos.ap-guangzhou.myqcloud.com/note_img/20211201103658.png)



小程序中的 JavaScript 是由ECMAScript 以及小程序框架和小程序 API 来实现的。同浏览器中的JavaScript 相比**没有 BOM 以及 DOM 对象，所以类似 JQuery、Zepto这种浏览器类库是无法在小程序中运行起来的**，同样的缺少 Native  模块和NPM包管理的机制，小程序中无法加载原生库，也无法直接使用大部分的 NPM 包。

![](https://cjpark-1304138896.cos.ap-guangzhou.myqcloud.com/note_img/20211201103911.png)



## 执行环境

开发者需要在项目设置中，勾选 ES6 转 ES5 开启此功能。