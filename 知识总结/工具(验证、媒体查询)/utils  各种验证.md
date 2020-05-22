utils  各种验证



邮箱验证

```javascript
/**
 * 邮箱验证
 * @param {String} value - 邮箱
 * @returns {Promise}
 */
export function validEmail(rules, value, callback) {
  let reg = /[\w\d]+@[a-z0-9]+\.[a-z]{2,4}$/
  if (value && !reg.test(value)) {
    callback('邮箱格式不正确')
  } else {
    callback()
  }
}
```

手机号码验证

```javascript
/**
 * 手机号码验证
 * @param {String} value - 手机
 * @returns {Promise}
 */
export function validTel(rules, value, callback) {
  let reg = /^1[0-9]{10}$/
  if (value && !reg.test(value)) {
    callback('手机号码格式不正确')
  } else {
    callback()
  }
}
```

