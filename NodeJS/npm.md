# npm

全局更新 npm：`npm i npm -g`

强制清理缓存(高版本禁用)：`npm cach`

安装 cnpm：`npm install -g cnpm --registry=https://registry.npm.taobao.org`

![](https://cjpark-1304138896.cos.ap-guangzhou.myqcloud.com/note_img/image-20210730161344324.png)

## 命令

### 版本

```shell
npm -v
```

### 初始化

```shell
npm init   //引导创建package.json
```

### 安装

```shell
npm install 模块名 -g           //全局安装
npm install 模块名              //本项目安装
npm install 模块名@latest       //最新版本安装
npm install 模块名@版本号        //指定版本
npm install 模块1 模块2 模块3    //一次性安装多个
npm install 模块名 --save-dev   //安装开发时依赖包
npm install 模块名 --save       //安装运行时依赖包
npm uninstall 模块名            //卸载某个模块
```

标注 `--save-dev` 的模块会被加入 `package.json` 中的 `devDependencies`

标注 `--save` 的模块会被加入 `package.json` 中的 `dependencies`

> npm install 可缩写为 npm i

### 查看各种信息

查看已经安装的模块

```shell
npm list            //直接全部列出
npm list --depth=0  //限制层级深度
```

查看模块安装位置

```shell
npm root      //查看本项目中模块安装位置
npm root -g   //查看全局模块安装位置
```

以关键词搜索已经存在的 npm 包

```shell
npm search 关键词    //例如搜vue会出来一堆vue r
```

### 缓存相关

```shell
npm cache clean            //清除缓存
npm cache clean --force    //强制清除缓存
```

## 注意

1. 通常不建议 `npm install -g`，因为这样可能导致版本冲突问题，毕竟可能有些项目用的是 Webpack 4，有些是 Webpack 5
2. 
