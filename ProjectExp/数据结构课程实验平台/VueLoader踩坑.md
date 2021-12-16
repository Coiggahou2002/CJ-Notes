2021.12.16

起因：`npm install webpack-bundle-analyzer` 一直安装失败，于是删了锁版本的 `package-lock.json`，清空了 npm 缓存，删了 node_modules，重新 `npm install` 了一遍

然后：项目一直 run 不起来，一直报有关 eslint 和 loader 的错，貌似是解析不了 vue 模板文件

解决：按照 https://vue-loader.vuejs.org/zh/migrating.html#%E5%80%BC%E5%BE%97%E6%B3%A8%E6%84%8F%E7%9A%84%E4%B8%8D%E5%85%BC%E5%AE%B9%E5%8F%98%E6%9B%B4 配了一下 Webpack，可以了

原因推断：删了锁版本配置文件，版本就不受控了，vue-loader 自动升到了高版本(v15+)，而高版本存在不兼容变更，需要手动在 webpack 配置 `VueLoaderPlugin` 以及 `.vue` 文件的解析规则，否则无法运行项目

耗费：几乎一整个下午

教训：`package-lock.json` 不要随便删！