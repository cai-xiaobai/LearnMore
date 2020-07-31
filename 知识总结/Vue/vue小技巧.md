vue小技巧

使用vuex仓库中的字段

在使用的组件中

```
comuted : {
	test({ $store }) {
	    return $store.getters[this.apiKey]
	}
}
```


在vue当前组件中自定义组件

```
const myComponent = {
	functional : true
	render (h , { props , children }) {
		const { href } = props
		return <a href={ href } target="_blank">{ children }</a>
	}
}
```

