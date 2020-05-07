**Vue指令**

指令 (Directives) 是带有 `v-` 前缀的特殊 attribute。当表达式的值改变时，将其产生的连带影响，响应式地作用于 DOM。

`v-once`:数据改变时，插值处的内容不会更新。

`v-html=“abc”`:指令中的标签内容会被替换成为属性值 `abc` ，直接作为HTML

`v-bind` 指令可以用于响应式地更新 HTML attribute。

完整	v-bind:href='url'

缩写	:href='url'

动态参数的缩写	:[key]='url'

`v-on` 指令，它用于监听 DOM 事件

完整	v-on:click='doSome'

缩写	@click='doSome'

动态参数的缩写	@[event]='doSome'



`v-if   v-else   v-else-if`:条件渲染

`v-else` 元素必须紧跟在带 `v-if` 或者 `v-else-if` 的元素的后面，否则它将不会被识别。

`v-show`的元素始终会被渲染并保留在 DOM 中。`v-show` 只是简单地切换元素的 CSS 属性 `display

``v-if ``  VS  `v-show`

`v-if` 有更高的切换开销，而 `v-show` 有更高的初始渲染开销。因此，如果需要非常频繁地切换，则使用 `v-show` 较好；如果在运行时条件很少改变，则使用 `v-if` 较好。



`v-for`  列表渲染

`v-for="(value, name, index) in object"`

第一个参数为value(键值)，第二个参数为key(键名),第三个参数作为索引

key请用字符串或数值类型的值

```html
<div v-for="item in items" v-bind:key="item.id">   
    <!-- 内容 --> 
</div>
```


**动态参数**

从 2.6.0 开始，可以用方括号括起来的 JavaScript 表达式作为一个指令的参数：

