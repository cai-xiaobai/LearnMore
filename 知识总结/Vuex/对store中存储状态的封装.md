# 对store中存储状态的封装



一般使用到Vuex仓库内数据的状态有四种：

1、state仓库  2、getters 计算属性

3、mutations 更新数据的方法  

4、actions 和mutation功能大致相同

首先是对这几种状态的不同进行方法封装。



```javascript
/**
 * 开火车
 * @param ks [str..]
 * @param ps [obj|fn]
 * @param rs {...}
 */
export function train (ks , ps , rs = { }) {
  Object.keys(ks).forEach(k =>
    rs[k] =
      typeof ps !== 'function' ? ps : ps.length <= 1 ? ps(k) : // state
        (a , b , c) => ps(k , a , b , c)) //..
  return rs;
}
```

第一个参数为 我们获取到封装好的apis的js文件，然后进行循环遍历，以命名的api存入对象rs中，相对应的值则进行下一步判断。

第二个参数若为 对象或其他 直接输出，若为方法，则判断方法内的参数是否小于等于1 ，是则将命名的api作为方法的参数；大于1的将命名的api作为方法的第一参数，其他为剩余参数。

然后对整个rs对象进行输出。

这里拿取 【系统常量】和 【下拉框选项】 的state进行举例说明

```javascript
//系统常量 默认从浏览器缓存获取
  state     : train(keys , key => storage.get(key)),
//下拉框选项 默认 null
  state     : train(keys , null),
```

```javascript
/**
 * 获取订单类型
 */
export const ORDER_TYPE = {
  prefix , method: 'GET' , url: '/getOrderType'
}
```

订单类型普遍使用的较多，我们放入系统常量的apis文件【constant.js】中

train的第一个参数 keys 便是 constant.js ； 第二个参数为箭头函数，是方法且参数为1，则直接将 `ORDER_TYPE`  作为key，从浏览器缓存获取 `ORDER_TYPE` 

而像下拉框选项，一般我们在使用到时才会获取，那么第二个参数为null，默认为null
