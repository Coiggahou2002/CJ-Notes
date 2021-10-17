# UDP 协议

## 概况

User Datagram Protocol，用户数据报协议.

```python
# Python 中创建 UDP Socket 的写法
udp_socket = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
```

一个 UDP socket 由一个二元组标识：

$$(IP Address, \ Port\  Number)$$

## 优点

### Finer application-level control

UDP 没有拥塞控制机制，适合能容忍一定程度数据丢失且不希望数据过分延迟发送的实时应用.

比如说 FaceTime 这种视频电话应用，用户可以忍受偶尔丢帧或卡顿，但一定无法忍受类似音画不同步的延迟情况发生.

### No connection establishment

UDP 不需要做任何准备（TCP 在传输前需要三次握手）就可以开始传输数据，因此不会引入连接时延，这也是 DNS 运行在 UDP 而不是 TCP 上的原因之一.

简单来讲，UDP 说传就传，TCP 要传送就要先开一条专用管道并维持它不堵塞.

### No connection state

UDP 不需要像 TCP 那样维护连接状态和追踪各种参数

### Smaller packet header

每个 TCP 报文段的首部需要占用 20 Bytes，UDP 报文的首部只需要占用 8 Bytes.

## 争议

UDP 没有拥塞控制，一旦网络陷入拥塞状态，会导致路由器的分组大量溢出，出现大量丢包的情况.

## 报文段结构

一个 UDP 报文段称为 UDP Segment，结构如下

+ 首部
  + sourcePortNumber，2 Bytes
  + destPortNumber，2 Bytes
  + length，2 Bytes，指示整个 UDP 报文段的总字节数，即 (8 + 数据字节数) B
  + checkSum，2 Bytes
+ 数据

## 检验和 Checksum

检验和用于确定当 UDP 报文段到达目的地时，其内容相比发出时是否发生改变.

计算方法如下：

1. 将整个 UDP 报文段切割成若干个 16 bit 字
2. 将这些 16 bit 字全部加起来，得到 $\alpha$
3. 如果 $\alpha$ 溢出，需要回卷（意思就是把溢出的那部分拿出来，加到未溢出部分上）
4. 给 $\alpha$ 取反码，得到 $\beta$，$\beta$ 作为校验和，放入 segment 的首部中
5. 报文到达目的地后，再切割成若干 16 bit 字
6. 将切割出的所有 16 bit 字全部加起来，如果结果是 `1111111111111111` ，说明传输过程中报文未发生更改，否则说明报文出现了差错.

注意，UDP 只能提供差错检测，并不能提供差错恢复功能.