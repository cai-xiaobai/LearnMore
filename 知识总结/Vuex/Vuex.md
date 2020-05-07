Vuex开始

Vuex 是一个专为 Vue.js 应用程序开发的**状态管理模式**

安装Vuex后，创建一个store仅需一个初始state对象和一些mutation。就可以通过store.state来获取状态对象，以及通过store.commit()方法触发状态变更。

为了在Vue组件中访问 `this.$store` property，需要为Vue实例提供创建好的store。

```javascript
new Vue({
  el: '#app',
  store: store,
})
```

触发变化也仅仅是在组件的 methods 中提交 mutation。能更明确地追踪到状态的变化