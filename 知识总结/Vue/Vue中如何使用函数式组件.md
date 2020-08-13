# Vue中如何使用函数式组件

[TOC]

### **使用场景**

<font color="green">`Vue`</font> 推荐在绝大多数情况下使用 `template` 来创建你的HTML。但是现实的业务场景中经常会遇到根据用户权限判断，显示不同的按钮。

接下来我们由浅入深，通过两个例子来了解。首先了解什么是函数式组件。

### **什么是函数式组件**

函数式组件在 <font color="blue">`React`</font> 中非常流行，那在 <font color="green">`Vue`</font>  中我们也可以玩得很 6 吗？

> 通过官方文档了解到 函数式组件 属于渲染函数能够使用 JavaScript 的完全编程的能力。没有管理任何状态，也没有监听任何传递给它的状态，也没有生命周期方法。它只是接受一些 prop 的函数。

我们可以把函数式组件想象成组件里的一个函数，入参是 渲染 上下文 (render context)，返回值是渲染好的HTML

Stateless无状态：组件自身没有状态

Instanceless无实例：组件自身没有实例，也就是没有this

### **如何创建一个函数式组件**

首先将组件标记为 <font color="red">`functional`</font> ，这意味它无状态、无实例。一个函数式组件就像这样：

```jsx
Vue.component('my-component',{
	functional : true,
	render : function(createElement , context){
		//...
	}
})
```

<font color="red">`context`</font> 中包含以下字段的对象：

- <font color="red">`props`</font> ：提供所有 prop 的对象
- <font color="red">`children`</font> ：VNode子节点的数组
- <font color="red">`slots`</font> ：一个函数，返回了包含所有插槽的对象
- <font color="red">`scopedSlots`</font> ：（2.6.0+）一个暴露传入的作用域槽的对象。也以函数形式暴露普通插槽。
- <font color="red">`data`</font>：传递给组件的整个数据对象，作为 <font color="red">`createElement`</font> 的第二个参数传入组件  
- <font color="red">`parent`</font> ：对父组件的引用
- <font color="red">`listeners`</font> ：（2.3.0+）一个包含了所有父组件为当前组件注册的事件监听器的对象。这是 <font color="red">`data.on`</font> 的一个别名 
- <font color="red">`injuctions`</font> ：（2.3.0+）如果使用了 <font color="red">`inject`</font> 选项，则该对象包含了应当被注入的 property。 

### **为什么使用函数式组件**

第一，因为函数式组件只是函数，渲染开销会低很多。

第二，更灵活，它能根据传入 prop 的值来代为渲染更具体的组件。

### **简单的例子**

假若我们封装一套按钮组件，通过后台返回的type进行渲染，首先可能想到的是 <font color="green">`v-if`</font> 实现

```vue
<div v-if="type === 'success'">success</div>
<div v-else-if="type === 'error'">error</div>
<div v-else-if="type === 'warm'">warm</div>
<div v-else>default</div>
```

初看是没有问题的，但这样并不好扩展，若想扩展就难道一直 `v-else-if` ， 难顶

这里我们就可以用到函数式组件了，提前说明：

1. 对比传统的 Vue.component 这样定义，我更喜欢用变量定义。

2. h 即是 createElement ，以下是尤雨溪在一个回复中提到的

> It comes from the term "hyperscript", which is commonly used in many virtual-dom implementations. "Hyperscript" itself stands for "script that generates HTML structures" because HTML is the acronym for "hyper-text markup language".
> 它来自单词 hyperscript，这个单词通常用在 virtual-dom 的实现中。Hyperscript 本身是指
> 生成HTML 结构的 script 脚本，因为 HTML 是 hyper-text markup language 的缩写（超文本标记语言）

```jsx
const typeButton = {
	functional:true,
	render(h , { props }){
		const { type } = props
        return <div class={ type }>{type}</div>
	}
}
```

这就是一个简单的函数式组件，根据传入的type不同，渲染出不同的

样式的按钮

### **业务场景下的函数式组件**

通过【绑定状态】、【操作状态】进行不同按钮的渲染。话不多说，上代码

```jsx
const Confirm = {
	functional : true,
	render (h , { parent , props , listeners }) {
		// 绑定状态：0-待确认绑定；1-已绑定；2-待确认解绑；3-已解绑；4-已取消
    	const { vo } = props , { bindStatus } = vo
    	
    	// 操作状态 1-同意绑定；2-解绑；3-同意解绑；4-取消添加
        //根据操作状态选择文案
        const btnText = ['','同意绑定','解绑','同意解绑','取消添加'][bindStatus]
        
        //为组件添加点击方法
        const btnClick = () => listeners.click(vo , doState)
        return doState > 0 ? <el-button type="text" onClick={ btnClick }>{ btnText }</el-button> : ''
	}
}
```

```vue
<template>
	<handle-confirm :vo="row" @click="handleConfirm"/>
</template>
```

其实这只是个精简版，真实业务场景下可能还要判断【账户类型】以及多账户交互，这些就要多加逻辑了，相信到这应该了解在Vue中如何使用函数式组件了。

PS: 如果无需逻辑判断 return 也可以是以下这样子

```jsx
const style = 'display: inline-flex; width: 150px;'
return h('el-input' , { style , props: { size : 'small' } })
```

