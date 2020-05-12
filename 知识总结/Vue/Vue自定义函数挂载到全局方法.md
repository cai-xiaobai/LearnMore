***Vue自定义函数挂载到全局方法***

看了很多方法介绍，基本思路是，定义方法->在main.js中引入->就能全局使用，提高代码的复用性。我这里只写下工作中常见和常用的方法

**使用export default + install + Vue.prototype**

​	方法写在哪，怎么写，一般按项目规则和个人习惯

​	我这里以$http为例

1.创建request文件夹，创建index.js文件,写入方法

```javascript
const $http = function(...){ //全局方法最好用$开头
	...
}
export default vueHttp = {
	install(Vue){
        ...
        Object.defineProperty(Vue.prototype,'$http',{
            value:$http,
            writable:false
        })//这里使用了数据绑定的方法，下面给出学习链接
    }
}
export {$http}
```



2.在main.js中写入函数

```javascript
import http from '@/request'
Vue.use(http)
```



3.在所有组件里可调用函数

```
this.$http(...);
```



数据绑定学习链接:https://www.jianshu.com/p/c02cb881bea8

参考：

https://www.cnblogs.com/conglvse/p/10062449.html

https://www.cnblogs.com/linjiangxian/p/11457517.html

https://www.jianshu.com/p/8ec5e1233ffe