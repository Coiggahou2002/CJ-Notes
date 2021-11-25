# Vue CLI

启动网页 UI 界面：`vue ui`

创建项目：`vue create <app-name>`

创建项目很慢的解决方法：`gedit ~/.vuerc`，将 `useTaobaoRegistry` 改为 true

解决办法！！！

用 `vue ui` 创建 Vue 2.x 项目的时候：

+ 不要开梯子
+ 修改 `~/.vuerc` 将使用淘宝镜像设置为 true
+ 怀有一颗虔诚的心



Vue CLI 创建的项目没有 `webpack.config.js`，只有 `vue.config.js`

如果需要添加 webpack 配置，需要在 `vue.config.js` 中用 `configureWebpack` 将自定义配置部分包裹起来：

```javascript
const MonacoEditorPlugin = require("monaco-editor-webpack-plugin");

module.exports = {
  transpileDependencies: ["vuetify"],
  configureWebpack: {
    // 加入你自己的配置项
  },
};

```

