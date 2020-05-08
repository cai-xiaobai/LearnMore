从浅入深——理解JSONP的实现原理

由于浏览器的安全性限制，不允许AJAX访问  协议不同、域名不同、端口号不同的 数据接口，浏览器认为这种访问不安全；

可以通过动态创建script标签的形式，把script标签的src属性，指向数据接口的地址，因为script标签不存在跨域限制，这种数据获取方式，称作JSONP（注意：根据JSONP的实现原理，知晓，JSONP只支持Get请求）;

实现过程：

​	1.在客户端定义一个回调方法，预定义对数据的操作；

​	2.再把这个回调方法的名称，通过URL传参的形式，提交到服务器的数据接口；

​	3.服务器数据接口组织好要发送给客户端的数据，再拿着客户端传递过来的回调方法名称，拼接出一个调用这个方法的字符串，发送给客户端去解析执行；

​	4.客户端拿到服务器返回得字符串之后，当作Script脚本去解析执行，这样就能够拿到JSONP的数据了；



```html
//客户端.html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>
<body>
  <script>
    function show() {
      console.log('ok')
    }
  </script>
  <script src="http://127.0.0.1:3000/getscript"></script>
<!--  上面的script就相当于下面
  <script>
    show()
  </script>  -->
</body>
</html>
```

```javascript
//app.js
const http = require('http')

const server = http.createServer()

server.on('request', function (req, res) {
  const url = req.url

  if (url === '/getscript') {
    //拼接一个合法的JS脚本，这里拼接的是一个方法的调用
    var scriptStr = 'show()'
    //res.end 发送给 客户端， 客户端去把 这个 字符串，当作JS代码去解析执行
    res.end(scriptStr)
  } else {
    res.end('404')
  }
})
server.listen(3000, function () {
  console.log('server')
})
```

**既然知道了原理就可以对其进行升级改造了。**

上面我们的方法是写死的，但是后端并不知道我们的方法名是什么，我们用带参的形式传递给后端。

```javascript
//客户端.html
...
 <script>
    function show() {
      console.log('ok')
    }
  </script>
  <script src="http://127.0.0.1:3000/getscript?callback=show"></script>
```

```javascript
//app.js
const http = require('http')
const urlModule = require('url')

const server = http.createServer()

server.on('request', function (req, res) {

  const {pathname: url,query} = urlModule.parse(req.url, true)

  if (url === '/getscript') {
    //拼接一个合法的JS脚本，这里拼接的是一个方法的调用
    var scriptStr = `${query.callback}()`
    //res.end 发送给 客户端， 客户端去把 这个 字符串，当作JS代码去解析执行
    res.end(scriptStr)
  } else {
    res.end('404')
  }
})
server.listen(3000, function () {
  console.log('server')
})
```

现在前端修改了方法名，只要改变请求参数，后端依旧能够使用。但是这只是单纯方法的调用，并没有发挥后端的作用呀。别着急，接下来就是获取数据

```
//客户端.html
...
<script>
    function showInfo123(data) {
      console.log(data)
    }
  </script>
  <script src="http://127.0.0.1:3000/getscript?callback=showInfo123"></script>
```

```
//app.js
...
server.on('request', function (req, res) {
  const {
    pathname: url,
    query
  } = urlModule.parse(req.url, true)

  if (url === '/getscript') {
    //拼接一个合法的JS脚本，这里拼接的是一个方法的调用
    var data = {
      name: 'caicaicai',
      age: 18,
      gender: 'man'
    }
	//data对象不能直接往字符串里拼，所以使用JSON.stringfy()，对data进行转换
    var scriptStr = `${query.callback}(${JSON.stringify(data)})`
    //res.end 发送给 客户端， 客户端去把 这个 字符串，当作JS代码去解析执行
    res.end(scriptStr)
  } else {
    res.end('404')
  }
})
...
```

