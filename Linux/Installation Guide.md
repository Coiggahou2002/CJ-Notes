# 一、基本初始配置工作

## 更换镜像源

参考：[Ubuntu 20.04 软件源更换 - 知乎](https://zhuanlan.zhihu.com/p/142014944)

```bash
# Use gedit to edit sources.list
sudo gedit /etc/apt/sources.list
```

## 中文输入法安装

参考：

[搜狗输入法 for Linux 安装指南 - 搜狗官网](https://pinyin.sogou.com/linux/help.php)

[Ubuntu安装搜狗输入法 -- 知乎专栏](https://zhuanlan.zhihu.com/p/34270907)

## 翻墙 electron-ssr

要去人家的 [GitHub Release](https://github.com/shadowsocksrr/electron-ssr/releases) 上安装最新版的（我装的时候是0.2.7），否则会有依赖库问题

[ubuntu20.04(electron-ssr-0.2.6) 初始配置参考 - CSDN](https://blog.csdn.net/qq_22644927/article/details/106991213)

```bash
# 还要装一下这个，否则翻不出去
sudo apt-get install libcanberra-gtk-module
```

# 二、常见命令

文件操作

```bash
cd /usr/                 # 进入/usr/目录
mkdir folder_name        # 在当前目录下创建文件夹folder_name
ls                       # 列出当前目录下文件列表
rm file_name             # 删除当前目录下的file_name文件
cp src_path dest_path    # 将src_path路径文件拷贝到dest_path
mv src_path dest_path    # 剪切
gedit file_name          # 用gedit编辑器编辑file_name文件
code file_name           # 用VS Code编辑file_name文件
cat file_name            # 显示文本文件内容
```

应用安装

```bash
sudo apt-get install app_name    # 安装app_name应用
sudo apt-get remove app_name     # 卸载app_name应用
sudo apt-get -f install          # 出现依赖问题时使用
```

系统级

```bash
reboot      # 重启电脑
```

权限类

```bash
sudo su # 获取SuperUser权限
```

# 三、日常应用配置

## 百度云

没错，谢天谢地，百度云有 Linux 客户端.

## Flash Player

不装就看不了b站了hhh

Chrome只需要

```bash
sudo apt-get install flashplugin-installer
```

如果是 Firefox 需要去 adobe 官网下载 NPAPI 版本的 .tar.gz 包，解压之后将链接库手动放到 Mozilla 插件文件夹中

```bash
sudo tar -xvf flash_player_npapi_linux.x86_64.tar.gz
sudo cp libflashplayer.so /usr/mozilla/plugins/
```

# 四、开发环境配置

## Git 安装

```bash
sudo apt-get install git
```

## npm 包管理器安装

```bash
sudo apt-get install npm
```

## VS Code

下载 deb 包直接装就行

## C/C++ 开发环境

```bash
sudo apt-get install build-essential # 一键搞定C环境
```

搞定环境之后，去jb官网下载CLion压缩包，然后解压到想安装的目录，然后运行 /bin/clion.sh

（相当于是绿色版软件）

激活的话，安装一个30天刷新插件即可

需要手动添加桌面快捷方式，具体见

[【软件安装笔记】ubuntu 18.04添加 Clion 桌面快捷方式_Juicy B的博客-CSDN博客](https://blog.csdn.net/qq_42554780/article/details/104240748)

## Python 开发环境

一般自带 python3 ，若没有就先装 Python3，然后安装 PyCharm 就可以

## Wireshark

Download wireshark on its website.

# 五、界面美化

## 系统界面

先安装基本工具

```bash
sudo apt-get update
sudo apt-get install gnome-tweak-tool
sudo apt-get install gnome-tweaks
sudo apt-get install gnome-shell-extensions
```

还要去 gnome 插件网安装一个 user-theme 才能自定义shell主题（需要一个浏览器插件来快捷安装）

用户自己下载的素材应该存放在下面的目录，才能被 gnome-tweaks 检测到

```bash
/usr/share/themes # 存放主题
/usr/share/icons  # 存放图标和鼠标指针
```

参考主题获取网站：

[www.pling.com](https://www.pling.com/)

[Browse Latest | ](https://www.gnome-look.org/browse/cat/)https://www.gnome-look.org/browse/cat/

## 更换终端

### 安装 ZSH 与 oh-my-zsh

我从 bash 换到了 zsh，然后安装了 oh-my-zsh，装了自定义主题 agnosterzak

ref:

[Ubuntu | 安装oh-my-zsh](https://www.jianshu.com/p/ba782b57ae96)

下面是需要了解的命令

```bash
# 查看系统当前使用的 shell
echo $SHELL

# 查看系统自带的 shell
cat /etc/shells

# 安装 zsh
sudo apt-get install zs

# 将终端改为 zsh (注意这一步之后需要重启系统)
chsh -s /bin/zsh

# 安装 oh-my-zsh
sh -c "$(curl -fsSL <https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh>)"

# 编辑修改 zsh 的配置文件
# 修改主题找到 ZSH_THEME="" 填入主题名即可（修改之前主题必须已经安装）
sudo gedit ~/.zshrc
```

我安装的是 agnosterzak 主题

```bash
cd ~/.oh-my-zsh/themes
wget <https://raw.githubusercontent.com/zakaziko99/agnosterzak-ohmyzsh-theme/master/agnosterzak.zsh-theme>
```

我使用的 oh-my-zsh 主题还需要一些依赖包

```bash
font-ancient-scripts_2.60-1_all.deb
ttf-ancient-fonts_2.60-1_all.deb
```

这些依赖包可以去 https://pkgs.org/download/ttf-ancient-fonts 下载

你可能会发现三角形图案显示不出来，这时候需要装 Powerline 字体

https://github.com/powerline/fonts

```bash
git clone <https://github.com/powerline/fonts.git>
cd fonts
./install.sh
```

### 安装 zsh 插件

自动补全插件 incr

`cd ~/.oh-my-zsh/plugins/`

`mkdir incr && cd incr`

`wget <http://mimosa-pudica.net/src/incr-0.2.zsh`

`vi ~/.zshrc`

添加一行 `source ~/.oh-my-zsh/plugins/incr/incr*.zsh`

`source ~/.zshrc`