Vue组件通信

一般是三种情况：

父组件给子组件传递信息；子组件给父组件传递信息；子组件之间传递信息

父传子——props

子组件只能调用，不能修改。如果需要修改，在子组件内部定义数据，或使用计算属性

```javascript
props:['title']
//1.子组件内部定义数据
data(){
    return{
        local_title:this.title
    }
}
//2.使用计算属性
computed:{
    local_title:()=>{
        return this.title.trim()
    }
}
```

子传父——$emit

调用父组件传递过来的方法，还可以指定参数传递过去。

```javascript
//父组件
<ListItem @modifyItem />
...
methods:{
    modifyItem(index,changeValue)=>{
        this.lists[index].title = changedValue;
    }
}
//子组件
<input @blur=$emit('modifyItem',index,local_title) />
```

子组件通信——Vuex

将变量的内容提到最高层级。