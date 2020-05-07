koa2  设置cookie踩坑

```javascript
ctx.cookies.set(name, value, [options]) 
```

在设置cookie时存在一些配置选项

```javascript
options={
    maxAge:"000000000"       //cookie有效时长，单位：毫秒数
    expires:"0000000000"      //过期时间，unix时间戳
    path:"/"         //cookie保存路径, 默认是'/，set时更改，get时同时修改，不然会保存不上，服务同时也获取不到
    domain:".xxx.com"       //cookie可用域名，“.”开头支持顶级域名下的所有子域名
    secure:"false"       //默认false，设置成true表示只有https可以访问
    httpOnly:"true"     //true，客户端不可读取
    overwrite:"true"    //一个布尔值，表示是否覆盖以前设置的同名的 cookie (默认是 false). 如果是 true, 在同一个请求中设置相同名称的所有 Cookie（不管路径或域）是否在设置此Cookie 时从 Set-Cookie 标头中过滤掉。
}
```

前端的地址是:http://localhost:8080

后台访问地址是:http://localhost:3000

如果在后台设置cookie配置，添加了 domain: 'localhost' 这项，前端就会报错。这里不知道为何获取不到localhost，具体原因也不清楚，不填的话能在前端设置成功。

