封装缓存

```javascript
/* storage
 * By rat1991
 *
 */
function Storage(type) {
  if (!window && !window[type]) throw Error('当前环境不支持' + type)
  this.storage = window[type]
}
Storage.prototype = {
  /**
   * 设置缓存，可以设置过期时间，单位：秒
   * @param key 缓存名称
   * @param val 缓存的值，如果未定义或为null，则删除该缓存
   * @param exp 缓存的过期时间
   * @returns {*}
   */
  set(key, val, exp) {
    if (typeof key !== 'string') {
      console.warn(`param error! ${key} is not a string`)
      key = key + ''
    }
    if (exp && typeof exp !== 'number') {
      console.error(`param error! ${exp} is not a number`)
      exp = false
    } else {
      let toTime = +new Date()
      exp = toTime + exp * 1000
    }
    let _item = exp ? { v: val, e: exp } : val

    this.storage.setItem(key, JSON.stringify(_item))
    return _item
  },
  /**
   * 设置修改缓存的过期时间
   * @param key 缓存的名称
   * @param exp 新的过期时间 单位：秒
   */
  setExp(key, exp) {
    let _item = this.get(key)
    if (_item.e && typeof _item.e === 'number') {
      exp = +new Date() + exp * 1000
      this.set(key, _item.v, exp)
    } else if (_item) {
      this.set(key, _item, exp)
    }
  },
  /**
   * 获取缓存，如果已经过期，会主动删除缓存，并返回null
   * @param key 缓存名称
   * @returns {*} 缓存的值，默认已经做好序列化
   */
  get(key) {
    let _item = this.storage.getItem(key)
    if (_item === null || _item === undefined) return null
    try {
      _item = JSON.parse(_item)
    } catch (err) {
      console.warn(err.message)
      return null
    }
    if (_item && _item.e !== undefined && typeof _item.e === 'number') {
      let toTime = +new Date()
      if (toTime < _item.e) {
        return _item.v
      } else {
        this.remove(key)
        console.warn(`localStorage of '${key}' has expired`)
      }
    } else {
      return _item
    }
  },
  /**
   * 获取缓存的过期时间
   * @param key 缓存的名称
   * @returns {*} 单位：秒
   */
  getExp(key) {
    let _item = this.get(key)
    if (_item && _item.e) {
      return _item.e
    }
  },
  /**
   *  删除指定的缓存
   * @param key 要删除缓存的主键
   * @returns {*}
   */
  remove(key) {
    this.storage.removeItem(key)
  },
  /**
   *  清空缓存
   */
  clear() {
    this.storage.clear()
  },
  /**
   *  删除所有过期的缓存
   * @returns {Array}
   */
  clearExp() {
    const storageKeys = Object.keys(this.storage)
    const len = storageKeys.length
    if (len) return
    const caches = []
    const toTime = +new Date()
    for (let i = 0; i < len; i++) {
      let _key = storageKeys[i],
        _itme = this.get(_key)
      if (_itme.e && toTime > _itme.e) {
        caches.push(_key)
      }
    }
    caches.forEach(item => {
      this.remove(item)
    })
    return caches
  }
}
export default Storage

```



```javascript
//调用
/**
 * sessionStorage 工具
 * @returns {Object} 工具方法
 */
export const sessionUtil = new Storage('sessionStorage')
/**
 * localStorage 工具
 * @returns {Object} 工具方法
 */
export const localUtil = new Storage('localStorage')
```

