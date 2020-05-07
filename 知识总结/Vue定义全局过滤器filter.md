**Vue定义全局过滤器filter**

这里介绍的是多个过滤器一起添加到全局中

1.创建方法

首先src下新建plugin文件夹，用来存放插件。

在plugin文件夹内新建filters.js，编写方法（如隐藏手机号码等等...）

```javascript
/**
 * 隐藏手机号码
 * @param val {Number, String} 转换的字符串对象
 * @param retain {Number} 保留位数
 * @return {String}
 */
export privatePhone = function(val,retain = 4){
    if(!NUMBER(val) || String(val).length !== 11 || retain==0 ) return val;
    let phone = String(val)
    let digit = 11 - 3 - retain
    let reg = new RegExp(`^(\\d{3})\\d{${digit}}(\\d{${retain}})$`)
    return mobile.replace(reg,`$1${'*'.repeat(digit)}$2`)
}
```

2.添加到Vue全局中

在main.js中引入,添加

```javascript
import * as filters from './plugins/filters.js'
Object.keys(filters).forEach(key=>{
    Vue.filter(key,filters[key])//插入过滤器名和对应方法
})
```

3.使用

使用方法有两种

​		a.在双花括号插值（用的较多）

```javascript
{{ phone | privatePhone }}
```

​		b.在v-bind表达式中使用

```javascript
<div v-bind:data=" phone | privatephone "></div>
```

PS:

参数的写法：上述代码中privatePhone的第一个参数即是phone

详细的大家可以看这:

https://www.jianshu.com/p/ad21df1914c5