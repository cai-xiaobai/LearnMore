OpenSSL SSL_read: Connection was reset, errno 10054



在git clone vuex 项目时报错

![git-SSL报错](D:\个人资料\LearnMore\知识总结\图片\git\git-SSL报错.png)

这是服务器的SSL证书没有经过第三方机构的签署，所以报错。

解决办法：

```
git config --global http.sslVerify "false"
```

