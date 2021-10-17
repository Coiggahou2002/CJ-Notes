# JWT

组成结构

+ Header
+ Payload
+ Signature



Signature

产生过程：

+ header 和 payload 写好后分别用 Base64 编码，然后用 `.` 拼接
+ 拼接好之后用 header 里写的加密算法去加密上述拼接串（加盐），得到 signature

```javascript
var encodedString = base64UrlEncode(header) + '.' + base64UrlEncode(payload);
var signature = HMACSHA256(encodedString, 'secret');
```

