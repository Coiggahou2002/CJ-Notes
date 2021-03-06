# Nginx

## 基础概念

### 跨域问题

同源策略是由Netscape提出的一个著名的安全策略，它是浏览器最核心也最基本的安全功能，现在所有支持JavaScript的浏览器都会使用这个策略。所谓同源是指协议、域名以及端口要相同。同源策略是基于安全方面的考虑提出来的，这个策略本身没问题，但是我们在实际开发中，由于各种原因又经常有跨域的需求，传统的跨域方案是JSONP，JSONP虽然能解决跨域但是有一个很大的局限性，那就是只支持GET请求，不支持其他类型的请求，而今天我们说的CORS（跨域源资源共享）（CORS，Cross-origin resource sharing）是一个W3C标准，它是一份浏览器技术的规范，提供了Web服务从不同网域传来沙盒脚本的方法，以避开浏览器的同源策略，这是JSONP模式的现代版。

### 角色

在客户端和服务器 Tomcat 之间加一层，用来做负载均衡.

![](https://cjpark-1304138896.cos.ap-guangzhou.myqcloud.com/note_img/nginx.webp)

**负载均衡服务器**

就是进行请求转发，降低某一个服务器的压力。

**反向代理服务器**

反向代理是与正向代理相对的概念. 

正向代理是指：多个客户端通过一个 Proxy Server 作为中间人，访问服务端（如外网 Google），起到隐藏身份或者翻墙的作用.

反向代理是指：一个客户端向服务端发送的请求会被一个“中间人”拦截，然后在众多服务器端中选择一个比较空闲的服务器，分配给这个客户端，来处理请求的应答.

![](https://cjpark-1304138896.cos.ap-guangzhou.myqcloud.com/note_img/rev_proxy.webp)

