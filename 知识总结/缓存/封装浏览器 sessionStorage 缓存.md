封装浏览器 sessionStorage 缓存

sessionStorage 用于临时保存同一窗口(或标签页)的数据，在关闭窗口或标签页之后将会删除这些数据。

```javascript
const
  // sessionStorage
  storage = {
    set : (k , v) => {
      try {
        const str = JSON.stringify(v)
        sessionStorage.setItem(k , str)
      } catch (e) {
        console.warn('storage.set => JSON.stringify:', k , v)
        return v
      }
    },
    get : k => {
      const r = sessionStorage.getItem(k)
      try {
        return r && JSON.parse(r)
      } catch (e) {
        console.warn('storage.get => JSON.parse:', k , r)
        return r
      }
    },
    del : k => k && sessionStorage.removeItem(k)
  }
;
```

