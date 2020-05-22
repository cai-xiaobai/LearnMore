**webpack + vue img 的v-bing:src 传参问题**

问题描述：

父组件传递图片地址到子组件，子组件在computed计算属性中获取后，放入template的v-bind:src中，webpack不进行编译，还保持着相对路径的状态，图片无法显示。

```javascript
//父组件
data(){
    return{
        image_url: "../assets/imgs/Mainlogo.jpg",
    }
}
```

```javascript
//子组件
computed: {
    transtedInfo () {
        return {
          title: this.item.title,
          cover: this.item.image_url || "",
        };
    },
  },
```

![image-20200427111454452](C:\Users\monda\AppData\Roaming\Typora\typora-user-images\image-20200427111454452.png)



浏览器保持着相对路径：
![image-20200427111239528](C:\Users\monda\AppData\Roaming\Typora\typora-user-images\image-20200427111239528.png)



解决方法：

通过require（transtedInfo.cover）再传入src，但是require报错了，

![image-20200427112018762](C:\Users\monda\AppData\Roaming\Typora\typora-user-images\image-20200427112018762.png)

**原因是require路径上无法使用变量(会因找不到上下文环境而查找失败)，报依赖错误**

- 将父组件传递address改为Mainlogo.jpg

```javascript
//父组件
data(){
    return{
        image_url: "Mainlogo.jpg",
    }
}
```

- 在子组件中 transtedInfo. cover : require("相对路径地址"+this.item.address)

```javascript
//子组件
computed: {
    transtedInfo () {
        return {
          title: this.item.title,
          cover: require("../assets/imgs/" + this.item.image_url) || "",
        };
    },
  },
```

- 效果实现了

src赋值问题解决：https://www.dazhuanlan.com/2019/10/26/5db43c650f9d6/

require图片失败问题解决：http://www.luyixian.cn/news_show_294991.aspx