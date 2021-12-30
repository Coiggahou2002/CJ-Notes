# NodeJS

Node 赋予了 JavaScript 文件 I/O 和网络功能

Node 的核心模块相当于其他语言的标准库

文件系统库：fs、path

TCP 客户端和服务端库：net

HTTP 库：http 和 https

域名解析库：dns

测试断言库：assert

用于查询平台信息的操作系统库：os

Node 程序主要有：

+ Web 应用（如用 Express 框架构建的应用）
+ 命令行工具（如 npm, gulp, Webpack）
+ 后台程序
+ 桌面程序（如 Electron）

## 模块化

`require` 是 Node 中为数不多的**同步 I/O** 操作



如果 Node `require` 引入的时候没有写明文件相对位置关系，则查找文件的顺序是：

1. Node 核心模块
2. node_modules
3. 环境变量 `NODE_PATH` 指定的目录下

