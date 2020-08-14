## 解决 el-form 异步校验导致重复校验的问题

### 业务逻辑

最近项目上有个业务，确认表单，需要用到手机验证码，验证是否为本人操作。这挺常见的，但是需要用户填写完，输入框失去焦点时就将验证码发送后台进行验证，而且验证码只能输一次。

### 触发场景

若用户输入完验证码后，鼠标焦点并未离开，直接点击确认的话，因为 validator 异步的关系，鼠标离开焦点触发一次校验，确认也触发一次校验，导致 重复校验，用户体验极差

### 解决办法

用watch 动态监听 form 表单里验证码输入框的值，并进行相应判断，输入长度为6时自动触发验证

```html
<template>
	<el-form ref="form" :model="form" :rules="rules" label-width="160px" label-suffix=" :">
		<el-form-item label="手机验证码" prop="sms" :rules="{ validator : sms , required : true , trigger : 'blur'}" key="sms">
            	<!-- 验证码输入框 -->
                <el-input v-model="form.sms"  maxlength="6" show-word-limit/>
            	<!-- 发送验证码组件 -->
                <simple-sms  :api="POST_DSP_SEND" />
            </el-form-item>
	</el-form>
</template>
```

```javascript
<script>
watch : {
	'form.sms': function (val) { //单个数据验证
  		if(val!= undefined && val.length == 6){
    		this.validateField('sms')
  		}
	}
} ,
methods : {
	//表单单项验证
    validateField(fieldName) {
      this.$refs.form.validateField(fieldName)
    } ,
    // 验证码验证
    sms(r , v , c) {
      if (!v) {
        return c(new Error('请输入验证码'))
      } else if (this.validPass){  // validPass 字段代表验证成功
        c()
      }
      if(!this.loading){
        this.loading = true
        http.post(this.POST_DSP_VERIFY, {code: v}).then(() => {
          // 验证成功
          this.validPass = true
          return c()
        }).catch(() => {
          return c(new Error('请输入正确验证码'))
        }).finally(()=>{
          // 按钮启用
          this.loading = false
        })
      }
    },
}
</script>
```



这里主要是提供一个思路，利用watch去监听form的数据，如果你有更好的办法，非常欢迎你在评论教教我