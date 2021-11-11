# VuePress 部署静态博客
#### 1. 安装node.js
到官网下载稳定版安装即可
#### 2. 按照官网指示进行基本安装配置
* 创建目录
* 包管理器初始化（yarn, npm二选一）
* 安装vuepress为本地依赖
* 创建docs目录，并在下面手动创建README.md
* package.json添加两条scripts
* 用`npm run docs:dev`命令启动服务器
* 在localhost上初步预览
>如果出现显示中文乱码问题，是cmd创建的md文件编码不对，删掉自己建一个即可

#### 3. 创建官网推荐的目录结构（区分大小写）
>注意：所有文件的相对路径都相对于`docs`目录

#### 4. 了解默认路由地址

| 文件相对路径 | 页面路由地址 |
| --- | --- |
| /README.md | / |
| /guide/README.md | /guide/ |
| /config.md | /config.html |
>注意：如果是要指某目录下默认的README.md，注意目录最后也要加一个斜杠

#### 5. 了解基本原理
md文件放到.docs目录下后，在根目录下执行`npm run docs:dev`命令，就会在指定的输入目录（默认在`../dist`目录下）以`docs`下的文件目录结构为参照，输出相同的目录结构，将对应目录下的`.md`文件生成`html`静态页面

#### 6. 什么时候需要重新执行dev命令？
>注意：dev命令是包含了build命令在内的
* 凡是更改了配置文件`/config.js`或者其他配置文件的，必须重新执行一次`npm run docs:dev`，但如果是对已经存在的且已经build过的markdown文件进行更改，本地`localhost`是可以自动刷新的，不需要重新build或者dev（当然，如果vuepress部署在github或者虚拟主机，当然要重新上传）

* 每次build，相当于是将docs下的目录结构copy到dist下，再每个目录一一对应地，将md文件转换为静态html页面

#### 7.如何部署到云？
* 无论是放到虚拟主机、码云还是github上，都只需要将dist目录下的所有东西（也就是全部的静态页面）扔上去，就可以了，虚拟主机需要放到public_html文件夹下，如果是github需要放到对应的repository下