# 🎈访问一个网站的全过程

**Step 1. 在浏览器地址栏键入网址并按回车**

例如输入了: `http://drive.google.com/folder/file.txt`

浏览器首先解析你输入的 URL 地址，第一步解析协议，发现是 http 协议，需要传输 HTML 文件.

下一步，浏览器拿着域名去询问 DNS 服务器，找到这个网址对应的 IP 地址（Google Drive 的服务器 IP 地址）

你的电脑和 Google Drive 的服务器建立 TCP 连接，向服务器发送请求，申请主机目录下 `/folder/file.txt` 文件的访问权限.

服务器端审核通过后，将文件传输到你的个人电脑上

