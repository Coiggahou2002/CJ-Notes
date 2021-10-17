# HTTP 协议

## 概述

HTTP 全称 HyperText Transfer Protocol，超文本传输协议，是 Web 的应用层协议.

基本流程是：Client 向 Server 发送 HTTP 请求，Server 给予 HTTP 响应.

HTTP 使用 TCP 作为传输协议.

HTTP 是无状态协议，不保存 Client 的任何信息.

HTTP 的默认端口号：80

## 连接方式

HTTP 使用的连接方式有两种——持续连接与非持续连接.

默认使用持续连接.

### 1. 非持续连接

每个请求-响应对经由一个单独的 TCP 连接发送

缺点：

+ 需要为每一个请求的对象维护一个 TCP 连接，过于耗费资源
+ 每个对象经受两倍 **RTT (Round-Trip Time, 往返时间)** 的交付时延

### 2. 持续连接

Server 在发送响应后保持 TCP 连接打开，在相同的 Client - Server 之间，后续的请求和报文依旧通过该 TCP 连接传送.

一般来说，一条持续连接经过一定时间仍未被使用，Server 就关闭该连接.

## HTTP 报文格式

### 请求报文

总体框架

```yaml
GET /somedir/page.html  HTTP/1.1
Host: www.hit.edu.cn
Connection: close
User-agent: Mozilla/5.0
Accept-language: fr
```

+ 请求行
  + 方法字段
    + GET，请求对象
    + POST，填表单
    + HEAD，请求报文
    + PUT，上传
    + DELETE，删除
  + URL 字段
  + HTTP 版本字段
+ 首部行
  + Host，主机名
  + Connection，告诉 Server 发完请求对象就关闭该连接
  + User-agent，Client 的有关版本信息
  + Accept-language，要求返回所请求对象的某个语言的版本

### 响应报文

```yaml
HTTP/1.1  200  OK
Connection: close
Date: Tue, 18 Aug 2015 15:44:00 GMT
Server: Apache/2.2.3 (CentOS)
Last-Modified: Tue, 18 Aug 2015 15:11:00 GMT
Content-Length: 6821
Content-Type: text/html
(data...data...)
```

+ 状态行
+ 首部行
+ 实体体 (entity body)

常见状态码：

```yaml
200  OK
301  Moved Permanently           所请求对象已被永久转移
400  Bad Request                 服务器无法理解请求
404  Not Found                   在Server上找不到被请求的文档
505  HTTP Version Not Supported  服务器不支持当前所用的HTTP版本
```

## cookie

cookie 技术有 4 个组件：

+ HTTP 响应报文里有一个 cookie 首部行
+ HTTP 请求报文里有一个 cookie 首部行
+ Client 端浏览器管理着 cookie 文件
+ Web 站点后端数据库中存着识别码