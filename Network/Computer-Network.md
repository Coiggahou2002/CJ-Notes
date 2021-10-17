# 计算机网络基础

## 前置知识

### 比特与字节

比特 (bit)：也叫位，每个比特的值为 0 或 1

字节 (Byte)：即平常说的 `1kB` 中的 `B`，一个字节等于 8 比特 1 *Byte* = 8 *bits*

### 关于传输速率

1 *kbps* = 1000 *bits*/*s*

1 *kBps* = 1000 *bytes*/*s*

1 *kB*/*s* = 8 *kb*/*s*

`kbps` 表示每秒能够传输多少千个比特。 `kBps` 则表示每秒能够传输多少千个字节

### 英文术语

| 缩写   | 详写                                 | 翻译                                     | 备注                                                         |
| ------ | ------------------------------------ | ---------------------------------------- | :----------------------------------------------------------- |
| LAN    | Local Area Network                   | 局域网                                   |                                                              |
| WAN    | Wide Area Network                    | 广域网                                   |                                                              |
| WWW    | World Wide Web                       | 万维网                                   |                                                              |
|        | internet                             | 网际网                                   | 将多个网络连接构成更大的网络                                 |
|        | The Internet                         | 互联网                                   | 互联全世界的计算机网络                                       |
| ISP    | Internet Service Provider            | 互联网服务提供商                         |                                                              |
| URI    | Uniform Resource Identifier            | 统一资源标识符          |                                                              |
| URL    | Uniform Resource Locator             | 统一资源定位器                           |                                                              |
| MAC    | Media Access Control address        | MAC地址                                  | 一个网卡一个MAC地址                                          |
| DNS    | Domain Name System                   | 域名管理系统                             | 相当于电话簿，根据域名查询IP地址                             |
| ADSL   | Asymmetric Digital Subscriber Line   | 非对称数字用户环路                       |                                                              |
| DSL    | Digital Subscriber Line              | 数字用户线                               |                                                              |
| FTTH   | Fiber To The Home                    | 光纤到户                                 |                                                              |
| TCP/IP |                                      | 利用IP进行通信时所必须用到的协议群的总称 |                                                              |
| IP     | Internet Protocol                    | 网际协议                                 | 根据IP地址把数据送给正确的计算机                             |
| UDP    | User Datagram Protocol               | 用户数据报协议                           | 根据端口号把数据送给正确的程序                               |
| TCP    | Tramisson Control Protocol           | 传输控制协议                             | 所有数据必须到达。可处理乱序、丢包问题并自动检测网络拥堵程度，实时调整发包频次 |
| OSI    | Open System Interconnection model    | 开放式系统互联通信参考模型               |                                                              |
|        | Physical Layer                       | 物理层                                   |                                                              |
|        | Data Link Layer                      | 数据链路层                               |                                                              |
|        | Network Layer                        | 网络层                                   |                                                              |
|        | Transport Layer                      | 传输层                                   |                                                              |
|        | Session Layer                        | 会话层                                   |                                                              |
|        | Presentation Layer                   | 表示层                                   |                                                              |
|        | Application Layer                    | 应用程序层                               |                                                              |
| HTTP   | Hyper Text Transfer Protocol         | 超文本传输协议                           |                                                              |
| HTML   | Hyper Text Markup Language           | 超文本标记语言                           |                                                              |
| CSS    | Cascading Style Sheet                | 层叠样式表                               |                                                              |
| FTP    | File Transfer Protocol               | 文件传输协议                             |                                                              |
| SMTP   | Simple Mail Transfer Protocol        | 邮件传输协议                             |                                                              |
| POP3   | Post Office Protocol - Version 3     | 邮件传输协议                             |                                                              |
| IMAP   | Internet Message Access Protocol     | 邮件传输协议                             |                                                              |
| MIME   | Multipurpose Internet Mail Extension | 邮件传输协议                             |                                                              |
|        | Router                               | 路由器                                   | 找出最高效的传输线路                                         |

### 端口

常见端口


| 协议/应用 | 端口号 |
|  :---  | :---  |
| HTTP 协议 | 80 |
| HTTPS 协议 | 443 |
| SSH | 22 |
| MySQL | 3306 |
| Redis | 6379 |
| Web 前/后端项目 | 8080 |
| 代理 | 1080 |

## 概念

### 节点和主机

节点：配有 IP 地址但不进行路由控制的主机

