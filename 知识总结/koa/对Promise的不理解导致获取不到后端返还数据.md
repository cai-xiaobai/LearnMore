对Promise的不理解导致获取不到后端返还数据

背景：将 axios 封装成 service，service是一个Promise对象。

问题描述：在apis.js中我使用了.then和.catch，致使return出来的是里面表达式的结果。然后在SignUp.vue中再次使用.then，此时post（） 是一个是表达式，而不是Promise对象。

解决办法：

1.在apis.js中的post里添加一个Promise对象，将原有的值放进去，返回出来的便是一个Promise对象即可使用.then。

2.将apis.js中的post里的.then .catch删除

关于Promise和async await好文推荐：

https://mp.weixin.qq.com/s/U46Z0lc1HwzEw6cXMhmudw

以下是原错误代码：

```javascript
//SignUp.vue
import { post } from '../../axios/apis';
methods: {
    async register () {
      await post('/api/users/create', {
        loginName: this.registerForm.name,
        pwd: md5(this.registerForm.pass),
        email: this.registerForm.email,
      }).then((response) => {
        console.log(response);
        if (response.status === 200) {
          this.$Message.success('注册成功');
          this.$router.push({ name: 'home' });
        } else {
          this.$Message.error(response.data.msg);
        }
      });
	},
}
```

``` javascript
//apis.js
export const post = (url, data) => {
  return service({
    method: 'post',
    url,
    data
  }).then(function (response) {
    console.log(response);
  })
  .catch(function (error) {
    console.log(error);
  });
};
```

