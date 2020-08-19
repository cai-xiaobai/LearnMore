## 利用Less修改element-ui组件的样式

在vue项目中用的最多的便是嵌套，利用Less嵌套的方法，在全局中划出了一个作用域，这样便能直接修改当前使用的element-ui组件样式，且不会破坏全局其他样式。

```less
<style lang="less">
.sms-alert {
  .el-dialog__header {
    font-size: 16px;
    font-weight:bold;
  }
  .el-dialog__body {
    padding-top: 0; padding-bottom: 0;
  }
}
</style>
```

Less手册:https://less.bootcss.com/