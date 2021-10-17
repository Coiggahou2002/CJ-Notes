# 自己搭梯子实践记录

## 准备 VPS 服务器

### 购买

我自己买的是 [HostUs](https://hostus.us/) 家的 VPS 服务器（有三种规格，我选择了最便宜的 OPENVZ VPS）

选择类型之后，下一步是选择 Plans，也就是要购买的服务规格.

可选项有：

+ CPU 核心数
+ 硬盘空间，内存，交换空间
+ 流量 (Bandwidth)
+ IPv4 地址个数
+ 服务器位置

![](https://cjpark-1304138896.cos.ap-guangzhou.myqcloud.com/note_img/image-20210727163204612.png)

其中服务器位置比较重要，尽量选择离自己所在地区近的位置.

再者我们关心的就是每月流量了，我选择了最低的 250 GB，只要不看太多视频基本可以满足.

总的价格折算成人民币大概是 65 块钱 / 3 个月，也就是一个月 20 块左右，还是很划算的.

### 连接

购买之后记录下基本信息，即服务器的 **IP 地址和登录密码**，用于后面连接.

Linux 端使用 `openssh-server` 进行远程连接（Windows 端使用 XShell 建立远程会话）

没有的话需要先安装

```shell
sudo apt-get install openssh-server
```

安装完成之后终端输入下列命令，按回车之后输入密码，即可登录服务器

```shell
ssh -l <用户名> <服务器IP地址>
# 如 ssh -l root 45.163.33.123
```

看见终端用户名切换成远程服务器的用户名即表示登录成功.

## Shadowsocks 服务端

### 安装

首先前往 Shadowsocks 的大本营 https://github.com/shadowsocks

![](https://cjpark-1304138896.cos.ap-guangzhou.myqcloud.com/note_img/image-20210727164319305.png)

可以看到有很多版本的 Shadowsocks，我使用的是轻量级的命令行版本 [shadowsocks-libev](https://github.com/shadowsocks/shadowsocks-libev)

因为我买的 VPS 服务器上自带的系统是 `Red Hat Enterprise Linux` 也就是 RHEL（一般来说，大多数服务器装的不是 CentOS 就是 Red Hat），所以使用 Red Hat 对应安装命令来安装.

```shell
cd /etc/yum.repos.d/

curl -O https://copr.fedorainfracloud.org/coprs/librehat/shadowsocks/repo/epel-7/librehat-shadowsocks-epel-7.repo

yum install -y shadowsocks-libev
```

如果安装报错

```shell
Error: Package: shadowsocks-libev-3.1.3-1.el7.centos.x86_64 (librehat-shadowsocks)
           Requires: libsodium >= 1.0.4
Error: Package: shadowsocks-libev-3.1.3-1.el7.centos.x86_64 (librehat-shadowsocks)
           Requires: mbedtls
```

说明系统没有启用 EPEL (Extra Packages for Enterprise Linux)，那就需要先启用，再安装 shadowsocks

```shell
yum install -y https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
yum install -y shadowsocks-libev
```

然后通过运行 `ss-server` 命令来验证安装是否成功.

### 配置

shadowsocks-libev 默认读取 `/etc/shadowsocks-libev/config.json` 这个配置文件

我们根据自己的需要修改这个配置文件.

+ `server` 填 `0.0.0.0` 即可，表示服务端接受来自任何网络接口的连接

+ `server_port` 是服务端要监听的端口，自己选一个就可以（端口号要在 1025 ~ 65535 之间）

+ `password` 填服务器登录密码

+ `method` 表示加密方式，直接默认也行，我用的是 `rc4-md5`
+ `mode` 表示服务器要监听的协议，默认值是 tcp，我填了 `tcp_and_udp`

```json
{
	"server": "0.0.0.0",
	"server_port": 10443,
	"password": "Your password",
	"method": "aes-256-cfb",
	"mode": "tcp_and_udp"
}
```

注意，以上配置将来在客户端配置时也需要保持一致

### 防火墙放行端口

CentOS 和 RHEL 系统自带了防火墙，会封锁除了 22 端口（用于 SSH 连接）以外的所有端口，所以，要时我们的 Shadowsocks 服务能跑，需要给我们刚刚填的 `server_port` 开放防火墙权限.

例如我们在服务端配置文件填了 `"server_port": 50996` ，那么我们接下来需要在终端执行

```shell
firewall-cmd --permanent --add-port=50996/tcp
firewall-cmd --permanent --add-port=50996/udp
firewall-cmd --reload
```

注意，有些 VPS 服务器还需要在服务器的网页控制面板再开放一次端口权限.

### 启动服务端

启动命令

```shell
systemctl start shadowsocks-libev
```

查看服务状态

```shell
systemctl status shadowsocks-libev
```

关闭或重启

```shell
systemctl stop shadowsocks-libev
systemctl restart shadowsocks-libev
```

配置服务开启自启动（可选）

```shell
systemctl enable shadowsocks-libev
```

## Shadowsocks 客户端

> 注意，如果你的客户端也使用 shadowsocks-libev，才按照下列步骤配置，如果你有其他的 Shadowsocks 客户端（如 electron-ssr），则不必遵循下列步骤，只需要把关键信息配置好，也一样可以使用.

### 安装

由于我是用的是 Ubuntu 20.04，所以直接用命令安装（Windows 系统请使用 Shadowsocks 客户端）

```shell
sudo apt-get install shadowsocks-libev
```

### 配置

我们复制一份 `config.json` 并改名为 `local.json`，以示这是**客户端的**配置文件

```shell
sudo cp /etc/shadowsocks-libev/config.json /etc/shadowsocks-libev/local.json
sudo vim /etc/shadowsocks-libev/local.json
```

然后更改配置文件的读取位置

```shell
sudo vi /lib/systemd/system/shadowsocks-libev-local@.service
```

替换掉其中的 ExecStart 的路径（告诉服务读取配置的时候读 `local.json` 而不是其他配置文件）

```shell
ExecStart=/usr/bin/ss-local -c /etc/shadowsocks-libev/local.json
```

然后我们来编辑配置文件

```json
{
  "server": "服务器地址",
  "server_port": "服务器监听端口",
  "local_port": 1081,
  "password": "服务器登录密码",
  "timeout": 60,
  "method": "aes-256-cfb",
  "mode": "tcp_and_udp"
}
```

注意以下几项要与服务端的配置保持一致

+ `server_port`
+ `password`
+ `method`
+ `mode`

同时建议 `local_port` 不要使用 1080，避免与本机已经存在的代理软件冲突，1081 或 1082 等等都可以.

### 启动服务

```shell
# 启动
sudo systemctl start shadowsocks-libev-local@.

# 查看运行情况
sudo systemctl status shadowsocks-libev-local@.

# 配置开机自启
sudo systemctl enable shadowsocks-libev-local@.
```

## 浏览器代理

安装 Chrome 的插件 `Proxy SwitchyOmega`，进行如下设置

![](https://cjpark-1304138896.cos.ap-guangzhou.myqcloud.com/note_img/image-20210727171945077.png)

然后在 Chrome 右上角插件处选择自己的 Proxy 策略.

## 多账号配置

使用 ss-manager 可配置多个登录账号

在 `/etc/shadowsocks-libev` 下新建 `manager.json` 如下，填入你希望增加的账号（主账号写在 `config.json` 中，不写在这里）

```json
{
	"port_password": {
		"16666": "password1",
		"16667": "password2",
		"16668": "password3"
	},
	"timeout": 60,
	"method": "rc4-md5"
}
```

下一步很重要，用防火墙放行端口（不然防火墙默认是锁住了除 22 以外的其他端口的）

```shell
firewall-cmd --permanent --add-port=16666/tcp
firewall-cmd --permanent --add-port=16666/udp
firewall-cmd --reload
# 其余两个端口同理
```

启动服务之后再启动一下 ss-manager 即可

```shell
systemctl start shadowsocks-libev
ss-manager -c /etc/shadowsocks-libev/manager.json
```

## 全局代理设置

### 安装 Polipo

```shell
wget http://archive.ubuntu.com/ubuntu/pool/universe/p/polipo/polipo_1.1.1-8_amd64.deb
sudo dpkg -i polipo_1.1.1-8_amd64.deb
```

### 修改配置文件

修改 `/etc/polipo/config`

```properties
# This file only needs to list configuration variables that deviate
# from the default values.  See /usr/share/doc/polipo/examples/config.sample
# and "polipo -v" for variables you can tweak and further information.

logSyslog = true
logFile = /var/log/polipo/polipo.log

proxyAddress = "0.0.0.0"

# 此处1080换成你的Shadowsocks代理服务器的Client端口
socksParentProxy = "127.0.0.1:1080"
socksProxyType = socks5

chunkHighMark = 50331648
objectHighMark = 16384

serverMaxSlots = 64
serverSlots = 16
serverSlots1 = 32
```

### 重启服务

```shell
systemctl restart polipo
```

每次打开全局代理都需要设置环境变量（重启后消失），也可以设置永久全局变量

```shell
export http_proxy="http://127.0.0.1:8123"
```

验证全局代理成功（能返回含内容的 HTML 文本就算成功）

```hell
curl www.google.com
```

---

恭喜，来到这里，你已经成功搭建了自己的梯子~

以后进入系统，想翻墙就

```shell
systemctl start shadowsocks-libev-local@.
```

想开全局代理就

```shell
systemctl start po
```

