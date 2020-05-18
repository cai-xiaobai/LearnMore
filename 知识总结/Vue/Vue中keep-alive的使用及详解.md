# Vue中keep-alive的使用及详解

概念
    keep-alive 是 Vue 的内置组件，当它包裹动态组件时，会缓存不活动的组件实例，而不是销毁它们。和 transition 相似，keep-alive 是一个抽象组件：它自身不会渲染成一个 DOM 元素，也不会出现在父组件链中。

作用
    在组件切换过程中将状态保留在内存中，防止重复渲染DOM，减少加载时间及性能消耗，提高用户体验性

原理
    在 created 函数调用时将需要缓存的 VNode 节点保存在 this.cache 中／在 render（页面渲染） 时，如果 VNode 的 name 符合缓存条件（可以用 include 以及 exclude 控制），则会从 this.cache 中取出之前缓存的 VNode 实例进行渲染。

    VNode：虚拟DOM，其实就是一个JS对象

Props
include - 字符串或正则表达式。只有名称匹配的组件会被缓存。
exclude - 字符串或正则表达式。任何名称匹配的组件都不会被缓存。
max - 数字。最多可以缓存多少组件实例。
生命周期函数
  1. activated

      在 keep-alive 组件激活时调用
      该钩子函数在服务器端渲染期间不被调用

  2. deactivated

      在 keep-alive 组件停用时调用
      该钩子在服务器端渲染期间不被调用

    被包含在 keep-alive 中创建的组件，会多出两个生命周期的钩子: activated 与 deactivated

    使用 keep-alive 会将数据保留在内存中，如果要在每次进入页面的时候获取最新的数据，需要在 activated 阶段获取数据，承担原来 created 钩子函数中获取数据的任务。

    注意： 只有组件被 keep-alive 包裹时，这两个生命周期函数才会被调用，如果作为正常组件使用，是不会被调用的，以及在 2.1.0 版本之后，使用 exclude 排除之后，就算被包裹在 keep-alive 中，这两个钩子函数依然不会被调用！另外，在服务端渲染时，此钩子函数也不会被调用。
————————————————
版权声明：本文为CSDN博主「什_么」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/fu983531588/article/details/90321827