路由器：既配有 IP 地址又具备路由控制能力的主机

**节点**是主机和路由器的总称

### 信源、信道和信宿

信源：信息发送者

信道：传送信息的媒体

信宿：信息的接收者

### 网络拓扑

网络拓扑指的是网络的连接形态 + 总线型 + 环型 + 星型 + 网状型

### 分组交换

将不同大小的信息按统一的大小拆分，加上辅助信息（如发送地址）后，重新封装成数据包来传输（可以混合次序传输）

### 调制解调器 Modem

**调制(Modulation)**

在计算机信息传输进电话线路之前，将数字信号转换为模拟信号

**解调(Demodulation)**

将经由电话线路传来的模拟信号转换为数字信号，再给计算机处理

> 其实就是我们平常所说的“猫”

### 路由器 Router

可以连接不同环境的局域网和广域网，还能选出两个网络节点之间最近、最快的传输路径。

相当于在各种范围的网络之间进行跳转的“中间人”

### 统一资源定位器 URL

**统一格式**

```
协议://主机名.域名.顶级域名/路径/文件名.扩展名
```

## 🌎 不同范围的网络

### 局域网 LAN

+ 有限地理范围内（几千米）
+ 传输速度快
+ 常见类型
  + 以太网 Ethernet
  + 令牌环网 Token Ring
  + 令牌总线网 Token Bus
+ 无线局域网
  + Wi-Fi
  + Bluetooth

### 广域网 WAN

+ 相比局域网，时延更大
+ 需要借助公共电信系统
+ 常见广域网
  + PSTN (Public Switch Telephone Network)
  + DDN 专线 (Digital Data Network)
  + ISDN (Integrated Service Digital Network)
  + ADSL (Asymmetric Digital Subscriber Line)
+ 无线广域网
  + 3G 网络
    + CDMA 2000
    + WCDMA
    + TD-SCDMA
  + 4G 网络
    + LTE
    + HSPA+
    + WiMax

### 互联网（网际网） internet

将若干个局域网、广域网之间通过路由器互联，形成网际网

### 因特网 The Internet

是世界上最大的互联网，是由广域网连接的局域网的最大集合

## 📜 几种协议的解释

### TCP 协议

+ 数据包 (Packets) 有自己的序号，到达目的地后可排序并重新组合
+ 要求接收方收到数据并确认无误后，发回确认码 (ACK)
+ 根据确认码发回的成功率和来回时间，推断网络的拥挤程度，自动调整发包频次

### HTTP 协议

属于 OSI 模型中应用层的协议

### HTML 协议

属于 OSI 模型中表示层的协议

### 文件传输协议

即指 FTP 协议，传输时可以选择二进制或文本方式进行传输

**FTP 是 OSI 应用层，不是传输层**

### 邮件传输协议

+ SMTP 协议 (Simple Mail Transfer Protocol)
+ POP3 协议 (Post Office Protocol - Version 3)
+ IMAP 协议 (Internet Message Access Protocol)
+ MIME 协议 (Multipurpose Internet Mail Extension)

以上协议都属于 OSI 模型中的应用层

## 🍔 TCP/IP 分层模型

### 一、物理层

负责机电信号的传输与控制

### 二、数据链路层

#### 概述

传输数据帧

属于接口层，类似于“驱动程序”的功能 

### 三、网络层

#### 概述

负责解析信息的源和目标地址。

该层主要由 IP 协议和 ICMP 协议组成。

#### IP 协议

IP 协议根据 IP 地址将分组数据包发送到目的主机。

虽然 IP 也是分组交换的一种协议，但它不具有重发机制，属于非可靠性传输协议。

#### 路由控制 Routing

#### 与数据链路层的关系

数据链路层就像机票/火车票，只负责某个区间的通信传输，而网络层就像旅行日程表，起到引导全程的作用。

### 四、传输层

该层最主要的功能是让**应用程序之间**实现通信。

如何识别特定的应用程序？靠的是端口号。

该层代表性的协议有： + UDP + 不会关注接收端是否真的收到了数据 + TCP + 保证两端主机之间通信可达 + 能够处理丢包、乱序的情况 + 能有效利用带宽、缓解网络拥堵

### 五、应用层

TCP/IP 应用的架构绝大多数属于**客户端/服务端模型 (Client / Server model)**

客户端发送请求给服务端，服务端接受请求并发回数据包

