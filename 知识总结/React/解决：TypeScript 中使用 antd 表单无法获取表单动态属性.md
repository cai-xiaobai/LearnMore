## 解决：TypeScript 中使用 antd 表单无法获取表单动态属性

主要的技术栈：TypeScript + React + antd

#### 问题描述：

在使用antd 的表单Form时，根据官网文档，

| 参数     | 说明                             | 类型             |
| -------- | -------------------------------- | ---------------- |
| onFinish | 提交表单且数据验证成功后回调事件 | Function(values) |

这里为了方便观看，只给出主要代码。这是一个类组件
```react
//return中的代码
<Form
    {...layout}
    name="basic"
    className="form"
    initialValues={{ remember: true}}
    onFinish={this.onFinish}
    onFinishFailed={this.onFinishFailed}
>
    <Form.Item
        label="username"
        name="username"
    >
        <Input />
    </Form.Item>
</Form>
```

如果按照我们使用JS的习惯写这个方法，应该是从value中得到username属性的值。

```typescript
onFinish =  (values: object) => {
    console.info(values.username)
    //编译器报错：类型“object”上不存在属性“username”
}
```

但是因为TypeScript的预定义方法，并不能获取动态属性。

当时自己也懵😵，没有去看TypeScript的文档。自己去看antd的源码。古人诚不我欺，文档才是问题的最优解。

我们来梳理一下

```typescript
//FormProps接口 
onFinish?: Callbacks['onFinish']
```

第一层：onFinish 在 FormProps接口 里面是一个可选属性，对应的是 Callbacks 接口中的 onFinish 属性



```typescript
//Callbacks接口
onFinish?: (values: Store) => void;
```

第二层：onFinish 在 Callbacks接口 里面是一个可选属性，对应的是 一个方法，说明这是约束了 onFinish 必须是一个方法。但是我们的问题还没有解决，继续挖。从这我们得知 方法中一个参数 `values` 被 Store 约束。



```typescript
export interface Store {
    [name: string]: StoreValue;
}
```

第三层：Store接口 中有一种我还没见过的代码，我以为是 [属性名：属性类型] ：值类型。

到这我认为 我们自定义方法里的参数 values 需要一个 自定义接口进行约束。我进行了尝试

```typescript
//自定义接口
interface userVo {
    username? : string
}
//方法
onFinish =  (values: userVo) => {
    console.info(values.username)
}
```

没错 成功了，但是！！！！

只有 可选参数 才不报错，为什么？ 要怎么理解呢？

如果参数或者表格多，且表格各不相同就需不同接口，那工作量不就很大吗

后面我进行了修改，我把 Store 照搬过来，发现不需向之前那样自定义参数了。

```typescript
interface userVo {
    [name: string]: string
}
onFinish =  (values: lala) => {
    console.info(values.username)
}
```

`[name: string]: string` 这个究竟是什么呢，一看文档发现这叫

索引类型（Index types）

使用索引类型，编译器就能够检查使用了动态属性名的代码。

我晕了。明天再补了剩下的。