## 确保Web安全的HTTPS

<font color="orange">HTTP</font> 的不足：

​	通信使用明文（不加密），内容可能被窃听。

​    不验证通信方的身份，因此可能遭遇伪装。

​    无法证明报文的完整性，所以有可能已遭篡改。

在HTTP协议中有可能存在信息窃听或身份伪装等安全问题，使用 <font color="orange">HTTPS</font> 通信机制可以有效地防止这些问题。

HTTPS并非是应用层的一种新协议。只是 <font color="orange">HTTP</font> 通信接口部分用<font color="orange">`SSL`</font>（Secure Socket Layer） 和 <font color="orange">`TLS`</font>（ Transport Layer Security）协议代替而已。

| 应用（HTTP） | 应用（HTTP） |
| :----------: | :----------: |
|     TCP      |     SSL      |
|      IP      |     TCP      |
|              |      IP      |

在采用<font color="orange">`SSL`</font>后，<font color="orange">HTTP</font>就拥有了<font color="orange">HTTPS</font>的加密、证书和完整性保护这些功能。

<font color="orange">`SSL`</font>是独立于<font color="orange">HTTP</font>协议，所以不光是<font color="orange">HTTP</font>协议，其他运行在应用层的SMTP和Telnet 等协议均可配合<font color="orange">`SSL`</font>协议使用。可以说<font color="orange">`SSL`</font>是当今世界上应用最为广泛的网路安全技术。

