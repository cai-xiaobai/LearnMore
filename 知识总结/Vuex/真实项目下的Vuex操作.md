真实项目下的Vuex最细每步操作

这里以user为例子

Vuex 允许我们将 store 分割成**模块（module）**

1.首先在modules文件夹里创建user.js，初始 state 对象

```javascript
// user.js
const user = {
  state: {
    username: '',
    userid: '',
    ucHost: ''
  },

  mutations: {},
  actions:{}
}

export default user
```

2.使用常量替代 Mutation 事件类型

```javascript
//mutation-type.js
/* ==========================
USER modules
============================*/
// 获取用户资料
export const GET_USER_INFO = 'GET_USER_INFO'
```

3.在user.js添加mutation，事件类型为mutation-type.js的GET_USER_INFO。

将向 `store.commit` 传入额外的参数，即 mutation 的 **载荷（payload）**赋值给state

```javascript
// user.js
import { GET_USER_INFO } from '../mutation-types.js'
const user = {
  ...
  mutations: {
    [GET_USER_INFO]: (state, payload) => {
      state.username = payload.name
      state.userid = payload.id
      state.ucHost = payload.ucHost
    }
  },
  actions: {}
}

export default user
```

4.加入Action函数，`commit`使用ES2015 的 [参数解构](https://github.com/lukehoban/es6features#destructuring) 来简化代码。这里的$htto封装了一个axios的实例

```javascript
// user.js
...
import { $http } from '@/request'
const user = {
  ...
  actions: {
    getUserInfo({ commit }) {
      return $http('GET_USER_INFO').then(res => {
        if (res.data.retCode === 'success') {
          commit('GET_USER_INFO', res.data.result)
        }
        return res
      })
    }
  }
}
export default user
```

5.将module放入到Vuex.Store中

```javascript
//index.js
import Vue from 'vue'
import Vuex from 'vuex'
import user from './modules/user.js'
export default new Vuex.Store({
  modules:{
      user
  },
  state: {},
  mutations: {},
  actions: {},
  getters
})
```

当我们的模块越来越多时，这样index的代码将越来越长，我们可以进行优化，对modules各个模块循环存入store里

```javascript
import Vue from 'vue'
import Vuex from 'vuex'
const modContext = require.context('./modules', false, /\.js$/)

Vue.use(Vuex)

export default new Vuex.Store({
  modules: modContext.keys().reduce((result, mod) => {
    if (mod.startsWith('./index')) return
    const value = modContext(mod)
    const key = mod.split(/\.\/(\w+)\.js$/)[1]
    result[key] = value.default || value
    return result
  }, {}),
  state: {},
  mutations: {},
  actions: {},
  getters
})
```



现在你可以通过这制作一个全局前置守卫

```javascript
//beforeEach.js
import store from '@/store'
export default function(to, from, next) {
  const isAuth = to.matched.some(record => record.meta.requireAuth)
  //
  if (isAuth) {
    store
      .dispatch('getUserInfo')
      .then(res => {
        switch (res.data.retCode) {
          case 'success':
            next()
            break
          case '401':
            window.location.href = `${res.data.result}/#/admin/login?goto=${window.location.href}`
            break
          case '403':
            next('/403')
            break
        }
      })
      .catch(() => {
        next('/500')
      })
  } else {
    next()
  }
}
```

