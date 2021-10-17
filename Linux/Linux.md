# Linux

## 系统环境

### 环境变量

环境变量（environment variables）一般是指在操作系统中用来指定操作系统运行环境的一些参数，这些参数会对系统行为产生影响。

比如常用的PATH环境变量，当要求系统运行一个程序而没有告诉它程序所在的完整路径时，系统除了在当前目录下面寻找此程序外，还会到PATH中指定的路径去找。你可以在终端使用printenv PATH查看当前PATH变量的值。

**常用命令**

设置临时环境变量

```shell
export TOMCAT=/home/coiggahou/Developer_Tools/apache-tomcat-9.0.50
```

查看环境变量值

```shell
echo $TOMCAT
```

**环境变量存放位置**

+ 系统环境变量
  + `/etc/profile`
  + `/etc/profile.d/`
  + `/etc/bash.bashrc`
+ 用户环境变量
  + `~/.profile`
  + `~/.bashrc`
  + `~/.bash_profile`
  + `~/.bash_login`

其中 `~/.profile` 中的环境变量在图形界面程序和 Shell 中都起作用，但 `~/.bashrc` 这一类文件中设置的环境变量只会在命令行中起作用.

如果想要设置永久环境变量，就要根据需要选择上方的其中一个位置，在文件末尾添加 `export xxx=xxx`，然后执行 `source ~/.profile` 命令让其立即生效.

## 软件安装

安装：`apt-get install <软件包名>`

卸载：`apt-get remove <软件包名>`

完全卸载：`apt-get remove --purge <软件包名>`

列出所有 deb 安装包：`dpkg -l`

卸载 deb 安装包：`dpkg -r <deb包名>`

更新：`sudo apt-get update && sudo apt-get install <软件包名>`

## 进程

列出含有 `关键字` 的进程名和 id

```shell
ps -e | grep <关键字>
```

## 文件操作

删除指定空目录

```shell
rm <empty_dir_name>
```

删除指定目录及其下面的所有文件

```shell
rm -r <dir_name>
```

## 网络

测试 ICMP 连接

```shell
ping <IP地址>
```

测试 TCP 连接

```shell
telnet <IP地址> <端口号>
```

查看端口占用

```shell
netstat -tunlp | grep <端口号>
```

## 系统服务

systemctl 用于管理系统服务，详情见 http://manpages.ubuntu.com/manpages/focal/zh_TW/man1/systemctl.1.html

