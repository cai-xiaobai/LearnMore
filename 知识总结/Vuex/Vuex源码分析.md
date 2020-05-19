# Vuex源码分析

示例

```javascript
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)
```

1.Vuex的注册

```javascript
let Vue // bind on install

export function install (_Vue) {
  if (Vue && _Vue === Vue) {
    if (__DEV__) {
      console.error(
        '[vuex] already installed. Vue.use(Vuex) should be called only once.'
      )
    }
    return
  }
  Vue = _Vue
  applyMixin(Vue)
}
```



```javascript
//applyMixin
export default function (Vue) {
  const version = Number(Vue.version.split('.')[0])

  if (version >= 2) {
    Vue.mixin({ beforeCreate: vuexInit })
  } else {
    // override init and inject vuex init procedure
    // for 1.x backwards compatibility.
    const _init = Vue.prototype._init
    Vue.prototype._init = function (options = {}) {
      options.init = options.init
        ? [vuexInit].concat(options.init)
        : vuexInit
      _init.call(this, options)
    }
  }

  /**
   * Vuex init hook, injected into each instances init hooks list.
   */

  function vuexInit () {
    const options = this.$options
    // store injection
    // 当我们在执行new Vue的时候，需要提供store字段
    if (options.store) {
      // 如果是root，将store绑到this.$store
      this.$store = typeof options.store === 'function'
        ? options.store()
        : options.store
    } else if (options.parent && options.parent.$store) {
      // 否则拿parent上的$store
      // 从而实现所有组件共用一个store实例
      this.$store = options.parent.$store
    }
  }
}
```



2.store初始化

```javascript
export class Store {
  constructor (options = {}) {
    if (!Vue && typeof window !== 'undefined' && window.Vue) {
      // 挂载在window上的自动安装，也就是通过script标签引入时不需要手动调用Vue.use(Vuex)
      install(window.Vue)
    }

    if (process.env.NODE_ENV !== 'production') {
      // 断言必须使用Vue.use(Vuex),在install方法中会给Vue赋值
      // 断言必须存在Promise
      // 断言必须使用new操作符
      assert(Vue, `must call Vue.use(Vuex) before creating a store instance.`)
      assert(typeof Promise !== 'undefined', `vuex requires a Promise polyfill in this browser.`)
      assert(this instanceof Store, `Store must be called with the new operator.`)
    }

    const {
      plugins = [],
      strict = false
    } = options

    // store internal state
    this._committing = false
    this._actions = Object.create(null)
    this._actionSubscribers = []
    this._mutations = Object.create(null)
    this._wrappedGetters = Object.create(null)
    // 这里进行module收集，只处理了state
    this._modules = new ModuleCollection(options)
    // 用于保存namespaced的模块
    this._modulesNamespaceMap = Object.create(null)
    // 用于监听mutation
    this._subscribers = []
    // 用于响应式地监测一个 getter 方法的返回值
    this._watcherVM = new Vue()

    // 将dispatch和commit方法的this指针绑定到store上，防止被修改
    const store = this
    const { dispatch, commit } = this
    this.dispatch = function boundDispatch (type, payload) {
      return dispatch.call(store, type, payload)
    }
    this.commit = function boundCommit (type, payload, options) {
      return commit.call(store, type, payload, options)
    }

    // strict mode
    this.strict = strict

    const state = this._modules.root.state

    // 这里是module处理的核心，包括处理根module、action、mutation、getters和递归注册子module
    installModule(this, state, [], this._modules.root)

    // 使用vue实例来保存state和getter
    resetStoreVM(this, state)

    // 插件注册
    plugins.forEach(plugin => plugin(this))

    if (Vue.config.devtools) {
      devtoolPlugin(this)
    }
  }
}
```



2.1模块收集

调用ModuleCollection构造函数，通过path的长度判断是否为根module，首先进行根module的注册，然后递归遍历所有的module，子module添加到其父module的_children属性上，最终形成一棵树。

接着，一些变量的初始化，然后

2.2绑定commit和dispatch的this指针

将this绑定为store

2.3模块安装